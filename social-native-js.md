## Social Verification

  

### native call js

  
  正式环境
  http://dev.ont.io:8081
  
  测试环境
  http://47.96.138.224:8081
  

```
http://dev.ont.io:8081?context=claim:linkedin_authentication&ontid=6469643A6F6E743A5450516E4D576B5943524746513274736147314E566E4468446B75466E4D50476D62&encryptedPrivateKey=6PYNuYx2gMxbVGNe7BBUkK8dCg76R2nhuEFmDZVU92JToRqNj9ggFNyDyV&deviceCode=deviceafd9d0874ad847ea8250c62fa9018cd0&lang=zh_hans
```

  

`context`:  

**claim:twitter_authentication** is for twitter authentication.

**claim:facebook_authentication** is for facebook authentication.

**claim:github_authentication** is for github authentication.

**claim:linkedin_authentication** is for linkedin authentication.

  

`ontid`    

需要传hex编码的

​    

`encryptedPrivateKey`  

`6PYNuYx2gMxbVGNe7BBUkK8dCg76R2nhuEFmDZVU92JToRqNj9ggFNyDyV`  

  

`deviceCode`  



`lang`:   

​    `en_us`     英文  

​    `zh_hans`   中文简体  

  

### js return to native



```
window.prompt('Ont://returnToNative?params=' + result)
```



`result`:

```
{
  Error: 0,
  Desc: 'SUCCESS'
}
```



| Error | Desc            |
| ----- | --------------- |
| 0     | SUCCESS         |
| 101   | 用户取消        |
| 102   | 网络故障        |
| 103   | 超时            |
| 104   | 参数错误        |
| 105   | 托管失败        |
| 106   | 入链失败        |
| 107   | onchain签名失败 |



**H5中的流程类似于一次事务，如果有任何错误发生，Native不需要对具体错误进行处理，只需要监听到prompt事件就把当前webview销毁，用户只能重来一次；**

# 
