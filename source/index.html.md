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
## Check login status
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



## User information
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





## get VIP information	

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




## Example

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


<aside class="warning"> Authentication - Needed </aside> 

## Example

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


<aside class="warning"> Authentication - Needed </aside> 

