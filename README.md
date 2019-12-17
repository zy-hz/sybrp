# 随义读经计划

目标：圣经阅读的推动者

## 一、设计理念


### 操作

​	推动个人读经是首要目的。实现的途径是读经计划。希望通过读经计划帮助个人形成按计划读经的习惯。因此，计划是整体设计的核心元素。个人对计划有四种基本操作：查看、创建/修改、执行、统计。通过这些操作，帮助个人在一个周期内完成一定数量的阅读。

​	群组同步阅读是保证计划推进的有效途径。因此、整体设计引入小组的概念。小组的定义是有共同计划的一些个人。也就是说小组是**虚拟**的，是依附于计划的。所以、个人对计划有两种进阶操作：邀请、加入。通过这些操作，一些个人通过计划形成小组。参与计划的个人有四种可选的身份：计划发起者、计划参与者、计划管理员、计划观察员。

### 复用

​	读经计划的复用性很高。因此、计划被设计为当创建后，可以被多人查看（类似于推荐）。推荐的策略根据读经计划被使用次数或者其他的评价标准而定。当可以成为推荐读经计划时，在计划作者许可的情况下，成为系统推荐的读经计划，从而被所有人查看。其他人可以使用这个读经计划作为模板，来创建自己的读经计划。当系统初始化的时候，有一些默认的读经计划，也是这样的设计思想。

​	如果个人同时加入多个读经计划时，重复的阅读内容只要在一个计划中执行一次标记，系统自动标记其他的计划。

### 数据

​	从个人、小组、计划三个维度可以产生数据积累。通过这些数据可以帮助个人、小组更好的推进计划。数据相关的应用归口在统计操作中。包括：报表、分享、导出。

​	计划维度按照日期进行统计，形成两个数据概念：进度、达标。进度是指当前日期所完成的阅读内容相对于计划所有的阅读内容的百分比。有个人计划进度和小组计划进度。小组计划进度是小组成员进度的平均值。达标是相对于进度的概念，表示达到按照进度进行内容阅读。达标的数据统计点为北京时间次日的4:30。

## 二、核心元素

### 用户 user

​	参与系统的个体，分一般用户和系统用户。一般用户是真实的个体；系统用户是一些特殊的、系统操作时需要使用的保留用户。一般用户又分为匿名用户和正式用户。随义读经计划系统被设计为支持匿名使用，也就是说不需要注册而是使用本机的cookie就可以使用。这些没有注册的用户称为匿名用户；正式用户是指提供邮件地址或者QQ号（其实是QQ邮箱）作为用户名的用户。使用该用户名，用户可以在任何设备上使用随义读经计划系统。一个用户包括以下属性：

| 标识          | 名称         | 类型        | 说明                                        |
| ------------- | ------------ | ----------- | ------------------------------------------- |
| uuid          | 用户唯一标识 | char(36)    |                                             |
| loginName     | 登录名称     | string(256) | email地址或者QQ号                           |
| loginPassword | 登录密码     | string(16)  | 0-9 a-z A-Z *&^%$#@!-_+?                    |
| nickName      | 昵称         | string(32)  |                                             |
| userType      | 用户类型     | tinyint     | 10 - 系统用户，20 - 匿名用户，40 - 一般用户 |
| avatar        | 用户头像     | string(256) | url                                         |

### 计划 plan

​	计划是在一段时间内，完成一定量阅读内容的时间表。阅读计划包括以下属性：

| 标识               | 名称           | 类型       | 说明                   |
| ------------------ | -------------- | ---------- | ---------------------- |
| id                 | 计划编号       | bigint     | 自动编号               |
| author             | 计划作者       | char(36)   |                        |
| title              | 计划标题       | string(32) |                        |
| subjectImageId     | 主题图片编号   | bigint     |                        |
| subjectScriptureId | 主题经文编号   | bigint     |                        |
| commentId          | 计划备注编号   | bigint     |                        |
| bkgImageId         | 背景图片编号   | bigint     |                        |
| createOn           | 创建时间       | datetime   |                        |
| createFrom         | 计划模板       | bigint     |                        |
| createPathId       | 创建路径编号   | bigint     | 记录从根计划开始的路径 |
| beginOn            | 开始时间       | datetime   |                        |
| endOn              | 结束时间       | datetime   |                        |
| totalDays          | 天数           | smallint   |                        |
| scheduleId         | 计划时间表编号 | bigint     |                        |

### 计划用户 plan_user

​	参与计划的用户。每个计划允许多个用户参与。用户也可以参与多个计划。包括以下属性：

| 标识          | 名称               | 类型     | 说明 |
| ------------- | ------------------ | -------- | ---- |
| planId        | 计划编号           | bigint   |      |
| userUUID      | 用户唯一标识       | char(36) |      |
| jonOn         | 参加时间           | datetime |      |
| asAuthor      | 作为发起人         | bool     |      |
| asParticipant | 作为参与者         | bool     |      |
| asManager     | 作为管理员         | bool     |      |
| asMonitor     | 作为观察员         | bool     |      |
| isDeny        | 是否被拒绝参与计划 | bool     |      |
|               |                    |          |      |

### 计划时间表 plan_schedule

​	 计划的时间表是一些计划单元的组合。计划单元是计划的最小执行单位。包括以下属性：

| 标识     | 名称           | 类型        | 说明           |
| -------- | -------------- | ----------- | -------------- |
| id       | 计划时间表编号 | bigint      |                |
| cellId   | 计划单元序列号 | smallint    |                |
| contents | 计划内容的列表 | string(256) | json格式的数组 |

### 计划内容 plan_content

​	计划内容是指在计划单元中需要阅读的内容，指向的图书库中的内容索引表。包括以下属性：

| 标识      | 名称     | 类型        | 说明         |
| --------- | -------- | ----------- | ------------ |
| id        | 内容编号 | bigint      |              |
| bookSid   | 图书编号 | string(16)  | 例如：BS46   |
| chapterId | 章编号   | smallint    |              |
| verseIds  | 节编号   | string(256) | 例如：1，2-5 |

### 用户计划进度 user_plan_progress

​	记录用户执行计划的进度。包括以下属性：

| 标识           | 名称             | 类型        | 说明                              |
| -------------- | ---------------- | ----------- | --------------------------------- |
| uuid           | 用户唯一标识     | char(36)    |                                   |
| planId         | 计划编号         | bigint      |                                   |
| scheduleId     | 计划时间表编号   | bigint      |                                   |
| cellId         | 计划单元序列号   | smallint    |                                   |
| progressDetail | 进度的详情       | string(256) | 时间戳:cid1-cid2,时间戳:cid3-cid4 |
| cellIsDone     | 工作单元是否完成 | bool        |                                   |
| cellDoneOn     | 工作单元完成时间 | datetime    |                                   |
| isFollowed     | 是否跟随计划进度 | bool        | 是否在截至时间前完成阅读任务      |

## 三、功能清单


### A.查看

| 功能号 | 功能描述                                                 | 开发状态 |
| ------ | -------------------------------------------------------- | -------- |
| A100   | 列出读经计划清单                                         | 规划中   |
| A101   | 选择一个或多个读经计划                                   | 规划中   |
| A102   | 选择所有的个人读经计划（进行中的读经计划）               | 规划中   |
| A110   | 查看一个读经计划的概要信息                               | 规划中   |
| A111   | 查看一个读经计划的个人进度                               | 规划中   |
| A120   | 查看一个读经计划的小组进度                               | 规划中   |
| A121   | 查看一个读经计划的小组同学清单                           | 规划中   |
| A200   | 按日历模式查看选择的读经计划                             | 规划中   |
| A201   | 在日历中合并显示个人所有读经计划的完成情况               | 规划中   |
| A202   | 按日期合并显示个人所有读经计划的阅读内容                 | 规划中   |
| A203   | 按日期分别显示个人所有读经计划的阅读内容                 | 规划中   |
| A300   | 按时间轴模式查看选择的读经计划                           | 规划中   |
| A301   | 按日期合并显示个人所有读经计划的阅读内容以及完成情况     | 规划中   |
| A302   | 按日期分别显示个人所有读经计划的阅读内容以及完成情况     | 规划中   |
| A303   | 滚动载入若干个历史日期或者未来日期的计划内容以及完成情况 | 规划中   |
| A400   | 按封面模式查看选择的读经计划                             | 规划中   |
| A401   | 滑动载入一个历史日期或者未来日期的计划内容以及完成情况   | 规划中   |
| A402   | 封面图片分享给好友                                       | 规划中   |
| A403   | 封面图片保存为桌面                                       | 规划中   |

### B.创建/修改

| 功能号 | 功能描述                                           | 开发状态 |
| ------ | -------------------------------------------------- | -------- |
| B100   | 从一个已有的读经计划作为模板创建为个人新的读经计划 | 规划中   |
| B200   | 设置读经计划的标题                                 | 规划中   |
| B201   | 设置读经计划的说明                                 | 规划中   |
| B202   | 设置读经计划的起始日期                             | 规划中   |
| B210   | 设置读经计划的默认背景                             | 规划中   |
| B211   | 设置读经计划指定日期的背景                         | 规划中   |
| B212   | 设置读经计划的随机背景                             | 规划中   |
| B220   | 设置读经计划的默认主题经文                         | 规划中   |
| B221   | 设置读经计划指定日期的主题经文                     | 规划中   |
| B222   | 设置读经计划的随机主题经文                         | 规划中   |
| B301   | 设置是否可以提前阅读                               | 规划中   |
| B302   | 设置是否可以滞后阅读                               | 规划中   |

### C.执行

| 功能号 | 功能描述         | 开发状态 |
| ------ | ---------------- | -------- |
| C100   | 标记已读计划内容 | 规划中   |
| C101   | 标记未读计划内容 | 规划中   |

### D.统计

| 功能号 | 功能描述                                           | 开发状态 |
| ------ | -------------------------------------------------- | -------- |
| D100   | 统计一个读经计划的个人进度达标的时间分布           | 规划中   |
| D101   | 统计一个读经计划的小组进度达标的时间分布           | 规划中   |
| D102   | 统计一个读经计划的指定层级小组的进度达标的时间分布 | 规划中   |
| D801   | 导出统计报表为pdf                                  | 规划中   |
| D802   | 导出统计报表为jpg                                  | 规划中   |
| D803   | 导出统计报表为ppt                                  | 规划中   |

### E.邀请

| 功能号 | 功能描述                                                | 开发状态 |
| ------ | ------------------------------------------------------- | -------- |
| E100   | 创建邀请同学的链接                                      | 规划中   |
| E101   | 创建邀请同学的二维码                                    | 规划中   |
| E111   | 设置加入本计划的密码                                    | 规划中   |
| E112   | 设置加入本计划的人数上限                                | 规划中   |
| E113   | 设置加入本计划的时间，超过时间后链接失效                | 规划中   |
| E201   | 设置加入同学对于本计划的权限 - 允许从本计划创建子计划   | 规划中   |
| E202   | 设置加入同学对于本计划的权限 - 允许修改子计划的标题     | 规划中   |
| E203   | 设置加入同学对于本计划的权限 - 允许修改子计划的背景     | 规划中   |
| E204   | 设置加入同学对于本计划的权限 - 允许修改子计划的主题经文 | 规划中   |
| E211   | 设置加入同学对于本计划的身份 - 管理员\|观察员\|参与者   | 规划中   |

### F.加入

| 功能号 | 功能描述                   | 开发状态 |
| ------ | -------------------------- | -------- |
| F100   | 通过邀请链接，确认加入计划 | 规划中   |
| F101   | 退出计划                   | 规划中   |
|        |                            |          |
|        |                            |          |

### T.用户

| 功能号 | 功能描述         | 开发状态 |
| ------ | ---------------- | -------- |
| T100   | 匿名用户注册     | 规划中   |
| T101   | 正式用户注册     | 规划中   |
| T110   | 用户登录         | 规划中   |
| T120   | 重置密码         | 规划中   |
| T201   | 设置用户昵称     | 规划中   |
| T202   | 设置用户头像     | 规划中   |
| T203   | 设置用户默认头像 | 规划中   |

### U.公用

| 功能号 | 功能描述                  | 开发状态 |
| ------ | ------------------------- | -------- |
| U100   | 导入圣经书卷表            | 规划中   |
| U201   | 导入和合本圣经            | 规划中   |
| U281   | 导入StrongNumber数据库    | 规划中   |
| U801   | 导入麦琴读经计划          | 规划中   |
| U811   | 导入 365天逐章读经计划    | 规划中   |
| U812   | 导入365天历史顺序读经计划 | 规划中   |
| U821   | 导入全年每周6天读经计划   | 规划中   |

## 四、版权声明

1. 随义读经计划项目采用各个版本的圣经内容是从网络收集，其版权归原所有人。
2. 随义读经计划的代码发布在 [github](https://github.com/zy-hz)，使用者请遵循MIT开源软件协议。

