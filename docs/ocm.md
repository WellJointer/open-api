# 自动外呼

# 1. 名单上传

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
- 请求体是一个txt文件
- 文件名不做要求，建议使用英文名
- 文件大小不要超过10MB
- 文件的每一行由名单Id, 产品id, 手机号组成。三者之间要用英文逗号隔开

示例：

```
12345,S,17412345678
12345,S,17412345678
12345,S,17412345678
```

**响应体说明**

状态码 | 说明
---|---
200 | 名单调用成功
404 | 地址错误
500 | 内部服务错误