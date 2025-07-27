 Wallet Risk Scoring From Scrath
 
 Problem Statement

You are provided with 100 wallet addresses and asked to build a wallet-level risk scoring model(range: 0 to 1000) using on-chain transaction data from the **Compound V2/V3 protocol**.

---

Data Collection Method

We used the (Covalent API) to fetch on-chain transaction data for each wallet address. For each wallet:

Queried endpoint: `/v1/{chain_id}/address/{wallet}/transactions_v2/`
Chain used: Ethereum (chain\_id: 1)
Data collected:


All records were merged into a single file: (wallet_transactions.csv)

 Feature Engineering--

From the raw transactions, we derived the following **wallet-level aggregated features**:

 | Description                                          |
 | ---------------------------------------------------- |
 | Total number of transactions                         |
 | Sum of ETH value transferred                         |
 | Mean ETH value per transaction                       |
 | Total gas used by wallet                             |
 | Average gas per transaction                          |
 | Fraction of successful transactions                  |
 | Days since first transaction (relative to last date) |
 | Days since last transaction                          |


Scoring Method

We normalized the wallet features using **Min-Max Scaling** and applied weighted scoring:
 Risk Score (0-1000)

(0.3 × transaction activity score) +
(0.2 × gas usage score) +
(0.3 × value transferred score) +
(0.2 × recency score

Each component was rescaled to 0–1, then multiplied by 1000 to get the final risk score.


Risk Indicators Justification

Infrequent Activity or Dormant Wallets** (long gap since last transaction) may indicate lost keys or compromised wallets.
Low Success Rate** suggests transaction failures, potentially signaling technical mismanagement or bots.
Low Gas Usage** might mean low participation in high-value protocols.
Low ETH Value Transferred** could imply poor economic activity.

These indicators are aligned with real-world DeFi wallet risk metrics.



Output

Final output is saved in:

 submission.csv with 2 columns: :wallet_id,score
