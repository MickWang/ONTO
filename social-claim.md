## 社交媒体认证可信声明模板

#### 标准可信声明模板示例

```json
{
	"Id":"ca4ab2f56d106dac92e891b6f7fc4d9546fdf2eb94a364208fa65a9996b03ba0",
	"Context":"claim:staff_authentication",
	"Content":{
		"Name":"张三",
		"UserId":86076389,
		"JobTitle":"Engineer",
		"Alias":"tony"
	},
	"Metadata":{
		"CreateTime":"2017-01-01T22:01:20Z",
		"Issuer":"did:ont:8uQhQMGzWxR8vw5P3UWH1j#1",
		"Subject":"did:ont:4XirzuHiNnTrwfjCMtBEJ6",
		"IssuerName":"onchain",
		"Expires":"2018-01-01",
		"Revocation":"RevocationList",
		"Crl":"http://onchain.com/claim/rev/crl"
	},
	"Signature":{
		"Format":"pgp",
		"Algorithm":"ECDSAwithSHA256",
		"Value": "rsjaenrxJm8qDmhtOHNBNOCOlvz/GC1c6CMnUb7KOb1jmHbMNGB63VXhtKflwSggyu1cVBK14/0t7qELqIrNmQ=="
	},
	"Proof":{
		"Type":"MerkleProof",
		"TxnHash":"c89e76ee58ae6ad99cfab829d3bf5bd7e5b9af3e5b38713c9d76ef2dcba2c8e0",
		"BlockHeight":10
	}
}
```

字段说明：

| Field     |     Type |   Description   | Necessary|
| :--------------: | :--------:| :------: |:------: |
|    Id|   String|  可信声明唯一标识id  |Y|
|    Context|   String|  可信声明模板标识  |Y|
|    Content|   Object|  可信声明具体内容，key-value形式，自定义  |Y|
|    Metadata|   Object|  可信声明元数据  |Y|
|    Metadata.CreateTime|   String|  创建时间,格式：yyyy-MM-dd'T'HH:mm:ss'Z'  |Y|
|    Metadata.Issuer|   String|  可信声明颁发者ONTID和其公钥索引  |Y|
|    Metadata.Subject|   String|  可信声明被颁发者ONTID  |Y|
|    Metadata.IssuerName|   String|  可信声明颁发者名称  |N|
|    Metadata.Expires|   String|  过期时间，格式：yyyy-MM-dd  |N|
|    Metadata.Revocation|   String|  声明注销类型，注销列表RevocationList或注销实时查询接口RevocationUrl  |N|
|    Metadata.Crl|   String|  注销查询API|N|
|    Signature|   Object |  签名信息  |Y|
|    Signature.Format|   String |  签名格式  |Y|
|    Signature.Algorithm|   String |  签名算法  |Y|
|    Signature.Value|   String |  签名值  |Y|
|    Proof|   Object |  梅克尔树证明  |N|
|    Proof.Type|   String |  验证类型，MerkleProof  |N|
|    Proof.TxnHash|   String |  存证交易hash值  |N|
|    Proof.BlockHeight|   int|  交易所在的区块高度  |N|



**说明**：可信声明主要分两种类型

- 自签名可信声明，该种声明不包含MerkleProof证明，Claim里没有Proof对应字段
- 第三方签名可信声明，该种声明包含了MerkleProof证明，Claim里有Proof对应字段信息




### twitter认证可信声明模板

```json
{
	"Id":"2e6e38fc5516832d78307d0ebaed55332b88e163d671a30e18df9f97cfe01fb5",
	"Context":"claim:twitter_authentication",
	"Content":{
		"Id": "424209562",
		"Name": "leewi9在上海",
		"Alias": "leewi9_shanghai",
		"Bio": "",
		"Avatar": "https://pbs.twimg.com/profile_images/627454413213315073/NDaMGG_a_normal.jpg",
		"Location": "",
		"WebSite": "",
		"HomePage": "https://twitter.com/leewi9_shanghai",
		"TwitterUrl": "https://twitter.com/leewi9_shanghai/status/968687917853036544",
		"TwitterCreateTime": "Wed Feb 28 03:22:51 +0000 2018"
	},
	"Metadata":{
		"CreateTime":"2017-01-01T22:01:20Z",
		"Issuer":"did:ont:8uQhQMGzWxR8vw5P3UWH1j#1",
		"Subject":"did:ont:8uQhQMGzWxR8vw5P3UWH1j"
	},
	"Signature":{
		"Format":"pgp",
		"Algorithm":"ECDSAwithSHA256",
		"Value": ""
	}
}
```

可信声明具体内容Content对应字段说明：

| Field     |     Type |   Description   | 
| :--------------: | :--------:| :------: |
|    Id|   String|  账户Id  |
|    Name|   String|  账户名称  |
|    Alias|   String|  账户别名  |
|    Bio|   String|  个人简介  |
|    Avatar|   String|  个人头像  |
|    Location|   String|  位置  |
|    WebSite|   String|  个人网站  |
|    HomePage|   String|  个人社媒主页  |
|    TwitterUrl|   String |  推文链接  |
|    TwitterCreateTime|   String |  推文发送时间  |

### github认证可信声明模板

```json
{
	"Id":"e1f0d3751eb02c439239b061949e11f4fec953afd1887bf28aa18a6ef4007f09",
	"Context":"claim:github_authentication",
	"Content":{
		"Id": "10832544",
		"Name": "",
		"Company": "",
		"Alias": "leewi9",
		"Bio": "",
		"Avatar": "https://avatars2.githubusercontent.com/u/10832544?v=4",
		"Email": "leewi9@yahoo.com",
		"Location": "",
		"GistUrl": "https://gist.github.com/42298ebb0c44054c43f48e1afd763ff6",
		"GistCreateTime": "2018-02-28T03:24:48Z"
	},
	"Metadata":{
		"CreateTime":"2017-01-01T22:01:20Z",
		"Issuer":"did:ont:8uQhQMGzWxR8vw5P3UWH1j#1",
		"Subject":"did:ont:8uQhQMGzWxR8vw5P3UWH1j"
	},
	"Signature":{
		"Format":"pgp",
		"Algorithm":"ECDSAwithSHA256",
		"Value": ""
	}
}
```

可信声明具体内容Content对应字段说明：

| Field     |     Type |   Description   | 
| :--------------: | :--------:| :------: |
|    Id|   String|  账户Id  |
|    Name|   String|  账户名称  |
|    Company|   String|  账户绑定的公司  |
|    Alias|   String|  账户别名  |
|    Bio|   String|  个人简介  |
|    Avatar|   String|  个人头像  |
|    Email|   String|  账户绑定的邮箱  |
|    Location|   String|  位置  |
|    GistUrl|   String |  Gist文章链接  |
|    GistCreateTime|   String |  Gist文章发送时间  |


### linkedin认证可信声明模板

**linkedin认证是由机构签发，所以包含MerkleProof**

```json
{
	"Id":"1865e6419c06768428388a1ba74d20947fc213d69d786feecffbf0d0764dfa27",
	"Context":"claim:linkedin_authentication",
	"Content":{
		"Id": "yL5FdXB-um",
		"Name": "feng li",
		"FirstName": "feng",
		"LastName": "li",
		"Bio": "Blockchain App Developer",
		"Avatar": "https://media.licdn.com/mpr/mprx/0_-HOmp1u9zNCxbF3iKoYjplm9clNP53AiyuoAplgLHN8Cs56_YaaCtAdIJ0qS66rf1IpK19_gajZa",
		"HomePage": "https://www.linkedin.com/in/%E4%BA%9A%E5%B3%B0-%E6%9D%8E-b56b8b79"
	},
	"Metadata":{
		"CreateTime":"2017-01-01T22:01:20Z",
		"Issuer":"did:ont:4XirzuHiNnTrwfjCMtBEJ6#1",
		"Subject":"did:ont:8uQhQMGzWxR8vw5P3UWH1j",
		"IssuerName":"onchain"
	},
	"Signature":{
		"Format":"pgp",
		"Algorithm":"ECDSAwithSHA256",
		"Value": ""
	},
	"Proof":{
		"Type":"MerkleProof",
		"TxnHash":"c89e76ee58ae6ad99cfab829d3bf5bd7e5b9af3e5b38713c9d76ef2dcba2c8e0",
		"BlockHeight":10
	}
}
```

可信声明具体内容Content对应字段说明：

| Field     |     Type |   Description   | 
| :--------------: | :--------:| :------: |
|    Id|   String|  账户Id  |
|    Name|   String|  名称  |
|    FirstName|   String|  名  |
|    LastName|   String|  姓  |
|    Bio|   String|  个人简介  |
|    Avatar|   String|  个人头像  |
|    HomePage|   String|  社媒主页  |



### facebook认证可信声明模板

**facebook认证是由机构签发，所以包含MerkleProof**

```json
{
	"Id":"328acfd4a155da291d0796d3c938efd5ad6b2713d6d0f7426ed83077837aaf62",
	"Context":"claim:facebook_authentication",
	"Content":{
		"Id": "1803639093262686",
		"Name": "lifeng",
		"FirstName": "feng",
		"LastName": "li",
		"Avatar": "https://graph.facebook.com/v2.3/1803639093262686/picture",
		"Gender": "male",
		"Locale": "zh_CN",
		"HomePage": "https://www.facebook.com/1803639093262686"
	},
	"Metadata":{
		"CreateTime":"2017-01-01T22:01:20Z",
		"Issuer":"did:ont:4XirzuHiNnTrwfjCMtBEJ6#2",
		"Subject":"did:ont:8uQhQMGzWxR8vw5P3UWH1j",
		"IssuerName":"onchain"
	},
	"Signature":{
		"Format":"pgp",
		"Algorithm":"ECDSAwithSHA256",
		"Value": ""
	},
	"Proof":{
		"Type":"MerkleProof",
		"TxnHash":"c89e76ee58ae6ad99cfab829d3bf5bd7e5b9af3e5b38713c9d76ef2dcba2c8e0",
		"BlockHeight":10
	}
}
```

可信声明具体内容Content对应字段说明：

| Field     |     Type |   Description   | 
| :--------------: | :--------:| :------: |
|    Id|   String|  账户Id  |
|    Name|   String|  名称  |
|    FirstName|   String|  名  |
|    LastName|   String|  姓  |
|    Avatar|   String|  个人头像  |
|    Gender|   String|  性别  |
|    Locale|   String|  地区  |
|    HomePage|   String|  社媒主页  |
