### Date: 2020-03-04

### Summary
The market oracle data provider operated by Ampleforth detected that one of it’s previously reported values was incorrect and rolled back its own report before use.

### Impact
No user impact

### Root Causes
Estimation error in the VWAP aggregation logic of the data provider’s webservice.

### Trigger
Extreme trader activity on day of calculation:

![Trade](/incident-reports/assets/2020-03-04_trade.png "The trade")

### Resolution 
Data provider rolled back its own report by calling the oracle’s [purgeReport()](https://github.com/ampleforth/market-oracle/blob/11d8b95372fb2d03de03f53343187f445cb234a4/contracts/MedianOracle.sol#L140) function.

The oracle design allows data providers to do this, but only to their own reports.

This is not considered an administrator or governance action, because it was executed as a data provider as part of the expected fallback flow.

### Detection
Dev team alerted to high oracle rate on dashboard:

![Price](/incident-reports/assets/2020-03-04_price.png "The price")

### Action Items
1. Data provider updated its aggregation logic to the most precise form, eliminating estimation. (In progress)
2. Added monitoring logic to detect when >x% change occurs within time window (Done)

### Lessons Learned
- Team had already been working on the more precise calculation algorithm, but had not completed testing. We should have prioritized rolling this out earlier.

### Timeline
**Wednesday 3/4/20**
- 5:33pm Team alerted to high oracle value on dashboard
- 5:44pm purgeReport() called to remove bad value:

Transaction ID: [0xac2444133717b97b27cada14deb4b51a2c2e10efd4741bfbf9526f37ed022b80](https://etherscan.io/tx/0xac2444133717b97b27cada14deb4b51a2c2e10efd4741bfbf9526f37ed022b80)
