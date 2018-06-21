**1.1 请求示例**

```
// general
POST http://tpiag.wellcloud.cc/p/api/operation/operation/apply/token?appId=abcdefg&appSecret=123456&type=tenant

// request headers
Content-Type: application/json;charset=utf-8

// response
{
  "token": "12345678"
}
```

**1.2 路径与查询字符串参数模型**

`POST http://tpiag.wellcloud.cc/p/api/operation/operation/apply/token?appId={{appId}}&appSecret={{appSecret}}&type={{type}}`

名称 | 是否必须 | 说明
---|---|---
appId | 是 | appId
appSecret | 是 | appSecret
type | 是 | 租户类型。partner代表合作伙伴，tenant表示租户

**响应体说明**

名称 | 说明
---|---
token | token


**异常响应与解决方案**

异常响应体 | 问题原因 |解决方案
--- | --- | ---
{"message":"RESTEASY003065: Cannot consume content type","code":"500"} | 发送的请求Content-Type有问题，导致服务端无法解析 | 请检查发送的数据 Content-Type 头的值是否是 application/json;charset=utf-8