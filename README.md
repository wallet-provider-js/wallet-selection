# Wallet-selection
Lightweight module to provide wallet selection for DApp users.

## Assumptions

* For use in a WebApp UI for blockchain smart contracts

## User Flow

* When the user clicks on the WebApp [ Login / Connect Wallet ] button, a function from this libary is called
* The function presents the user a screen to select a wallet.
* When the user selects a wallet, the function resolves into a `ProviderInformation` structure
* `ProviderInformation` contains data about the wallet: name, capabilities, url to wallet-provider.js file for this particular wallet
* All `wallet-providers.js` implement the [wallet-provider-interface](https://github.com/wallet-provider-js/wallet-provider-interface) 
* The DApp can use ES2020 Dynamic Import to download `wallet-provider.js`, or it can have several providers pre-included and just choose which one to activate
* The DApp uses the selected `wallet-provider` capabilities
 * Sign Message [required] (just sign an arbitrary message)
 * Unsigned View-Call [optional] (call a view-function not requiring signature in the chain) 
 * Send Tx (Sign & Send) [optional] (sign and send a Tx into the blockchain)
 * Login/Logout [required]

In order to keep the providers as simple as possible, key management, contract ABI, Tx composition and other functionality are intentionally kept outside of the scope of a wallet provider, but can be added by separate supporting libraries.

## Provider basic Functionalities

### Sign Message [required] 

The provider receives a message string and returns a signed message. 

### View-Call (optional)

The provider queries general data from the blockchain, or calls a function in a smart contract that does not require the tx to be signed.

### Send Tx (Sign & Send) [optional]

The provider receives a transaction, signs the Tx, send it to the blockchain and returns: `Tx-hash` & `status` to the webApp. Providers can be "complete-tx-flow" vs "pending-tx-flow". Providers with "complete-tx-flow" capabilities always return status "completed" (when the blockchain finality is under 10 secs, e.g. NEAR). Providers with "pending-tx-flow" capabilities return a tx-hash and status "pending". In this case, the webApp must periodically use an `unsigned-view-call` to check the tx status.

### Login/Logout [required]

The provider receives the smart contract account id, and perform login/logout actions depending on the walllet. For some wallet/connection mechanisms no actions might be needed.

The wallet-provider-interface is defined here: https://github.com/wallet-provider-js/wallet-provider-interface


## Technology

This lightweight library should use DOM-injection and plain typescript to be usable on most DApps.

## References

https://gov.near.org/t/wallet-selection-sdk/215/6

https://docs.blocknative.com/onboard

