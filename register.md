# description
This flow allows an Afirmoney customer to create an alias that any other switch sender can use to send him money. 

The customer won't enter any information manually. All informations required in this process will be retrieved from Afrimoney DB.

# USSD menu

```mermaid
flowchart LR
    A-->|A|B
```



```mermaid
flowchart LR

step0[
    *161#
]

 step1[AfriMoney:
1.Send Money
2.Pay Bill
3.Top-up/Data
4.Pay Merchant
5.Africredit
6.Cashout
1.Pending Trans.
2.My Account
9.Bank
10.Vouchers
]

step2[
1.Send to Afrimoney Registered
2.Send to Afrimoney Unregistered
3.Donation
4.Send to Orange
5.Send to QCell
6.Salone switch
0 Prev
]

step2b[
1.Send money
2.Register
0 Prev
]

step3[Enter PIN to confirm]

step4[You request has been submtted. 
You will receive a confirmation SMS]

step0-->step1-->|1|step2-->|6|step2b-->|2|step3-->step4
```

# service flow

```mermaid
sequenceDiagram
    autonumber
    participant customer
    participant ussd
    participant txnserver
    participant eig
    participant nss

    activate customer
    customer ->> ussd: select register and enter pin
    activate ussd
    ussd ->> eig: register
    activate eig
    eig -->> ussd: ack
    ussd -->> customer: ack
    deactivate ussd
    deactivate customer
    eig ->> txnserver: validate pin
    activate txnserver
    txnserver -->> eig: ack
    deactivate txnserver
    eig ->> nss: get customer by phone number
    nss -->> eig: customer details
    alt customer don't exisit
        eig ->> nss: create customer
        nss -->> eig: customer details
    end
    eig ->> nss: create alias
    nss -->> eig: response
    eig ->> nss: link alias
    nss -->> eig: response
    eig ->> txnserver: bank link
    activate txnserver
    txnserver -->> eig: response
    deactivate txnserver
    deactivate eig
    txnserver ->> customer: sms notification


```


# requests

## Get customer by phone number

### Request
```bash
curl -X POST 'http://172.16.10.181:23473/api/fp/customer/accounts' \
--header 'Content-Type: application/json' \
--data '{
  "mobileNumber": "65889902"
}'
```

### Response success

response[0].customer.id, (75 in this example) is the id of the customer

```json
{
    "errorCode": null,
    "errorDescription": null,
    "description": null,
    "response": [
        {
            "id": 54,
            "iban": "TEST00044",
            "accountNumber": "077000000",
            "status": 1,
            "technicalCounter": null,
            "overDraftLimit": null,
            "netPosition": null,
            "minimumDeposit": null,
            "balance": null,
            "type": 0,
            "remainingBalance": null,
            "lowLimit": null,
            "baseLimit": null,
            "highLimit": null,
            "currency": "925",
            "bic": "ACELSLFR",
            "customer": {
                "id": 75,
                "name": "Joe",
                "surname": "Koroma",
                "nickName": "Ash",
                "gender": "MALE",
                "birthDate": "1990-04-15",
                "document": "SL077000000",
                "type": "PERSON",
                "participant": null,
                "mobileNumber": "077000000",
                "documentValidityDate": "2024-02-16",
                "customerForAlias": null
            },
            "default": false
        }
    ]
}
```

### Response fail

```json
```


## create customer

### request

```bash
curl -X POST 'http://172.16.10.181:23471/api/fp/customer' \
--header 'Content-Type: application/json' \
--data '{
"name": "Joe",
"surname": "Koroma",
"nickName": "Ash",
"gender": "MALE",
"birthDate": "1990-04-15",
"document": "SL077928000",
"type": "PERSON",
"mobileNumber": "077928000",
"documentValidityDate": "2024-01-16"
}'
```

### response success

```json
{
    "errorCode": null,
    "errorDescription": null,
    "description": "Customer has been created!",
    "response": {
        "id": 76,
        "name": "Joe",
        "surname": "Koroma",
        "nickName": "Ash",
        "gender": "MALE",
        "birthDate": "1990-04-15",
        "document": "SL077000001",
        "type": "PERSON",
        "participant": null,
        "mobileNumber": "077000000",
        "documentValidityDate": "2024-02-16",
        "customerForAlias": null
    }
}
```

### reponse fail

```json
```

## create alias

### request

76 in the url is the customer ID retrieved from previous APIs

```bash
curl -X POST 'http://172.16.10.181:23473/api/fp/customer/76/aliases/' \
--header 'Content-Type: application/json' \
--data '[
    {
        "type": "MOBILE",
        "value": "077000000",
        "status": 1,
        "startDate": "2024-01-04",
        "expireDate": "2026-12-11"
    }
]'
```

### response success

```json
{
    "errorCode": null,
    "errorDescription": null,
    "description": null,
    "response": [
        {
            "id": 73,
            "type": "MOBILE",
            "value": "077000000",
            "status": 1,
            "expireDate": "2026-12-11",
            "startDate": "2024-01-04"
        }
    ]
}
```
### reponse fail
```json
```

## link alias

### request

```bash
curl -X POST 'http://172.16.10.181:23473/api/fp/accounts/linked/alias' \
--header 'Content-Type: application/json' \
--header 'Cookie: csrftoken=qSBp5B4Cq8sePao7RNLN900SDkGzXNCy' \
--data '[{
"aliasValue":"077000000", 
"bic":"ACELSLFR",
"iban":"077000000",
"accountNumber":"077000000",
"type":0,
"status":1,
"isDefault":false,
"currency":"925"
}]'
```

### response success

```json
{
    "errorCode": null,
    "errorDescription": null,
    "description": null,
    "response": [
        {
            "id": 54,
            "aliasValue": "077000000",
            "bic": "ACELSLFR",
            "iban": "TEST00044",
            "accountNumber": "077000000",
            "type": 0,
            "status": 1,
            "isDefault": false,
            "currency": "925"
        }
    ]
}
```

### reponse fail
```json
```


[Main](README.MD)