

# USSD menu


```mermaid
graph LR



step0["
    *161#
"]

 step1["
 AfriMoney:
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
"]

step2["
1.Send to Afrimoney Registered
2.Send to Afrimoney Unregistered
3.Donation
4.Send to Orange
5.Send to QCell
6.Salone switch
0 Prev
"]

step2b["
1.Send money
2.Register
0 Prev
"]

step3[Enter receive alias]

step4["You are about to transfer to {receiver name}. Select destination account
1.SLCB
2.Orange
3.RCB
"]

step5["Enter amount to send"]

step6["
    fees: NLe 0
    tax: NLe 0
    Enter your PIN to confirm
"]

step7["Your request has been submitted. 
You will receive a confirmation SMS"]

step0 -->step1-->|1|step2-->|6|step2b-->|1|step3-->step4-->step5-->step6-->step7

```


# flow
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
    eig ->> eig: set PKI
    eig ->> nss: pacs.008
    nss -->> eig: ack
    eig -->>txn: ack
```


# APIs

## pay

### request pacs.008.001.10
```xml
<fpenvelope
	xmlns="urn:iso:std:iso:20022:tech:xsd:payment_request"
	xmlns:document="urn:iso:std:iso:20022:tech:xsd:pacs.008.001.10"
	xmlns:header="urn:iso:std:iso:20022:tech:xsd:head.001.001.03">
	<header:apphdr>
		<header:fr>
			<header:fiid>
				<header:fininstnid>
					<header:othr>
						<header:id>ACELSLFR</header:id>
					</header:othr>
				</header:fininstnid>
			</header:fiid>
		</header:fr>
		<header:to>
			<header:fiid>
				<header:fininstnid>
					<header:othr>
						<header:id>GTBISLFR</header:id>
					</header:othr>
				</header:fininstnid>
			</header:fiid>
		</header:to>
		<header:bizmsgidr>ST231121.1329.B005</header:bizmsgidr>
		<header:msgdefidr>pacs.008.001.10</header:msgdefidr>
		<header:credt>2023-11-21T13:27:10.792+00:00</header:credt>
	</header:apphdr>
	<document:document>
		<document:fitoficstmrcdttrf>
			<document:grphdr>
				<document:msgid>ST231121.1329.B005</document:msgid>
				<document:credttm>2023-11- 21T13:27:10.792+00:00</document:credttm>
				<document:nboftxs>1</document:nboftxs>
				<document:sttlminf>
					<document:sttlmmtd>CLRG</document:sttlmmtd>
					<document:clrsys>
						<document:prtry>ACELSLFR</document:prtry>
					</document:clrsys>
				</document:sttlminf>
				<document:pmttpinf>
					<document:lclinstrm>
						<document:prtry>P2P</document:prtry>
					</document:lclinstrm>
				</document:pmttpinf>
				<document:instgagt>
					<document:fininstnid>
						<document:othr>
							<document:id>ACELSLFR</document:id>
						</document:othr>
					</document:fininstnid>
				</document:instgagt>
				<document:instdagt>
					<document:fininstnid>
						<document:othr>
							<document:id>GTBISLFR</document:id>
						</document:othr>
					</document:fininstnid>
				</document:instdagt>
			</document:grphdr>
			<document:cdttrftxinf>
				<document:pmtid>
					<document:endtoendid>ST231121.1329.B005</document:endtoendid>
					<document:txid>ST231121.1329.B005</document:txid>
				</document:pmtid>
				<document:intrbksttlmamt ccy="SLE">10</document:intrbksttlmamt>
				<document:accptncdttm>2023-11- 21T13:27:10.792+00:00</document:accptncdttm>
				<document:instdamt ccy="SLE">10</document:instdamt>
				<document:chrgbr>SLEV</document:chrgbr>
				<document:dbtr>
					<document:nm>Test Test</document:nm>
					<document:pstladr>
						<document:adrline>Freetown</document:adrline>
					</document:pstladr>
				</document:dbtr>
				<document:dbtracct>
					<document:id>
						<document:othr>
							<document:id>123123212313</document:id>
							<document:schmenm>
								<document:prtry>ACCT</document:prtry>
							</document:schmenm>
						</document:othr>
					</document:id>
				</document:dbtracct>
				<document:dbtragt>
					<document:fininstnid>
						<document:othr>
							<document:id>ACELSLFR</document:id>
						</document:othr>
					</document:fininstnid>
				</document:dbtragt>
				<document:cdtragt>
					<document:fininstnid>
						<document:othr>
							<document:id>GTBISLFR</document:id>
						</document:othr>
					</document:fininstnid>
				</document:cdtragt>
				<document:cdtr>
					<document:nm>MV</document:nm>
				</document:cdtr>
				<document:cdtracct>
					<document:id>
						<document:othr>
							<document:id>1234567890</document:id>
							<document:schmenm>
								<document:prtry>ACCT</document:prtry>
							</document:schmenm>
						</document:othr>
					</document:id>
				</document:cdtracct>
				<document:rmtinf>
					<document:ustrd>Testing...</document:ustrd>
				</document:rmtinf>
			</document:cdttrftxinf>
		</document:fitoficstmrcdttrf>
	</document:document>
</fpenvelope>
```

### response success
```xml
```

### reponse fail
```xml
```



[Main](README.MD)