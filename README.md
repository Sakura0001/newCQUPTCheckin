# ⚙ newCQUPTCheckin
***！！！请各位同学自行打卡，禁止代为打卡，如有疫情相关情况请及时停止脚本打卡并如实上报，错误使用带来的后果自行承担***

***！！！请各位同学自行打卡，禁止代为打卡，如有疫情相关情况请及时停止脚本打卡并如实上报，错误使用带来的后果自行承担***

***！！！请各位同学自行打卡，禁止代为打卡，如有疫情相关情况请及时停止脚本打卡并如实上报，错误使用带来的后果自行承担***

## 提示

Github Action部署方式，有些部署细节发生变化！！！

在添加了推送功能以后，命令行参数变得比较多。采用之前命令行参数形式太过于繁琐，于是采用了环境变量的方式指定参数，在修改时变动了以下参数，如需更新新版本，需要变动Github Action中的secrets变量中的：

1. 变量`ADDRESS`改名为`JZDXXDZ`（居住地详细地址），以匹配脚本中的变量名。
2. 添加通知相关参数的对应变量，如需要部署通知功能，详见下方[通知推送](#3.推送通知:mailbox:)

## 1. 简介 📃
本脚本适配了CQUPT企业微信版最新的打卡。

全程只需要**统一认证码**和**密码**即可完成打卡任务。使用了验证码识别，面对可能的验证码验证。

*已添加经纬度随机扰动增加随机性

## 2. 使用方式 📖

### a.本地运行 

1. 拉取代码到本地

2. 使用pip安装所需库

   ```shell
   pip install -r requirements.txt
   ```

3. 打开 `cqupt_checkin.py`修改个人打卡信息

4. 执行

   ```
   python3 cqupt_checkin.py
   ```

### b. Github Action运行（推荐）

使用 Github Action 实现 CI/CD，即每日自动化填报。

1. fork本仓库
2. 脚本默认打卡信息为**无任何异常**，如需更改可使用Github在线更改`cqupt_checkin.py`内的信息
3. 给本仓库secrets环境变量添加字段`USERNAME`、`PASSWORD`、`LONGITUDE`、 `LATITUDE`、 `JZDXXDZ`并分别对应填入学校统一认证码、密码、打卡经度、打卡维度、详细地址，以及如需使用推送通知功能，对应再添加要求环境变量。

![image-20220907143942963](https://oneindex.joshuasu.tech/images/2022/09/07/BtDWE5sqnM/image-20220907143942963.png)

1. 已默认设置Github Action每天9点打卡，如需更改，请自行给改action配置文件中的schedule中的cron字段，注意时间单位为UTC
2. 在仓库的`Actions`选项里激活配置的workflow。

## 3.推送通知:mailbox:

各个推送方式的使用详情请参见各自方式的官方使用文档，这里只列出脚本已支持的参数，以及对应的在Github Action 对应的Env变量名称。

首先需要配置变量`NOTIFICATIONTYPES`，指明需要使用的推送通知类型，支持同时设置多种，各个种类之间用`,`分割。类型的名称见下方各个方式的括号内。

如：我想使用telegram bot 和 push plus的推送功能，则我需要定义变量：

```
NOTIFICATIONTYPES="telegram_bot,push_plus"
#push plus推送方式的相关参数
PUSHPLUSTOKEN="xxxx"
#telegram bot推送方式的相关参数
TELEGRAMBOTTOKEN=""
TELEGRAMBOTCHATID=""
```

### a. push plus（push_plus）

部署成本低，对应平台注册下即可，参见文档

文档：[pushplus 消息接口文档 V1.5 | pushplus(推送加) 文档中心](https://www.pushplus.plus/doc/guide/api.html#一、发送消息接口)

|       参数        | Github Action环境变量名 |                             说明                             |
| :---------------: | :---------------------: | :----------------------------------------------------------: |
|  push_plus_token  |      PUSHPLUSTOKEN      |                  必填，push_plus推送token。                  |
|  push_plus_topic  |      PUSHPLUSTOPIC      | 选填，push_plus群组编码，不填仅发送给自己；channel为webhook时无效 |
| push_plus_channel |    PUSHPLUSTCHANNEL     |   选填，推送渠道，不填默认为微信；可选参数见push plus文档    |

### b. WxPusher（wx_pusher）

部署成本低，对应平台注册下即可，参见文档

文档：[[WxPusher微信推送服务 (zjiecode.com)](https://wxpusher.zjiecode.com/docs/#/?id=发送消息-1)](https://www.pushplus.plus/doc/guide/api.html#一、发送消息接口)

|        参数         | Github Action环境变量名 |                             说明                             |
| :-----------------: | :---------------------: | :----------------------------------------------------------: |
|   wx_pusher_token   |      WXPUSHERTOKEN      |                 必填， wx_pusher推送token。                  |
|   wx_pusher_uids    |      WXPUSHERUIDS       | 需要推送目标的UID，是一个数组,元素类型为string。注意uids和topicIds可以同时填写，也可以只填写一个 e.g. ["UID_XXXXX", "UID_XXXXX"] |
| wx_pusher_topic_ids |    WXPUSHERTOPICIDS     | 发送目标的topicId，是一个数组,元素类型为int，也就是群发，使用uids单发的时候，可以不传。 e.g. [123] |

### c. telegram bot（telegram_bot）

适合自己部署有telegram bot的用户

文档：[Telegram Bot API](https://core.telegram.org/bots/api#sendmessage)

至于这里怎么获取自己的chat_id，方式有很多种，我使用的方式是：

使用bot：https://t.me/userinfobot，输入/start，返回的id就是你的chart id

|         参数         | Github Action环境变量名 |               说明               |
| :------------------: | :---------------------: | :------------------------------: |
|  telegram_bot_token  |    TELEGRAMBOTTOKEN     |   必填，telegram_bot推送token    |
| telegram_bot_chat_id |    TELEGRAMBOTCHATID    | 必填，telegram_bot推送给的用户id |



