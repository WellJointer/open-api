# 组织架构管理

最近更新日期：{docsify-updated}

# 1 创建部门

## 1.1 请求示例 {docsify-ignore}
 
请求头部字段`Authorization`字段值即申请到的`token`值

```
// general
POST http://tpisdk.wellcloud.cc/api/security/security/org

//body
{
  "name": "部门创建示例",
  "description": "部门创建示例",

  "parent" : {
     "id":"88d34fe2-6ca3-4038-aab8-de5f239735ee"
  }
}

// request headers
Authorization: 12345678
Content-Type: application/json;charset=utf-8

// response
{
  "id": "bf5f4202-79a9-423b-b2f8-7479bcf4290a",
  "name": "部门创建示例",
  "description": "部门创建示例",
  "treePos": "88d34fe2-6ca3-4038-aab8-de5f239735ee_1"
}
```

## 1.2 路径与查询字符串参数模型 {docsify-ignore}

`POST http://tpisdk.wellcloud.cc/api/security/security/org`

## 1.3 请求体说明 {docsify-ignore}

属性 | 类型 | 约束 | 说明
--- | --- | --- | ---
name|string|必选|部门名称
description|string|非必选|对此部门的描述
parent||必选|上级部门

## 1.4 响应体说明 {docsify-ignore}

属性|说明
---|---
treePos|树形结构标识

# 2 查询顶级部门

## 2.1 请求示例 {docsify-ignore}

```
// general
GET http://tpisdk.wellcloud.cc/api/security/security/org/topOrg

// request headers
Authorization: 12345678

// response
{
  "id": "88d34fe2-6ca3-4038-aab8-de5f239735ee",
  "name": "newtest",
  "description": "newtest",
  "treePos": "88d34fe2-6ca3-4038-aab8-de5f239735ee",
  "tenantId": "960e92dc-3bea-4fd9-96ec-cfc6a8fd84a4",
  "children": [
    {
      "id": "bf5f4202-79a9-423b-b2f8-7479bcf4290a",
      "name": "部门创建示例",
      "description": "部门创建示例",
      "treePos": "88d34fe2-6ca3-4038-aab8-de5f239735ee_1",
      "children": []
    }
  ]
}
```

## 2.2 路径与查询字符串参数模型 {docsify-ignore}

`GET http://tpisdk.wellcloud.cc/api/security/security/org/topOrg`

## 2.3 响应体说明 {docsify-ignore}

属性 | 类型 | 约束 | 说明
--- | --- | --- | ---
id|string|非必选|部门id，唯一标识部门
name|string|必选|部门名称
description|string|非必选|对此部门的描述
data|string|非必选|存放一些业务数据，可以忽略
treePos|string|非必选|可以忽略
children|list|非必选|是一个数组，其中的元素也是一个部门对象，表示其下级部门
parent|array|必选|表示其上级部门的id，以此为根据创建下级部门
说明：
children是一个数组，其中的每个元素都是一个部门的信息。

# 3. 查询部门信息

## 3.1 请求示例 {docsify-ignore}

请求头部字段`Authorization`字段值即申请到的`token`值

```
// general
GET http://tpisdk.wellcloud.cc/api/security/security/org/bf5f4202-79a9-423b-b2f8-7479bcf4290a

// request headers
Authorization: 12345678
Content-Type: application/json;charset=utf-8

// response
{
  "id": "bf5f4202-79a9-423b-b2f8-7479bcf4290a",
  "name": "部门创建示例",
  "description": "部门创建示例",
  "treePos": "88d34fe2-6ca3-4038-aab8-de5f239735ee_1",
  "children": []
}
```

## 3.2 路径与查询字符串参数模型 {docsify-ignore}

`GET http://tpisdk.wellcloud.cc/api/security/security/org/{orgId}`

## 3.3 响应体说明 {docsify-ignore}

属性 | 类型  | 说明
--- | --- |  ---
id|string|部门id，唯一标识部门
name|string|部门名称
description|string|对此部门的描述
data|string|存放一些业务数据，可以忽略
treePos|string|可以忽略
children|list|是一个数组，其中的元素也是一个部门对象，表示其下级部门
parent|array|表示其上级部门的id，以此为根据创建下级部门

# 4 查询部门及子部门信息 

## 4.1 请求示例 {docsify-ignore}

```
// general
GET http://tpisdk.wellcloud.cc/api/security/security/org/subOrgs/88d34fe2-6ca3-4038-aab8-de5f239735ee

// request headers
Authorization: 12345678

// response
{
  "root": [
    {
      "id": "bf5f4202-79a9-423b-b2f8-7479bcf4290a",
      "name": "部门创建示例",
      "description": "部门创建示例",
      "treePos": "88d34fe2-6ca3-4038-aab8-de5f239735ee_1",
      "children": []
    },
    {
      "id": "88d34fe2-6ca3-4038-aab8-de5f239735ee",
      "name": "newtest",
      "description": "newtest",
      "treePos": "88d34fe2-6ca3-4038-aab8-de5f239735ee",
      "tenantId": "960e92dc-3bea-4fd9-96ec-cfc6a8fd84a4",
      "children": [
        {
          "id": "bf5f4202-79a9-423b-b2f8-7479bcf4290a",
          "name": "部门创建示例",
          "description": "部门创建示例",
          "treePos": "88d34fe2-6ca3-4038-aab8-de5f239735ee_1",
          "children": []
        }
      ]
    }
  ],
  "total": 2
}
```

## 4.2 路径与查询字符串参数模型 {docsify-ignore}

`GET http://tpisdk.wellcloud.cc/api/security/security/org/subOrgs/{orgId}`

## 4.3 响应体说明 {docsify-ignore}

属性 | 类型  | 说明
--- | --- |  ---
id|string|部门id，唯一标识部门
name|string|部门名称
description|string|对此部门的描述
data|string|存放一些业务数据，可以忽略
treePos|string|可以忽略
children|list|是一个数组，其中的元素也是一个部门对象，表示其下级部门
parent|array|表示其上级部门的id，以此为根据创建下级部门


# 5 更新部门信息

## 5.1 请求示例 {docsify-ignore}

```
// general
PUT http://tpisdk.wellcloud.cc/api/security/security/org

//body
{
  "id": "bf5f4202-79a9-423b-b2f8-7479bcf4290a",
  "name": "部门更新示例"
}
说明：表示要更新部门id是bf5f4202-79a9-423b-b2f8-7479bcf4290a的部门名称，无返回值。

// request headers
Authorization: 12345678
```

## 5.2 路径与查询字符串参数模型 {docsify-ignore}

`PUT http://tpisdk.wellcloud.cc/api/security/security/org`


# 6 删除部门

## 6.1 请求示例 {docsify-ignore}

```
// general
DELETE http://tpisdk.wellcloud.cc/api/security/security/org/bf5f4202-79a9-423b-b2f8-7479bcf4290a
说明：删除部门id为bf5f4202-79a9-423b-b2f8-7479bcf4290a的部门，无返回值。

// request headers
Authorization: 12345678
```

## 6.2 路径与查询字符串参数模型 {docsify-ignore}

`DELETE http://tpisdk.wellcloud.cc/api/security/security/org/{orgId}`










