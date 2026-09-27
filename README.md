# Bank Customer & Transaction Risk Profiling

## Business Problem

The goal of this project was to create a tool supporting the Fraud team in identifying potentially high-risk transactions. The analysis focused on identifying transactions that combine key risk factors, determining which merchant categories generate the highest number and proportion of HIGH_RISK transactions, and assessing how their volume translates into financial exposure. The project also included the development of a consistent and clearly defined logic for prioritizing cases for manual review.

## Dataset

The project uses a synthetic banking dataset consisting of 20 customers and 200 transactions. The data includes customer segments, initial risk categories, account age, transaction timestamps and amounts, transaction status, transaction channels, merchant categories, and night transaction indicators.

The dataset was designed to support both transaction-level risk analysis and customer profiling. Customer profiles were enriched with aggregated metrics such as transaction volume, total and average transaction amount, decline rate, night transaction rate, and the number of transactions classified as HIGH_RISK. These metrics were then used to calculate a customer risk score and identify customers with potentially elevated levels of risk or activity.

Additionally, a separate sample of 20 HIGH_RISK transactions was manually reviewed to simulate an operational review process and assess the application of the prioritization logic to a selected sample of flagged transactions.

## Risk Logic

The transaction risk logic evaluates multiple transaction-level factors, including transaction time, transaction amount, merchant category, and payment status.

A transaction is classified as `HIGH_RISK` when it meets one of the following conditions:

- `is_night_transaction = 1` AND `amount > 3,000 transaction value`
- `merchant_category IN (Crypto, Gambling)` AND `status = DECLINED`

The logic was implemented in Excel using `IF`, `AND`, and `OR` functions.

The manual review priority is then assigned based on the customer segment, transaction risk flag, and transaction status:

- `URGENT` — HIGH_RISK transaction for VIP or Corporate customers
- `HIGH` — HIGH_RISK transaction for Retail customers
- `MEDIUM` — DECLINED transaction that does not meet the HIGH_RISK conditions
- `LOW` — all remaining transactions

## Customer Risk Profiling

Customer-level analysis was used to identify behavioral patterns that may indicate elevated risk or unusual activity.

For each customer, the following metrics were calculated:

- Transaction count
- Total transaction amount
- Average transaction amount
- Decline rate
- Night transaction rate
- Number of HIGH_RISK transactions

Based on these metrics, several behavioral indicators were created:

- `HIGH_DECLINE` — decline rate ≥ 40%
- `NIGHT_ACTIVITY` — night transaction rate ≥ 50%
- `HIGH_RISK_ACTIVITY` — at least 5 HIGH_RISK transactions
- `HIGH_ACTIVITY` — at least 15 transactions

These indicators were combined into a customer risk score:

| Indicator | Score |
|---|---:|
| HIGH_DECLINE | +1 |
| NIGHT_ACTIVITY | +1 |
| HIGH_RISK_ACTIVITY | +2 |
| HIGH_ACTIVITY | +1 |

The resulting score was used to segment customers according to the number and severity of observed risk indicators and to identify customers that may require additional review.

## Manual Review

A sample of 20 HIGH_RISK transactions was selected for manual review to simulate an operational fraud investigation process.

Each transaction was assessed based on the available risk signals and assigned one of two review outcomes:

- `CONFIRMED_RISK` — multiple independent risk signals supported further investigation
- `PENDING` — the available information was insufficient to conclusively classify the transaction

The review sample also included the assigned manual review priority and customer risk score, allowing transaction-level risk signals to be considered alongside the broader customer profile.

Within the selected sample:

- 15 transactions were classified as `CONFIRMED_RISK`
- 5 transactions were classified as `PENDING`
- 75% of the reviewed sample was classified as `CONFIRMED_RISK`

The results should be interpreted as observations from a selected manual review sample rather than as a measure of model or rule accuracy.

## Dashboard

The Excel dashboard provides a consolidated view of transaction and customer risk indicators.

Key performance indicators include:

- Total Transactions
- HIGH_RISK Transactions
- HIGH_RISK Rate
- Reviewed Transactions
- Confirmed Risk Reviews
- Confirmed Risk Rate

The dashboard also includes visual analysis of:

- HIGH_RISK transaction volume by merchant category
- HIGH_RISK rate by merchant category
- Customer distribution by risk score

A dedicated Key Insights section highlights the most relevant findings from the analysis, allowing users to quickly identify merchant categories with higher risk exposure and understand the distribution of customer risk scores.

## Key Insights

### Merchant Category

- Crypto had the highest HIGH_RISK rate at 53.1%, followed by Gambling at 47.4%.
- Gambling generated the highest number of HIGH_RISK transactions (18), followed by Crypto (17).
- Retail had the lowest HIGH_RISK rate at 7.5%.

### Transaction Priority and Financial Exposure

- HIGH and URGENT transactions represented 30.5% of total transaction volume but accounted for approximately 50.5% of total transaction value.
- URGENT transactions represented 10.5% of transaction volume and accounted for 186,174 transaction value of transaction value.

### Channel Analysis

- ONLINE generated the highest HIGH_RISK transaction value, at 224,820 transaction value.
- The channel analysis indicates that risk exposure should be evaluated not only by transaction count but also by financial value.

### Customer & Operational Analysis

- 15 of 20 manually reviewed HIGH_RISK transactions were classified as `CONFIRMED_RISK`.
- HIGH and URGENT transactions had the same 75% `CONFIRMED_RISK` rate in the reviewed sample.
- Customer Risk Score and Manual Review Priority serve different purposes: the score profiles the customer, while priority determines the operational handling of an individual transaction.

## Tools & Skills

- Microsoft Excel
- Excel Tables and Structured References
- XLOOKUP
- COUNTIF / COUNTIFS / SUMIF
- IF / IFS / IFERROR
- AND / OR
- PivotTables and PivotCharts
- Data aggregation and customer profiling
- Risk scoring and rule-based prioritization
- Manual review analysis
- Dashboard development
- Business analysis and interpretation

## Limitations

- The dataset is synthetic and does not represent real customer behavior or actual fraud patterns.
- The HIGH_RISK classification is based on predefined business rules and should be treated as a risk indicator rather than proof of fraud.
- The customer risk score is a rule-based profiling mechanism and is not a predictive fraud model.
- The manual review sample contains only 20 selected HIGH_RISK transactions and therefore cannot be used to measure overall rule or model accuracy.
- The review outcomes are based on the available transaction-level signals and do not represent confirmed real-world fraud cases.
