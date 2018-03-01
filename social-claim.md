## 社交媒体认证可信声明模板

#### 标准可信声明模板示例

```json
{
	"Context":"claim:standard",
	"Id":"12312fdfvdf",
	"Content":{
		"Name":"张三",
		"UserId":86076389,
		"Alias":"tony"
	},
	"Metadata":{
		"CreateTime":"2017-01-01T22:01:20Z",
		"Issuer":"did:ont:8uQhQMGzWxR8vw5P3UWH1j",
		"Subject":"did:ont:4XirzuHiNnTrwfjCMtBEJ6",
		"IssuerName":"onchain",
		"Expires":"2018-01-01",
		"Revocation":"RevocationList",
		"Crl":"http://192.168.1.1/rev/crl"
	},
	"Signature":{
		"Format":"pgp",
		"Algorithm":"ECDSAwithSHA256",
		"Value": ""
	}
}
```

字段说明：

| Field     |     Type |   Description   | Necessary|
| :--------------: | :--------:| :------: |:------: |
|    Context|   String|  可信声明模板标识  |Y|
|    Id|   String|  可信声明唯一标识id  |Y|
|    Content|   Object|  可信声明具体内容，key-value形式，自定义  |Y|
|    Metadata.CreateTime|   String|  创建时间,格式：yyyy-MM-dd'T'HH:mm:ss'Z'  |Y|
|    Metadata.Issuer|   String|  可信声明颁发者ONTID  |Y|
|    Metadata.Subject|   String|  可信声明被颁发者ONTID  |Y|
|    Metadata.IssuerName|   String|  可信声明颁发者名称  |N|
|    Metadata.Expires|   String|  过期时间，格式：yyyy-MM-dd  |N|
|    Metadata.Revocation|   String|  声明注销类型，注销列表RevocationList或注销实时查询接口RevocationUrl  |N|
|    Metadata.Crl|   String|  注销查询API|N|
|    Signature.Format|   String |  签名格式  |Y|
|    Signature.Algorithm|   String |  签名算法  |Y|
|    Signature.Value|   String |  签名值  |Y|


### twitter认证可信声明模板

```json
{
	"Context":"claim:twitter_authentication",
	"Id":"ajyc6wjc1299njcqkcs234df",
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
		"Issuer":"did:ont:8uQhQMGzWxR8vw5P3UWH1j",
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
	"Context":"claim:github_authentication",
	"Id":"6KJKbctyin962nfdfvdf",
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
		"Issuer":"did:ont:8uQhQMGzWxR8vw5P3UWH1j",
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

```json
{
	"Context":"claim:linkedin_authentication",
	"Id":"7Hykw927cnsg002fdfvdf",
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
		"Issuer":"did:ont:4XirzuHiNnTrwfjCMtBEJ6",
		"Subject":"did:ont:8uQhQMGzWxR8vw5P3UWH1j",
		"IssuerName":"onchain"
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
|    Name|   String|  名称  |
|    FirstName|   String|  名  |
|    LastName|   String|  姓  |
|    Bio|   String|  个人简介  |
|    Avatar|   String|  个人头像  |
|    HomePage|   String|  社媒主页  |



### facebook认证可信声明模板

```json
{
	"Context":"claim:facebook_authentication",
	"Id":"58Jgyc230cb131d",
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
		"Issuer":"did:ont:4XirzuHiNnTrwfjCMtBEJ6",
		"Subject":"did:ont:8uQhQMGzWxR8vw5P3UWH1j",
		"IssuerName":"onchain"
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
|    Name|   String|  名称  |
|    FirstName|   String|  名  |
|    LastName|   String|  姓  |
|    Avatar|   String|  个人头像  |
|    Gender|   String|  性别  |
|    Locale|   String|  地区  |
|    HomePage|   String|  社媒主页  |