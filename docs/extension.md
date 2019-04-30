# 分机管理

最近更新日期：{docsify-updated}

# 1.创建分机

## 1.1 请求示例 {docsify-ignore}

```
// general
POST http://tpisdk.wellcloud.cc/api/operation/tenant/resources/extensions

//body
{
	"code": "8146",
	"networkAddress": "ces",
	"password": "Aa123456",
	"_type": "TExtension",
	"org": {
		"id": "5f8d11e7-5013-4602-8603-b1f5b6bc662b"
	}
}

// request headers
Authorization: 12345678
Content-Type: application/json;charset=utf-8

// response
{
	"id": "2596dc5d-578d-48e1-b805-8278a4e0b84f",
	"code": "8146",
	"state": "Ok",
	"tenantId": "365b7927-ea41-4825-8491-134493dc21a7",
	"namespace": "008.cc",
	"updateTime": "2018.06.21 11:35:04",
	"password": "Aa123456",
	"networkAddress": "ces",
	"featureRecord": "Enable",
	"org": {
		"id": "5f8d11e7-5013-4602-8603-b1f5b6bc662b",
		"name": "售后组",
		"description": "售后组",
		"treePos": "38b48ca4-e392-41e8-805e-c97367aa4072_6_2",
		"children": []
	}
}
```

## 1.2 路径与查询字符串参数模型 {docsify-ignore}

`POST http://tpisdk.wellcloud.cc/api/operation/tenant/resources/extensions
`

## 1.3 请求体说明 {docsify-ignore}

属性|类型|约束|说明
---|---|---|---
code|String|必填|分机号
networkAddress|String|非必填
password|String|必填|密码
_type|String|必填|类型
org||非必填|组织架构

## 1.4 响应体说明 {docsify-ignore}

属性|说明
---|---
id|分机ID
state|状态
tenantId|租户ID
namespace|域名
updateTime|更新时间
