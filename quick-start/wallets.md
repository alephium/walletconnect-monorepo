---
description: Quick Start For Wallets With Examples
---

## Quick Start For Wallets \(Client SDK - react-native\)

{% hint style="info" %}
You can use the **Example Dapp** to test your integration at [example.walletconnect.org](https://example.walletconnect.org) \([Source code](https://github.com/WalletConnect/walletconnect-example-dapp)\)
{% endhint %}

### Install

{% tabs %}
{% tab title="yarn" %}
```bash
/**
 *  Install NPM Package
 */

yarn add @walletconnect/react-native

/**
 *  Polyfill NodeJS modules for React-Native
 */

npm install --save rn-nodeify

rn-nodeify --install --hack
```
{% endtab %}

{% tab title="npm" %}
```bash
/**
 *  Install NPM Package
 */
 
npm install --save @walletconnect/react-native

/**
 *  Polyfill NodeJS modules for React-Native
 */

npm install --save rn-nodeify

rn-nodeify --install --hack
```
{% endtab %}
{% endtabs %}

### Initiate Connection

```javascript
import RNWalletConnect from '@walletconnect/react-native'

/**
 *  Create WalletConnector
 */
const walletConnector = new RNWalletConnect(
  {
    uri: 'wc:8a5e5bdc-a0e4-47...TJRNmhWJmoxdFo6UDk2WlhaOyQ5N0U=',       // Required
  },
  {
    clientMeta: {                                                       // Required
      description: "WalletConnect Developer App",
      url: "https://walletconnect.org",
      icons: ["https://walletconnect.org/walletconnect-logo.png"],
      name: "WalletConnect",
      ssl: true
    },
    push: {                                                             // Optional
      url: "https://push.walletconnect.org",
      type: "fcm",
      token: token,
      peerMeta: true,
      language: language
    }
  }
);

/**
 *  Subscribe to connection events
 */
walletConnector.on("call_request", (error, payload) => {
  if (error) {
    throw error;
  }

  // Handle Call Request
  
  /* payload:
  {
    id: 1,
    jsonrpc: '2.0'.
    method: 'eth_sign',
    params: [
      "0xbc28ea04101f03ea7a94c1379bc3ab32e65e62d3",
      "My email is john@doe.com - 1537836206101"
    ]
  }
  */
});

walletConnector.on("disconnect", (error, payload) => {
  if (error) {
    throw error;
  }

  // delete walletConnector
});
```

### Manage Connection

```javascript
/**
 *  Approve Session
 */
walletConnector.approveSession({
  accounts: [
    '0x4292...931B3',
    '0xa4a7...784E8',
    ...
  ],
  chainId: 1
})

/**
 *  Reject Session
 */
walletConnector.rejectSession()


/**
 *  Kill Session
 */
walletConnector.killSession()
```

### Manage Call Requests

```javascript
/**
 *  Approve Call Request
 */
walletConnector.approveRequest({
  id: 1,
  result: "0x41791102999c339c844880b23950704cc43aa840f3739e365323cda4dfa89e7a"
});

/**
 *  Reject Call Request
 */
walletConnector.rejectRequest({
  id: 1,
  result: null
});
```
