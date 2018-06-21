# 座席管理

最近更新日期：{docsify-updated}

# 1.创建坐席
**1.1 请求示例**

```
// general
POST http://tpisdk.wellcloud.cc/api/operation/tenant/agents

// body
{
  "loginName": "testAgent",
  "userName": "testAgent",
  "status": "Ok",
  "roles": [
    {
      "id": "32ad74aa-d975-48c9-8d48-c6d963c778d5"
    }
  ],
  "org": {
    "id": "88d34fe2-6ca3-4038-aab8-de5f239735ee"
  },
  "tenantId": "960e92dc-3bea-4fd9-96ec-cfc6a8fd84a4",
  "namespace": "n01.cc",
  "code": "5003",
  "agentQueues": [],
  "useOnlineService": false
}

// request headers
Authorization: 12345678
Content-Type: application/json;charset=utf-8

// response
{
  "id": "da642253-15b1-4ca7-b701-dc9a3abfe292",
  "loginName": "testAgent",
  "userName": "testAgent",
  "status": "Ok",
  "roles": [
    {
      "id": "32ad74aa-d975-48c9-8d48-c6d963c778d5"
    }
  ],
  "org": {
    "id": "88d34fe2-6ca3-4038-aab8-de5f239735ee",
    "name": "newtest",
    "description": "newtest",
    "tenantId": "960e92dc-3bea-4fd9-96ec-cfc6a8fd84a4",
    "children": []
  },
  "tenantId": "960e92dc-3bea-4fd9-96ec-cfc6a8fd84a4",
  "namespace": "n01.cc",
  "updateTime": "2017.12.13 09:55:59",
  "code": "5003",
  "agentQueues": [],
  "useOnlineService": false
}
```

**1.2 路径与查询字符串参数模型**

`POST http://tpisdk.wellcloud.cc/api/operation/tenant/agents`

**请求参数说明**

属性 | 类型 | 约束 | 说明
--- | --- | --- | ---
code | string | 必选|  座席工号
loginName| string | 必选|  登录名
userName| string | 必选|   用户名
roles |list|非必选|座席角色
org |array| 非必选|座席所在的组织结构，默认在总公司下
tenantId|string |非必选|租户id
namespace|string|非必选|租户域名
agentQueues|list|非必选|座席技能组
useOnlineService|string|非必选|是否使用第3方集成配置
password|string|非必选|如果不指定，默认为Aa123456
gender|string|非必选|Male,Female
phone|string|非必选|手机号码
email|string|非必选|邮箱

**响应体说明**

属性 | 说明
--- | ---
id|创建座席的唯一标识
status |创建座席的状态, Ok表示可用
updateTime|最近一次修改这个座席信息的时间点