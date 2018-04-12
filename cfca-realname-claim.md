## CFCA实名认证可信声明模板

```json
{
	"Id":"ca4ab2f56d106dac92e891b6f7fc4d9546fdf2eb94a364208fa65a9996b03ba0",
	"Context":"claim:cfca_authentication",
	"Content":{
		"IDNumber": "510806199002122991",
		"Name": "zhangsan"
	},
	"Metadata":{
		"CreateTime":"2018-03-01T12:01:20Z",
		"Issuer":"did:ont:TRAtosUZHNSiLhzBdHacyxMX4Bg3cjWy3r#1",
		"Subject":"did:ont:SI59Js0zpNSiPOzBdB5cyxu80BO3cjGT70",
		"IssuerName":"onchain"
	},
	"Signature":{
		"Format":"pgp",
		"Algorithm":"ECDSAwithSHA256",
		"Value": "rsjaenrxJm8qDmhtOHNBNOCOlvz/GC1c6CMnUb7KOb1jmHbMNGB63VXhtKflwSggyu1cVBK14/0t7qELqIrNmQ=="
	},
	"Proof":{
		"Type":"MerkleProof",
		"TxnHash":"c89e76ee58ae6ad99cfab829d3bf5bd7e5b9af3e5b38713c9d76ef2dcba2c8e0",
		"BlockHeight":11
	}
}
```

可信声明具体内容Content对应字段说明：

| Field     |     Type |   Description   |
| :--------------: | :--------:| :------: |
|    IDNumber|   String|  身份证号  |
|    Name|   String|  姓名  |
