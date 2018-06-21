# 自动外呼

# 1. 名单上传(文件方式)

**请求示例**

```
// general
POST http://ocm.wellcloud.cc:8099/ocm/fileupload?activity_id=12345678

// request headers
Authorization: 12345678
Content-Type: Multipart/File

// reuest body is a txt file, like names.txt
// format: id,productId,phoneNumber
12345,S,17412345678
12345,S,17412345678
12345,S,17412345678

```

**路径与查询字符串参数模型**

`POST http://ocm.wellcloud.cc:8099/ocm/fileupload?activity_id={{activity_id}}`

名称 | 是否必须 | 说明
---|---|---
activity_id | 是 | 活动id，创建活动后由外呼平台提供

**请求体说明**
- 请求体是一个文本文件，可以使用txt、csv以及dat格式的文件
- 文件名不做要求，建议使用英文名
- 文件大小不要超过10MB
- 文件的每一行由名单Id, 产品id, 手机号，客户id组成。四者之间要用英文逗号隔开

注：客户id为可选字段，可以不用上传

示例：

```
12345,S,17412345678,a01
12345,S,17412345678,a02
12345,S,17412345678,a03
```

**响应体说明**

状态码 | 说明
---|---
200 | 名单调用成功
404 | 地址错误
500 | 内部服务错误

# 2. 名单上传(接口方式)




**请求示例**

```
// general
POST http://ocm.wellcloud.cc:8099/ocm/v1/rosterinfo/create/123456

// request headers
Authorization: 12345678
Content-Type: application/json;charset=utf-8

// request body
[
    {
        "rosterId":"12345",
        "prdType":"E",
        "number":"123456",
        "agent":"5001"
    },
    {
        "rosterId":"12345",
        "prdType":"E",
        "number":"123456",
        "agent":"5001"
    }
]
```

**路径与查询字符串参数模型**

`POST http://ocm.wellcloud.cc:8099/ocm/v1/rosterinfo/create/{activityId}`

**请求URl参数**

名称 | 是否必须 | 说明
---|---|---
activity_id | 是 | 活动id，创建活动后由外呼平台提供

**请求体参数**

名称 | 是否必须 | 说明
---|---|---
rosterId | 是 | 名单的流水号，字符类型 
prdType | 是 | 活动类型，可以自定义，不影响业务 
number | 是 | 客户号码 
agent | 否 | 为客户预先分配的座席 

# 3. 数据回传

当使用WellCloud平台自动外呼功能的时候，可以选择将自动外呼的呼叫结果自动推送到设置的回传结果地址中去。

使用这个功能，可以从名单的角度去统计名单拨打的状态。

**开启方式**

结果回传地址为活动的属性，如果需要开启结果回传功能，则需要执行以下步骤：

1. 进入租户控制台console.wellcoud.cc


2. 在左侧选择自动外呼->活动管理

![](http://p3alsaatj.bkt.clouddn.com/20180621141108_esthh6_tm-ocm.jpeg)

3. 选择需要回传结果的活动，点击设置，选择回传字段标签页

![](http://p3alsaatj.bkt.clouddn.com/20180621141147_UVPPPR_ocm-config.jpeg)
![](http://p3alsaatj.bkt.clouddn.com/20180621141202_RRhEze_postresult.jpeg)


4. 配置结果回传地址，勾选需要回传的字段

**推送方式**

自动外呼活动开启后，每小时会统计上一小时的自动外呼呼叫结果，根据在租户控制台勾选的会传字段，包装成JSON数组，推送到结果回传地址。

包体中最多包含1000条数据，如果结果过多，将会多次调用接口发送。

**字段说明**

| **字段**    | **类型** | **说明**                                                     |
| ----------- | -------- | ------------------------------------------------------------ |
| id          | string   | 上传名单中的id                                               |
| callId      | string   | 呼叫的唯一id，只要该名单进行过外呼就会生成                   |
| prdType     | string   | 上传名单中的活动类型                                         |
| activityId  | string   | 活动id                                                       |
| msg         | string   | 呼叫结果                                                     |
| failMsg     | string   | 失败原因                                                     |
| round       | int      | 代表这条记录是第几次拨打的                                   |
| state       | int      | 名单过期    -10  业务禁播    -5  线路异常    -3  挂断    -4  成功    0  用户忙  1  网络忙  10  超时    11  短音忙  12  长音忙  13  其它    14  来电提醒    2  无法接通    3  呼叫限制    4  呼叫转移    5  关机    6  停机    7  空号    8  正在通话中  9 |
| startTime   | long     | 外呼发起时间,unix时间戳                                      |
| endTime     | long     | 外呼结束时间，unix时间戳  呼叫未被用户接通时为外呼结束时间，呼叫被用户接通时为呼叫转接到坐席的时间 |
| respondTime | ling     | 外线应答时间，unix时间戳                                     |
| batchId     | string   | 名单所属批次                                                 |
| agent       | string   | 呼叫分配的坐席                                               |

**推送结果示例**

![](http://p3alsaatj.bkt.clouddn.com/20180621141230_mNYNiO_postall.jpeg)

```json
[
    {
        "respondTime":0,
        "state":-3,
        "round":1,
        "batchId":"58-20171109095000391",
		"prdType":"123T",
        "callId":"686ee96b-5c67-4911-9c9d-d6e730a1335e",
        "endTime":1510211809000,
        "agent":"5902@ocm.test",
        "msg":-3,
        "id":"21120964",
        "startTime":1510211793000,
        "activityId":"cffb8899976a11e79ac30050569505de",
        "failMsg":"线路异常"
    },
    {
        "respondTime":1510211831000,
        "state":0,
        "round":1,
        "batchId":"58-20171109095000391",
		"prdType":"123T",
        "callId":"f596dbad-7346-4fd8-a515-cff96ecc2b2c",
        "endTime":1510211831000,
        "agent":"5902@ocm.test",
        "msg":0,
        "id":"20711693",
        "startTime":1510211815000,
        "activityId":"cffb8899976a11e79ac30050569505de",
        "failMsg":"成功"
    }
]
```