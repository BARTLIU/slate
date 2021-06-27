---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - Java
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

本文件為BTSE提供給BITGIN的API服務描述，其包含以下資訊：<br>
1. Endpoints Name<br>
2. Endpoints Description<br>
3. Request URL Parameter<br>
4. Request Format<br>
5. Response Format<br>


# Authentication

*   API Key (btse-api)
    *   Parameter Name: **btse-api**, in: header. API key is obtained from BTSE platform as a string

*   API Key (btse-nonce)
    *   Parameter Name: **btse-nonce**, in: header. Representation of current timestamp in long format

*   API Key (btse-sign)
    *   Parameter Name: **btse-sign**, in: header. A composite signature produced based on the following algorithm: `Signature=HMAC.Sha384 (secretkey, (urlpath + btse-nonce + bodyStr))` (note: bodyStr = '' when no data):

##### Example: Get wallet
1.  btse-nonce: 1582258739280
2.  btse-api: ZDllY2VhNGE2NzU3NDljYmE0ZmIzNjE4MWNjMTMyNjA=
3.  btse-sign: 6632bceab44c4259716578b799482b5334f432da1095931f4295a64b8bbd41c47f08c32d9f63cb22f6f44c57270e9c99 (=HMAC.Sha384(secretKey, /api/v3.2/user/wallet1582258739280))

##### Example: Place an order
1.  btse-nonce: 1583833946481
2.  btse-api: ZDllY2VhNGE2NzU3NDljYmE0ZmIzNjE4MWNjMTMyNjA=
3.  btse-sign: 78c5a1414f3de7aebf60d497fa8417a7daffcfe0503ecd57c0782edc1fcc3718f3ca20b6049ad27e62f93a1f44f9c856 (=HMAC.Sha384(secretKey, /api/v3.2/order1583833946481{"size":0.002,"price":8500,"side":"BUY","type":"LIMIT","symbol":"BTC-USD"}))

*   API Key (btse-api)
    
    *   Parameter Name: **btse-api**, in: header. API key is obtained from BTSE platform as a string
*   API Key (btse-nonce)
    
    *   Parameter Name: **btse-nonce**, in: header. Representation of current timestamp in long format


<aside class="notice">
Some of the actions must required the <code>API key</code>.
</aside>


Rate Limits
-----------

Rate limits for BTSE is as follows:

**Query**

*   Per API: `15 requests per second`
*   Per User: `30 requests per second`

**Orders**

*   Per API: `75 requests per second`
*   Per User: `75 requests per second`


API Status
----------

Each API will return one of the following HTTP status:

*   200 - API request was successful, refer to the specific API response for expected payload
*   400 - Bad Request. Server will not process this request. This is usually due to invalid parameters sent in request
*   401 - Unauthorized request. Server will not process this request as it does not have valid authentication credentials
*   403 - Forbidden request. Credentials were provided but they were insufficient to perform the request
*   404 - Not found. Indicates that the server understood the request but could not find a correct representation for the target resource
*   405 - Method not allowed. Indicates that the request method is not known to the requested server
*   408 - Request timeout. Indicates that the server did not complete the request. BTSE API timeouts are set at 30secs
*   429 - Too many requests. Indicates that the client has exceeded the rates limits set by the server. Refer to Rate Limits for more details
*   500 - Internal server error. Indicates that the server encountered an unexpected condition resulting in not being able to fulfill the request


BTSE Enum
---------

When connecting up the BTSE API, you will come across number codes that represents different states or status types in BTSE. The following section provides a list of codes that you are expecting to see.

*   1: MARKET\_UNAVAILABLE = Futures market is unavailable
*   2: ORDER\_INSERTED = Order is inserted successfully
*   4: ORDER\_FULLY\_TRANSACTED = Order is fully transacted
*   5: ORDER\_PARTIALLY\_TRANSACTED = Order is partially transacted
*   6: ORDER\_CANCELLED = Order is cancelled successfully
*   8: INSUFFICIENT\_BALANCE = Insufficient balance in account
*   9: TRIGGER\_INSERTED = Trigger Order is inserted successfully
*   10: TRIGGER\_ACTIVATED = Trigger Order is activated successfully
*   12: ERROR\_UPDATE\_RISK\_LIMIT = Error in updating risk limit
*   15: ORDER\_REJECTED = Order is rejected
*   16: ORDER\_NOTFOUND = Order is not found with the order ID or clOrderID provided
*   28: TRANSFER\_UNSUCCESSFUL = Transfer funds between spot and futures is unsuccessful
*   27: TRANSFER\_SUCCESSFUL = Transfer funds between futures and spot is successful
*   41: ERROR\_INVALID\_RISK\_LIMIT = Invalid risk limit was specified
*   64: STATUS\_LIQUIDATION = Account is undergoing liquidation
*   101: FUTURES\_ORDER\_PRICE\_OUTSIDE\_LIQUIDATION\_PRICE = Futures order is outside of liquidation price
*   1003: ORDER\_LIQUIDATION = Order is undergoing liquidation
*   1004: ORDER\_ADL = Order is undergoing ADL

Supported Currencies
--------------------

Currencies listed in this section are the available crypto currencies that can be used for address creation, and withdrawals.

*   BTC
*   BTC-LIQUID
*   USDT-OMNI
*   USDT-ERC20
*   USDT-LIQUID
*   ETH




# Account
## Check Login Status
This endpoint can chech whether user login or not

### HTTP Request
`GET /v1/login_status`

> Response Example: LoginStatusData

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1624415325469,
    "data": {
        "isLogin": false
    },
    "success": true
}
```
### Query Parameters
None

### Response

LoginStatusData

FE | BE | Type | Description 
--------- | ----------- | --------- | -----------
isLogin | | Boolean | 使用者是否是登入狀態


<aside class="success"> Authentication - No Needed </aside> 



## User Information
1. 2FA<br>
2. KYC<br>
3. permissions<br>

### HTTP Request
`GET /v1/user`

### URL Parameters
None


### Response

> Response Example: UserData

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1624415969731,
    "data": {
        "username": "chiakai5",
        "email": "chiakai.chenggga@nogle.com",
        "is2FA": true,
        "isKyc": true,
        "userStatus": 1,
        "kycLevel": 3,
        "allowCryptoDeposit": true,
        "allowCryptoWithdrawal": true,
        "allowFiatDeposit": true,
        "allowFiatWithdrawal": true,
        "allowTrade": true,
        "allowTransfer": true
    },
    "success": true
}
```

UserData

|FE|BE|Type|Description|
|--- |--- |--- |--- |
|username||String|使用者帳號|
|email||String|使用者信箱|
|is2FA||Boolean|是否綁定 Google 驗證|
|kycLevel||Integer|KYC 等級|
|isKyc||Boolean|是否通過 KYC|
|userStatus||Integer|使用者狀態|
|allowCryptoDeposit||Boolean|是否可以存入USDT (kycLevel >= 1 )|
|allowCryptoWithdrawal||Boolean|是否可以提取USDT (kycLevel >= 2 )|
|allowFiatDeposit||Boolean|是否可以存入TWD (kycLevel == 3 )|
|allowFiatWithdrawal||Boolean|是否可以提取TWD (kycLevel == 3 )|
|allowTrade||Boolean|是否可以交易USDT-TWD(kycLevel >= 2 )|

#### ResponseData: 
UserData (null if token is expired)

#### UserStatus:
1 = Normal //已启用 <br>
4 = NotActivated //未验证 <br>
7 = Locked

<aside class="warning"> Authentication - Needed </aside> 





## Get VIP Information	

### HTTP Request
`GET /v1/user/vip_info	`

> Response Example: BankData

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1624436755006,
    "data": {
        "isVIP": false,
        "spread": 0.1
    },
    "success": true
}
```

### URL Parameters
None


### Response
BankData

|FE|BE|Type|Description|
|--- |--- |--- |--- |
|isVIP||Boolean|是否是 VIP|
|spread||Number|spread (percentage)|


<aside class="warning"> Authentication - Needed </aside> 




## Get Bank Account list	

### HTTP Request
`POST /v1/user/bank_accounts` <br>
`application/json`	


### URL Parameters
|Key|Type|Required|Description|
|--- |--- |--- |--- |
|currency|String|N||

> Response Example: Array - BankData

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1624418754484,
    "data": [
        {
            "id": 1135,
            "name": "臺灣土地銀行　基隆分行",
            "code": "005",
            "account": "60090100005777",
            "accountHolder": "XXX",
            "swiftCode": ""
        }
    ],
    "success": true
}
```

### Response
BankData

|FE|BE|Type|Description|
|--- |--- |--- |--- |
|id||init|銀行的 id (流水號)|
|name||String|銀行名稱|
|code||String|銀行代碼|
|account||String|帳戶|
|accountHolder||String|帳戶持有人名稱|
|swiftCode||String||



<aside class="warning"> Authentication - Needed </aside> 

## Forget User Password	

### HTTP Request
`POST /v1/user/forget_password` <br>
`application/json`	

### URL Parameters

> Response Example: UserData<br>

```json
{
    "code1": 1,
    "data": {
        "code": 1,
        "data": null,
        "msg": "Success",
        "success": true,
        "time": 1624419343275
    },
    "msg": "Success",
    "success": true,
    "time": 1624419343298
}
```

|FE|Type|Required|Description|
|--- |--- |--- |--- |
|username|String|Y|使用者名稱|
|codeEmail|String|Y|email驗證碼|
|newPassword|String|Y|新密碼|


### Response
error code: 10029  驗證碼錯誤<br>
error code : -1 更新失敗

<aside class="success"> Authentication - No Needed </aside> 






## Change User Password	
> Request Example

```json
{
    "oldPassword": "22c5c8d254e101d1ecc41a400d1ecdc21eb49aae5fff472549d389febd5e1455",
    "newPassword": "22c5c8d254e101d1ecc41a400d1ecdc21eb49aae5fff472549d389febd5e1455"
}
```
> Response Example

```json
{
    "code": 1,
    "data": {
        "code": 1,
        "data": null,
        "msg": "Success",
        "success": true,
        "time": 1624419343275
    },
    "msg": "Success",
    "success": true,
    "time": 1624419343298
}
```


### HTTP Request

`POST /v1/user/change_password`<br>
`application/json`	

### URL Parameters

|FE|Type|Required|Description|
|--- |--- |--- |--- |
|oldPassword|String|Y|舊密碼|
|newPassword|String|Y|新密碼|



### Response
error code: 10006  用戶名或密碼錯誤<br>
error code : -1 更新失敗<br>



<aside class="warning"> Authentication - Needed </aside> 

## Apply Google Authenticator QR Code

> Request Example: 

```shell
  curl 'https://bitgin.oa.btse.io/v1/user/apply_google_device_authenticator' \
  -H 'token: USER_TOKEN_LOGIN_567b8f5f423af15818a068235807edc0_d6ac6f576e554cb88862b73a60a58b9a' \
  -H 'content-type: application/json' \
  --compressed
```


> Response Example: 

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1602670006888,
    "data": {
        "qecode": "otpauth://totp/castiello520@bitgin@BTSE?secret=IKIEJ74IDKOSJLNF",
        "code": 1,
        "totpKey": "IKIEJ74IDKOSJLNF"
    },
    "success": true
}
```

### HTTP Request
`GET /v1/user/apply_google_device_authenticator	`

### URL Parameters
Please refer request example

### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 







## Bind Google Device	

> Request Example

```json
{
    "totpCode": "168668",
    "totpKey": "IKIEJ74IDKOSJLNF"
}
```

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1602670006888,
    "data": "绑定成功",
    "success": true
}
```
> Response Example: 非法请求

```json
{
    "code": -19,
    "msg": null,
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

> Response Example: 您没有设置谷歌

```json
{
    "code": 10011,
    "msg": "您没有设置谷歌",
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

> Response Example: 谷歌验证码错误多次,请2小时后再试

```json
{
    "code": 10026,
    "msg": null,
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

> Response Example: 谷歌验证码错误

```json
{
    "code": 10027,
    "msg": null,
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

### HTTP Request
`POST /v1/user/bind_google_device_authenticator	`<br>
`application/json`

### URL Parameters

|Key|Type|Required|Descript|Example|Note|
|--- |--- |--- |--- |--- |--- |
|totpCode|String|Y||168668||
|totpKey|String|Y||IKIEJ74IDKOSJLNF||


### Response
Please refer response example

<aside class="warning"> Authentication - Needed </aside> 

## Unbind Google Device	

> Request Example 

```json
{
    "totpCode": "168668"
}
```

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1602670006888,
    "data": "解绑成功",
    "success": true
}
```

> Response Example: 您没有设置谷歌

```json
{
    "code": 10011,
    "msg": "您没有设置谷歌",
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

> Response Example: 谷歌验证码错误多次,请2小时后再试

```json
{
    "code": 10026,
    "msg": null,
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

> Response Example: 谷歌验证码错误

```json
{
    "code": 10027,
    "msg": null,
    "time": 1602670006888,
    "data": null,
    "success": false
}
```


### HTTP Request
`POST /v1/user/unbind_google_device_authenticator	`<br>
`application/json`
	

### URL Parameters

|Key|Type|Required|Descript|Example|Note|
|--- |--- |--- |--- |--- |--- |
|totpCode|String|Y||168668||



### Response
Please refer response example

<aside class="warning"> Authentication - Needed </aside> 

## Reauthenticate With Credential

> Request Example 

```json
{
    "totpCode": "168668"
}
```

```shell
curl 'http://bitgin.local.io:8080/bitgin/v1/user/reauthenticate_with_credential' \
  -H 'token: USER_TOKEN_LOGIN_567b8f5f423af15818a068235807edc0_628897e40ad54c9ab97f924cf9e19354' \
  -H 'Content-Type: application/json' \
  --data-binary $'{\n    "totpCode": "310355"\n}' \
  --compressed \
  --insecure
```


> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1602670006888,
    "data": "解绑成功",
    "success": true
}
```

> Response Example: 您没有设置谷歌

```json
{
    "code": 10011,
    "msg": "您没有设置谷歌",
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

> Response Example: 谷歌验证码错误多次,请2小时后再试

```json
{
    "code": 10026,
    "msg": null,
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

> Response Example: 谷歌验证码错误

```json
{
    "code": 10027,
    "msg": null,
    "time": 1602670006888,
    "data": null,
    "success": false
}
```


### HTTP Request
`POST /v1/user/reauthenticate_with_credential	`<br>
`appliction/json`

### URL Parameters
|Key|Type|Required|Descript|Example|Note|
|--- |--- |--- |--- |--- |--- |
|totpCode|String|Y||168668||



### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 




## Authenticate 2FA When Login	
綁定2FA使用者登入時輸入驗證碼須呼叫此API	


> Response Example: 200

```json
{  
    "code": 1,
    "data":
        {
            "accountTokenInfo": null,
            "bandGoogle": false,
            "errorcode": null,
            "fiat": false,
            "isFiat": false,
            "key": null,
            "msg": null,
            "secretKey": null,
            "token": "USER_TOKEN_LOGIN_9f8785c7f9b578bec2c09e616568d270_29aab9c0e5754bb5922fc37729bf8004",
            "tokenUuid": "29aab9c0e5754bb5922fc37729bf8004",
            "userinfo": null,
            "value": null
        },
    "msg": null,
    "success": true,
    "time: 1624247316665
}

```

> Response Example: 二次认证失败

```json
{  

{
    "code": 10070,
    "msg": "",
    "time": 1602670006888,
    "data": null,
    "success": false
}
```

### HTTP Request
`POST /v1/user/check/2FA	`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Descript|Example|Note|
|--- |--- |--- |--- |--- |--- |
|totpCode|String|Y||168668||



### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 


## User Login/Logout History

> Request Example: 

```shell
curl 'https://bitgin.oa.btse.io/v1/user/login_log' \
  -H 'cookie: token=USER_TOKEN_LOGIN_5eed6c6e569d984796ebca9c1169451e_94bf84e7131e4b3c811f5de88be775c3' \
  --compressed
```

> Response Example: 

```json
{
  "code": 1,
  "msg": "Success",
  "time": 1604387008182,
  "data": {
    "dataList": [
      {        
        "area: "-",
        "browser": "Chrome 9 91.0.4472.114",
        "city": null,
        "ip": "10.1.10.109",
        "logTime": 1624419855823,
        "operationCode": 1001,
        "os": "Windows 10"
        },
      {        
        "area": "-",
        "browser": "Chrome 9 91.0.4472.114",
        "city": null,
        "ip": "10.1.10.109",
        "logTime": 1624418959045,
        "operationCode": 1002,
        "os": "Windows 10"       
        }
    ]
  }
}
```


### HTTP Request
`GET /v1/user/login_log	`

### URL Parameters
|Key|Type|Required|Descript|Example|Note|
|--- |--- |--- |--- |--- |--- |
|pageSize|int|N|單一頁筆數|10|Default 10|
|pageNum|int|N|第幾頁|1|Default 1|


### Response
operationCode List :
<br>1000, "注册" ,
<br>1001, "登录",
<br>1002, "登出",
<br>1003, "登录失败",
<br>1004, "绑定邮箱",
<br>1005, "修改邮箱",
<br>1006, "绑定手机",
<br>1007, "修改手机",
<br>1008, "绑定谷歌" ,
<br>1009, "修改谷歌",
<br>1010, "设置登录密码",
<br>1011, "修改登录密码",
<br>1012, "设置交易密码",
<br>1013, "重置交易密码",
<br>1014, "实名",
<br>1015, "购买VIP6",
<br>1016, "使用充值码",
<br>1017, "提交提问",
<br>1018, "回复提问",
<br>1019, "添加银行卡",
<br>1020, "添加虚拟币充值地址",
<br>1021, "添加虚拟币提现地址",
<br>1022, "短信",
<br>1023, "邮件",
<br>1024, "积分",
<br>1025, "找回密码",
<br>1026,"wx关联用户",
<br>1027,"qq关联用户",
<br>1028,"迁移账号",
<br>1029, "代理充值",
<br>1030,"撤单",
<br>1031,"删除银行卡",
<br>2000, "买",
<br>2001, "卖",
<br>3000, "RMB充值",
<br>3001, "RMB提现",
<br>3002, "管理员RMB充值",
<br>3003, "虚拟币充值",
<br>3004, "虚拟币提现",
<br>3005, "管理员虚拟币充值",
<br>3006, "虚拟币钱包更新失败",
<br>3007, "RMB等待提现",
<br>3008, "RMB撤销提现",
<br>3009, "虚拟币等待提现",
<br>3010, "虚拟币撤销提现",
<br>3011, "法币提现",
<br>9999,"记录日志";



<aside class="warning"> Authentication - Needed </aside> 



# KYC

## Submit Basic Data	
請完成驗證程序，已解鎖更多功能<br>
Post Condition:<br>
insert / update basic data to KYC and User


> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1596794050933,
    "data": {
        "statusLevel3": 1,
        "statusLevel2": 1
    },
    "success": true
}
```

> Response Example: token过期

```json
{
    "code": 10002,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 身份证号码已被实名验证,不能重复验证

```json
{
    "code": 10031,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 实名信息已提交,请等待审核

```json
{
    "code": 5601,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 用户已完成实名认证

```json
{
    "code": 5605,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: kyc历史记录失效

```json
{
    "code": 5606,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

### HTTP Request
`POST /v1/kyc/basic_data	`<br>
`application/json	`

### URL Parameters
|Key|Type|Required|Description|
|--- |--- |--- |--- |
|name|String|Y|姓名|
|gender|Integer|Y|性別0 = 女, 1 = 男|
|birthday|String|Y|生日YYYY/MM/DD|
|identityNumber|String|Y|身分證號碼 or ARC 號碼|



### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Check Mobile Phone Number Is Unique	
檢查手機號碼是否重複	

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 手机号已存在

```json
{
    "code": 10012,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 请填写真实手机号

```json
{
    "code": 10017,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```


### HTTP Request
`POST /v1/kyc/check_mobile	`<br>
`application/json	`


### URL Parameters
|Key|Type|Required|Description|
|--- |--- |--- |--- |
|mobilePhone|String|Y|手機號碼|


### Response
Please refer response example



<aside class="success"> Authentication - Mo Needed </aside> 

## Verify Basic Data	
請完成驗證程序，已解鎖更多功能<br>

Post Condition:<br>
pass KYC LV1 automatically<br>


> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1601865917948,
    "data": null,
    "success": true
}
```

> Response Example: 401 token过期

```json
{
    "code": 10002,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 400 谷歌验证码错误

```json
{
    "code": 10027,
    "msg": null,
    "time": 1601874080042,
    "data": null,
    "success": false
}
```


### HTTP Request
`POST /v1/kyc/verify_basic_data`<br>
`application/json	`

### URL Parameters
|Key|Type|Required|Description|
|--- |--- |--- |--- |
|mobilePhone|String|Y|手機號碼|
|code|String|Y|6 digit|


### Response
UploadData


|FE|BE|Type|Description|
|--- |--- |--- |--- |
|contentType||String|uploaded file's content type|
|size||Integer|uploaded file's size|



<aside class="warning"> Authentication - Needed </aside> 

## Upload Image	
僅支援jpg、png或pdf格式，檔案大小不得超過5M<br>

Post Condition:<br>
file uploaded to server<br>

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1601865917948,
    "data": {        
        "contentType": "image/jpeg"
        "filename": "1617789786791.jpg"
        "id": "4995782445914243215_1624326465572"
        "size": 61516    
    },
    "success": true
}
```

> Response Example: token过期

```json
{
    "code": 10002,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: Invalid file

```json
{
    "code": -2,
    "msg": "Invalid file.",
    "time": 1601874080042,
    "data": null,
    "success": false
}
```

> Response Example: Invalid type

```json
{
    "code": -2,
    "msg": "Invalid type.",
    "time": 1601874080042,
    "data": null,
    "success": false
}
```


### HTTP Request
`POST /v1/kyc/image`<br>
`multipart/form-data`

### URL Parameters
|Key|Type|Required|Description|
|--- |--- |--- |--- |
|file|File|Y|File Content(Binary)|
|type|Integer|Y|KYC Image types:<br>1 = 身分證 / ARC 正面照片<br>2 = 身分證 / ARC 反面照片<br>3 = 使用者與身分證 / ARC的合照<br>4 = 第二證件照片<br>5 = 地址證明<br>6 = 銀行帳戶資訊|



### Response
UploadData

|FE|BE|Type|Description|
|--- |--- |--- |--- |
|contentType||String|uploaded file's content type|
|size||Integer|uploaded file's size|




<aside class="warning"> Authentication - Needed </aside> 

## Apply USDT Buy / Sell Permission
繼續升級為LV2，開啟買入和賣出USDT功能<br>

Post Condition:<br>
The documents will be submitted to BG admin for review and approval.<br>


> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: Missing image

```json
{
    "code": -2,
    "msg": "Missing image.",
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 非法请求

```json
{
    "code": -19,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: Invalid questionnaire

```json
{
    "code": 5608,
    "msg": null,
    "time": 1601874080042,
    "data": null,
    "success": false
}
```


### HTTP Request
`POST /v1/kyc/apply_level_2	`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|
|--- |--- |--- |--- |
|q1.value|String|Y|問題1的答案:'investment' = 投資, 'payment' = 支付, 'other' = 其他|
|q1.otherValue|String|N|選擇「其他」時必填|
|q2.value|String|Y|問題2的答案:'no' = 否,'yes' = 是|
|q2.otherValue|String|N|選擇「是」時必填|
|q3.value|String|Y|問題3的答案:'first_time' = 初次接觸, 'less_than_1' = 1年以下, '1_to_3' = 1年至3年, 'over_3' = 3年以上|
|q4.value|String|Y|問題4的答案:'less_than_50' = 新台幣50萬以下, 'between_50_to_100' = 新台幣50萬至100萬, 'over_100' = 新台幣100萬以上|



### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Apply TWD Deposit / Withdrawal Permission	
繼續升級為LV3，開啟存入與提取新台幣功能<br>

Post Condition:<br>
The documents will be submitted to BG admin for review and approval.<br>

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: token过期

```json

{
    "code": 10002,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: Missing bank account

```json
{
    "code": -2,
    "msg": "Missing bank account.",
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: Missing image

```json
{
    "code": -2,
    "msg": "Missing image.",
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 非法请求

```json

{
    "code": -19,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```


### HTTP Request
`POST /v1/kyc/apply_level_3	`<br>
`multipart/form-data`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|code|String|Y|bank code||
|branch|String|Y|bank branch code||
|account|String|Y|bank account||
|file|File|Y|File Content(Binary)||



### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Get KYC Verify Status	
KYC Overview	

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1596794050933,
    "data": {
        "name": "林*得",
        "gender": 1,
        "identityNumber": "A12****89",
        "mobilePhone": "0912***678",
        "level": 1,
        "statusLevel1": 3,
        "idPhotoVerification": 4,
        "idSelfieVerification": 3,
        "secondIdPhotoVerification": 4,
        "addressVerification": 3,
        "statusLevel2": 4,
        "bankAccountVerification": 1,
        "statusLevel3": 1,
        "allowChangeBankAccount": true
    },
    "success": true
}
```

> Response Example: Token过期

```json
{
    "code": 10002,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: KYC历史记录失效

```json
{
    "code": 5606,
    "msg": null,
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

### HTTP Request
`GET /bitgin/v1/kyc/status`<br>
`application/json`

### URL Parameters
None


### Response
|FE|BE|Type|Description|
|--- |--- |--- |--- |
|name||String|姓名#13|
|gender||Integer|性別0 = 女, 1 = 男|
|identityNumber||String|身分證號碼 or ARC 號碼#13|
|mobilePhone||String|手機號碼 #13|
|level||Integer|KYC 等級|
|statusLevel1||Integer|See KycStatus|
|idPhotoVerification||Integer|身分證 / ARC正反面照片See ImageStatus|
|idSelfieVerification||Integer|您與身分證 / ARC的合照See ImageStatus|
|secondIdPhotoVerification||Integer|第二證件照片See ImageStatus|
|addressVerification||Integer|地址證明See ImageStatus|
|statusLevel2||Integer|See KycStatus|
|bankAccountVerification||Integer|銀行帳戶資訊See ImageStatus|
|statusLevel3||Integer|See KycStatus|
|allowChangeBankAccount||Boolean|是否允許變更銀行帳戶|

#### KycStatus  
1 尚未驗證 <br>
2 驗證中<br>
3 驗證成功 <br>
4 待補交 (被admin reject, 驗證未通過)


#### ImageStatus:  
身分證, 第二身份證件照片, 或是銀行照片的狀態, 與KycStatus共用


<aside class="warning"> Authentication - Needed </aside> 






# Landing

## All tickers
條列每個交易對的最新價格與漲跌幅度

> Response Example: 

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1619431135427,
    "data": [
        {
            "baseCurrency": "USDT",
            "quoteCurrency": "TWD",
            "changePricePercent": 0.0,
            "lastPrice": null
        },
        {
            "baseCurrency": "ETH",
            "quoteCurrency": "TWD",
            "changePricePercent": 1.588807927108616,
            "lastPrice": 65219.1557985
        }
    ],
    "success": true
}
```

### HTTP Request
`GET /v1/allTickers	`<br>
`application/json`

### URL Parameters
None


### Response
changePricePercent 單位是 %<br>
lastPrice 單位是 TWD



<aside class="success"> Authentication - No Needed </aside> 







# Register & Login

## Register

> Request Example: 

```json
{
    "username": "test123",
    "email": "test123@test.com",
    "password": "22c5c8d254e101d1ecc41a400d1ecdc21eb49aae5fff472549d389febd5e1455",
    "captchaId": "6284240789364782730",
    "captchaNumber": "3UE8"
}
```

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1596794050933,
    "data": null,
    "success": true
}
```

> Response Example: 用戶或郵箱已存在

```json
{
    "code": 10086,
    "msg": "用户名或邮箱已存在",
    "time": 1596794140893,
    "data": null,
    "success": false
}
```

> Response Example: 圖形验证码错误

```json
{
    "code": 10083,
    "msg": "圖形验证码错误",
    "time": 1596794140893,
    "data": null,
    "success": false
}
```

> Response Example: 邮箱地址已绑定,请更换邮箱地址

```json
{
    "code": 10013,
    "msg": "邮箱已存在",
    "time": 1596794140893,
    "data": null,
    "success": false
}
```


### HTTP Request
`POST	 /v1/register	`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|username|String|Y|||
|email|String|Y|||
|password|String|Y|||
|captchaId|String|Y|||
|captchaNumber|String|Y|||
|lang|String|N|default: zh-TW||


### Response
Please refer response example



<aside class="success"> Authentication - No Needed </aside> 

## Login

> Request Example: 

```json
{
    "loginName": "test014",
    "password": "22c5c8d254e101d1ecc41a400d1ecdc21eb49aae5fff472549d389febd5e1455"
}
```

> Response Example: 登录成功 (已完成註冊驗證)

```json
{
    "code": 1,
    "msg": null,
    "time": 1596791165968,
    "data": {
        "email": "gerry014@nogle.com",
        "username": "gerry014",
        "userStatus": 1,
        "is2FA": true,
        "isKyc": true
    },
    "success": true
}
```

> Response Example: 登录成功 (未完成註冊驗證)

```json

{
    "code": 1,
    "msg": null,
    "time": 1600222051339,
    "data": {
        "email": "gerry014@nogle.com",
        "username": "gerry014",
        "userStatus": 4,
        "is2FA": false,
        "isKyc": false
    },
    "success": true
}
```

> Response Example: 驗證碼錯誤

```json
{
    "code": 10083,
    "msg": null,
    "time": 1596789089906,
    "data": {
        "wrongTime":3
    },
    "success": false
}
```

> Response Example: 用户为空

```json
{
    "code": 10003,
    "msg": null,
    "time": 1596789089906,
    "data": {
        "wrongTime": 1
    },
    "success": false
}
```

> Response Example: 用户名或密码错误

```json
{
    "code": 10006,
    "msg": null,
    "time": 1596789089906,
    "data": {
        "wrongTime": 1
    },
    "success": false
}
```


### HTTP Request
`POST	 /v1/login	`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|loginName|String|Y|accept username or email ;|peter0105|
|password|String|Y|||
|captchaId|String|N|連續輸入錯誤三次才會有||
|captchaNumber|String|N|連續輸入錯誤三次才會有||
|lang|String|N|default: zh-TW||



### Response
UserStatus:
<br>1 = Normal //已启用
<br>3 = Blocked //受限
<br>4 = NotActivated //未验证
<br>5 = Deleted
<br>6 = Suspended
<br>7 = Locked
<br>8 = NotTradable



<aside class="success"> Authentication - No Needed </aside> 

## logout

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Logout",
    "time": 1597657271356,
    "data": null,
    "success": true
}
```

> Response Example: Token过期

```json
{
    "code": 10002,
    "msg": "token过期",
    "time": 1599554975808,
    "data": null,
    "success": false
}
```


### HTTP Request
`POST /v1/logout	`

### URL Parameters
None


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## 2FA Verify
for regist flow, 6 number


> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1596424292058,
    "data": null,
    "success":true
}
```

> Response Example: 400 Empty loginName

```json
{
    "code": -2,
    "msg": "[empty loginName]",
    "time": 1602036947437,
    "data": null,
    "success": false
}
```

> Response Example: 400 邮箱验证未通过，链接仅5分钟有效

```json
{
    "code": 10022,
    "msg": null,
    "time": 1596424292058,
    "data": null,
    "success":true
}
```


### HTTP Request
`POST /v1/verify`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|loginName|String|Y|accept username or email ;|peter0105|
|code|String|Y|6 digit|123456|



### Response
Please refer response example



<aside class="success"> Authentication - No Needed </aside> 

## Get Captcha

> Response Example: 

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1596419834727,
    "data": {
        "img": "/9j/4AAQSkZJRgABAgAAAQAB...",
        "captchaId": "8954095715907911473"
    },
    "success": true
}
```


### HTTP Request
`GET /v1/captcha/image	`

### URL Parameters
None


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Get Verification Code 
apply verification

> Response Example: 200 验证邮件已发送,请及时验证 (sendToSMS = false)

```json
{
    "code": 1,
    "msg": null,
    "time": 1596419834727,
    "data": {
        "email": "123@223.323"
    },
    "success": true
}
```

> Response Example: 200 验证邮件已发送,请及时验证 (sendToSMS = true)

```json

{
    "code": 1,
    "msg": null,
    "time": 1601104128951,
    "data": {
        "mobilePhone": "+886912345678"
    },
    "success": true
}
```

> Response Example: 400 empty loginName

```json
{
    "code": -2,
    "msg": "[empty loginName]",
    "time": 1602036947437,
    "data": null,
    "success": false
}
```

> Response Example: 400 用户不存在

```json
{
    "code": 10004,
    "msg": null,
    "time": 1596419834727,
    "data": null,
    "success": false
}
```

> Response Example: 400 邮件发送失败，请稍后再试

```json
{
    "code": 10063,
    "msg": null,
    "time": 1596419834727,
    "data": null,
    "success": false
}
```

> Response Example: 400 获取短信验证码失败,请稍候重试

```json
{
    "code": 10018,
    "msg": null,
    "time": 1596419834727,
    "data": null,
    "success": false
} 
```


### HTTP Request
`POST /v1/apply_verification_code	`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|loginName|String|Y|accept username or email ;|peter0105|
|mobilePhone|String|N|手機號碼，須包含國際碼send by email if empty|+886912345678|


### Response
Please refer response example



<aside class="success"> Authentication - No Needed </aside> 


# Send to 

## Wallet Operation

> Request Example: Transfer

```json
{
"fromWallet": [{"wallet": "SPOT@", "asset": "USDT", "amount": 1}],
"operation": "TRANSFER",
"receiver": "chiakai5",
"receiverEmail": "chiakai.chenggga@nogle.com",
"twoFAcode": 812991
}
```

> Request Example: Convert

```json
{
"operation": "CONVERT",
"fromWallet": [{wallet: "SPOT@", asset: "USD", amount: 100}],
"toAsset": "ETH"
}
```

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1623033619171,
    "data": 3019,
    "success": true
}
```


### HTTP Request
`POST /v1/capital/wallet/operation`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|operation|String|Y||TRANSFER,CONVERT,PAY_FOR_SERVICE,PRE_PAY_FOR_SERVICE,GET_BALANCE_IN_QUOTE|
|fromUser|String||||
|fromWallet|List||See "From wallet List"|[{"wallet": "SPOT@", "asset": "USDT", "amount": 1}]|
|receiver|String||||
|receiverEmail|String||||
|twoFAcode|String||||
|toAsset|String||||
|toAmount|String||||


#### From wallet List
|name|type|
|--- |--- |
|wallet|String|
|asset|String|
|amount|double|
|amountInToAsset|double|

### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Transfer Cancel	

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1623034305606,
    "data": null,
    "success": true
}
```


### HTTP Request
`GET /v1/capital/transfer/cancel`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|id|String|Y|balance_audit id||



### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Transfer email resend	

> Response Example: 200

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1623034450525,
    "data": 3019,
    "success": true
}
```


### HTTP Request
`GET /v1/validate/emailtransfer_resend`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|balance_audit_id|String|Y|||



### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Email confirm	

> Response Example: success

```json
redirect: domain/defaultPath?balance_audit_id=%s&coin_id=%s&type=confirm
```


> Response Example: fail

```json
redirect: domain/defaultPath?alertcode=%s&alerttype=%s
```




### HTTP Request
`GET /v1/validate/emailtransfer`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|code|String|Y|system generate code||


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Web confirm	

> Response Example: 

```json
{
    "code": 1,
    "msg": "Success",
    "time": 1623034450525,
    "data": 3019,
    "success": true
}
```


### HTTP Request
`GET /v1/validate/transfer_confirm`<br>
`application/json`

### URL Parameters
|Key|Type|Required|Description|Example|
|--- |--- |--- |--- |--- |
|balance_audit_id|String|Y|||



### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

# Wallet

## Example

> Request Example: 

```json

```

> Response Example: 

```json

```


### HTTP Request
`GET /v1/user`<br>
`application/json`

### URL Parameters
None


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Example

> Request Example: 

```json

```

> Response Example: 

```json

```


### HTTP Request
`GET /v1/user`<br>
`application/json`

### URL Parameters
None


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Example

> Request Example: 

```json

```

> Response Example: 

```json

```


### HTTP Request
`GET /v1/user`<br>
`application/json`

### URL Parameters
None


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Example

> Request Example: 

```json

```

> Response Example: 

```json

```


### HTTP Request
`GET /v1/user`<br>
`application/json`

### URL Parameters
None


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Example

> Request Example: 

```json

```

> Response Example: 

```json

```


### HTTP Request
`GET /v1/user`<br>
`application/json`

### URL Parameters
None


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 

## Example

> Request Example: 

```json

```

> Response Example: 

```json

```


### HTTP Request
`GET /v1/user`<br>
`application/json`

### URL Parameters
None


### Response
Please refer response example



<aside class="warning"> Authentication - Needed </aside> 
