# 录音调听

最近更新日期：{docsify-updated}

# 1. 根据callId查录音(说明：由于通话结束到写入数据库有3秒延迟，4秒后再获取录音)

## 1.1 请求示例 {docsify-ignore}

请求头部字段`Authorization`字段值即申请到的`token`值

```
// general
GET http://tpisdk.wellcloud.cc/api/operation/tenant/calls/c19e9298-fb83-445f-8206-19b59ea7e337/recordings2

// request headers
Authorization: 12345678
Content-Type: application/json;charset=utf-8

// response
[
  {
    "id": "a0981ac5-b1ec-495c-a17f-0d28f5ccef3d",
    "ani": "8010@zhen04.cc",
    "dnis": "933255490@zhen04.cc",
    "callDirection": "Outbound",
    "startTime": "2018.01.29 16:08:23",
    "endTime": "2018.01.29 16:08:34",
    "callLength": 10,
    "fileSize": 0,
    "deviceId": "8010@zhen04.cc",
    "callId": "c19e9298-fb83-445f-8206-19b59ea7e337",
    "state": "Success",
    "tenantId": "61739917-7ba2-43e9-b81a-5a8eb41461b4"
  }
]
```
## 1.2 路径与查询字符串参数模型 {docsify-ignore}

`GET http://tpisdk.wellcloud.cc/api/operation/tenant/calls/{{callId}}/recordings2`

| 名称     | 是否必须 | 说明     |
| ------ | ---- | ------ |
| callId | 是    | callId |

## 1.3 响应体说明 {docsify-ignore}

由于一个callId可能会有多段录音，所以返回的响应体是一个数组。录音数据模型说明。

| 名称            | 说明                                       |
| ------------- | ---------------------------------------- |
| id            | 录音id                                     |
| ani           | 主叫                                       |
| dnis          | 被叫                                       |
| callDirection | 呼叫方向。Inbound表示呼入，Outbound表示呼出，Internal表示分机间内部呼叫，Offhook表示摘机，PreOccupied表示预占式外呼，PreDictive表示预测式外呼 |
| startTime     | 开始时间                                     |
| endTime       | 结束时间                                     |
| callLength    | 呼叫时长                                     |
| fileSize      | 文件大小                                     |
| deviceId      | 设备Id。指分机id, 如8001@test.cc                |
| callId        | callId                                   |
| state         | 状态。Fail表示失败，Success表示成功                  |
| tenantId      | 租户id                                     |

## 1.4 错误码说明 {docsify-ignore}

| 错误码       | 说明                             |
| -------- | ------------------------------ |
| 448  | 录音文件不存在                           |

# 2 录音流下载与调听

## 2.1 请求示例 {docsify-ignore}

```
// general
GET http://tpisdk.wellcloud.cc/api/operation/tenant/recording/{{audioId}}/stream2?download=false

// request headers
Authorization: 12345678

// response
stream
```

## 2.2 路径与查询字符串参数模型 {docsify-ignore}

`GET http://tpisdk.wellcloud.cc/api/operation/tenant/recording/{{audioId}}/stream2?download={{downlaod}}`

| 名称       | 是否必须 | 说明                             |
| -------- | ---- | ------------------------------ |
| audioId  | 是    | 录音id                           |
| download | 是    | 是否需要下载。true表示下载，false表示不下载(默认) |


## 2.3 响应体说明 {docsify-ignore}

响应体是以流的形式下载或调听

## 2.4 错误码说明 {docsify-ignore}

| 错误码       | 说明                             |
| -------- | ------------------------------ |
| 448  | 录音文件不存在                           |
| 425  | 根据录音id找不到这通记录                  |

# 3 根据callId下载与调听录音

## 3.1 请求示例 {docsify-ignore}

请求头部字段`Authorization`字段值即申请到的`token`值

```
//general
GET http://tpisdk.wellcloud.cc/api/operation/tenant/calls/{{callId}}/recording/stream?download=false

// request headers
Authorization: 12345678

//response
stream
```
## 3.2 路径与查询字符串参数模型 {docsify-ignore}

`GET http://tpisdk.wellcloud.cc/api/operation/tenant/calls/{{callId}}/recording/stream?download={{downlaod}}`

| 名称       | 是否必须 | 说明                             |
| -------- | ---- | ------------------------------ |
| callId   | 是    | callId                         |
| download | 是    | 是否需要下载。true表示下载，false表示不下载(默认) |


## 3.3 响应体说明 {docsify-ignore}

响应体是以流的形式下载或调听

## 3.4 错误码说明 {docsify-ignore}

| 错误码       | 说明                             |
| -------- | ------------------------------ |
| 448  | 录音文件不存在                           |




