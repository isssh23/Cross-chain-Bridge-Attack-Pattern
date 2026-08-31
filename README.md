# Cross-chain-Bridge-Attack-Pattern
Empirical dataset of 44 verified cross-chain bridge exploits (2021–2026) mapped by attack patterns.

## `crosschain_bridge_transactions.csv`

Live cross-chain transfer records pulled from [Axelar Network](https://axelarscan.io)'s public
GMP (General Message Passing) Explorer API (`api.axelarscan.io`, no auth required). This is
general bridge transaction data (source → destination transfers), separate from the verified
exploit dataset above — useful as a baseline of normal bridge activity for comparison.

| Column | Description |
|---|---|
| `source` | Source chain and sender address, combined (`chain : address`) |
| `destination` | Destination chain and destination contract address, combined (`chain : address`) |
| `source_chain` | Chain the transfer originated on |
| `destination_chain` | Chain the transfer was delivered to |
| `source_address` | Sender address on the source chain |
| `destination_address` | Destination contract address on the destination chain |
| `transaction_time_utc` | Timestamp of the source-chain call event (ISO 8601 UTC) |
| `source_lock_time_utc` | When funds/message were locked and sent on the source chain (same event as above) |
| `destination_mint_time_utc` | When the message was executed / funds minted on the destination chain; blank if not yet executed |
| `transaction_duration_seconds` | Time elapsed between source lock and destination mint, in seconds |
| `transaction_duration_hh_mm_ss` | Same duration, formatted as `HH:MM:SS` |
| `transaction_amount` | Token amount transferred |
| `token_symbol` | Token symbol (e.g. `axlUSDC`) |
| `transaction_amount_usd` | USD value of the transfer at time of transaction |
| `refunded_or_not` | `Refunded` (refund settled), `Pending Refund` (flagged for refund, not yet settled), or `Not Refunded` |
| `status` | Raw Axelar transaction status (e.g. `executed`, `called`, `express_executed`) |
| `simplified_status` | Axelar's simplified status (`sent` / `received`) |
| `is_insufficient_fee` | Whether the transfer was flagged for insufficient gas/fee |
| `gas_paid` | Whether gas for destination execution was paid |
| `source_tx_hash` | Transaction hash on the source chain |
| `message_id` | Axelar GMP message ID |
| `bridge_protocol` | Bridge protocol used (`Axelar Network (GMP)`) |
| `data_source` | API endpoint the record was fetched from |

Generated with `fetch_axelar_transactions.ps1`, which can be re-run to pull a fresh batch
(up to 1000 records per run, paginated 25 at a time per Axelar's API limit).
