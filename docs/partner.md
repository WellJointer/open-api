# 合作伙伴接口

最近更新日期：{docsify-updated}

# 1 查询租户所有分机
## 1.1 请求示例 {docsify-ignore}

请求头部字段`Authorization`字段值即申请到的`partner`类型的`token`值

```
// general
GET http://tpisdk.wellcloud.cc/api/operation/operation/partner/tenant/extensions?domain=dengchangying.cc

// request headers
Authorization: 12345678


// response
{
  "root": [
    {
      "id": "56c11ffd-7611-498c-b6b6-4de5012e5d6e",
      "code": "8001",
      "state": "Ok",
      "externalId": "27148",
      "tenantId": "12b554b7-a33e-474a-b52d-78cdc3596fe3",
      "namespace": "dengchangying.cc",
      "updateTime": "2017.12.01 09:46:53",
      "password": "vm3II472",
      "featureRecord": "Enable",
      "org": {
        "children": [
          {
            "id": "0564907a-c698-45a1-a366-e25a9f729a72",
            "name": "测试部",
            "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_2",
            "children": [
              {
                "id": "2cabc36d-7b94-4e1e-82d3-bd689cadc5c5",
                "name": "测试人员3",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_2_3",
                "children": [
                  
                ]
              },
              {
                "id": "6602ec8f-4164-4468-a0d9-08069cec2936",
                "name": "测试人员1",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_2_1",
                "children": [
                  
                ]
              },
              {
                "id": "dbc18011-027d-4edd-90b4-6183b24aab94",
                "name": "测试人员2",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_2_2",
                "children": [
                  
                ]
              }
            ]
          },
          {
            "id": "50ed2e79-3815-4f79-9475-4791b96424a0",
            "name": "需求分析部",
            "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_4",
            "children": [
              {
                "id": "a476d6b7-a5a7-4742-9995-c02ac6d0a32c",
                "name": "需求分析1",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_4_1",
                "children": [
                  
                ]
              },
              {
                "id": "e6006c33-e97a-40ed-8837-2fab85b0378e",
                "name": "需求分析2",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_4_2",
                "children": [
                  
                ]
              },
              {
                "id": "fce601ff-bee4-4fa3-bd78-ed839fdc7f1b",
                "name": "需求分析3",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_4_3",
                "children": [
                  
                ]
              }
            ]
          },
          {
            "id": "5b47bea7-691d-4a17-b702-2e5b927b6674",
            "name": "运维部",
            "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_3",
            "children": [
              {
                "id": "79dfc8d9-d12e-43dd-9719-3fc5275f12c1",
                "name": "运维人员1",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_3_1",
                "children": [
                  
                ]
              },
              {
                "id": "95a21a85-85a3-4a48-b69f-993c7a2e6b55",
                "name": "运维人员3",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_3_3",
                "children": [
                  
                ]
              },
              {
                "id": "b6a59768-f169-491e-bf3d-7364a2154f4a",
                "name": "运维人员2",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_3_2",
                "children": [
                  
                ]
              }
            ]
          },
          {
            "id": "cd21d329-5bb0-4109-baeb-e982d1f9c6d4",
            "name": "开发部",
            "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_1",
            "children": [
              {
                "id": "2dd62ac9-c599-483e-804c-c255d314f725",
                "name": "开发人员2",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_1_2",
                "children": [
                  
                ]
              },
              {
                "id": "93d9f0c5-790c-45d9-b30b-ec4ccf1995ec",
                "name": "开发人员1",
                "treePos": "e4ee2031-fd55-45c6-9edf-5d7474173192_1_3",
                "children": [
                  
                ]
              }
            ]
          }
        ]
      }
    }
  ],
  "total": 1
}
```
## 1.2 路径与查询字符串参数模型 {docsify-ignore}

`GET http://tpisdk.wellcloud.cc/api/operation/operation/partner/tenant/extensions?domain={domain}`

## 1.3 参数说明 {docsify-ignore}

名称 | 约束 | 说明
---|---|---
domain| 必填 | 域名

## 1.4 响应体说明 {docsify-ignore}

属性|说明
---|---
id|分机ID
state|状态
tenantId|租户ID
namespace|域名
updateTime|更新时间
code|分机号
password|密码
org|组织架构

