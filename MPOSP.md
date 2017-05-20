#mposp接口规范
> **Beta:**
> 该API仍处于 Beta 阶段

```
1. 该接口适用对象：中汇自有APP
2. 该接口实现功能：用户收单理财
3. 该接口调用规范：采用HTTP请求与中汇的前置POSP进行通信
```
> **注：**
> 文中所有 `<>` 标注的字段，均需根据你的实际情况替换（无需 `<>` 符号，仅作标注之用）
> 文中所有 `:id` 标注的字段，均需根据该资源的实际 `id` 值替换
> 文中所有 `{x|y|...}` 标注的字段，均需根据你的实际情况用其中一个 `x` 或者 `y`（ `|` 分割）替换

## API 接口地址
```
暂无 # 生产环境
http://192.168.1.240:29110 # 测试环境
```

## 公共请求参数
```
appVersion: "ios.未知.1.1.813"//平台版本号

```

## 标准请求
```sh
curl -X POST \
    http://mposp.21er.tk/<资源路径>.action\
    # 其他可选参数，参数以键值对呈现...
```

## 标准响应
* 订单校验通过的情况下  
```
HTTP/1.1 200 OK
Server: Nginx
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
WSSESSION: ""//session信息
Content-Length: 100

...body...
```

* 订单校验未通过的情况下  
```
HTTP/1.1 401 Unauthorized
```
* 请求 Method 不被支持的情况下  
```
HTTP/1.1 405 Method Not Allowed
```
* 请求参数不正确的情况下  
```
HTTP/1.1 403 Forbidden
```
<a id="content-title"></a>
## 功能路径列表
| 资源名称     | 路径                                     | Content-Type         | 请求方式     | 维护人     | 是否需要登录|
|-------------|-----------------------------------------|----------------------|---------------|---------------|---------------|
| 获取验证码| [/sendMobileMessage](#sendMobileMessage)                      | urlencoded           | POST   | 张树彬     | 否   |
| 获取验证码| [/sendCustomerMessage](#sendCustomerMessage)		       | urlencoded	      | POST   | 张攀攀	 | 否   |
| 验证验证码| [/checkMobileMessage](#checkMobileMessage)		       | urlencoded	      | POST   | 张攀攀	 | 否   |
| 验证密码| [/validatePassword](#validatePassword)		       | urlencoded	      | POST   | 张攀攀	 | 否   |
| 登录| [/login](#login)                      | urlencoded           | POST      | 李飞     | 否   |
| 退出| [/logout](#logout)                      | urlencoded           | POST      | 李飞     | 是   |
| 注册| [/register](#register)                      | urlencoded           | POST   |  李飞     | 否   |
| 签到| [/signin](#signin)                      | urlencoded           | POST   | 李飞     | 是   |
| 获取强制更新的参数 | [/getForceUpdate](#getForceUpdate)    | urlencoded           | GET   | 李飞     | 否   |
| ICkey回调接口| [/downloadFinished](#downloadFinished)                      | urlencoded           | POST   | 李飞     | 是   |
| 修改密码| [/resetPassword](#resetPassword)                      | urlencoded           | POST   | 李飞     | 是   |
| 忘记密码| [/forgetPassword](#forgetPassword)                      | urlencoded           | POST   | 李飞     | 否   |
| 查询交易状态| [/transStatus](#transStatus)                      | urlencoded           | POST   | 李飞     | 是   |
| 发送交易小票接口| [/transMessage](#transMessage)                      | urlencoded           | POST   | 李飞     | 是   |
| 联行号查询| [/bankQuery](#bankQuery)                      | urlencoded           | GET   | 李飞     | 否   |
| 绑定/解绑用户银行卡| [/bindBankCard](#bindBankCard)                      | urlencoded           | GET   | 李飞     | 是   |
| 获取用户银行卡列表| [/listBandCard](#listBandCard)                      | urlencoded           | GET   | 李飞     | 是   |
| 激活绑定设备| [/activeAndBindEquip](#activeAndBindEquip)                      | urlencoded           | POST   | 张树彬     | 是   |
| 实名认证| [/realNameAuth](#realNameAuth)                      | urlencoded           | POST   | 张树彬     | 是   |
| 实名认证信息回显| [/realNameAuthStatus](#realNameAuthStatus)                      | urlencoded           | GET   | 张树彬  | 是   |
| 商户认证| [/merchantAuth](#merchantAuth)                      | urlencoded           | POST   | 张树彬     | 是   |
| 商户认证信息回显| [/merchantAuthStatus](#merchantAuthStatus)                      | urlencoded           | GET   | 张树彬  | 是   |
| 账户认证| [/accountAuth](#accountAuth)                      | urlencoded           | POST   | 张树彬     | 是   |
| 账户认证信息回显| [/accountAuthStatus](#accountAuthStatus)                      | urlencoded           | GET   | 张树彬    | 是   |
| 签名认证| [/signatureAuth](#signatureAuth)                      | urlencoded           | POST   | 张树彬    | 是   |
| 签名认证信息回显| [/signatureAuthStatus](#signatureAuthStatus)                      | urlencoded           | GET   | 张树彬| 是   |
| 消费 | [/sale](#sale)                      | urlencoded           | POST   | 李飞     | 是   |
| 余额查询 | [/queryBalance](#query)                      | urlencoded           | POST   | 李飞     | 是   |
| 认证图片下载 | [/downloadImg](#downloadImg)                      | urlencoded           | GET   | 张树彬     | 是   |
| IC回调 | [/transNotify](#transNotify)                      | urlencoded           | POST   | 张树彬     | 是   |
| 更换设备 | [/swiperChange](#swiperChange)                      | urlencoded           | POST   | 李飞     | 是   |
| 静态页面显示 | [/showHtml](#showHtml)                      | urlencoded           | GET   | 李飞     | 否   |
| 获取消息接口/变更消息状态 | [/modifyMessage](#modifyMessage)                      | urlencoded           | GET   | 张攀攀     | 是   |
| 用户协议 | [/showProtocol](#showProtocol)                      | urlencoded           | GET   | 李飞     | 是   |
| 获取消息接口 | [/message](#message)                      | urlencoded           | GET   | 李飞     | 否   |
| 获取广告位信息 | [/banner](#banner)                      | urlencoded           | GET   | 张树彬     | 否   |
| 获取商户资质(3.0新加接口)| [/merchantQualify.action](#merchantQualify)| urlencoded           | POST |李飞| 是   |
| 交易列表查询| [/findTransList.action](#findTransList)              | urlencoded           | POST |李飞| 是   |
| 交易明细查询| [/tranInfo.action](#tranInfo)              | urlencoded           | POST |李飞| 是   |
| 结算列表查询| [/settleList.action](#settleList)          | urlencoded           | POST |李飞| 是   |
----------------------------------------------------------------------------------
<a id="sendMobileMessage"></a>
### 获取验证码  /sendMobileMessage
#### 1\. 通过手机号获取验证码
请求：  
```
POST /sendMobileMessage HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

mobile: "15801376995"
type: "registe" //注册、忘记密码必传(registe/forget)  (非必传项)
```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
   "respTime":"20151125161740",
   "isSuccess":true,
   "respCode":"SUCCESS",
   "respMsg":"发送验证码成功,注意查收"
   
}
```
##### [返回目录↑](#content-title)
<a id="sendCustomerMessage"></a>
### 获取验证码  /sendCustomerMessage
#### 1\. 通过手机号获取验证码
请求：  
```
POST /sendCustomerMessage HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

accountName: "张无忌" //姓名
idNumber   ： "010876765543456543" //身份证
mobile     ： "15201059026" //手机号
bankCard   ： "62202010904267897" // 银行卡号
```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
   "respTime":"20151125161740",
   "isSuccess":true,
   "respCode":"SUCCESS",
   "respMsg":"发送验证码成功,注意查收"
   
}
```
##### [返回目录↑](#content-title)
<a id="checkMobileMessage"></a>
### 验证验证码是否正确 /checkMobileMessage
#### 1\. 验证验证码是否正确
请求：  
```
POST /checkMobileMessage HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30
需要设置Cookie idCode : xxxxxxxxxxxxxxxxxxxx

appVersion: "ios.未知.1.1.813"
mobile   ： "15800000001" //手机号
idCode   ： "1234" //验证码
 
```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
   "respTime":"20170516190732",
   "isSuccess":true,
   "respCode":"SUCCESS",
   "respMsg":"验证成功" 
}
```
##### [返回目录↑](#content-title)

<a id="validatePassword"></a>
### 验证密码是否正确 /validatePassword
#### 1\. 验证密码是否正确
请求：  
```
POST /validatePassword HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

appVersion: "ios.未知.1.1.813"
password   ： "xxxxxxx" //密码
 
```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
   "respTime":"20170519153057",
   "isSuccess":true,
   "respCode":"SUCCESS",
   "respMsg":"Account and password are ok"
}
```
##### [返回目录↑](#content-title)

<a id="login"></a>
### 登录  /login
#### 1\. 手机号登录
请求：  
```
POST /login HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

"position": "116.379062,39.97077"
"password": "qqqqqq"
"loginName": "18911156118"
"reqTime": "20151228143806"
"registId": "1122312233"//推送ID

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime": "20151228143800",
    "isSuccess": true,
    "respCode": "SUCCESS",//当有新版本需要更新的商户返回特殊code ： UPGRADE_SYSTEM
    "respMsg": "登录成功",
    "ksnNo": "700000000056" ,//设备KSN号，没有绑定设备则没有该字段
    "model": "landim35",//设备类型，没有绑定设备则没有该字段
    "merchantQualify": {
	"terminalAuth": 0 //设备绑定状态 (0未绑定 ,1绑定激活成功),
	"realNameAuth": 0 //实名认证 (0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
	"merchantAuth": 0 //商户认证 (0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
	"accountAuth": 0 //账户认证 (0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
	"signatureAuth": 0 //签名认证 (0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
    }
}
```

##### [返回目录↑](#content-title)
<a id="logout"></a>
### 退出  /logout
#### 1\. 退出
请求：  
```
POST /logout HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151126184737",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"退出成功."
}
```
##### [返回目录↑](#content-title)
<a id="register"></a>
### 注册  /register
#### 1\. 通过手机号注册
请求：  
```
POST /register HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

mobile: "15801376995"
password: "123456"
idCode: 1234 //验证码

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151126184737",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"祝贺您成功注册."
}
```
##### [返回目录↑](#content-title)
<a id="downloadFinished"></a>
### ICkey完成回调接口  /downloadFinished
#### 1\. ICkey完成回调接口
请求：  
```
POST /downloadFinished HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151126184737",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"已更新状态."
}
```
##### [返回目录↑](#content-title)
<a id="signin"></a>
### 签到  /signin
#### 1\. 签到
请求：  
```
POST /signin HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

"position":"117.194778,39.113809",

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime": "20160308185017", 
    "isSuccess": true, 
    "respCode": "SUCCESS", 
    "respMsg": "签到成功", 
    "model": "itron15-9", 
    "needUpdateIC": false, 
    "device": {
        "keyType": "3DES", 
        "keyValue": "7806C6F5AEF0F01160F11390F9B97EFD", 
        "checkValue": "C086F687", 
        "isBluetooth": true, 
        "macAddress": "8C:DE:52:C3:51:0D", 
        "ksnNo": "7000100000008177"
        "bluetoothName": "AC079158", 
    },
    "traceNo": 20
    }
}

```
##### [返回目录↑](#content-title)
<a id="resetPassword"></a>
### 修改密码  /resetPassword
#### 1\. 修改密码
请求：  
```
POST /resetPassword HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30
//两中情况
//如果是忘记密码找回密码islogin=0 
//如果是已经登录变更密码 islogin=1
//islogin=0时需要传递下面参数
islogin : "0"
mobile: "15800000000"
password: "354689"
appVersion: "ios.未知.1.1.813"

//isLogin=1 时需要传递下面参数
islogin : "1"
oldPassword: "xxxxxxxx" 
password:    "xxxxxxxx"
appVersion: "ios.未知.1.1.813"
```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "reqNo": 645254,
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"验证成功"
}
```
##### [返回目录↑](#content-title)
<a id="forgetPassword"></a>
### 忘记密码  /forgetPassword
#### 1\. 忘记密码
请求：  
```
POST /forgetPassword HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

password: "123456"
idNumber: "413023199101259999" //身份证
mobile: "15801376995"
idCode: "5741"//验证码
```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "reqNo": 645254,
    "isMerchant": true,    //是否需要输入身份证
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"验证成功"
}
```

##### [返回目录↑](#content-title)


<a id="getForceUpdate"></a>
### 获取强制更新的参数  /getForceUpdate
#### 1\. 获取强制更新的参数
请求：  
```
GET /getForceUpdate HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "forceUpdate":false,//强制更新参数
    "minVersion":'1.2.126',//IOS最低版本
    "respTime":"20151130125253",
    "isSuccess":true,
}
```

##### [返回目录↑](#content-title)
<a id="transMessage"></a>
### 发送交易小票接口  /transMessage
#### 1\. 发送交易小票接口
请求：  
```
POST /transMessage HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

amount: "11111"
terminalNo: "44444"
merchantNo: "2333"
batchNo: "12"
reqNo: "1111"
mobile: "13500001111"
origReqTime: "20151124111059"

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
   "respTime":"20151125161740",
   "isSuccess":true,
   "respCode":"SUCCESS",
   "respMsg":"短信发送成功,注意查收"
}
```
##### [返回目录↑](#content-title)

<a id="queryTrans"></a>
### 查询接口  /queryTrans
#### 1\. 查询接口
请求：  
```
POST /queryTrans HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

date: "20151201"
respNo: "1111"

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

respNo: "111"
merchantName: "啊啊啊啊"
merchantNo: "1111"
terminalNo: "1111"
transactions: 
{
    "reqNo": 645254,
    "transType": "sale",
    "voucherNo": 2222,//交易流水号
    "respCode": "00",
    "amount": 1234,
    "merchantNo": 111111111,
    "terminalNo": 22222,
    "currency": "CNY",    
    "immediatePay": false,    //是否是DO交易
    "transTime": 20151130125253,
    "refNo": 1234,
    "authNo": 1234    
    "cardTail": "6666",//卡号后四位    
    "cardNoWipe": 645***254,
    "operatorNo": 01,
    "issuer": "XX银行",
    "batchNo": 123
}，
{
    "reqNo": 645254,
    "transType": "sale",
    "voucherNo": 2222,//交易流水号
    "respCode": "00",
    "amount": 1234,
    "merchantNo": 111111111,
    "terminalNo": 22222,
    "currency": "CNY",    
    "immediatePay": false,    //是否是DO交易
    "transTime": 20151130125253,
    "refNo": 1234,
    "authNo": 1234    
    "cardTail": "6666",//卡号后四位    
    "cardNoWipe": 645***254,
    "operatorNo": 01,
    "issuer": "XX银行",
    "batchNo": 123

}
"respTime":"20151130125253",
"isSuccess":true,
"respCode":"SUCCESS",
"respMsg":"验证成功",

```
##### [返回目录↑](#content-title)
<a id="bankQuery"></a>
### 联行号查询  /bankQuery
#### 1\. 联行号查询
请求：  
```
GET /bankQuery HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

keyWord: "中国 银行"
reqNo: "111"
max: "111"
p: "111"
reqTime: "20151124111059"

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "total": 100,
    "tip": "",
    "reqNo": 100,
    "banks": {[bankDeposit:"2222", unionBankNo: "333"],[bankDeposit:"33", unionBankNo: "444"]},
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"验证成功"
}
```

##### [返回目录↑](#content-title)

<a id="bindBankCard"></a>
### 绑定/解绑用户银行卡  /bindBankCard
#### 1\. 绑定/解绑用户银行卡
请求：  
```
POST /bindBankCard HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

isDelete: "false" //是否解绑
cardIds: "123 124"//解绑卡ID列表 , //解绑时必传

//绑定本人信用卡
bankCard: "XXXX"
idNumber:"XXXXX",//身份证
mobile: "13777775555"
reqTime: "20151124111059"

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "bankCard": "XXXX",
    "bankName": "xx银行",
    "bankIndex": 11,
    "card_type": "借记卡",
    "bankAccountName": XX银行,
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"验证成功"
}
```
##### [返回目录↑](#content-title)

<a id="listBandCard"></a>
### 获取商户绑定银行卡列表  /listBandCard
#### 1\. 获取商户绑定银行卡列表
请求：  
```
GET /listBandCard HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
list:[
{
    "accountNo": "XXXX",
    "bankName": "xx银行",
    "accountName": "罗小苗",
    "status": 1,//认证状态(0:未认证通过, 1:通过)
    "cardId": 1//绑定的ID
},{
    "accountNo": "XXXX",
    "bankName": "xx银行",
    "accountName": "罗小苗",
    "status": 1,//认证状态
    "cardId": 2
}]
]
```
##### [返回目录↑](#content-title)
<a id="activeAndBindEquip"></a>
### 激活绑定设备  /activeAndBindEquip
#### 1\. 激活绑定设备
请求：  
```
POST /activeAndBindEquip HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

ksnNo: "5010100000023402"//ksn号
activeCode: "11C718FF1FD14531"//非必传项(激活码方式激活时必传)
product: "ZFT" //产品型号,//掌富通
model: "landim35" //设备型号
macAddress:"XX:XX:XX:XX"//终端对应的MAC地址
```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",//respCode为ACTIVECODE_IS_NOT_NULL,需跳转到输入激活码界面
    "respMsg":"激活绑定设备成功"
}
```
##### [返回目录↑](#content-title)
<a id="realNameAuth"></a>
### 实名认证  /realNameAuth
#### 1\. 实名认证
请求：  
```
POST /realNameAuth HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

name: "狗剩"
idNumber: "341225199005063896"
personal: 图片名称 //身份证正面照
personalBack: 图片名称 //身份证背面照
hPersonal： 图片名称 //手持身份证照片
```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"实名认证信息已提交,请耐心等待"
}
```
##### [返回目录↑](#content-title)
<a id="realNameAuthStatus"></a>
### 实名认证信息回显  /realNameAuthStatus
#### 1\. 实名认证信息回显
请求：  
```
GET /realNameAuthStatus HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "authStatus":(0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
    "name":"张三",//真实姓名
    "idNumber":"341225199005063796",//身份证
    "personal":"d500000000620995.png",//身份证正面照图片名称
    "personalBack":"b500000000620995.png",//身份证背面照图片名称
    "hPersonal":"b500000000620995.png",//手持身份证照片名称
    "realReason":"用户签名与身份证名字不符",//认证失败原因
    "respMsg":"查询成功"
}
```
##### [返回目录↑](#content-title)
<a id="merchantAuth"></a>
### 商户认证  /merchantAuth
#### 1\. 商户认证
请求：  
```
POST /merchantAuth HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

companyName: "企业名称"
regPlace: "经营地址"
businessLicense: "营业执照号"
business: 图片 //营业执照照片
isCheckMerchantInfo:"1",//是否校验商户资质(0:否，1:是)
```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"商户认证信息已提交,请耐心等待"
}
```
##### [返回目录↑](#content-title)
<a id="merchantAuthStatus"></a>
### 商户认证信息回显  /merchantAuthStatus
#### 1\. 商户认证信息回显
请求：  
```
GET /merchantAuthStatus HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "authStatus":(0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
    "companyName":"大众食品商店",//企业名称
    "business":"gsdfsdfsdfsdfsd,1",//营业执照图片名称
    "regPlace":"兴华大道",//经营地址
    "businessLicense":"5DSF5SDFS5DF",//营业执照号
    "merchantReason":"商户不行",//认证失败原因
    "respMsg":"查询成功"
}
```
##### [返回目录↑](#content-title)
<a id="accountAuth"></a>
### 账户认证  /accountAuth
#### 1\. 账户认证
请求：  
```
POST /accountAuth HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

name: "姓名" //名字
identityCard : 370828199902019876 //身份证号
bankName: "银行名称" 
unionBankNo: "联行号"
accountNo: "银行卡号"
card: 图片 //银行卡图片
mobile: 15877987678 // 手机号
verificationCode : 2343 //验证码
```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"账户认证信息已提交,请耐心等待"
}
```
##### [返回目录↑](#content-title)
<a id="accountAuthStatus"></a>
### 账户认证信息回显  /accountAuthStatus
#### 1\. 账户认证信息回显
请求：  
```
GET /accountAuthStatus HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "authStatus":(0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
    "accountNo":"6217000010012052348",//银行卡号
    "card":"c500000000620995.png",//银行卡图片名称
    "name": "姓名" //名字
    "identityCard" : "370828199902019876" //身份证号
    "mobile": "15877987678" // 手机号
    "bankName":"建设银行",//银行名称
    "unionBankNo":"56SDFSD56SDF",//联行号
    "accountReason":"账户信誉差",//认证失败原因
    "respMsg":"查询成功"
}
```
##### [返回目录↑](#content-title)
<a id="signatureAuth"></a>
### 签名认证  /signatureAuth
#### 1\. 签名认证
请求：  
```
POST /signatureAuth HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

signature: 图片 //签名图片
```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"签名认证信息已提交,请耐心等待"
}
```
##### [返回目录↑](#content-title)
<a id="signatureAuthStatus"></a>
### 签名认证信息回显  /signatureAuthStatus
#### 1\. 签名认证信息回显
请求：  
```
GET /signatureAuthStatus HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "name":"张三",
    "authStatus":(0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
    "signature":"s500000000620995.png",//签名图片名称
    "signatureReason":"签名丑",//认证失败原因
    "respMsg":"查询成功"
}
```

##### [返回目录↑](#content-title)
<a id="sale"></a>
### 消费  /sale
#### 1\. 消费
请求：  
```
POST /sale HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

ksnNo: "800090000004",//设备ksn号
reqNo: "129",//流水号
position: "117.194778,39.113809",//地理位置经纬度信息
amount: "300",//交易金额
cardSerialNum: "001",//卡片序列号
icData: "XXXXXXXX",//IC数据
encPinblock: "XXXXX",//PIN密文
encTracks: "TRACK2",//磁道密文，IC卡必传
checksum:"XXX",//MAC密文
tradeFlag:false//是否为D0交易

```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "reqNo":"129",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "merchantName":"XXX",
    "merchantNo":"111",    
    "terminalNo":"22222",    
    "operatorNo":"01",    
    "resultCode":"00",    
    "cardNoWipe":"333**8233",    
    "amount":"300",    
    "currency":"CNY",    
    "voucherNo":"000130",   
    "batchNo":"001234",   
    "transTime":"20151212125959",   
    "refNo":"666666",
    "authNo":"666666777777",
    "script":"ic55"
}

//当交易失败原因如下时，要求用户去绑卡
{
    "respTime": "20170330195156",
    "isSuccess": false,
    "respCode": "LIMIT_AMOUNT",
    "respMsg": "单笔交易最大限额不得大于1000元，可通过绑定本人信用卡提高交易额度"
}

```
##### [返回目录↑](#content-title)
<a id="queryBalance"></a>
### 余额查询  /queryBalance
#### 1\. 余额查询
请求：  
```
POST /queryBalance HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

ksnNo: "800090000004",//设备ksn号
reqNo: "129",
cardSerialNum: "001",
icData: "XXXXXXXX",
encPinblock: "XXXXX",

```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100
{
    "reqNo":"129",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "balance":"XXX",
    "resultCode":"00",    
    "currency":"CNY",    
    "issuer":"XX银行",
    "cardNoWipe":"4444****888",
    "transTime":"20151212125959", 
    "script":"ic55"
}
```

##### [返回目录↑](#content-title)
<a id="downloadImg"></a>
### 认证图片下载  /downloadImg
#### 1\. 认证图片下载
请求：  
```
GET /downloadImg HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

fileName  : "c5000000000000000.png" //图片名称

```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    
    图片文件
}
```

##### [返回目录↑](#content-title)
<a id="transNotify"></a>
### IC回调  /transNotify
#### 1\. IC回调
请求：  
```
POST /transNotify HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

reqNo:"1234"//本次请求的流水号
origTransTime:"20151212070809"//原始交易时间
origTransType:"sale"//原始交易类型
origReqNo:"1234"//原始请求流水号
icData:"asfakfjasklfdsa"//IC数据
cardNo:"622266000000"//交易卡号
cardSerialNum:"01"//卡序列号
tradeFlag:false //是否为D0交易
```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"查询成功"
}
```
##### [返回目录↑](#content-title)
<a id="swiperChange"></a>
### 更换设备  /swiperChange
#### 1\. 更换设备
请求：  
```
POST /swiperChange HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

reqNo:"1234"//流水号
ksnNo:"XXXXXX"//新设备ksnNo
model:"landim35"
macAddress:"XXXXXXX"// 蓝牙设备必传
```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime":"20151130125253",
    "isSuccess":true,
    "respCode":"SUCCESS",
    "respMsg":"查询成功"
}
```

##### [返回目录↑](#content-title)
<a id="showHtml"></a>
### 静态页面显示  /showHtml
#### 1\. 静态页面显示
请求：  
```
POST /showHtml HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

html: "XXX.html"//文件名称(agreement.html：协议说明，faq-zft.html：常见问题，register-agreement.html：注册协议)

```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
	HTML页面
}
```

##### [返回目录↑](#content-title)

<a id="showProtocol"></a>
### 用户协议 /showProtocol
#### 1\. 用户协议
请求：  
```
GET /showProtocol HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

view: "XXX" //offline_product_protocol:用户协议

```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
	jsp页面
}
```
##### [返回目录↑](#content-title)

<a id="message"></a>
### 获取消息接口  /message
#### 1\. 获取消息接口
请求：  
```
GET /message HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

//获取消息列表
{
    "respTime": "20160606142841", 
    "isSuccess": true, 
    "respCode": "SUCCESS", 
    "respMsg": "成功", 
    "body": [
        {
            "content": "就问你怕不怕！！！", //消息内容
            "linkAddress": "", //链接地址
            "title": "商户通内部测试", //消息标题
            "newsId": "575513d784aeddcc333a7a10",//消息id 
            "linkText": "", //链接地址文本
            "hasLink": false, //是否有链接
            "createTimeStr": "20160606140803",//创建时间 
            "businessType": "1", //消息所属业务(0：理财, 1：vcPos, 2：Pos)
            "newsType": "0", //消息类型(0：公告, 1：通知)
            "isRead": 0//是否已阅读(0:"否", 1:"是")
        }, 
        {
            "content": "通知测试之所有", 
            "linkAddress": "", 
            "title": "通知测试之所有", 
            "newsId": "5753d725737f247007d596db", 
            "linkText": "", 
            "hasLink": false, 
            "createTimeStr": "20160605153917", 
            "readTimeStr": "20160605162312", 
            "businessType": "1", 
            "newsType": "1", 
            "isRead": 1
        }, 
        {
            "content": "通知测试之安卓", 
            "linkAddress": "", 
            "title": "通知测试之安卓", 
            "newsId": "5753d711737f247007d596da", 
            "linkText": "", 
            "hasLink": false, 
            "createTimeStr": "20160605153857", 
            "businessType": "1", 
            "newsType": "1", 
            "isRead": 0
        }
    ], 
    "head": {
        "hasUnRead": true, //是否有未读消息
        "unReadNotice": false,//是否有未读通知
        "unReadBulletin": false,//是否有未读公告
        "totalCount": 3, //消息总条数
        "readCount": 1, //已阅读数
        "unReadCount": 2//未阅读数
    }
}

```

##### [返回目录↑](#content-title)

<a id="banner"></a>
### 获取广告位信息  /banner
#### 1\. 获取广告位信息
请求：  
```
GET /message HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime": "20160115104632", 
    "isSuccess": true, 
    "respCode": "SUCCESS", 
    "respMsg": "成功", 
    "head": {
        "total": "4"
    }, 
    "body": [
        {
            "title": "广告位1", 
            "message": "广告信息1", 
            "imageUrl": "http://image.21er.tk/1.jpg",  //图片路径
            "targetUrl": "http://image.21er.tk/11.jpg" //图片跳转的路径
        }, 
        {
            "title": "广告位2", 
            "message": "广告信息2", 
            "imageUrl": "http://image.21er.tk/2.jpg", 
            "targetUrl": "http://image.21er.tk/21.jpg"
        }, 
        {
            "title": "广告位3", 
            "message": "广告信息3", 
            "imageUrl": "http://image.21er.tk/3.jpg", 
            "targetUrl": "http://image.21er.tk/31.jpg"
        }, 
        {
            "title": "广告位4", 
            "message": "广告信息4", 
            "imageUrl": "http://image.21er.tk/4.jpg", 
            "targetUrl": "http://image.21er.tk/41.jpg"
        }
    ]
}
```

##### [返回目录↑](#content-title)

<a id="merchantQualify"></a>
### 获取商户资质  /merchantQualify
#### 1\. 获取商户资质（包含四审资质以及商户设备绑定状态）
请求：  
```
POST /merchantQualify HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime": "20151228143800",
    "isSuccess": true,
    "respCode": "SUCCESS",
    "respMsg": "成功",
    "merchantQualify"{
	    "terminalAuth": 0 //设备绑定状态 (0未绑定 ,1绑定激活成功),
	    "realNameAuth": 0 //实名认证 (0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
	    "merchantAuth": 0 //商户认证 (0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
	    "accountAuth": 0 //账户认证 (0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
	    "signatureAuth": 0 //签名认证 (0:未认证, 1:认证成功, 2:认证失败, 3:审核中),
    }
}
```

##### [返回目录↑](#content-title)

<a id="findTransList"></a>
### 交易列表查询  /findTransList
#### 1\. 交易列表查询
请求：  
```
POST /findTransList HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

"settleType": "1",//结算类型 1-TN 2-D0
"lastID": "", --上次请求最后一笔交易的ID
"startTime": "2016-06-06", --起始时间 yyyy-MM-dd格式
"endTime": "2016-06-06", --结束时间 yyyy-MM-dd格式 

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime": "20151228143800",
    "isSuccess": true,
    "respCode": "SUCCESS",
    "respMsg": "成功",
    "count": 134,//交易笔数
    "amount": 13684228,//交易总额
    "transList": [    
      {
        "tranid": 676453,--交易id
        "transType": "sale",--交易类型 sale-消费/sale_void-撤销/auth_comp-预授权完成/auth_comp_cancel-预授权完成撤销/refund-退货 
        "transTime": "2016-05-15 15:56:25",--交易时间
        "settleType": "D+0",--结算类型
        "amount": 100 --交易金额(分)
      },
    ...
    ]
}
```

##### [返回目录↑](#content-title)

<a id="tranInfo"></a>
### 交易明细查询  /tranInfo
#### 1\. 交易明细查询
请求：  
```
POST /tranInfo HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

"transId": "21891", //交易ID

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
    "respTime": "20151228143800",
    "isSuccess": true,
    "respCode": "SUCCESS",
    "respMsg": "成功",
    "tranInfo": {
	       "transId": "21891", //交易ID
		"merchantNo": "500000000621891",//商户编号
		"merchantName": "xxx",//商户名称
		"transTime": "2016-05-15 15:56:25",//交易时间
		"batchNo": "000001",//交易批次
		"voucherNo": "000001",//交易流水号
		"terminalNo": "XXXXX",//交易终端号
		"cardNoWipe": "62226******5655",//带星号卡号
		"transType": "sale",//交易类型 -- sale-消费/sale_void-撤销/auth_comp-预授权完成/auth_comp_cancel-预授权完成撤销/refund-退货 
		"transStatus": 1,//交易状态 -- 0失败/1成功/2已冲正/3已撤销/4已退款
		"settleType": "D+0",--结算类型
		"amount": 11111,//交易金额(分)
	}
	
}
```

##### [返回目录↑](#content-title)

<a id="modifyMessage"></a>
### 更新消息状态  /modifyMessage
#### 1\. 更新消息状态
请求：  
```
GET /message HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

messageId: "56711f5b84aea1954cade27b"
detail: false

```

响应： 

```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
     
   "respTime":"20170516165834",
   "isSuccess":true,
   "respCode":"SUCCESS",
   "respMsg":"成功",
   "des":"状态修改成功!",
   "messageId":"584fb24384aee8582ca85da7"

    
}
```

##### [返回目录↑](#content-title)

<a id="settleList"></a>
### 结算列表查询  /settleList
#### 1\. 结算列表查询
请求：  
```
POST /settleList HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

"startTime": "2016-3-14",//起始时间yyyy-MM-dd格式
"endTime": "2016-3-16",//结束时间yyyy-MM-dd格式
"settleType": "1",//结算类型 1-TN 2-D0
"uniqueRecord":"10790-6228480010970642611-995100-d0"//最后一条记录的唯一标识(非必传项)

```
响应：  
```
HTTP/1.1 200 OK
Server: Nginx
Date: Thu, 09 Apr 2015 11:36:53 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Cache-Control: no-cache
Content-Length: 100

{
 "respTime": "20160612174733", 
    "isSuccess": true, 
    "respCode": "SUCCESS", 
    "respMsg": "成功", 
    "count": 7, //总条数
    "amount": 6413673//结算总额(单位:分)
    "settleList": [
        {
            "settleId": 10794, //结算id
            "settleStatus": 2, //结算状态(1:成功, 2:失败)
            "transAmount": 29853, //交易金额(单位:分)
            "settleMoney": 29853, //结算金额(单位:分)
            "accountNum": "6228480010970642611", //结算账户 
            "settleType": "D+0", //结算类型
            "merchantName": "旧数据企业", //商户名称
            "merchantNo": "500000000876552", //商户号
            "settleDate": "2016-03-15", //结算日期
            "uniqueRecord": "10794-6228480010970642611-298530-d0"//唯一标识
			
        }
	...
    ], 
}
```

##### [返回目录↑](#content-title)
