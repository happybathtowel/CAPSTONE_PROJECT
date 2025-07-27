# CAPSTONE_PROJECT

## This project analyzes Bitcoin blockchain data alongside global events to identify potential correlations in transaction behavior, block congestion, and network activity.

## Data Dictionary 

### Bitcoin Blocks Table

| Column Name        | Description |
|--------------------|-------------|
| `block_id`         | Unique identifier (hash) for the block |
| `previous_block`   | Hash of the previous block in the chain |
| `merkle_root`      | Root hash of the Merkle tree summarizing transactions in the block |
| `timestamp`        | Time the block was mined (UNIX timestamp format) |
| `difficultyTarget` | Mining difficulty target at the time the block was mined |
| `nonce`            | A random number used during mining to solve the block |
| `version`          | Version number of the block protocol |
| `work_terahash`    | Estimated computational work performed (in terahashes) |
| `work_error`       | Error estimate in computational work (often null) |
| `transactions`     | List of transaction hashes included in this block |
| `row_number`       | Query-generated index (not part of actual blockchain data) |

### Bitcoin Transactions Table
| Column Name        | Description |
|--------------------|-------------|
| `timestamp`        | The time the transaction was included in a block (UNIX timestamp format) |
| `transaction_id`   | Unique identifier (hash) of the transaction |
| `inputs`           | JSON-formatted list of input references, showing which unspent coins are being used |
| `outputs`          | JSON-formatted list of outputs showing where the coins are being sent and in what amounts |
| `block_id`         | Hash of the block that includes this transaction |
| `previous_block`   | Hash of the previous block in the chain (same for all tx in the same block) |
| `merkle_root`      | Root hash summarizing all transactions in the block |
| `nonce`            | Mining nonce used to solve the block (duplicated from blocks table) |
| `version`          | Version number of the transaction format/protocol |
| `work_terahash`    | Estimated amount of computational work needed to mine the block (same across all tx in a block) |
| `work_error`       | Estimation error in work calculation (often null or not used) |

### Bitcoin Outputs Table
| Column Name            | Description |
|------------------------|-------------|
| `transaction_hash`     | Unique identifier (hash) of the transaction this output belongs to |
| `block_hash`           | Hash of the block that includes this transaction |
| `block_number`         | Height of the block in the blockchain (i.e., how many blocks came before it) |
| `block_timestamp`      | Time the block was mined (already in human-readable datetime format) |
| `index`                | Position of this output in the list of outputs for the transaction (e.g., 0 = first) |
| `script_asm`           | Output script in human-readable assembly format (used in Bitcoin's scripting system) |
| `script_hex`           | Output script in hexadecimal format |
| `required_signatures`  | Number of required signatures to spend this output (always null in your sample) |
| `type`                 | Type of script used (e.g., "pubkeyhash", "nulldata", etc.) |
| `addresses`            | List of recipient Bitcoin addresses associated with this output |
| `value`                | Amount sent in the output (usually in satoshis or BTC) |

### Data Summary
This project uses threee Bitcoin blockchain tables from Google BigQuery:
1. **Blocks Table**
    - Contains metadata for each mined block on the Bitcoin network.
    - Key features: block hash, timestamp, difficulty, and transaction count.
    - Column:11: includes some unused or emptry fields such as `work_error`

2. **Transactions Table**
    - Contains individual transaction records linked to each block.
    - Includes nested input/output data, which may require further transformation.
    - Some fields (e.g., `merkle_root`, `nonce`) are duplicated from the blocks table.
    - Columns: 11

3. **Outputs Table**
    - Contains output details from each transaction, including value sent and recipient addresses.
    - Key columns include `value`, `script_asm`, and `addresses`.
    - Column `required_signatures` is mostly null in the sampled data.
    - Columns: 11

### Data Source

This project uses three public datasets hosted on **Google BigQuery**:

1. **Bitcoin Blocks Table**
    - Table: `bigquery-public-data.bitcoin_blockchain.blocks`  
    - Contains block-level metadata such as block hash, timestamp, nonce, and number of transactions.  
    - [View on BigQuery Console](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=bitcoin_blockchain&t=blocks&page=table)

2. **Bitcoin Transactions Table**  
    - Table: `bigquery-public-data.bitcoin_blockchain.transactions`  
    - Contains transaction-level data including inputs, outputs, and associated block info.  
    - [View on BigQuery Console](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=bitcoin_blockchain&t=transactions&page=table)

3. **Bitcoin Outputs Table**  
    - Table: `bigquery-public-data.crypto_bitcoin.outputs`  
    - Contains transaction outputs with values, recipient addresses, and output scripts.  
    - [View on BigQuery Console](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=crypto_bitcoin&t=outputs&page=table)

These datasets are free and publicly accessible through the Google Cloud BigQuery platform. Access in this project was managed through a Google Cloud service account and authenticated using a credentials file provided locally.