

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
<FPEnvelope
	xmlns="urn:iso:std:iso:20022:tech:xsd:payment_request"
	xmlns:document="urn:iso:std:iso:20022:tech:xsd:pacs.008.001.10"
	xmlns:header="urn:iso:std:iso:20022:tech:xsd:head.001.001.03">
	<header:AppHdr>
		<header:Fr>
			<header:FIId>
				<header:FinInstnId>
					<header:Othr>
						<header:Id>SLCBSLFR</header:Id>
					</header:Othr>
				</header:FinInstnId>
			</header:FIId>
		</header:Fr>
		<header:To>
			<header:FIId>
				<header:FinInstnId>
					<header:Othr>
						<header:Id>FP</header:Id>
					</header:Othr>
				</header:FinInstnId>
			</header:FIId>
		</header:To>
		<header:BizMsgIdr>MSG-ID2024020803205408300000</header:BizMsgIdr>
		<header:MsgDefIdr>pacs.008.001.10</header:MsgDefIdr>
		<header:CreDt>2024-02-08T00:00:00.000Z</header:CreDt>
		<document:Sgntr
			xmlns:document="urn:iso:std:iso:20022:tech:xsd:head.001.001.03">
			<ds:Signature
				xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
				<ds:SignedInfo>
					<ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
					<ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
					<ds:Reference URI="#_eac570e9-62e2-44dd-a648-b8b794d5a28f">
						<ds:Transforms>
							<ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
						</ds:Transforms>
						<ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
						<ds:DigestValue>lhar4R+ae7MlGjzZgmlDahuqjgN8gMQ0HEJ8MXprL6Y=</ds:DigestValue>
					</ds:Reference>
					<ds:Reference Type="http://uri.etsi.org/01903/v1.3.2#SignedProperties" URI="#_6d28216b-9306-401d-8438-f5c8551b3fb5-signedprops">
						<ds:Transforms>
							<ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
						</ds:Transforms>
						<ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
						<ds:DigestValue>KjUrSiiTyfdvW1g9EmGgxDr51ax46o1SSiiq2NXH2xI=</ds:DigestValue>
					</ds:Reference>
					<ds:Reference>
						<ds:Transforms>
							<ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
						</ds:Transforms>
						<ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
						<ds:DigestValue>M4NxBK6pcFib+GdduyLIA7Jqitn4sOZSyX1IKQRT+tY=</ds:DigestValue>
					</ds:Reference>
				</ds:SignedInfo>
				<ds:SignatureValue>UVzcK+WjMK5zPOwhSEU/PKAp4F7TbnL8yjZInte4VwIVop1tb+CBcKCw9UDYNf5XThtJ4il3Kpa10xRAiPrrHMzxaZFMs1tZsoZhq43A0GnE+0bSDLJSUPewPtsoWhDnCCwSKxJEeuZK341YhOQVPMpzrnTw2tNbfbPszZj4tnGeb81TSjsLPt7pQRlGuxIjBTiO0DmqMbGmwB9UmKQOA2jkEhNlKE9wCORna7CAkztdHCl+msaoqcwKQewHaLSjT+WV7U2MR6MyRwYUsYL3kGw56DUULSkFlIQ/9skV/0cuUvRSuPQ6OJqSUeMhVOMZTaGENKKJETwtThcKyj7ZAg==</ds:SignatureValue>
				<ds:KeyInfo Id="_eac570e9-62e2-44dd-a648-b8b794d5a28f">
					<ds:X509Data>
						<ds:X509IssuerSerial>
							<ds:X509IssuerName>CN=BSL-ISSUER-CA01, DC=saps, DC=gov, DC=sl</ds:X509IssuerName>
							<ds:X509SerialNumber>2274676010371594674665261008416944010192486423</ds:X509SerialNumber>
						</ds:X509IssuerSerial>
					</ds:X509Data>
				</ds:KeyInfo>
				<ds:Object>
					<xades:QualifyingProperties
						xmlns:xades="http://uri.etsi.org/01903/v1.3.2#">
						<xades:SignedProperties Id="_6d28216b-9306-401d-8438-f5c8551b3fb5-signedprops">
							<xades:SignedSignatureProperties>
								<xades:SigningTime>2024-02-08T14:16:53Z</xades:SigningTime>
							</xades:SignedSignatureProperties>
						</xades:SignedProperties>
					</xades:QualifyingProperties>
				</ds:Object>
			</ds:Signature>
		</document:Sgntr>
	</header:AppHdr>
	<document:Document>
		<document:FIToFICstmrCdtTrf>
			<document:GrpHdr>
				<document:MsgId>MSG-ID2024020803205408300000</document:MsgId>
				<document:CreDtTm>2024-02-08T00:00:00.000+00:00</document:CreDtTm>
				<document:NbOfTxs>1</document:NbOfTxs>
				<document:SttlmInf>
					<document:SttlmMtd>CLRG</document:SttlmMtd>
					<document:ClrSys>
						<document:Prtry>FP</document:Prtry>
					</document:ClrSys>
				</document:SttlmInf>
				<document:PmtTpInf>
					<document:LclInstrm>
						<document:Prtry>P2P</document:Prtry>
					</document:LclInstrm>
				</document:PmtTpInf>
				<document:InstgAgt>
					<document:FinInstnId>
						<document:Othr>
							<document:Id>SLCBSLFR</document:Id>
						</document:Othr>
					</document:FinInstnId>
				</document:InstgAgt>
				<document:InstdAgt>
					<document:FinInstnId>
						<document:Othr>
							<document:Id>RCBKSLFR</document:Id>
						</document:Othr>
					</document:FinInstnId>
				</document:InstdAgt>
			</document:GrpHdr>
			<document:CdtTrfTxInf>
				<document:PmtId>
					<document:EndToEndId>-</document:EndToEndId>
					<document:TxId>SLCBSLFR5041898316306</document:TxId>
				</document:PmtId>
				<document:IntrBkSttlmAmt Ccy="SLE">10</document:IntrBkSttlmAmt>
				<document:AccptncDtTm>2024-02-08T17:42:39.071+00:00</document:AccptncDtTm>
				<document:InstdAmt Ccy="SLE">10</document:InstdAmt>
				<document:ChrgBr>SLEV</document:ChrgBr>
				<document:Dbtr>
					<document:Nm>MSCWT</document:Nm>
					<document:PstlAdr>
						<document:AdrLine>MOSCOW</document:AdrLine>
					</document:PstlAdr>
				</document:Dbtr>
				<document:DbtrAcct>
					<document:Id>
						<document:Othr>
							<document:Id>131234567890</document:Id>
							<document:SchmeNm>
								<document:Prtry>ACCT</document:Prtry>
							</document:SchmeNm>
							<document:Issr>C</document:Issr>
						</document:Othr>
					</document:Id>
				</document:DbtrAcct>
				<document:DbtrAgt>
					<document:FinInstnId>
						<document:Othr>
							<document:Id>SLCBSLFR</document:Id>
							<document:Issr>ATM</document:Issr>
						</document:Othr>
					</document:FinInstnId>
				</document:DbtrAgt>
				<document:CdtrAgt>
					<document:FinInstnId>
						<document:Othr>
							<document:Id>RCBKSLFR</document:Id>
						</document:Othr>
					</document:FinInstnId>
				</document:CdtrAgt>
				<document:Cdtr>
					<document:Nm>test</document:Nm>
				</document:Cdtr>
				<document:CdtrAcct>
					<document:Id>
						<document:Othr>
							<document:Id>0201303135001</document:Id>
							<document:SchmeNm>
								<document:Prtry>ACCT</document:Prtry>
							</document:SchmeNm>
						</document:Othr>
					</document:Id>
				</document:CdtrAcct>
				<document:RmtInf>
					<document:Ustrd>SLCB Transfer funds to RCB</document:Ustrd>
				</document:RmtInf>
			</document:CdtTrfTxInf>
		</document:FIToFICstmrCdtTrf>
	</document:Document>
</FPEnvelope>

```

### response success
```xml
```

### reponse fail
```xml
```



[Main](README.MD)