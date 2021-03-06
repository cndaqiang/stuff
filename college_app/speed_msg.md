
### 发速信操作流程：
1. 如果有附件先上传文件，得到文件(音频)的file_ids
2. 客户端组织成速信消息发送给服务端：
	* 请求方法：POST
	* 请求体：json格式数据如下
3. 服务端要做的处理：		
	* 推送消息（提醒接收用户） ->  接收者客户端刷新
	* 环信消息（透传消息） ->  接收者客户端刷新
4. 接收者客户端收到消息后，手动点击[确认收到消息]按钮， 发送确认命令消息给服务器
5. 服务端通过透传消息通知发送者客户端，消息已经确认收到
		

### 名称解释：
__透传消息(CMDMessage)：__

	把整条速信的json的数据通过环信的cmd消息发送给所有接收者，用户在线时立即接到、用户不在线时没有推送，但是当用户上线时会收到消息


---


##### 上传图片、音频接口
__请求：__
		
	form表单上传，
  	需要有上传用户的user_id，
  	区分图片还是音频的：type, 
  
__响应：__

```
{
	"status" : 2000,
	"message" : "上传成功",
	"data" : {
		"file_id": "附件的唯一id"
	}
}
```


---


#####  速信发消息 （需要先有数据上传功能）
__请求参数：__

	from:发送者, 
  	to:接收者(多个用,号分割)参数做URL编码, 
  	body:消息文字，参数做URL编码,  
  	is_push:是否推送 
    is_sms:是否短信
  	type:类型，文字text, 语言audio
  	files: 附件文件、图片的url
  
  	普通url编码方式POST数据：from=logined_user_id&to=encodeURIComponent("admin,user1,user2")&body=encodeURIComponent("这个是消息内容")&is_push=1  不够灵活
  
__注：这个接口特殊处理，使用POST请求体为json的格式如下__

``` 
{
  "from": "logined_user_id",
  "to": [
    "admin",
    "user1",
    "user2"
  ],
  "msg_id": "消息的唯一id",
  "body": "文本消息内容。。。。",
  "type": "text或者audio",
  "is_push": true,
  "is_sms": false,
  "create_time": "1432543534654",
  "comment_count": "数字型回复个数",
  "audio_id": "音频的file_id",
  "files": [
    {
      "file_id": "c2FkZmFzZGZz"
    },
    {
      "file_id": "c2FkZmFzZGZz"
    }
  ]
}
```


__响应：__
	
	msg_id: 消息id（不能和用户名群名称重复，最好加前缀）

```
{
	"status" : 2000,
	"message" : "发送成功",
	"data" : {
		"msg_id": "消息的唯一id"
	}
}
```


---


##### 速信消息列表 
__请求：__

	user_id:用户id
	form: 来源, all所有的，my我发出去的，other我接收的，trash回收站的消息
	如：user_id=logined_user_id&form=all
  
__响应：__

```
{
  "status": 2000,
  "message": "发送成功",
  "data": [     
    {
      "from": "logined_user_id",
      "to": [
        "admin",
        "user1",
        "user2"
      ],
      "readed": [
        "user1",
        "user2"
      ],
      "msg_id": "消息的唯一id",
      "body": "",
      "type": "text或者audio",
      "is_push": true,
      "is_sms": false,
      "create_time": "1432543534654",
      "comment_count": "数字型回复个数",
      "audio_url": "http://服务器/音频地址.amr",
      "files": [
        {
          "name": "照片.jpg",
          "file_id": "sdfasdfsdf",
          "url": "http://服务器/资源地址.jpg"
        },
        {
          "name": "演讲稿.ppt",
          "file_id": "sdfasdfsdf",
          "url": "http://服务器/资源地址.ppt"
        }
      ]
    },
    {
      "from": "logined_user_id",
      "to": [
        "admin",
        "user1",
        "user2"
      ],
      "readed": [
        "user1",
        "user2"
      ],
      "msg_id": "消息的唯一id",
      "body": "",
      "type": "text或者audio",
      "is_push": true,
      "is_sms": false,
      "create_time": "1432543534654",
      "comment_count": "数字型回复个数",
      "audio_url": "http://服务器/音频地址.amr",
      "files": [
        {
          "name": "照片.jpg",
          "file_id": "sdfasdfsdf",
          "url": "http://服务器/资源地址.jpg"
        },
        {
          "name": "演讲稿.ppt",
          "file_id": "sdfasdfsdf",
          "url": "http://服务器/资源地址.ppt"
        }
      ]
    }
  ]
}
```


---


##### 速信消息确认接口 
__请求：__
	
	user_id: 接收者的用户id
	msg_id：消息的id
	user_id=admin&msg_id=ding_00001

__响应：__

```
{
	"status" : 2000,
	"message" : "已确认",
	"data" : {
		"msg_id": "消息的唯一id"
	}
}
```



---


__速信回复接口：暂时不需要这个功能__









