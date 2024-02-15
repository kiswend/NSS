

```mermaid
sequenceDiagram
    autonumber
    participant customer
    participant ussd
    participant txn
    participant eig
    participant nss

    customer ->> ussd: enter alias
    ussd ->> eig: get accounts by phone
    eig ->> nss: get accounts by phone
    nss -->> eig: account list and name
    eig -->> ussd: account list and name
    ussd -->> customer: account list name
    customer ->> ussd: select account
    customer ->> ussd: enter amount
    ussd ->> txn: get fees and tax
    txn -->> ussd: fees and tax
    ussd -->> customer: fees and tax
    customer ->> ussd: enter PIN to confirm
    ussd ->> txn: W2B
    txn -->> ussd:ack
    ussd -->> customer: ack
    txn ->> eig: W2B
    eig ->> nss: pacs.008
    nss -->> eig: ack
    eig -->>txn: ack
```