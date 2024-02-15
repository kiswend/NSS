
```mermaid
sequenceDiagram
    autonumber
    participant customer
    participant txn
    participant eig
    participant nss

    nss ->> eig: pac.002
    eig ->> txn: resume txn
    txn ->> txn: complete transaction
    txn -->> eig: ack
    eig -->> nss: ack
    txn ->> customer: sms notification
```