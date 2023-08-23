## 说明
### secret参数说明 
接口调用，如发送消息，获取用户信息等接口调用时候，用来作为校验参数传入。更改后之前的secret会失效，请谨慎修改。可调用接口如下：
1. 发送消息
2. 获取群信息
3. 获取用户信息

### webhook地址
除了调用接口以外， 部分业务场景需要根据用户发送消息来实现对应响应。 此地址可以获得以下消息通知
1. 私聊消息 
2. 群消息
3. 新增好友通知


### api域名
http://api.bot.yingqia.com/

## 接口文档
### 调用api
#### 消息发送接口地址：post /api/send
```json
curl 'https://hook.api.qw360.cn/api/send?secret=#{secret}&conversation_id=#{conversation_id}' \
   -H 'Content-Type: application/json' \
   -d '
   {
    	"msgtype": "text",
    	"text": {
        	"content": "hello world"
    	}
   }'

```

当前支持文本, 图片，链接卡片，小程序类型消息

> 接口兼容企微官方机器人的webhook

##### 消息类型
* 文本
```json
{
    "msgtype": "text",
    "text": {
        "content": "hello world"
    }
 }
```

* 图片
```json
{
    "msgtype": "image",
    "image": {
        "url": "https://yingqia.com/template/eyou/pc/skin/images/logo.png"
    }
 }
```

* 小程序
```json
{
	"msgtype":"miniprogram",
  "miniprogram":{
     
  }
}
```

#### 获取用户会话：get /api/users
https://hook.api.qw360.cn/api/users?secret=#{secret}
```json
{
    "list": [{
      "name":"迎洽AI",
      "conversation_id":"-"
    }]
 }
 ```

#### 获取群组会话：get /api/groups
https://hook.api.qw360.cn/api/groups?secret=#{secret}
```json
{
    "list": [{
      "name":"迎洽AI",
      "conversation_id":"-"
    }]
 }
 ```

### WEBHOOK
后台填入webhook地址后， 会收到所有会话信息通知:
目前仅支持文本消息通知，需要更多消息类型通知，请联系客服。
```json
{
	"secret":"-",
	"msgtype": "text",
	"conversation_id": "",
  "sender": "",
  "text": {
  	"content":""
	}
}
```


