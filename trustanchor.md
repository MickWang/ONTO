### TrustAnchor表结构定义

| file      |     type |   description   |
| :--------: | :--------:| :------: |
| Id    |   varchar(255)|  唯一标识  |
| Name    |   varchar(255)|  机构名称  |
| Description    |   varchar(255)|  机构描述  |
| Type|   varchar(255)|  机构类型，具体字段参考**机构类型Type字典**  |
| CreateTime|   datetime|  创建时间  |
| ClaimContext|   varchar(255)|  对应的可信声明模板标识  |
| Region|   varchar(255)|  地区  |



**机构类型Type字典**

 |     value |   description   |
 | :--------:| :------: |
 |   social_media|  社交媒体，如github、twitter、linkedin、facebook  |
	 |  organization|  机构，如CFCA  |


**机构的ClaimContext字典**

 |     value |   description   |
 | :--------:| :------: |
 |   claim:cfca_authentication|  CFCA实名认证可信声明  |
 |  claim:github_authentication|  github可信声明  |
 |  claim:twitter_authentication|  twitter可信声明  |
 |  claim:linkedin_authentication|  linkedin可信声明  |
 |  claim:facebook_authentication|  facebook可信声明  |

