# 录音调听

最近更新日期：{docsify-updated}

# 1. 根据callId查询录音

**请求示例**

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
**路径与查询字符串参数模型**

`GET http://tpisdk.wellcloud.cc/api/operation/tenant/calls/{{callId}}/recordings2`

名称 | 是否必须 | 说明
---|---|---
callId | 是 | callId

**响应体说明**

由于一个callId可能会有多段录音，所以返回的响应体是一个数组。录音数据模型说明。

名称 | 说明
---|---
id | 录音id
ani | 主叫
dnis | 被叫
callDirection | 呼叫方向。Inbound表示呼入，Outbound表示呼出，Internal表示分机间内部呼叫，Offhook表示摘机，PreOccupied表示预占式外呼，PreDictive表示预测式外呼
startTime | 开始时间
endTime | 结束时间
callLength | 呼叫时长
fileSize | 文件大小
deviceId | 设备Id。指分机id, 如8001@test.cc
callId | callId
state | 状态。Fail表示失败，Success表示成功
tenantId | 租户id

# 2. 录音流下载与调听

**请求示例**

```
// general
GET http://tpisdk.wellcloud.cc/api/operation/tenant/recording/{{audioId}}/stream2?download=false

// request headers
Authorization: 12345678

// response
stream
```

**路径与查询字符串参数模型**

`GET http://tpisdk.wellcloud.cc/api/operation/tenant/recording/{{audioId}}/stream2?download={{downlaod}}`

名称 | 是否必须 | 说明
---|---|---
audioId | 是 | 录音id
download | 是 | 是否需要下载。true表示下载，false表示不下载(默认)


**响应体说明**
响应体是以流的形式下载或调听

# 3. 查询所有录音

**请求示例**

请求头部字段`Authorization`字段值即申请到的`token`值

```
// general
GET http://tpisdk.wellcloud.cc/api/operation/tenant/recordings

// request headers
Authorization: 12345678
Content-Type: application/json;charset=utf-8

// response
  [{
  "id":
  "00b1e04f-d9a8-4980-88c6-3a3353003ab5",
  "callDirection":
  "Internal",
  "startTime": "2016.07.07 09:33:14",
  "endTime": "2016.07.07 09:33:25",
  "callLength": 11,
  "fileName":
  "/mnt/volumes/recordings/2016/0707/8003@testwdd2.com/20160707093314_00b1e04f-d9a8-4980-88c6-3a3353003ab5.mp3",
  "deviceId":
  "8003@testwdd2.com",
  "agentId":
  "5004@testwdd2.com",
  "callId":
  "00b1e04f-d9a8-4980-88c6-3a3353003ab5",
  "state": "Success",
  "agentName": "张三",
  "tenantId": "e561449e-6d52-45e5-87ff-308fec2d5c5d"
  }]
```

**路径与查询字符串参数模型**

`GET http://tpisdk.wellcloud.cc/api/operation/tenant/recordings`

**响应体说明**

名称 | 说明
---|---
id | 录音id
ani | 主叫
dnis | 被叫
callDirection | 呼叫方向。Inbound表示呼入，Outbound表示呼出，Internal表示分机间内部呼叫，Offhook表示摘机，PreOccupied表示预占式外呼，PreDictive表示预测式外呼
startTime | 开始时间
endTime | 结束时间
callLength | 呼叫时长
fileSize | 文件大小
deviceId | 设备Id。指分机id, 如8001@test.cc
callId | callId
state | 状态。Fail表示失败，Success表示成功
tenantId | 租户id

# 4. 批量下载录音

**请求示例**

```
// general
GET http://tpisdk.wellcloud.cc/api/operation/tenant/batch/download

// request headers
Authorization: 12345678

// response
stream
```

**路径与查询字符串参数模型**

`GET http://tpisdk.wellcloud.cc/api/operation/tenant/batch/download`











