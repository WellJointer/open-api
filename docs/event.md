# 事件订阅与推送

最近更新日期：{docsify-updated}

# 1. 事件订阅

事件订阅接口可以根据请求体中topics字段的值不同，订阅不同类型以及不同范围的事件。

目前该接口支持订阅`挂断事件`与`座席状态改变事件`。详情参考`2 订阅规则topics字段说明`

## 1.1 请求示例 {docsify-ignore}

!> 请求头部字段`Authorization`字段值即申请到的`token`值

```
// general
POST http://tpisdk.wellcloud.cc/api/operation/tenant/createSubscription

// request headers
Authorization: 12345678
Content-Type: application/json;charset=utf-8

// request body
{
	"subscriber": "test-domain.cc",
	"topics": "event.call.test-domain_cc",
	"callbackUrl": "http://test-domain.cc/api",
	"token": "iosdfiajlsdfksdf"
}

// response
{
  "subscriptionId": "http://test-domain.cc/api"
}
```

## 1.2 路径与查询字符串参数模型 {docsify-ignore}

`POST http://tpisdk.wellcloud.cc/api/operation/tenant/createSubscription`

## 1.3 请求体说明 {docsify-ignore}

名称 | 是否必须 | 说明
---|---|---
subscriber | 是 | 订阅租户的域名
topics | 是 | 订阅规则。形式必须是：event.call.domain。注意：`domian(域名)中的点必须全部替换成下划线`。不同订阅规则可以订阅不同的事件，参见`2 订阅规则topics字段说明`
callbackUrl | 是 | 回调地址
token | 是 | 用于配置回调地址的验证。并非是目录1中申请的token
id | 否 | 唯一订阅标识


## 1.4 响应体说明 {docsify-ignore}

名称 |  说明
---|---
subscriptionId | 订阅成功后返回订阅id


# 2 订阅规则topics字段说明

!> domian(域名)中的点必须全部替换成下划线

topic | 含义 | 举例
--- | --- | ---
event.call.{domain} | 订阅租户的挂断事件。{domain}表示租户域名| event.call.google_com
event.agent-status.{agentCode}.{domain} | 订阅单个座席状态改变事件。{code}表示座席工号| event.agent-status.5001.google_com
event.agent-status.*.{domain} | 订阅租户下所有座席的状态改变事件 | event.agent-status.*.google_com


# 3 回调地址验证

订阅事件过程中，WellCloud平台会发送验证的GET请求到服务器配置的URL上，GET请求携带四个参数：signature（签名）、timestamp（当前时间戳）、nonce（随机数）、echostr（随机字符串）

`GET http://callbackURL/?signature={{signature}}&timestamp={{timestamp}}&nonce={{nonce}}&echostr={{echostr}}`

签名/校验流程如下：

1.	将token、timestamp、nonce三个参数进行字典序排序。
2.	将三个参数字符串拼接成一个字符串进行sha1加密。
3.	开发者获得加密后的字符串可与signature对比，如果相同，则说明数据是从我们平台发出的。
4.	返回echostr（随机字符串），返回示例：`{"result":"abxfewsvt23v7sxw"}`。如果无需校验，可在收到get请求后，跳过1，2，3步骤，直接按照示例格式返回echostr（随机字符串）

**sha1 算法 （java）**

```
public final class SHA1 {    
    private static final char[] HEX_DIGITS = {'0', '1', '2', '3', '4', '5',  
                           '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};    
    /** 
     * Takes the raw bytes from the digest and formats them correct. 
     * 
     * @param bytes the raw bytes from the digest. 
     * @return the formatted bytes. 
     */  
    private static String getFormattedText(byte[] bytes) {  
        int len = bytes.length;  
        StringBuilder buf = new StringBuilder(len * 2);  
        for (int j = 0; j < len; j++) {  
            buf.append(HEX_DIGITS[(bytes[j] >> 4) & 0x0f]);  
            buf.append(HEX_DIGITS[bytes[j] & 0x0f]);  
        }  
        return buf.toString();  
    }    
    public static String encode(String str) {  
        if (str == null) {  
            return null;  
        }  
        try {  
            MessageDigest messageDigest = MessageDigest.getInstance("SHA1");  
            messageDigest.update(str.getBytes());  
            return getFormattedText(messageDigest.digest());  
        } catch (Exception e) {  
            throw new RuntimeException(e);  
        }  
    }  
}  
```

**接口验证示例 （java）**


```

@GET
public String checkSignature(@Context HttpServletRequest request,
		@Context HttpServletResponse response) {
	// 验证URL真实性
	String signature = request.getParameter("signature");// 签名
	String timestamp = request.getParameter("timestamp");// 时间戳
	String nonce = request.getParameter("nonce");// 随机数
	String echostr = request.getParameter("echostr");// 随机字符串

	List<String> params = new ArrayList<String>();
	params.add(Token);
	params.add(timestamp);
	params.add(nonce);
	// 1. 将token、timestamp、nonce三个参数进行字典序排序
	Collections.sort(params, new Comparator<String>() {
		public int compare(String o1, String o2) {
			return o1.compareTo(o2);
		}
	});
	// 2. 将三个参数字符串拼接成一个字符串进行sha1加密
	String temp = SHA1.encode(params.get(0) + params.get(1) + params.get(2));
	if (temp.equals(signature)) { // 3.对比signature
		LogUtil.trace(this, "########## signature successful ##########");
		return echostr; // 4. 返回随机字符串
	} else {
		LogUtil.trace(this, "########## signature error ##########");
	}
	     return "error";
}

```


# 4 推送事件及其数据结构

在每通电话结束以后，WellCloud平台会向第三方回调地址推送电话的呼叫数据，数据格式是JSON(建议:接收推送事件的接口应该收到数据之间返回200ok,其他的操作应该是异步的,设置的超时时间为500ms)。

重推机制(只有挂断事件会重推):挂断事件推送失败后，会在15分钟后将这15分钟内失败的事件重新推送一遍，如果这次还失败了，会在30分钟后将这30分钟内失败的事件重新推送一遍，如果还失败会在60分钟后将这60分钟内失败的事件重新推送一遍，如果再次失败会进入一个手工重推的队列需要在租户控制台的联络中心管理->第三方系统集成配置里面的重推按钮进行重推，失败的事件只保留1天，1天后会写入文件并且删除，超过一天的失败事件可以向运维获取推送失败事件的文件

## 4.1 CallEndEvent 挂断事件说明

举例说明：

```json
{
    "eventName": "CallEndEvent",
    "eventTime": "2017.09.13 11:15:53",
    "eventType": "call",
    "serial": 1,
    "params": {
        "subscriptionId": "http://callbackUrl”
    },
    "_type": "component.cti.event.model.CallEndEvent",
    "topics": [
        "model"
    ],
    "namespace": "final.cc",
    "call": {
        "id": "517cefd0-65fb-4b8f-bc42-d0919a4a3ffe",
        "callType": "Outbound",
        "ani": "8017",
        "dnis": "915856975436",
        "startTime": "2017.09.13 11:15:06",
        "endTime": "2017.09.13 11:15:52",
        "talkLength": 0,
        "answerLength": 0,
        "ringLength": 0,
        "holdLength": 0,
        "abandLength": 0,
        "queuedLength": 0,
        "length": 0,
        "firstStation": "8017@final.cc",
        "endReason": "UserDrop",
		 "failedCode": "9",
        "conference": 0,
        "transferred": 0,
        "internalTransferCount": 0,
        "tenantId": "34bc437b-69ec-4219-90a1-e672dd9d2a9c",
        "namespace": "final.cc",
        "callPartyList": [],
        "callSegments": [],
        "attchedData": {
            "data": {
                "uui": "10000000,1",
                "originalCallId": "d13a82d0-2892-487a-b55f-9b427cff9fcc",
                "originalANI": "8017@final.cc",
                "originalDNIS": "917621180543@final.cc"
            }
        }
    }
}


```

### 4.1.1 call对象字段说明

> 所有时长单位都是秒

参数 | 类型 | 是否必须 | 描述
---|---|--- | ---
id | string | 是 | callId。呼叫id
callType | enum string | 是 | 呼叫类型。参加本页面 **4.1.2 呼叫类型 call.callType 字段说明**
ani | string | 是 | 主叫号码
dnis | string | 是 | 被叫号码
startTime | datetime | 是 | 呼叫开始时间
answerTime | datetime | 否 | 通话开始时间。未接通时为空，不会推送该字段
endTime | datetime | 是 | 呼叫结束时间
talkLength | int | 是 | 通话时长
answerLength | int | 是 | 应答时长
ringLength | int | 是 | 振铃时长
holdLength | int | 是 | 保持时长
queuedLength | int | 是 | 排队时长
length | int | 是 | 总时长
firstStation | string | 是 | 一个呼叫中第一次接听分机号
failedCode | enum string | 是 | 呼叫失败编码。参见本页面 **4.1.4 呼叫失败编码 call.failedCode 字段说明**
endReason | enum string | 是 | 挂机原因。参见本页面 **4.1.3 挂机原因 call.endReason 字段说明**
conference | int | 是 | 会议方数量，如为0则代表此通电话没有使用会议
transferred | int | 是 | 转接次数
internalTransferCount | int | 是 | 内线转接次数
tenantId | string | 是 | 租户唯一标示ID
namespace | string | 是 | 租户域名
firstAgentId | string | 是 | 第一次接听电话的座席号（如果座席没有登录，直接使用分机，不会推送）
attchedData | object | 是 | 随路数据

### 4.1.2 呼叫类型 call.callType 字段说明

名称 | 说明
---|---
Inbound | 呼入
Outbound | 呼出
Internal | 内线
Offhook | 摘机
PreOccupied | 预占式外呼
PreDictive | 预测式外呼

### 4.1.3 挂机原因 call.endReason 字段说明

名称 | 说明
---|---
Abandon | 放弃
UserDrop | 用户挂断
Transferred | 转接
ExtDrop | 分机挂断
Redirect | 重定向
Unknown | 未知
Failed | 呼叫客户失败

### 4.1.4 呼叫失败编码 call.failedCode 字段说明

编码 | 说明
---|---
-4 | 挂断
-3 | 线路异常
-2 | 暂无
-1 | 暂无
0 |  成功
1 | 用户忙
2 | 来电提醒
3 | 无法接通
4 | 呼叫限制
5 | 呼叫转移
6 | 关机
7 | 停机
8 | 空号
9 | 正在通话中
10 | 网络忙
11 | 超时
12 | 短音忙
13 | 长音忙
14 | 其他

## 4.2 AgentStatusEvent 座席状态改变事件说明

举例说明：

```json
{
  "eventName": "AgentStatusEvent",
  "eventSrc": "AS:5102@zhen04.cc",
  "eventTime": "2018.06.21 21:06:23",
  "eventType": "agentStatus",
  "serial": 1196812,
  "params": {
    "CallsInTalk": "0",
    "CallsInternal": "201",
    "CallsRinged": "0",
    "AuxCount": "603",
    "MaxTalkTime": "10",
    "AvgOutTalkTime": "0.0",
    "HoldTime": "0",
    "CallsTotalTalk": "201",
    "AvgDialTime": "0.0",
    "AgentStatus": "Logout",
    "CallsTotal": "201",
    "CallsAband": "0",
    "AuxTime": "6534",
    "AcdTime": "0",
    "CallsOutTalk": "0",
    "AcwTime": "0",
    "AvgAcwTime": "0.0",
    "AgentUseRate": "99.8004",
    "InternalTalkTime": "2010",
    "AvgAbandTime": "0.0",
    "RingTime": "0",
    "MaxAuxTime": "17",
    "CallsOffered": "0",
    "AbandTime": "0",
    "DialTime": "0",
    "AvgAnswerTime": "0.0",
    "AvgRingTime": "0.0",
    "AnswerTime": "0",
    "IdleTime": "4",
    "AvgInternalTalkTime": "10.0",
    "IdleCount": "411",
    "AnswerRate": "0.0",
    "AvgTalkTime": "10.0",
    "AvgAuxTime": "10.835821",
    "TalkTime": "2010",
    "AvgInTalkTime": "0.0",
    "CallsOut": "0",
    "CallsInternalTalk": "201",
    "InTalkTime": "0",
    "AcwCount": "0",
    "AvgWorkTime": "0.0",
    "OutTalkTime": "0"
  },
  "_type": "component.cti.event.model.AgentStatusEvent",
  "topics": [
    "model",
    "CtiWorker_ctiworker-uv6u4"
  ],
  "namespace": "zhen04.cc",
  "agent": {
    "agentId": "5102@zhen04.cc",
    "extensionId": "8102@zhen04.cc",
    "agentMode": "NotReady",
    "reason": "0",
    "agentName": "单独d",
    "org": "b833eb7b-309c-11e7-98a2-06076d833eaa",
    "namespace": "zhen04.cc",
    "agentStatus": "NotReady",
    "agentStatusTimestamp": "2018.06.21 21:06:23",
    "lastStatus": "Logout",
    "lastStatusLength": 331,
    "loginTime": "2018.06.21 21:06:23",
    "firstLoginTime": "2018.06.21 00:00:23"
  }
}
```

#### 4.2.1 agent.agentStatus 坐席状态说明


状态名|解释
---|---
NotReady|示忙
WorkNotReady|话后处理状态
Idle|就绪
Ready|就绪
OnCallIn|呼入通话
OnCallOut|呼出通话
Logout|登出
Ringing|振铃
Allocated|预占中
OffHook|摘机
CallInternal|内部通话
Dailing|外线已经振铃
Ringback|回铃
Conference|会议
OnHold|保持
Other|其他
OnCallPreOccupied|预占式外呼通话
OnPreDictive|预测式外呼通话
OnAgentAutoCall|自动外呼通话
OnPreView|预览式外呼通话


