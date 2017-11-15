#商户通——NEW接口规范
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
http://192.168.1.240:29002 # 测试环境
```

## 公共请求参数
```
appVersion: "ios.ZFT.1.3.611"//IOS 
```

## 标准请求
```sh
curl -X POST \
    http://192.168.1.240:29002/<资源路径> \
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
| 获取验证码| [/sendMessage](#sendMessage)                      | urlencoded           | POST   | 张树彬     | 否   |
| 校验验证码| [/checkMobileMessage](#checkMobileMessage)                      | urlencoded           | POST   | 张树彬     | 否   |
| 注册| [/register](#register)                      | urlencoded           | POST   | 张树彬     | 否   |
| 忘记密码| [/forgetPassword](#forgetPassword)                      | urlencoded           | POST   | 张树彬     | 否   |
|登录| [/login](#login)                      | urlencoded           | POST      | 张树彬     | 否   |
|获取用户设备状态| [/findUserEquipment](#findUserEquipment)                      | urlencoded           | GET      | ZPP    | 是   |

----------------------------------------------------------------------------------
<a id="sendMessage"></a>
### 获取验证码  /sendMessage
#### 1\. 通过手机号获取验证码(支持注册、找回密码)
请求：  
```
POST /sendMessage HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

mobile: "15801376995" //手机号
type: "register" //操作类型：注册:register 找回:forget
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
### 校验验证码 /checkMobileMessage
#### 1\. 校验验证码
请求：  
```
POST /checkMobileMessage HTTP/1.1
Host: mposp.21er.tk
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30

mobile: "15801376995" // 手机号
idCode: "1234" //验证码

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
    "respMsg":"校验通过"
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

mobile: "15801376995" // 手机号
password: "ERERWERIOWJERWE2112332Y_" // 密码(加密的密文，加密规则咨询维护人) 

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

password: "123456" //密码(加密的密文，加密规则咨询维护人) 
mobile: "15801376995"
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
    "respMsg":"修改成功"
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

"password": "qqqqqq" //密码(加密的密文，加密规则咨询维护人) 
"loginName": "18911156118" 
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
    "respCode": "SUCCESS",
    "respMsg": "登录成功"
}
```
##### [返回目录↑](#content-title)
<a id="findUserEquipment"></a>
### 登录  /findUserEquipment
#### 1\. 查询用户绑定设备状态
请求：
```
GET /login HTTP/1.1
Host: XXXXXX:XXXX
Date: Thu, 03 Dec 2015 10:22:53
Content-Type: application/x-www-form-urlencoded; charset=utf-8
Content-Length: 30
WSSESSION: ow5bblf8ethknl59vqp8qbbt-52
"userId": "18911156118"  //用户名

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
//如果用户已经绑定设备
{
    "respTime": "20151228143800",
    "isSuccess": true,
    "respCode": "BIND_EQUIPMENT",
    "respMsg": "以绑设备"
}
//如果用户没有绑定设备
{
   "respTime":"20171115152246",
   "isSuccess":false,
   "respCode":"NO_BIND_EQUIPMENT",
   "respMsg":"未绑设备"
}

```

