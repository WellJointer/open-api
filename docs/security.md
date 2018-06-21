# 安全认证

最近更新日期：{docsify-updated}

Token是WellCloud平台的全局唯一接口调用凭据，第三方调用各接口时都需使用Token, Token的过期时间是24小时。获取Token需要appId和appSecret，这两个字段在登陆租户控制台后，可以在联络中心管理菜单下 -> 第三方系统集成配置中找到。

> 文档格式说明
> url中类似/user/{{id}}/，表示最终{{id}}需要用实际的id来替换。例如：/user/12345678/

# 1. 申请Token

在成功获取token后，随后的所有请求都需要带有一个头部信息

```
Authorization: token
```

## 1.1 请求示例 {docsify-ignore}

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

## 1.2 路径与查询字符串参数模型 {docsify-ignore}

`POST http://tpiag.wellcloud.cc/p/api/operation/operation/apply/token?appId={{appId}}&appSecret={{appSecret}}&type={{type}}`

名称 | 是否必须 | 说明
---|---|---
appId | 是 | appId
appSecret | 是 | appSecret
type | 是 | 租户类型。partner代表合作伙伴，tenant表示租户

## 1.3 响应体说明 {docsify-ignore}

名称 | 说明
---|---
token | token


## 1.4 异常响应与解决方案 {docsify-ignore}

异常响应体 | 问题原因 |解决方案
--- | --- | ---
{"message":"RESTEASY003065: Cannot consume content type","code":"500"} | 发送的请求Content-Type有问题，导致服务端无法解析 | 请检查发送的数据 Content-Type 头的值是否是 application/json;charset=utf-8