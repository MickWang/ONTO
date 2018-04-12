## 可信声明App本地化设计


### 总体设计原则：
APP根据获取到的可信声明原始信息生成.json文件，在APP本地存储。OntPass服务器只存储可信声明的基本信息，如签发时间、模板标识、属主OntId、签发者OntId、存储位置等。

### 可信声明

目前主要分两种可信声明

- 自签发可信声明

github，twitter等可信声明

- 第三方签发可信声明

facebook、linkedin、CFCA实名认证及在职证明可信声明




#### 自签发可信声明设计方案

github，twitter两种自签发可信声明入口为H5页面。在H5页面完成可信声明签发后，H5需将可信声明原始信息回调给APP，APP做本地存储。同时H5需将可信声明基本信息托管到OntPass服务器。

#### 第三方签发可信声明设计方案

- facebook、linkedin、CFCA认证可信声明


这些可信声明是由TrustAnchor服务器代签发并加密托管至OntPass。APP需主动到OntPass服务器查询可信声明，获取到加密后的可信声明信息后，APP做解密并本地存储。并回调服务器进行确认，OntPass服务器会删除该可信声明原始信息。

- 在职证明可信声明

该可信声明是由TrustAnchor服务器代签发并加密托管至OntPass。OntPass服务器会用websocket方式推送加密后的可信声明，APP接收并解密后本地存储完成时，回调服务器进行确认，OntPass服务器会删除可信声明原始信息。



