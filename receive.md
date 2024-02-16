
```mermaid
sequenceDiagram
    autonumber
    participant customer
    participant txn
    participant eig
    participant nss

    nss ->> eig: pac.008
    eig ->> eig: validate PKI
    eig ->> txn: BIB2W
    txn ->> txn: complete transaction
    txn -->> eig: ack
    eig -->> nss: ack
    txn ->> customer: sms notification
```

[Main](README.MD)