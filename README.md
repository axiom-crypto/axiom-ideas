# axiom-ideas


## Existing Projects

### [MEVictim Rebate](https://github.com/howdymary/ETHNYC23)
Summary
- This project identifies victims of MEV attacks on ETH Goerli and grants these victims an ERC-721 token that qualifies the holder to access a token-gated Uniswap v4 pool on Scroll.

Axiom’s involvement
- Axiom generates proof that an address is indeed the victim in a frontrunning MEV attack and verifies the proof onchain to trigger airdrop

### [Uniswap LP Rebalancing](https://github.com/saucepoint/v4-axiom-rebalancing)
Summary
- This project allows anyone to rebalance LP’s liquidity trustlessly. 

Axiom’s involvement
- Anyone can observe a change in market conditions. Upon a sufficient change in market conditions (defined by LPs), the actor can use Axiom to generate a proof and initiate execution. 

### [Old Account](https://github.com/jdubpark/Uniswap-Hooks/blob/main/contracts/OldAccountHook.sol)
Summary
- Allows only old accounts to access Uniswap V4 pools. 

Axiom’s Involvement
- Use Axiom to retrieve and verify the birth block of an account and determine whether it is “old” enough to access the pool

### [Anonymous Gated Access](https://twitter.com/swarmsapp/status/1716443999127453922?s=46&t=JMNKcoJOBwP4NAft8U0YJA)
Summary
- A community that only allows addresses above a certain age to join. Users do not have to reveal their addresses in order to verify their account age. 

Axiom’s involvement
- Use Axiom to retrieve and verify the birthblock of an account. The anonymity is achieved via Semaphore. 

### [Ante Tests](https://x.com/AnteFinance/status/1707999323584360595?s=20)
Summary
- Ante is an on-chain security test protocol. It supports on-chain logic that verifies whether a smart contract meets certain conditions. With Axiom, Ante can support not only real-time tests but also historical tests

Axiom’s involvement
- Ante can call Axiom’s smart contract to retrieve and verify certain historical information about an address and check whether the ante test conditions are met

### Provable Social Graph
Summary
- Provable social graph is a smart contract that measures and proves relationships between addresses. The smart contract indexes addresses’ onchain activities and find common ground between them. The common ground can be that they have all owned the same NFTs before or they have all interacted a protocol before a certain date. The social graph helps addresses figure out these relationships. 

Axiom’s involvement
- The social graph smart contract can submit the data queries on multiple addresses to Axiom. The result is tied back to a callback address that can be a user of the social graph or other smart contracts. 
A more advanced iteration of the social graph would standarize the evaluation of relationships and write the evaluation framework into a circuit to be computed offchain. 

### Provable Cowswap
Summary
- Cowswap is DEX aggregator that supports limit and TWAP orders. Unlike other DEX, Cowswap first seeks a coincidence of wants within a batch of orders before settling them against DEX liquidity. Orders on Cowswap are settled by solvers, who stake to provide services. A solver can only receive full protocol rewards after all transactions have been settled successfully. Provable Cowswap improves this process by using Axiom to trustlessly guarantee that all orders are executed. 

Axiom’s involvement
- Axiom is used in the Cowswap protocol before the rewards are distributed and after orders are executed. After a driver submit and successfully execute a batch of orders onchain, the solver who wins this batch can claim rewards via the Cowswap protocol. When she claims, the Axiom contract is called to query receipt history and verify that all transactions have been completed successfully before releasing the rewards. 

### Unsupported Asset Refund
Summary
- An application (exchange) may only support a few assets and does not want to deal with the others. When users accidentally send them incompatible assets, Axiom can offer a scalable, decentralized way to refund them. For example, an application that only accepts USDC may want to refund users who send USDT. With Axiom, a user can trustlessly prove that they have sent USDT to the smart contract and claim the tokens back.

Axiom’s involvement
- The main problem with onchain refund is that app developers usually do not have the time, nor want to assume the liability, to issue customer refunds. With Axiom, the refund process can be self-serve and decentralized. A user can initiate a transaction to start refunding. The refund smart contract will call Axiom to check whether the user has indeed sent USDT. If such transaction exists, a proof will be submitted and verified onchain. Upon verification, a refund will be issued to the user. 


## New Ideas
### Circuit breaker based on rolling AUM
What is it?
- A smart contract circuit breaker that enables a DeFi protocol to stop accepting deposits whenever its rolling AUM drops below a certain threshold. This function helps protect user funds during rapid protocol-level attacks or volatile market movements.
How does it work? 
Whenever a withdraw transaction is initiated, the circuit breaker smart contract is also called. The circuit breaker smart contract then calls the Axiom contract to check on the protocol’s historical AUM (i.e. ETH balance in the smart contract address) and determine the rolling AUM. If the rolling AUM is lower than the threshold, then the circuit breaker will trigger a deposit lock. The TVL data can be accessed with Axiom V1. The calculation performed on top of the data can be achieved with Axiom V2. 

Why use Axiom? 
- It is possible to make a circuit breaker without Axiom. However, the circuit breaker can only access real time or up to ~1 hour historical TVL data. This could result in the circuit breaker being triggered prematurely, if the threshold is set too high, or never triggered, if the threshold is set too low. Axiom allows the circuit breaker to access TVL data from an arbitrary time range and keep the most up to date info on a rolling basis without taking up onchain storage. It gives more flexibility in terms of setting the right parameters for the circuit breaker.  

### Dynamic utilization rate for lending protocols
What is it?
- A smart contract that dynamically adjust the ultilization kink of a lending protocol based on historical data

How does it work? 
- Lending protocols such as Compound use the utilization rate to determine interest rates. The utilization rate includes a fixed kink factor, above which interest rates increase more rapidly. With Axiom, the kink can be dynamically determined. 
For example, we can design a kick adjustment contract for Compound V3 that is maintained by a keeper network. The contract calls Axiom contracts to retrieve the historical utilization rate (emitted as event when the lending contract is called). Additional logic can be applied to this data in the Axiom REPL circuit. The logic can be as simple as, if the rolling 30d utilization rate of cUSDC is greater than 80%, then reduce the kick by 1%. The kick will increase once the rolling rate goes back to 80%. The result will be returned to the adjustment contract in a callback function that either leads to no change or an update to the kick. 

Why use Axiom? 
- Finetuning the kick factor is one of the most important tasks of a lending protocol. This process is currently managed in a rather opaque manner because consulting firms such as Gauntlet are reluctant to pulicize their finetuning algorithms. Additionally, the lengthy governance process to update the kick adds unnecessary latency and could harm the protocol in extreme conditions. Axiom solves these problems by creating a trustless, flexible, and private framework for protocol finetuning. Consultants can opt to keep their logic private and offchain with Axiom, and the adjustment happens in real time when it is triggered. 

### Weighted votes based on account activities
What is it?  
- A smart contract that assigns more weight to votes cast by accounts that have interacted with certain protocols before, 

How does it work? 
- When a user calls the voting smart contract to cast a vote, Axiom’s contracts are subsequently called to check the voter’s past transactions. A simple example can be assigning any voters who have voted once before 2x weight to their current vote. The weight can be assigned onchain after Axiom returns the transaction result. 

Why use Axiom? 
- It was previously impossible to achieve weighted voting seamlessly onchain. To do this, developers had to set up a front end to collect user addresses, check their transaction history against their own nodes, then return a signed message for the transactions to continue to be processed onchain. Axiom drastically simplifies this workflow and upgrade it to protocol level so users can access it anywhere (instead of being limited to the front end).

### Uniswap V4 Preferred LP Hook
What is it?
- A hook that provides tailored rewards based on LPs’ on-chain credentials. 

How it works?
- The hook should be implemented before and/or after “modifyPosition.” Credentials may include cumulative historical liquidity, past LP PnL, etc. For example, when an LP initiates a transaction to add liquidity, the hook will call Axiom contracts to retrieve the LP's previous transactions to determine the total historical liquidity she has provided to this pool. If the number is above some threshold, she can deposit her liquidity into a higher fee tier.

Why use Axiom? 
- Similar to the weighted vote idea above, it is possible to do this without Axiom with a less elegant, partly offchain solution. Axiom allows a feature like this to be natively integrated into Uniswap V4 hooks. 


### Complex LP strategies
What is it?
- A hook that gives LP more flexibility in terms of adjusting their strategies based on market data.

How it works? 
- For example, an LP may want to adjust her position based on the liquidity delta in the pool over the past 6 hours. An automated LP hook can automatically



- She can use Axiom to fetch, calculate, and verify this data from the hook contract and trigger follow-up actions if conditions are met. Other historical data LPs may be interested in including trading volume, price, realized volatility, depth, etc.

  
### Automated ecosystem buybacks
What is it?
- A hook that initiates token buy-back and/or burn whenever the rolling price hits specific thresholds.


How it works? 
- For example, a buy-back is triggered whenever the 24-hour rolling price drops below $1. Every time a swap transaction takes place, a Uniswap V4 hook will use Axiom to retrieve historical price data (see an implementation example [here](https://blog.axiom.xyz/trustlessly-accessing-uniswap-twap-oracles-with-axiom/)), calculate its rolling average over 24 hours, and decide whether a buy-back is needed. 


