# Aave Wallet Credit Scoring

This project builds a machine learning pipeline that assigns a credit score (0–1000) to wallets interacting with the Aave V2 DeFi protocol.

## Objective

To score wallets based on their historical transaction behavior — higher scores mean safer, responsible users; lower scores may indicate risky or malicious behavior.

## Architecture

- **Input**: Raw JSON file of 100K+ user transactions.
- **Process**:
  1. Load JSON data
  2. Engineer wallet-level behavioral features
  3. Apply a heuristic + ML-inspired scoring function
- **Output**: CSV file with `{ wallet, score }`

## Feature Engineering

- Total transactions
- Number of borrows, repays, deposits
- Liquidation count
- Borrow-to-repay ratio
- Activity span (active days, avg tx gap)

## Scoring Model

A hybrid approach based on:
- Higher active usage
- High repayments relative to borrow
- Few or no liquidation calls
- Consistent, time-spread behavior (non-bot-like)

Weights are tunable; current version uses linear weighted scoring.

## How to Run

```bash
pip install pandas numpy scikit-learn tqdm
python score_wallets.py
