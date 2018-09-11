# Quick Start

## For Dapps (Browser SDK)

1.  Setup

```bash
yarn add walletconnect

# OR

npm install --save walletconnect
```

2.  Implementation

```js
import WalletConnect from 'walletconnect'

/**
 *  Create a webConnector
 */
const webConnector = new WalletConnect(
  {
    bridgeUrl: 'https://bridge.walletconnect.org',  // Required
    dappName: 'INSERT_DAPP_NAME',                   // Required
  }
)

/**
 *  Create a new session
 */
const session = await webConnector.initSession()

if (session.new) {
  const { uri } = session; // Display QR code with URI string

  const sessionStatus = await webConnector.listenSessionStatus() // Listen to session status

  const accounts = sessionStatus.data // Get wallet accounts
} else {
  const { accounts } = session // Get wallet accounts
}

/**
 *  Draft transaction
 */
const tx = {from: '0xab12...1cd', to: '0x0', nonce: 1, gas: 100000, value: 0, data: '0x0'}

/**
 *  Create transaction
 */
const transactionId = await webConnector.createTransaction(tx)

/**
 *  Listen to transaction status
 */
const transactionStatus = await webConnector.listenTransactionStatus(transactionId)

if (transactionStatus.success) {
  const { txHash } = transactionStatus // Get transaction hash
}
```

## For Wallets (React-Native SDK)

1.  Setup

```bash
/**
 *  Install NPM Package
 */

yarn add rn-walletconnect-wallet

# OR

npm install --save rn-walletconnect-wallet

/**
 *  Nodify 'crypto' package for cryptography
 */

# install "crypto" shims and run package-specific hacks
rn-nodeify --install "crypto" --hack
```

2.  Implementation

```js
import RNWalletConnect from 'rn-walletconnect-wallet'

/**
 *  Create WalletConnector (using the URI from scanning the QR code)
 */
const walletConnector = new RNWalletConnect(uri)

/**
 *  Send session data
 */
await walletConnector.sendSessionStatus({
  fcmToken: '12354...3adc',
  pushEndpoint: 'https://push.walletconnect.org/notification/new',  
  data: {
    accounts: [
      '0x4292...931B3',
      '0xa4a7...784E8',
      ...
    ]
  }
})

/**
 *  Handle push notification events & Get transaction data
 */
FCM.on(FCMEvent.Notification, event => {
  const { sessionId, transactionId } = event;

  const transactionData = await walletConnector.getTransactionRequest(transactionId);
});

/**
 *  Send transaction status
 */
await walletConnector.sendTransactionStatus(transactionId, {
  success: true,
  txHash: '0xabcd...873'
})

/**
 *  Get all transactions from bridge
 */
const allTransactions = await walletConnector.getAllTransactionRequests();

// allTransactions is a map from transactionId --> transactionData
const transactionData = allTransactions[someTransactionId];
```
