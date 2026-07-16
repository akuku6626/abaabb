📚 校园旧书好物交易平台
基于 腾讯云开发（CloudBase） 构建的校园二手物品交易与即时通讯平台，支持求购/出售发布、留言、私聊、支付状态同步、管理后台等功能。

✨ 功能特性
物品广场：浏览所有求购/出售帖子，按类型、分类、价格区间筛选，关键词搜索。

发布管理：用户可发布求购/出售信息，支持图片上传，编辑/删除/标记完成。

即时通讯：基于 CloudBase 数据库监听实现实时私聊，支持文本消息和支付请求卡片。

支付状态同步：在聊天中发起收款请求，接收方点击支付后状态实时更新，双方可见。

留言系统：在物品详情页留言，管理员可审核管理。

管理后台：数据概览、帖子管理（上架/下架/删除）、用户管理（封禁/解封）、留言审核、举报处理。

个人设置：头像上传、昵称修改、手机绑定、实名认证（模拟）、密码修改。

🛠️ 技术栈
前端：原生 HTML5 + CSS3 + JavaScript，无第三方 UI 库。

后端服务：腾讯云开发 CloudBase（云数据库、云存储、云函数）。

实时通信：CloudBase 数据库 watch 监听实现消息实时同步。

部署：静态网页托管在 CloudBase 或任意 Web 服务器。

📁 项目结构
text
/
├── adminComment.html       # 留言审核管理页
├── adminDashboard.html     # 管理后台数据概览
├── adminReports.html       # 举报管理页
├── adminSale.html          # 出售贴管理页
├── adminUsers.html         # 用户管理页
├── adminWanted.html        # 求购贴管理页
├── buy.html                # 物品广场（列表浏览）
├── chatList.html           # 消息列表（会话列表）
├── chatRoom.html           # 聊天室（含支付功能）
├── detail.html             # 物品详情页（含留言）
├── index.html              # 平台首页（热门推荐）
├── login.html              # 登录/注册/管理员注册
├── message.html            # 静态客服页面（演示）
├── myManage.html           # 我的管理（个人发布列表）
├── PublishBuy.html         # 发布求购页
├── PublishSale.html        # 发布出售页
├── setting.html            # 个人设置页
└── userProfile.html        # 用户信息展示页（名片）
🗄️ 数据库集合
集合名	用途	关键字段
user	用户信息	_id, username, realName, password, role, status, avatar, signature, userCode
wanted	求购帖子	_id, name, budget, quality, description, category, user, userDisplay, img, isFinish, status
sales	出售帖子	同 wanted，额外 price, trade
comment	留言	_id, itemId, user, content, createdAt
conversations	会话（私聊）	_id, userIdA, userIdB, nameA, nameB, updatedAt
message	聊天消息	_id, conversationId, senderId, toUser, content, msgType（text/payment）, isRead, createdAt
report	举报记录	_id, type, target, reporter, content, status, createdAt
注意：集合权限需设置为「所有用户可读写」或使用云函数/安全规则，以满足业务逻辑。

🚀 快速部署
1. 注册并初始化腾讯云开发
访问 CloudBase 控制台 创建环境，获取 ENV_ID。

在 数据库 中创建上述所有集合。

在 云存储 中创建 images/wanted/ 和 images/sale/ 目录用于存放图片。

2. 修改环境配置
在所有 HTML 文件中搜索 ENV_ID，替换为您的环境 ID：

javascript
const ENV_ID = '您的环境ID';
3. 部署静态文件
将全部 HTML 文件上传到 CloudBase 静态托管或任意 Web 服务器，确保所有页面处于同一目录。

4. 配置云函数（可选）
若使用支付功能，需部署云函数 updatePaymentStatus（代码参考 chatRoom.html 中的调用示例），或直接使用本代码中的数据库直接更新方案（已内置）。

5. 设置管理员
在 login.html 中点击右下角「管理员注册」，输入密钥 admin123 创建管理员账号。

管理员账号登录后将跳转管理后台。

🔧 核心功能说明
支付流程（聊天中）
用户 A 在聊天窗口点击💰按钮，填写商品名和金额，发送支付请求（消息类型 payment）。

用户 B 收到支付卡片，点击「立即支付」。

系统直接更新数据库该消息的 content 中 status 为 paid。

双方通过 watch 实时看到卡片变为「✅ 已支付」状态。

消息实时同步
使用 CloudBase 数据库 watch API 监听 message 集合。

同一 conversationId 下的所有变更会实时推送到双方页面，实现即时通讯。

图片上传
发布求购/出售时，支持选择图片并上传至云存储，返回 fileID 存入数据库。

预览时通过 getTempFileURL 获取临时访问链接。

📝 注意事项
登录状态：使用 localStorage 存储 currentUserId 和 currentUser，页面跳转时需保证一致。

权限控制：管理员页面会校验 user.role === 'admin'，普通用户无法访问。

数据安全：密码以明文存储（演示项目），生产环境建议加密。

支付状态：直接更新数据库操作需确保集合权限允许，否则会失败。

🤝 贡献与反馈
欢迎提交 Issue 或 Pull Request 改进项目。如有问题，可在聊天中联系客服（演示客服页面 message.html 为静态模拟）。

项目地址：https://github.com/akuku6626/abaabb
演示地址：https://campus-trading-d9g84nflw71c77ac3-1451642129.ap-shanghai.app.tcloudbase.com/