# CAPSTONE_PROJECT

## Project Overview: This project explores patterns and insights in Bitcoin blockchain activity during March 2020, a time of major global financial disruption due to the COVID-19 pandemic. Using a 100,000-row sample of transactions from the Bitcoin blockchain, the analysis focuses on transaction behavior, fee dynamics, and potential indicators of network congestion.

## Project Objective: Gain insight on network usage and fees of the bitcoin blockchain during one of the most consequential timeperiod in recent history from world economy point of view. By adding features and visualizations, this project attempts to explain what was happening to the bitcoin blockchain during the beginning of a global crisis. 

## Technologies Used:Python, Jupyter Notebook, Google BigQuery, SQLite, pandas, matplotlib, and seaborn.
### *For further information on the individual packages required to run this project, please refer to the requirements.txt file 
 
## Instructions on how to download, install, and run the project:

### Activate Python virtual environment first and install all packages before running the notebook.

**Note:** Please ensure you have the correct credentials files provided by me via slack message (should be titled "config.json" when you receive it from me) - Please create a folder titled Credentials and place the "config.json" file under it so your API call will be successful.

1. create a new virtual environment
```
python -m venv venv
```

2. activate the virtual environment
```
source venv/Scripts/activate
```

3. install packages
```
pip install -r requirements.txt
```

4. make sure your kernel is switched to the venv python kernel

5. run the project




## Data Dictionary 

### Bitcoin Blocks Table

| Column Name       | Description (how it's used in project)                                |
|-------------------|-----------------------------------------------------------------------|
| hash              | Unique block identifier (used as transaction_id in joined table)      |
| size              | Size of the block in bytes (not used)                                 |
| stripped size     | Size of the block without witness data (not used)                     |
| weight            | Blocik weight (not used)                                              |
| number            | Block height (used for join with transactions)                        |
| version           | Block version number (not used)                                       |
| merkle_root       | Merkle tree root of transactions in the block                         |
| timestamp         | Timestamp when the block was mined (used in joined query)             |
| timestamp_month   | Month extracted from timestamp column.(not used)                      |
| transaction_count | Number of transactions in this block (not used in final analysis)     |
| nonce             | Proof-of-work nonce (not used)                                        |
| merkle_root       | Merkle root of the block's transactions (not used)                    |

### Bitcoin Transactions Table
| Column Name      | Description                                                                    |
|------------------|--------------------------------------------------------------------------------|
| hash             | Unique identifier for each transaction (used as transaction_id in joined table)|
| size             | Size of the transaction in bytes (not used)                                    |
| virtual_size     | Virtual size of the transaction (not used)                                     |
| version          | Version number of the transaction protocol (not used)                          |
| lock_time        | Time when transaction is finalized (not used)                                  |
| block_hash       | Hash of the block that contains this transaction (not used)                    |
| block_number     | Height of the block containing this transaction (used for join)                |
| block_timestamp  | Timestamp when transaction was mined (used for extracting tx_hour and date)    |
| input_count      | Number of input addresses in the transaction (not used)                        |
| output_count     | Number of output addresses in the transaction (not used)                       |
| input_value      | Total input value of the transaction (used in fee_rate and value_diff)         |
| output_value     | Total output value of the transaction (used in value_diff)                     |
| is_coinbase      | Boolean indicator if this was a coinbase transaction(not used)                 |
| fee              | Fee associated with the transaction (used in fee_rate and average fee charts)  |
| inputs           | Raw input address information (not used in analysis)                           |
| outputs          | Raw output address information (not used in analysis)                          |
| tx_hour          | Engineered feature: hour of the day from block_timestamp                       |
| fee_rate         | Engineered feature: fee divided by input_value                                 |
| value_diff       | Engineered feature: output_value - input_value                                 |

### Data Summary
This project uses two Bitcoin blockchain tables from Google BigQuery:
1. **Blocks Table**
    - Contains metadata for each mined block on the Bitcoin network.


2. **Transactions Table**
    - Contains individual transaction records linked to each block.



### Data Source

This project uses three public datasets hosted on **Google BigQuery**:

1. **Bitcoin Blocks Table**
  - Dataset: `bigquery-public-data.crypto_bitcoin.blocks`  
  - Filtered to blocks from March 2020


2. **Bitcoin Transactions Table**  
  - Dataset: `bigquery-public-data.crypto_bitcoin.transactions`  
  - Filter: 100,000 rows from March 1–31, 2020

**Disclaimer:** This project uses a limited 100,000-row sample of transaction data for performance reasons. Therefore, findings represent trends in the sample, not the full Bitcoin network during that time.

These datasets are free and publicly accessible through the Google Cloud BigQuery platform. Access in this project was managed through a Google Cloud service account and authenticated using a credentials file provided locally.