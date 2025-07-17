# Aave V2 Credit Scoring – Score Distribution and Behavioral Insights

This analysis summarizes the behavior of DeFi wallets on Aave V2 based on historical transaction data. A custom credit scoring model outputs a score between 0 and 1000 for each wallet. The aim is to derive meaningful behavioral patterns and highlight how on-chain activity correlates with risk.

---

## Credit Score Distribution

**Total Wallets Scored:** 3,497  
**Score Range:** 0 to 1000

The distribution of credit scores is heavily skewed toward the lower end of the range.

![Score Distribution Histogram](insert-your-image-path-here)

### Observations:
- A large majority of wallets scored between 0 and 100.
- Very few wallets scored above 200.
- This skew suggests either:
  - Many wallets are inactive, non-repaying, or exhibit poor financial behavior.
  - Or, the current scoring function aggressively penalizes common DeFi patterns like high borrow-to-repay ratios and infrequent transactions.

---

## Scoring Logic Summary

Each wallet's score is derived from behavioral features extracted from their transaction history.

### Feature Breakdown:

| Feature Name            | Description                                                             |
|-------------------------|-------------------------------------------------------------------------|
| total_actions           | Number of total actions (deposit, borrow, repay, etc.)                  |
| active_days             | Number of unique days with any transaction                              |
| num_deposits            | Number of deposit actions                                               |
| num_borrows             | Number of borrow actions                                                |
| num_repays              | Number of repay actions                                                 |
| num_liquidations        | Number of times the wallet was liquidated                               |
| deposit_amount          | Total deposited amount                                                  |
| borrow_to_repay_ratio   | Ratio of borrowed amount to repaid amount                               |
| avg_tx_gap              | Average time (in seconds) between consecutive transactions              |

### Scoring Formula:

```python
raw_score = (
    usage_score
    + deposit_score * 2
    - repay_ratio_score * 5
    - liquidation_penalty
    - tx_gap_penalty
)
```
Where:

usage_score = total_actions + active_days

deposit_score = log(1 + deposit_amount)

repay_ratio_score = min(borrow_to_repay_ratio, 10) (999 if no repayment)

liquidation_penalty = num_liquidations * 5

tx_gap_penalty = min(avg_tx_gap / 86400, 30)

###The final score is linearly scaled to lie between 0 and 1000.

##Key Takeaways
###Most wallets are scoring poorly due to:

No repayments or poor repayment ratios.

Sparse or one-time usage patterns.

Liquidations that heavily impact the score.

###To improve the score distribution:

Introduce thresholds or soft caps on penalties.

Consider assigning more weight to positive behaviors like consistent deposits and repayments.


