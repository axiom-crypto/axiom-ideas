## Summary
Cowswap is DEX aggregator that supports limit and TWAP orders. Unlike other DEX, Cowswap first seeks a coincidence of wants within a batch of orders before settling them against DEX liquidity. Orders on Cowswap are settled by solvers, who stake to provide services. A solver can only receive full protocol rewards after all transactions have been settled successfully. Provable Cowswap improves this process by using Axiom to trustlessly guarantee that all orders are executed. 

## Axiom’s involvement
Axiom is used in the Cowswap protocol before the rewards are distributed and after orders are executed. After a driver submit and successfully execute a batch of orders onchain, the solver who wins this batch can claim rewards via the Cowswap protocol. When she claims, the Axiom contract is called to query receipt history and verify that all transactions have been completed successfully before releasing the rewards. 
