[TOC]

## 安装

### npm 安装

```javascript
npm i mighty-js-sdk
import "mighty-js-sdk/dist/mightyJsSdk.js";
//use in vue3
app.config.globalProperties.$mightyJsSdk = window.mightyJsSdk;
```

### script 引入

```javascript
<script src="mightyJsSdk/dist/mightyJsSdk.js" />
```

### 使用(webview 内嵌页面、H5 浏览器调用场景)

#### 通用说明

api 最后一个参数 options：唤起 app 失败或者成功的回调方法，默认唤起失败的回调方法是跳转到 Mighty 下载页

```javascript
{
  callAppSuccess:function(){},
  callAppFail:function(){}
}
```

#### 授权

```javascript
//@param String authToken  //商家授权token
//@param Function Callback
//@param Object? {callAppSuccess,callAppFail}
//@response Object {code:stinrg,data:Object,message:string}
mightyJsSdk.auth(
  authToken,
  (result) => {
    console.log(result);
  },
  options
);
```

调用参数：

```json
{
  "token": "477599683939872768",
  "appId": "475070042590339072"
}
```

Callback 回调：

```json
{
  "code": "0",
  "data": "55e7534dd2bd497793a25d430b4f61f0" //用户授权临时令牌（token)
}
```

其他：

```json
{
  "code": "10",
  "message": "用户放弃授权"
}
```

```json
{
  "code": "11",
  "message": "用户拒绝授权"
}
```

说明：
1.webview 网页情况：在回调函数中返回 token，
2.h5 唤起情况：在商家回调地址中获取 token 的查询参数。例如：
{{回调地址}}?token=55e7534dd2bd497793a25d430b4f61f0

#### 支付

```javascript
//@param String orderId
//@param Function Callback
//@param Object? {callAppSuccess,callAppFail}
//@response Object {code:stinrg,data:Object,message:string}
mightyJsSdk.pay(
  orderId,
  (result) => {
    console.log(result);
  },
  options
);
```

调用参数：

```
orderId:477599683939872768
```

Callback 回调：

```json
{
  "code": "0",
  "data": { "status": "2", "returnUrl": "http://return;" }
}
```

其他：

```json
{
  "code": "1",
  "message": "系统异常"
}
```

```json
{
  "code": "-1",
  "message": "用户取消支付"
}
```

```json
{
  "code": "2",
  "message": "支付成功"
}
```

```json
{
  "code": "3",
  "message": "订单已取消"
}
```

```json
{
  "code": "4",
  "message": "订单已超时关闭"
}
```

```json
{
  "code": "5",
  "message": "订单异常"
}
```

```json
{
  "code": "6",
  "message": "订单已支付"
}
```

```json
{
  "code": "8",
  "message": "订单支付中"
}
```

```json
{
  "code": "28506",
  "message": "账号被锁定"
}
```

**注：H5 外部浏览器调用无回调，由商家自行轮询订单支付结果**

#### 分享

```javascript
/* 
  @param Object {shared_desc,shared_url,shared_cover_url,app_id,shared_type,shared_name}
  param detail:
  shared_desc        商品详情介绍
  shared_url         分享链接
  shared_cover_url   封面图
  app_id             商户appId
  shared_type        1:小程序 2:链接分享 3：小程序商品

  @param Function Callback
  @param Object? {callAppSuccess,callAppFail}
  @response Object {code:stinrg,data:Object,message:string}
*/

mightyJsSdk.share(
  { token, appId },
  (result) => {
    console.log(result);
  },
  options
);
```

调用参数：

```json
{
  "shared_desc": "商品描述啊啊啊",
  "shared_url": "http://www.baidu.com",
  "shared_cover_url": "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRoIJQVHQ8NtlS02Vtno0b81X9BQNkg34e1tg&usqp=CAU",
  "app_id": "475070042590339072",
  "shared_type": "2",
  "shared_name": "来看看"
}
```

**Callback 回调：无**
