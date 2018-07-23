# ONT SDK 接口文档

注意：这个sdk 到处了一个包含所有内容的全局变量“Ont”。使用时需要在Ont的命名空间下。如如使用类Account，需要写成“Ont.Account”。

## 1 钱包 Wallet

钱包Wallet是一个Json格式的数据存储文件。在本体Ontology中， Wallet既可存储数字身份，也可以存储数字资产。

### Wallet 数据规范

```
{
	name: string;
    defaultOntid: string;
    defaultAccountAddress : string;
    createTime: string;
    version: string;
    scrypt: {
        "n": number;
        "r": number;
        "p": number;
    };
    identities: Array<Identity>;
    accounts: Array<Account>;
    extra: null;
}
```

`name` 是用户为钱包所取的名称。

```defaultOntid``` 是钱包默认身份的Ont ID，可在显示时使用。

``defaultAccountAddress`` 是钱包默认数字资产账户的地址。

```createTime``` 是ISO格式表示的钱包的创建时间，如 : "2018-02-06T03:05:12Z"。精确到秒。

`version` 目前为固定值1.0，留待未来功能升级使用。

`scrypt` 是加密算法所需的参数，该算法是在钱包加密和解密私钥时使用。

`identities` 是钱包中所有数字身份对象的数组。

```accounts``` 是钱包中所有数字资产账户对象的数组。

```extra``` 是客户端由开发者用来存储额外数据字段，可以为null。

希望了解更多钱包数据规范请参考[Wallet\_File_Specification](https://github.com/ONTIO-Community/ONTO/blob/master/Wallet_File_Specification.md).

### 1.1 创建新的身份和钱包文件

用户可以通过传递身份名称和密码来创建新的身份。创建新身份时会在本地新建钱包文件，并将创建的身份加入到钱包文件的**identities**数组中。 创建过程分为两步：**创建钱包文件**和**注册ONT ID**。

**创建钱包文件**所需参数说明如下：

**name** 身份的名称。

**password** 密码。用来加密解密私钥。

**payer** user agent账户地址，用来支付交易gas

**callback** 原生里定义的回调函数名，用来接收sdk返回的结果。关于sdk和原生交互的详细情况见 [sdk和原生的交互](#5)。
该方法会通过回调将创建好的钱包文件，以JSON字符串的形式，返回给原生。在创建的过程中，需要将钱包中身份的ONT ID注册到链上，注册成功时，代表钱包和身份创建成功。

注册的过程中先发送预执行的交易，来确认交易能够正常执行.

```
Ont.SDK.createWallet('zhangsan', '123456', 'payer', 'gasPrice', 'gasLimit','callback')

```
注册ONT ID上链需要花费gas，这个过程由user agent来完成。
该方法返回的结构如下：

```
{
	error //错误码，为0表示成功结果
	result //返回的结果，JSON格式字符串
	desc //描述信息
	tx //交易对象序列化后的字节
}
```
客户端取到结果里的tx，作为参数继续发送请求到user agent(对于ONTO是ONT pass)，由user agent完成ONT ID上链注册过程。当user agent 返回注册成功信息后，客户端可以认为注册完成，创建过程结束。

创建的流程图详见如下：

![创建身份流程图](http://on-img.com/chart_image/5a9919cce4b09ac3a0c5cbec.png)

如果返回的消息中包含错误，错误码详见[7.错误码](#7) ，原生可以根据错误情况，选择重新注册，或其他处理。

如果返回成功结果的消息，则继续做成功创建的后续处理。


### 1.2 通过导入身份创建钱包文件

导入身份需要用户提供**身份的名称，加密后的私钥**和**密码**。方法参数如下：


**label** 是要导入的身份的名称，可以为空，为空时，sdk会提供自定义生成的名称。

**encryptedPrivateKey** 加密后私钥，

**password** 密码。

**address** 地址

**salt** 盐值

**callback** 回调函数名

导入的过程是：先检查加密后的私钥和密码是否正确。然后根据私钥生成ONT ID, 并检查ONT ID是否在链上。如果不存在，回调会返回相应错误码；如果存在，则生成身份和钱包文件，把身份加入到钱包中，回调返回钱包文件。

导入身份的流程图如下：

![导入身份](http://on-img.com/chart_image/5a991d34e4b0337bad178b8c.png)

```
Ont.SDK.importIdentityAndCreateWallet(label,encryptedPrivateKey, password, address，salt, callback)
```

### 1.3 导入身份到本地钱包

可以导入身份,然后添加到本地钱包的**identities**数组中。

该方法需要传入的参数如下：

**label** 导入的身份的名称，可以为空值。

**encryptedPrivateKey** 加密后的私钥。

**password** 用户的密码。

**address** 地址

**salt** 盐值

**callback** 回调函数名。

如果成功导入，该方法会返回导入的身份，为JSON格式字符串。

导入的过程同[1.2 通过导入身份创建钱包](#2)

````
Ont.SDK.importIdentityWithWallet( walletDaatStr, label, encryptedPrivateKey, password,address, salt, callback)
````

### 1.4 导入数字资产账户(Account)到本地钱包

导入账户需要用户提供**账户的标签，加密后的私钥**和**密码**。


**label** 是要导入的账户的标签，可以为空，为空时，sdk会提供自定义生成的标签。

**encryptedPrivateKey** 加密后私钥，

**password** 密码。

**address** 地址

**salt** 盐值

导入的过程是：先检查加密后的私钥和密码是否正确，如果正确则通过该账户的私钥来创建账户，并加入到钱包的**accounts**数组中。

成功导入会返回导入的账户，为JSON格式字符串。

```
Ont.SDK.importAccountWithWallet(label,encryptedPrivateKey,address,salt, password,callback)
```

## 2. 身份Identity 

#### Identity 具有以下数据结构

```
{
  ontid : string,
  label : string,
  lock : boolean,
  controls : array of ControlData,
  extra : null
}
```

```ontid``` 是代表身份的唯一的id

`label` 是用户给身份所取的名称。

`lock` 表明身份是否被用户锁定了。客户端不能更新被锁定的身份信息。默认值为false。

`controls` 是身份的所有控制对象**ControlData**的数组。

`extra` 是客户端开发者存储额外信息的字段。可以为null。

#### ControlData 具有以下数据结构

```
{
  algorithm: string;
  parameters: {};
  id: string;
  key: string;
  address: string;
  salt: string
}
```

`algorithm` 是用来加密的算法名称。

`parameters` 是加密算法所需参数。

`id` 是control的唯一标识。

`key` 是NEP-2格式的私钥。该字段可以为null（对于只读地址或非标准地址时）。

```address``` 解密私钥时使用的地址。

```salt```  解密私钥时使用的盐值。

### 2.1 创建身份（本地已有钱包）

创建身份需要的参数说明如下：

**label** 身份的名称

**password** 密码，用来加密和解密身份的私钥

**payer** user agent账户地址，用来支付交易gas

**gasPrice** 花费的gas price

**gasLimit** 花费的gas limit

**callback** 回调函数名。

创建身份需要注册ONT ID到链上。注册ONT ID上链需要花费gas，这个过程由user agent来完成。
该方法返回的结构如下：

```
{
	error //错误码，为0表示成功结果
	result //返回的结果，JSON格式字符串
	desc //描述信息
	tx //交易对象序列化后的字节
}
```

客户端取到结果里的tx，作为参数继续发送请求到user agent(对于ONTO是ONT pass)，由user agent完成ONT ID上链注册过程。当user agent 返回注册成功信息后，客户端可以认为注册完成，创建过程结束。

````
var identityDataStr = Ont.SDK.createIdentity('zhangsan','123456', payer, gasPrice, gasLimit, callback)
console.log(accountDataStr)
````

### 2.2 备份身份
#### 2.2.1 备份身份为二维码
二维码的规范请参考[文档](https://ontio.github.io/documentation/ontology_wallet_file_specification_en.html)。

```identityDataStr``` 身份对象的JSON格式字符串

````
Ont.SDK.exportIdentityToQrcode(identityDataStr : string, callback : string)
````

#### 2.2.1 备份身份为Keystring
Keystring = 24个字符的salt + 34个字符的地址 + 加密后私钥

```identityDataStr``` 身份对象的JSON格式字符串

````
Ont.SDK.exportIdentityToKeystring(identityDataStr : string, callback : string)
````

## 3 账户 Account

#### Account 具有以下数据结构。

```
{
  address : string,
  label : string,
  lock : boolean,
  algorithm : boolean,
  parameters : {},
  key : string,
  enc-alg : string,
  salt : string,
  signatureScheme: string,
  publicKey: string,
  extra : null
}
```

```address``` 是base58编码的账户地址。

```label``` 是账户的名称。

`lock` 表明账户是否是被用户锁住的。客户端不能消费掉被锁的账户中的资金。

`algorithm` 是加密算法名称。

`parameters` 是加密算法所需参数。

`key`加密后的私钥。

```enc-alg``` 加密算法

```salt``` 加密算法所用盐值，为base64编码。

```signatureScheme``` 签名算法

```publicKey``` 公钥

`extra` 是客户端存储额外信息的字段。该字段可以为null。

### 3.1 创建账户（本地已有钱包）

用户需要传的参数说明如下：

**label** 账户的名称

**password** 密码

**callback** 回调函数名。

该方法会通过回调，将返回JSON格式的账户对象。

客户端可以将返回的account对象加入到本地钱包文件的**accounts**数组中。

```
//create an account , return a json string
var accountDataStr = Ont.SDK.createAccount('zhangsan', '123456', callback)
console.log(accountDataStr)
```

### 3.2 备份账户
#### 3.2.1 备份账户为二维码
二维码的规范请参考[文档](https://ontio.github.io/documentation/ontology_wallet_file_specification_en.html)。

```accountDataStr``` 账户对象的JSON格式字符串

```
Ont.SDK.exportAccountToQrcode(accountDataStr: string, callback : string) 
```

#### 3.2.2 备份账户为Keystring
Keystring = 24个字符的salt + 34个字符的地址 + 加密后私钥

```accountDataStr``` 账户对象的JSON格式字符串

```
Ont.SDK.exportAccountToKeystring(accountDataStr: string, callback : string) 
```


### 3.2 数字资产转账交易

该方法所需参数说明如下：

**token** 转账的数字货币类型。该值为“ONT”或“ONG”。

**from** 转账发起方的钱包地址。

**to** 转账接收者的钱包地址。

**value** 转账金额。

**encryptedPrivateKey** 用户数字钱包账户的加密私钥，即Account中的**key**字段。

**password** 用户用来解密私钥的密码。

**salt** 解密私钥所需的盐值。

**gasPrice** 转账所需gas price.

**gasLimit** 转账所需gas limit.

**payer** 转账花费gas地址。

**callback** 回调函数名。

转账交易发送成后，当回调返回成功结果。

````
Ont.SDK.transferAssets(token, from, to, value, encryptedPrivateKey, password, salt，gasPrice，gasLimit，payer, callback)
````

### 3.3 查询数字资产余额

该方法所需参数说明如下：

**address** 查询的数字钱包地址。

**callback** 回调函数名。

查询结果会通过回调返回。目前返回的结果是如下结构：

````
{
    error : 0,
    result : {
        ont : '123456',
        ong : '0'
    }
    desc : ''
}
````

````
Ont.SDK.getBalance(address, callback)
````

### 3.4 提取ong
参数如下：
**address** 提取ong的地址
**value** 提取的值
**encryptedPrivateKey** 加密后私钥
**password** 密码

**salt** 解密私钥所需的盐值。

**gasPrice** 提取所需花费的gas price.

**gasLimit** 提取所需花费gas limit.

**payer** 支付gas的地址

**callback** 回调函数名

```
Ont.SDK.claimOng(address, value, encryptedPrivateKey, password, salt，gasPrice, gasLimit, callback)
```

### 3.5 查询可提取的ong

参数如下：

**address** 查询的地址

**callback** 回调函数名

````
Ont.SDK.getUnclaimdOng(address, callback)
````

### 3.6 根据交易hash查询合约执行状态

参数如下：

**txHash** 交易hash

**callback** 回调函数名

````
Ont.SDK.querySmartCodeEventByTxhash(txHash, callback)
````

### 3.7 导入账户助记词

参数如下：

**mnemonic** 助记词

**password** 密码

**callback** 回调函数名

````
Ont.SDK.importAccountMnemonic(mnemonic, password, callback)
````

### 3.8 导出账户WIF私钥

参数如下：

**encryptedKey** 加密的私钥

**password**  密码

**address** 地址

**salt** 盐值

**callback** 回调函数名

###  3.9 通过WIF导入账户

参数如下：

**wif** wif格式私钥

**password** 密码

**callback** 回调函数名

````
Ont.SDK.importAccountWithWif(wif, password, callback)
````

### 3.10 通过私钥导入账户

参数如下：

**privateKey** 私钥

**password** 密码

**callback** 回调函数名

````
Ont.SDK.importAccountWithPrivateKey(privateKey, password, callback)
````

### 3.11 解密助记词

参数如下：

**mnemonicEnc** 加密后助记词

**address** 地址

**salt** 盐值

**password** 密码

**callback** 回调函数名

````
decryptMnemonicEnc(mnemonicEnc, address, salt, password, callback)
````

### 4 声明 Claim

#### Claim 具有以下数据结构

```
{
  Context : string,
  Id : string,
  Content : {},
  Metadata : Metadata,
  Signature : Signature
}
```

```Context``` 是声明模板的标识。目前有以下可选的值：

**claim:eid_authentication** 代表中国公民认证。

**claim:twitter_authentication** 代表twitter的认证。

**claim:facebook_authentication** 代表facebook的认证。

**claim:github_authentication** 代表github的认证。

**claim:linkedin_authentication** 代表linkedin的认证。

```Id``` 是声明对象的唯一标识。该值是声明对象中Context, Content, Metadata的hash值。

```Content``` 是声明的内容，格式为JSON对象。

```Metadata``` 是声明对象的元数据。

#### Metadata 具有以下数据结构

```
{
  CreateTime : datetime string
  Issuer : string,
  Subject : string,
  Expires : datetime string
  Revocation : string,
  Crl : string
}
```

```Createtime``` 是声明的创建时间。该值是ISO格式字符串，精确到秒，如‘‘2018-01-01T14:00:01Z’’。

```Issuer``` 是声明的发布者。对于自认证声明，该值是用户的ONT ID。

``Subject`` 是声明的主语。对于自认证声明，该值是用户的ONT ID。

```Expires``` 是声明的过期时间。该值可以为空，标识没有过期时间。

```Revocation``` 是撤销声明的方法。对于自认证声明，不需要该值。

```Crl``` 是撤销声明的记录列表的链接。对于自认证声明，不需要该值。

#### Signature 具有以下数据结构

```
{
	Format : string,
    Algorithm : string,
    Value : string
}
```

```Format``` 是签名的格式。

```Algorithm``` 是签名的算法。

```Value``` 是对声明对象计算后的签名值。

### 4.1 构造自认证声明并签名该声明

该方法所需参数说明如下：

**context** 声明模板的标识。该值为字符串。

**claimData** 要声明的内容，JSON字符串格式。

**ontid** 用户的ONT ID。

**encryptedPrivateKey** 用户加密后的私钥。

**password** 用户的密码。

**address** 

**salt**

**callback** 回调函数名。该值为可选参数。

当传入参数正确，返回声明对象。该声明对象中的**Siganature** 是该声明的签名对象。

如果传入的callback，会通过回调返回声明对象的JSON格式字符串。

````
var claim = Ont.SDK.signSelfClaim(context, claimData, ontid, encryptedPrivateKey, password, address, salt, callback)
````

### 4.2 工具方法 -- 生成签名

用于对输入内容生成签名的方法，该方法会返回符合格式要求的签名内容。

签名格式同上面的**Signature**。

````
{
	Format : 'pgp', //该值为固定的值
    Algorithm : ECDSAwithSHA256, //该值为固定的值
    Value : string //该值为签名后的值
}
````

该方法所需参数说明如下：

**content** 需要签名的内容， 字符串格式。

**encryptedPrivateKey** 加密后的私钥。

**password** 用户的密码。

**address** 地址

**salt** 盐值

**callback** 回调函数名，可选参数。

```
let signature = Ont.SDK.signData(content, encryptedPrivateKey, password,checksum,callback)
console.log(signature.Value)
```

### 4.3 工具方法 — 解密私钥

系统中为了安全，避免直接操作明文私钥。在需要使用明文私钥时，SDK提供了根据密码解密出明文私钥的方法。

如果能够解出私钥，该方法通过回调返回私钥；如果不能解出，会返回相应错误码。

```
var encryptedPrivateKey = Ont.SDK.decryptEncryptedPrivateKey( encryptedPrivateKey, password,address, salt, callback)
```

### 4.4 发送认证声明到链上并验证是否上链成功

用户可以调用SDK的方法将获得的认证声明发送到链上。该方法会构建相应的交易对象，然后通过websocket发送交易到链上，并通过监听返回的消息，确认声明是否上链成功。

用户需要传以下参数：

**claimId** 是发送的属性在DDO中存储的完整路径。规定该值为认证声明对象的唯一标识，即声明对象的**Id**。

**context** 是声明的模块标识。

**issuer** 声明签发者的ONT ID

**subject** 声明接收者的ONT ID

**encryptedPrivateKey** 用户加密后的私钥

**password** 用户的密码

**address** 解密时所需地址

**salt** 解密时所需盐值

**payer** 支付gas的地址

**gasPrice** 交易所需gas price

**gasLimit** 交易所需gas limit

**callback** 回调函数名。

当确认上链成功后，会通过回调返回以下结果：

````
{
    error : 0,
    result : TxnId //交易的id， string
}
````

```
Ont.SDK.getClaim(claimId, context, issuer, subject, encryptedPrivateKey, password, salt, payer, gasPrice, gasLimit,callback)
```


## 5   SDK和原生的交互 

### 5.1 原生调用SDK

原生可以通过于js交互的技术，直接调用SDK中暴露的内容，如“jsBridge”技术。

如果想要得到SDK中方法的返回值，需要通过传递参数callback给SDK的方法，callback是原生定义的回调方法名，SDK在方法调用结束后，通过发起prompt事件，将结果和callback发出，原生拦截prompt事件，获取到函数结果和callback，运行相应方法，处理函数结果。

### 5.2 SDK返回结果给原生

原生的webview和SDK之间定义的协议如下：

```
Ont://callback?params=result
```

```Ont``` 是固定值。

```callback``` 是原生定义的回调函数名。

```params=result``` result就是发送给原生的函数调用结果。该结果是JSON格式。

```
// native side call sdk to create a wallet, the callback name is 'getWalletDataStr'
var walletDataStr = Ont.Wallet.create('zhangsan', '123456', 'getWalletDataStr')
//sdk side will call prompt()
window.prompt('Ont://getWalletDataStr?params={walletDataStr}')
/*
* native side will intercept prompt and get the url
* then, native side can use getWalletDataStr method to handle result
*/
getWalletDataStr(walletSataStr) {
  //......
}
```

#### 返回结果是如下结构：

````
{
    error  : number,  //错误码
    result : string,  //JSON字符串格式的结果
    desc   : string   //描述
    tx     : string   //对于注册ONT ID时有此值，用来继续发送请求到user agent.
}
````

当错误码为0时，表示成功返回结果，result的值就是JSON格式的结果值；

当错误码不为0时，表示失败返回结果，result可能为空。

desc 描述信息，可能为空。

### 6 设置SDK连接网络和端口

SDK内置了默认的请求地址是本体的测试网络和相应的端口。 用户可以通过调用方法设置连接其他网络和端口。

设置需要在应用初始时调用。

````
//设置节点地址
Ont.SDK.setServerNode('139.219.111.50')

//设置restful接口的端口
Ont.SDK.setRestPort('20334')

//设置websocket连接端口
Ont.SDK.setSocketPort('20335')

````



### 7 二维码规范

详见[Wallet\_File_Specification](https://github.com/ONTIO-Community/ONTO/blob/master/Wallet_File_Specification.md) 中有关二维码部分。

### 8 错误码

| 返回代码 | 描述信息                      | 说明                              |
| :------- | ----------------------------- | --------------------------------- |
| 0        | SUCCESS                       | 成功                              |
| 41001 | SESSION_EXPIRED | 会话无效或已过期（ 需要重新登录） |
| 41002 | SERVICE_CEILING | 达到服务上限 |
| 41003 | ILLEGAL_DATAFORMAT | 不合法数据格式 |
| 41004 | INVALID_VERSION| 不合法的版本 |
| 42001 | INVALID_METHOD | 无效的方法 |
| 42002 | INVALID_PARAMS | 无效的参数 |
| 43001 | INVALID_TRANSACTION | 无效的交易 |
| 43002 | INVALID_ASSET | 无效的资产 |
| 43003 | INVALID_BLOCK | 无效的块 |
| 44001 | UNKNOWN_TRANSACTION | 找不到交易 |
| 44002 | UNKNOWN_ASSET | 找不到资产 |
| 44003 | UNKNOWN_BLOCK | 找不到块 |
| 44004 | UNKNWN_CONTRACT | 找不到合约 |
| 45001 | INTERNAL_ERROR | 内部错误 |
| 47001 | SMARTCODE_ERROR| 智能合约错误 |
| 51000 | UNKNOWN_ONTID  | 不存在的ONT ID |
| 52000 | NETWORK_ERROR  | 网络错误 |
| 53000 | Decrypto_ERROR | 解密错误 |
| 54000 | PreExec_ERROR  | 交易预执行错误 |


## 9 Token Swap相关
### 9.1 查询nep-5的 ONT余额
参数说明：

`address` 查询余额的地址，Base58格式

`callback` 回调名

返回值：
```
{
 error: 0,
 result: 金额
}
```

结果里的金额带有精度，需要除以 `100000000` 得到最终结果。

```
Ont.SDK.getNeoBalance(contractHash, address, callback)
```


### 9.2 转账nep-5的ONT
参数说明：

`from` 发送者地址。base58格式

`to` 接收者地址。base58格式。

`value` 转账金额

`encryptedPrivateKey` 加密后私钥

`password` 密码

`salt` 盐值

`callback` 回调名

返回值：
```
{
	error: 0,
	result: 交易hash
}
```

```
Ont.SDK.neoTransfer(
        from,
        to,
        value:
        encryptedPrivateKey,
        password,
        salt,
        callback
    )
```

### 10 发送交易
发送交易到链上。注意调用前先设定好连接的环境和端口。
返回的结果类似如下：

```
{
    "Action": "sendrawtransaction",
    "Desc": "",
    "Error":0,
    "Result": "",
    "Version": "1.0.0"
}
```
`Result` 是交易hash，可用来查询交易执行结果。


参数说明

`txData` 序列化后的交易对象。十六进制字符串形式。

`callback` 回调名。

```
Ont.SDK.sendTransaction(txData, callback)
```
