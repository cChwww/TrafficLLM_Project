# 密码计算方式
```js
let username = "u250114"
let password = "123"

let key = username
if(key.length>16){
  key = key.substr(0,16)
}else{
  const size = 16 - key.length
  for(let i=0;i<size;i++){ key = key + '0' }
}
password = CryptoJS.AES.encrypt(password, key, CryptoJS.mode.OFB).toString();
```
# 登陆
http://172.16.8.102:4051/auth/token
提交form表单
username:"example"
password:"U2FsdGVkX1/bzLWnLIPXa7n/q3R6XCpsl64ABDoGobE="
密码计算方式参考第一节
返回
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJleGFtcGxlIiwiZXhwIjoxNzM2ODQ3MzkxfQ.WdC6NqhvFXThGWsw01leyniEG66THlzKbaEGHm-1h2k",
  "token_type": "bearer"
}
# 登出
http://172.16.8.102:4051/auth/remove_token
header需携带 Authorization: bearer 用户token
如：
Authorization: bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJleGFtcGxlIiwiZXhwIjoxNzM2ODQ3NjgxfQ.j0qFqXF660_Q4MqDAtsc06yvhO5_W7x9aekDT1VvsGI
# 注册
http://172.16.8.102:4051/auth/register
提交form表单
username:"example"
password:"U2FsdGVkX1/bzLWnLIPXa7n/q3R6XCpsl64ABDoGobE="
密码计算方式同登陆
返回
{
  "status": 200,
  "message": "注册成功",
  "data": []
}
# 其他-刷新token
http://172.16.8.102:4051/auth/refresh_token
header携带
Authorization: bearer 用户token
返回
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJleGFtcGxlIiwiZXhwIjoxNzM2ODQ3ODA2fQ.9wTmNy1rokwSkCq5l4nGzd4KfXETqugTCsQ9IzXS5CA",
  "token_type": "bearer"
}
# 其他-验证token
http://172.16.8.102:4051/auth/check_token
header携带
Authorization: bearer 用户token
返回
{
  "status": 200,
  "message": "token有效！",
  "data": []
}

# 支持token的接口，请求需携带Authorization头
## 创建会话
http://172.16.8.102:4051/v2/chat/create_chat
请求参数：
{"title":"会话名称"}
若不传则title默认为"新会话"
返回数据
{
  "status": 200,
  "message": "操作成功",
  "data": []
}
## 获取用户会话列表
http://172.16.8.102:4051/v2/chat/get_chat_list
无请求参数
返回数据
{
  "status": 200,
  "message": "操作成功",
  "data": [
    {
      "id": 3,
      "title": "",
      "user_id": null,
      "is_deleted": 0,
      "add_date": "2025-01-16 05:48:38",
      "update_date": "2025-01-16 05:48:38"
    },
    {
      "id": 4,
      "title": "新聊天",
      "user_id": null,
      "is_deleted": 0,
      "add_date": "2025-01-16 06:03:33",
      "update_date": "2025-01-16 06:03:33"
    },
    {
      "id": 5,
      "title": "new chatxxx",
      "user_id": null,
      "is_deleted": 0,
      "add_date": "2025-01-16 06:03:44",
      "update_date": "2025-01-16 06:03:44"
    }
  ]
}
## 更新会话名称
http://172.16.8.102:4051/v2/chat/update_chat
请求参数：
{"chat_id":1,"title":"new chat111"}
返回数据
{
  "status": 200,
  "message": "操作成功",
  "data": []
}
## 删除会话
http://172.16.8.102:4051/v2/chat/delete_chat
请求参数
{"chat_id":1}
返回数据
{
  "status": 200,
  "message": "操作成功",
  "data": []
}
## 获取会话记录（历史记录）
http://172.16.8.102:4051/v2/chat/get_chat_data
请求参数
{"chat_id":"3"}
返回数据
{
  "status": 200,
  "message": "操作成功",
  "data": [
    {
      "id": 4,
      "chat_id": 3,
      "user_request": "{\"chat_id\":3,\"title\":\"新聊天\",\"upload_file\":null,\"target_model\":\"qwen2.5:72b\",\"query\":\"明天天气\",\"prompt_type\":\"BASIC\",\"temperature\":null,\"top_p\":null,\"history\":[],\"library\":{\"library_id\":1,\"top_k\":5,\"score\":700,\"custom\":false,\"custom_id\":\"\"},\"modify_search\":{},\"embedding_model\":null,\"stream\":true,\"images\":[]}",
      "user_request_tokens": 0,
      "llm_response": "{\"role\":\"assistant\",\"content\":\"我目前无法直接获取实时或未来的天气信息。你可以查阅常用的天气预报网站、手机应用或者相关平台，输入你所在的位置来获得准确的天气预报。这样可以确保你得到的信息是最新的和最准确的。如果你告诉我你的具体位置，我可以帮你构建一个查询天气的方法或提供一些通用的建议。\",\"images\":null,\"tool_calls\":null}",
      "llm_response_tokens": 0,
      "run_time": 6.213454723358154,
      "add_date": "2025-01-16 06:17:59",
      "update_date": "2025-01-16 06:17:59"
    },
    {
      "id": 5,
      "chat_id": 3,
      "user_request": "{\"chat_id\":3,\"title\":\"新聊天\",\"upload_file\":null,\"target_model\":\"qwen2.5:72b\",\"query\":\"你是谁\",\"prompt_type\":\"BASIC\",\"temperature\":null,\"top_p\":null,\"history\":[],\"library\":{\"library_id\":1,\"top_k\":5,\"score\":700,\"custom\":false,\"custom_id\":\"\"},\"modify_search\":{},\"embedding_model\":null,\"stream\":false,\"images\":[]}",
      "user_request_tokens": 0,
      "llm_response": "{\"role\":\"assistant\",\"content\":\"我是Qwen，由阿里云开发的超大规模语言模型。我被设计用于生成各种类型的文本，如文章、故事、诗歌和代码等，并能根据不同的场景进行创造性的思考。很高兴为您服务！如果您有任何问题或需要帮助，请随时告诉我。\",\"images\":null,\"tool_calls\":null}",
      "llm_response_tokens": 0,
      "run_time": 5.1708245277404785,
      "add_date": "2025-01-16 06:18:13",
      "update_date": "2025-01-16 06:18:13"
    },
    {
      "id": 6,
      "chat_id": 3,
      "user_request": "{\"chat_id\":3,\"title\":\"新聊天\",\"upload_file\":null,\"target_model\":\"qwen2.5:72b\",\"query\":\"今天是2025-01-15，前一天是几号\",\"prompt_type\":\"BASIC\",\"temperature\":null,\"top_p\":null,\"history\":[],\"library\":{\"library_id\":1,\"top_k\":5,\"score\":700,\"custom\":false,\"custom_id\":\"\"},\"modify_search\":{},\"embedding_model\":null,\"stream\":false,\"images\":[]}",
      "user_request_tokens": 0,
      "llm_response": "{\"role\":\"assistant\",\"content\":\"如果今天是2025年1月15日，那么前一天就是2025年1月14日。\",\"images\":null,\"tool_calls\":null}",
      "llm_response_tokens": 0,
      "run_time": 2.777829170227051,
      "add_date": "2025-01-16 07:49:39",
      "update_date": "2025-01-16 07:49:39"
    }
  ]
}