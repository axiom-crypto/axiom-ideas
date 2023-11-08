## Summary
An application (exchange) may only support a few assets and does not want to deal with the others. When users accidentally send them incompatible assets, Axiom can offer a scalable, decentralized way to refund them. For example, an application that only accepts USDC may want to refund users who send USDT. With Axiom, a user can trustlessly prove that they have sent USDT to the smart contract and claim the tokens back.

## Axiom’s involvement
The main problem with onchain refund is that app developers usually do not have the time, nor want to assume the liability, to issue customer refunds. With Axiom, the refund process can be self-serve and decentralized. A user can initiate a transaction to start refunding. The refund smart contract will call Axiom to check whether the user has indeed sent USDT. If such transaction exists, a proof will be submitted and verified onchain. Upon verification, a refund will be issued to the user. 
