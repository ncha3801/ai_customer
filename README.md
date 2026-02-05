# 🎯 AI Customer Service - 智能多坐席客服系统

<div align="center">

[![Go Version](https://img.shields.io/badge/Go-1.21+-00ADD8?style=flat&logo=go)](https://go.dev/)
[![Svelte Version](https://img.shields.io/badge/Svelte-5.0-FF3E00?style=flat&logo=svelte)](https://svelte.dev/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

一个功能完整的企业级智能客服系统，支持多商户 SaaS 架构、实时通讯、智能分配、会话管理等核心功能。

[快速开始](#-快速开始) • [功能特性](#-功能特性) • [技术架构](#-技术架构) • [部署指南](#-部署指南) • [文档](#-文档)

</div>

---

## 📸 系统预览

### 客服工作台
- 📊 实时数据统计面板（活跃会话、待处理、今日关闭、平均评分）
- 💬 多会话并发处理（支持同时处理多个客户咨询）
- 🎯 智能会话队列（等待分配自动排队）
- 📈 数据可视化报表（会话趋势、响应时间分析）

### 用户端
- 💡 简洁友好的聊天界面
- 📎 多种消息类型支持（文本、图片、文件、音频、视频）
- ⭐ 服务评价系统
- 🔔 实时消息通知

### 商户管理端
- 🏢 多商户 SaaS 架构
- 👥 客服团队管理
- 📊 商户数据统计
- 🎨 自定义配置

---

## ✨ 功能特性

### 🎨 核心功能

#### 👤 用户端功能
- ✅ **用户注册登录** - 支持邮箱/用户名注册，JWT 认证
- ✅ **创建咨询会话** - 一键发起客服咨询
- ✅ **实时消息通讯** - WebSocket 实时双向通信
- ✅ **多媒体消息** - 支持文本、图片、文件、音频、视频
- ✅ **图片预览** - 点击图片全屏查看
- ✅ **服务评价** - 5星评分及文字反馈
- ✅ **会话历史** - 查看历史咨询记录

#### 👨‍💼 客服端功能
- ✅ **智能工作台** - 数据统计面板 + 会话管理
- ✅ **会话接入系统** - 从等待队列接入会话
- ✅ **多会话并发** - 支持同时处理多个客户（可配置上限）
- ✅ **实时消息推送** - 自动接收客户消息
- ✅ **快捷回复** - 预设常用回复模板
- ✅ **会话转接** - 转接给其他客服
- ✅ **客户信息查看** - 实时显示客户详情
- ✅ **工作统计** - 个人工作数据统计
- ✅ **历史记录** - 查看历史服务记录
- ✅ **数据可视化** - 会话趋势图、响应时间分析

#### 🏢 商户管理功能（SaaS 多租户）
- ✅ **商户注册认证** - API Key/Secret 体系
- ✅ **客服团队管理** - 创建/删除客服账号
- ✅ **权限管理** - 基于角色的访问控制
- ✅ **数据隔离** - 商户数据完全隔离
- ✅ **统计报表** - 会话量、消息量、满意度统计
- ✅ **日报周报** - 自动生成数据报表
- ✅ **套餐管理** - 免费版/基础版/专业版/企业版
- ✅ **配额限制** - 客服数量、会话数量限制
- ✅ **小部件嵌入** - 提供 JS SDK 快速接入

#### 🤖 智能特性
- ✅ **会话自动超时** - 长时间无响应自动关闭（25分钟警告，30分钟关闭）
- ✅ **系统消息推送** - 超时提醒、关闭通知
- ✅ **会话状态管理** - waiting/active/closed/transferred
- ✅ **优先级队列** - low/normal/high/urgent
- ✅ **智能分配** - 基于客服负载的自动分配（待实现）

---

## 🏗️ 技术架构

### 后端技术栈
```
Golang 1.21+          高性能并发处理
├── Gin               轻量级 Web 框架
├── GORM              优雅的 ORM 框架
├── MySQL 8.0+        关系型数据库
├── WebSocket         实时双向通信
├── JWT               无状态认证
└── Goroutines        并发任务处理
```

### 前端技术栈
```
Svelte 5              响应式前端框架
├── TypeScript        类型安全
├── Tauri             跨平台桌面应用
├── Vite              极速构建工具
└── WebSocket Client  实时通信客户端
```

### 系统架构图
```
┌─────────────────────────────────────────────────────────────┐
│                         客户端层                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │ 用户端   │  │ 客服端   │  │ 商户端   │  │  SDK     │    │
│  │(Svelte)  │  │(Svelte)  │  │(Svelte)  │  │ (JS)     │    │
│  └─────┬────┘  └─────┬────┘  └─────┬────┘  └─────┬────┘    │
└────────┼─────────────┼─────────────┼─────────────┼──────────┘
         │             │             │             │
         └─────────────┴─────────────┴─────────────┘
                       │
         ┌─────────────▼──────────────┐
         │       API Gateway          │
         │    (Gin + Middleware)      │
         └─────────────┬──────────────┘
                       │
         ┌─────────────┴──────────────┐
         │                            │
┌────────▼──────────┐      ┌──────────▼─────────┐
│  WebSocket Hub    │      │   RESTful API      │
│  (实时消息分发)   │      │   (业务逻辑)       │
└────────┬──────────┘      └──────────┬─────────┘
         │                            │
         └─────────────┬──────────────┘
                       │
         ┌─────────────▼──────────────┐
         │      Handler Layer         │
         │  ┌──────────────────────┐  │
         │  │ Auth  │ Session  │... │  │
         │  └──────────────────────┘  │
         └─────────────┬──────────────┘
                       │
         ┌─────────────▼──────────────┐
         │      Database Layer        │
         │      MySQL + GORM          │
         └────────────────────────────┘
```

---

## 📦 项目结构

```
ai_customer/
├── server/                        # 后端服务 (Golang)
│   ├── cmd/
│   │   └── main.go               # 程序入口
│   ├── internal/
│   │   ├── config/               # 配置管理
│   │   │   └── config.go         # 环境变量加载
│   │   ├── database/             # 数据库层
│   │   │   └── db.go             # 数据库连接
│   │   ├── models/               # 数据模型
│   │   │   └── models.go         # 实体定义
│   │   ├── handlers/             # 业务处理器
│   │   │   ├── auth.go           # 认证处理
│   │   │   ├── session.go        # 会话管理
│   │   │   ├── message.go        # 消息处理
│   │   │   ├── agent.go          # 客服管理
│   │   │   ├── merchant.go       # 商户管理
│   │   │   ├── quick_reply.go    # 快捷回复
│   │   │   └── session_timeout.go # 会话超时检查器
│   │   ├── middleware/           # 中间件
│   │   │   ├── auth.go           # JWT 认证
│   │   │   └── cors.go           # 跨域处理
│   │   ├── websocket/            # WebSocket 管理
│   │   │   ├── hub.go            # 连接池
│   │   │   ├── client.go         # 客户端管理
│   │   │   └── handler.go        # 消息路由
│   │   ├── storage/              # 文件存储
│   │   │   ├── local.go          # 本地存储
│   │   │   ├── aliyun.go         # 阿里云 OSS
│   │   │   ├── tencent.go        # 腾讯云 COS
│   │   │   └── ...               # 其他云存储
│   │   └── utils/                # 工具函数
│   │       ├── jwt.go            # JWT 工具
│   │       ├── password.go       # 密码加密
│   │       └── file.go           # 文件处理
│   ├── database/
│   │   ├── schema.sql            # 数据库结构
│   │   └── merchant_migration.sql # 商户功能迁移
│   ├── .env                      # 环境配置
│   ├── go.mod                    # Go 依赖
│   └── Makefile                  # 构建脚本
│
├── client/                       # 前端应用 (Svelte + Tauri)
│   ├── src/
│   │   ├── lib/
│   │   │   ├── api.ts            # API 客户端
│   │   │   ├── websocket.ts      # WebSocket 客户端
│   │   │   ├── components/       # 组件库
│   │   │   │   ├── ChatBox.svelte        # 聊天框
│   │   │   │   ├── Login.svelte          # 登录组件
│   │   │   │   ├── SessionList.svelte    # 会话列表
│   │   │   │   └── Toast.svelte          # 消息提示
│   │   │   └── stores/           # 状态管理
│   │   │       ├── network.ts    # 网络状态
│   │   │       └── toast.ts      # 提示管理
│   │   └── routes/               # 页面路由
│   │       ├── +page.svelte              # 首页/登录
│   │       ├── customer/                 # 用户端
│   │       │   └── +page.svelte
│   │       ├── agent/                    # 客服端
│   │       │   ├── +page.svelte          # 工作台
│   │       │   ├── stats/                # 统计页
│   │       │   ├── history/              # 历史记录
│   │       │   └── settings/             # 个人设置
│   │       ├── merchant/                 # 商户端
│   │       │   ├── +page.svelte          # 概览
│   │       │   ├── agents/               # 客服管理
│   │       │   ├── sessions/             # 会话记录
│   │       │   └── overview/             # 数据统计
│   │       └── widget/                   # 嵌入式小部件
│   │           └── +page.svelte
│   ├── src-tauri/                # Tauri 配置
│   │   ├── src/
│   │   │   └── main.rs           # Rust 主程序
│   │   └── tauri.conf.json       # Tauri 配置
│   ├── static/
│   │   ├── jssdk.js              # JS SDK
│   │   └── sdk-demo.html         # SDK 示例
│   ├── package.json              # 前端依赖
│   └── vite.config.js            # Vite 配置
│
├── deploy_merchant.sh            # 商户功能部署脚本
├── test_merchant.sh              # 商户功能测试脚本
├── init_db.sh                    # 数据库初始化脚本
├── start.sh                      # 启动脚本 (Linux/Mac)
├── start.bat                     # 启动脚本 (Windows)
├── README.md                     # 本文档
├── DEVELOPMENT.md                # 开发文档
├── MERCHANT.md                   # 商户功能文档
└── LICENSE                       # 开源协议
```

---

## 🚀 快速开始

### 环境要求

- **Go** 1.21 或更高版本
- **Node.js** 18 或更高版本
- **MySQL** 8.0 或更高版本
- **操作系统**: Windows / macOS / Linux

### 1. 克隆项目

```bash
git clone https://github.com/yourusername/ai-customer-service.git
cd ai-customer-service
```

### 2. 数据库初始化

```bash
# 创建数据库
mysql -u root -p
CREATE DATABASE ai_customer_service CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
USE ai_customer_service;

# 导入基础表结构
SOURCE server/database/schema.sql;

# 如果需要商户功能，导入商户表
SOURCE server/database/merchant_migration.sql;

# 或者使用快捷脚本
chmod +x init_db.sh
./init_db.sh
```

### 3. 配置后端

```bash
cd server

# 复制配置文件
cp .env.example .env

# 编辑配置文件，修改数据库连接等信息
nano .env
```

**.env 配置示例：**

```env
# 服务器配置
SERVER_PORT=8180
SERVER_HOST=0.0.0.0

# 数据库配置
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=ai_customer_service

# JWT 配置
JWT_SECRET=your-secret-key-change-this-in-production
JWT_EXPIRE_HOURS=24

# 文件上传
MAX_UPLOAD_SIZE=10485760
UPLOAD_PATH=./uploads

# 会话超时配置（分钟）
SESSION_TIMEOUT_WARNING_MINUTES=25
SESSION_TIMEOUT_CLOSE_MINUTES=30
SESSION_TIMEOUT_CHECK_INTERVAL_MINUTES=5

# CORS 配置
ALLOWED_ORIGINS=http://localhost:1420,tauri://localhost
```

### 4. 启动后端服务

```bash
cd server

# 安装依赖
go mod download

# 运行服务
go run cmd/main.go

# 或者编译后运行
go build -o server cmd/main.go
./server
```

✅ 后端服务将在 `http://localhost:8180` 启动

### 5. 启动前端应用

```bash
cd client

# 安装依赖
npm install

# 开发模式（Tauri 桌面应用）
npm run tauri dev

# 或者纯 Web 模式
npm run dev
```

✅ 前端应用将在 `http://localhost:1420` 启动

### 6. 测试系统

#### 创建测试账号

```bash
# 客户端账号
curl -X POST http://localhost:8180/api/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "customer1",
    "password": "password123",
    "email": "customer1@example.com",
    "nickname": "测试客户",
    "user_type": "user"
  }'

# 客服账号
curl -X POST http://localhost:8180/api/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "agent1",
    "password": "password123",
    "email": "agent1@example.com",
    "nickname": "客服小王",
    "user_type": "agent"
  }'
```

#### 使用测试账号登录

- **用户端**: 使用 `customer1 / password123` 登录，选择"用户"身份
- **客服端**: 使用 `agent1 / password123` 登录，选择"客服"身份

---

## 🎯 核心功能说明

### 💬 消息类型支持

系统支持 6 种消息类型：

| 类型 | 说明 | 前端展示 |
|------|------|----------|
| `text` | 文本消息 | 普通文本气泡 |
| `image` | 图片消息 | 缩略图 + 点击放大 |
| `file` | 文件消息 | 文件名 + 下载按钮 |
| `audio` | 音频消息 | HTML5 音频播放器 |
| `video` | 视频消息 | HTML5 视频播放器 |
| `system` | 系统消息 | 居中灰色提示 |

### ⏰ 会话超时机制

系统自动检测会话空闲时间，防止长时间占用资源：

1. **25 分钟无互动** → 发送系统警告："会话将在 5 分钟后自动关闭"
2. **30 分钟无互动** → 自动关闭会话，发送关闭通知
3. **用户发送任意消息** → 重置计时器

可通过环境变量配置：
```env
SESSION_TIMEOUT_WARNING_MINUTES=25
SESSION_TIMEOUT_CLOSE_MINUTES=30
SESSION_TIMEOUT_CHECK_INTERVAL_MINUTES=5
```

### 🏢 多商户 SaaS 架构

#### 商户注册

```bash
curl -X POST http://localhost:8180/api/merchant/register \
  -H "Content-Type: application/json" \
  -d '{
    "company_name": "测试公司",
    "username": "merchant1",
    "password": "password123",
    "email": "merchant1@example.com",
    "contact_person": "张三",
    "phone": "13800138000"
  }'
```

返回 `api_key` 和 `api_secret`，用于 SDK 接入。

#### 嵌入式小部件

```html
<!-- 方式一：使用商户代码 -->
<iframe src="http://localhost:1420/widget?merchant=merchant1" 
        width="400" height="600"></iframe>

<!-- 方式二：使用 JS SDK -->
<div id="ai-cs-widget"></div>
<script src="http://localhost:1420/jssdk.js"></script>
<script>
  AICustomerSDK.init({
    apiBaseUrl: 'http://localhost:8180',
    widgetUrl: 'http://localhost:1420',
    apiKey: 'YOUR_API_KEY',
    apiSecret: 'YOUR_API_SECRET',
    container: '#ai-cs-widget',
    nickname: '访客'
  });
</script>
```

---

## 📚 API 文档

### 认证接口

#### 注册
```http
POST /api/register
Content-Type: application/json

{
  "username": "user123",
  "password": "password123",
  "email": "user@example.com",
  "nickname": "昵称",
  "user_type": "user"  // "user" | "agent"
}
```

#### 登录
```http
POST /api/login
Content-Type: application/json

{
  "username": "user123",
  "password": "password123",
  "user_type": "user"
}

响应:
{
  "token": "eyJhbGc...",
  "user_info": {
    "id": 1,
    "username": "user123",
    "nickname": "昵称",
    "email": "user@example.com"
  }
}
```

### 会话接口

#### 创建会话
```http
POST /api/sessions
Authorization: Bearer {token}
Content-Type: application/json

{
  "priority": "normal",  // "low" | "normal" | "high" | "urgent"
  "source": "desktop"    // "web" | "mobile" | "desktop"
}
```

#### 获取会话列表
```http
GET /api/sessions?status=active&page=1&page_size=20
Authorization: Bearer {token}
```

#### 接入会话（客服）
```http
POST /api/agent/sessions/{id}/assign
Authorization: Bearer {token}
```

#### 关闭会话
```http
POST /api/agent/sessions/{id}/close
Authorization: Bearer {token}
```

### 消息接口

#### 发送消息
```http
POST /api/messages
Authorization: Bearer {token}
Content-Type: application/json

{
  "session_id": 1,
  "content": "消息内容",
  "message_type": "text"
}
```

#### 上传文件
```http
POST /api/sessions/{session_id}/upload
Authorization: Bearer {token}
Content-Type: multipart/form-data

file: <binary>
```

### 客服接口

#### 更新状态
```http
PUT /api/agent/status
Authorization: Bearer {token}
Content-Type: application/json

{
  "status": "online"  // "online" | "offline" | "busy" | "away"
}
```

#### 获取统计数据
```http
GET /api/agent/stats
Authorization: Bearer {token}

响应:
{
  "total_sessions": 125,
  "active_sessions": 3,
  "closed_today": 15,
  "average_rating": 4.6,
  "total_messages": 1580
}
```

#### 获取会话趋势
```http
GET /api/agent/stats/session-trend
Authorization: Bearer {token}

响应:
[
  {"hour": 9, "count": 12},
  {"hour": 10, "count": 15},
  ...
]
```

#### 获取响应时间统计
```http
GET /api/agent/stats/response-time
Authorization: Bearer {token}

响应:
[
  {"date": "2026-02-05", "avg_response": 120},
  {"date": "2026-02-04", "avg_response": 95},
  ...
]
```

### WebSocket 接口

#### 连接
```javascript
const ws = new WebSocket('ws://localhost:8180/ws?token=' + jwtToken);

ws.onopen = () => {
  console.log('WebSocket 已连接');
};

ws.onmessage = (event) => {
  const message = JSON.parse(event.data);
  console.log('收到消息:', message);
};
```

#### 消息格式
```json
{
  "type": "message",
  "session_id": 1,
  "data": {
    "id": 123,
    "session_id": 1,
    "sender_type": "user",
    "content": "消息内容",
    "message_type": "text",
    "created_at": "2026-02-05T10:30:00Z"
  },
  "timestamp": "2026-02-05T10:30:00Z"
}
```

完整 API 文档请参考：[DEVELOPMENT.md](./DEVELOPMENT.md)

---

## 🛠️ 开发指南

### 代码规范

#### Go 代码规范
- 遵循 [Effective Go](https://go.dev/doc/effective_go) 官方指南
- 使用 `gofmt` 格式化代码
- 命名使用驼峰式（CamelCase）
- 错误处理不使用 panic

#### TypeScript/Svelte 规范
- 使用 TypeScript 严格模式
- 组件保持单一职责
- Props 明确类型定义
- 使用 Svelte 5 Runes API

### Git 提交规范

```bash
# 格式
<type>(<scope>): <subject>

# 类型
feat:     新功能
fix:      修复 Bug
docs:     文档更新
style:    代码格式（不影响功能）
refactor: 重构（不改变功能）
test:     测试相关
chore:    构建/工具相关
perf:     性能优化

# 示例
feat(agent): 添加会话转接功能
fix(message): 修复图片上传失败问题
docs(readme): 更新安装文档
```

### 本地开发

```bash
# 后端热重载（使用 air）
go install github.com/cosmtrek/air@latest
cd server
air

# 前端热重载（Vite 自带）
cd client
npm run dev
```

### 测试

```bash
# 后端测试
cd server
go test ./...

# 前端测试
cd client
npm run test
```

---

## 📦 部署指南

### Docker 部署（推荐）

```bash
# 构建镜像
docker-compose build

# 启动服务
docker-compose up -d

# 查看日志
docker-compose logs -f

# 停止服务
docker-compose down
```

### 手动部署

#### 后端部署

```bash
cd server

# 编译
go build -o customer-service cmd/main.go

# 运行（使用 systemd）
sudo cp customer-service /usr/local/bin/
sudo nano /etc/systemd/system/customer-service.service
```

systemd 配置：
```ini
[Unit]
Description=AI Customer Service
After=network.target mysql.service

[Service]
Type=simple
User=www-data
WorkingDirectory=/var/www/customer-service
ExecStart=/usr/local/bin/customer-service
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable customer-service
sudo systemctl start customer-service
```

#### 前端部署

```bash
cd client

# 构建桌面应用
npm run tauri build

# 生成的安装包位于
# - Windows: src-tauri/target/release/bundle/msi/
# - macOS: src-tauri/target/release/bundle/dmg/
# - Linux: src-tauri/target/release/bundle/deb/ 或 appimage/

# Web 部署
npm run build
# 将 build/ 目录部署到 Nginx
```

Nginx 配置：
```nginx
server {
    listen 80;
    server_name your-domain.com;

    # 前端静态文件
    location / {
        root /var/www/customer-service/client/build;
        try_files $uri $uri/ /index.html;
    }

    # API 代理
    location /api/ {
        proxy_pass http://localhost:8180;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

    # WebSocket 代理
    location /ws {
        proxy_pass http://localhost:8180;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
    }
}
```

---

## 🔧 配置说明

### 环境变量完整列表

| 变量名 | 说明 | 默认值 | 必填 |
|--------|------|--------|------|
| `SERVER_PORT` | 服务端口 | 8080 | 否 |
| `SERVER_HOST` | 绑定地址 | 0.0.0.0 | 否 |
| `DB_HOST` | 数据库地址 | localhost | 是 |
| `DB_PORT` | 数据库端口 | 3306 | 否 |
| `DB_USER` | 数据库用户 | root | 是 |
| `DB_PASSWORD` | 数据库密码 | - | 是 |
| `DB_NAME` | 数据库名称 | ai_customer_service | 是 |
| `JWT_SECRET` | JWT 密钥 | - | 是 |
| `JWT_EXPIRE_HOURS` | Token 有效期(小时) | 24 | 否 |
| `MAX_UPLOAD_SIZE` | 最大上传大小(字节) | 10485760 (10MB) | 否 |
| `UPLOAD_PATH` | 文件上传路径 | ./uploads | 否 |
| `SESSION_TIMEOUT_WARNING_MINUTES` | 超时警告时间(分钟) | 25 | 否 |
| `SESSION_TIMEOUT_CLOSE_MINUTES` | 超时关闭时间(分钟) | 30 | 否 |
| `SESSION_TIMEOUT_CHECK_INTERVAL_MINUTES` | 检查间隔(分钟) | 5 | 否 |
| `ALLOWED_ORIGINS` | CORS 允许源 | http://localhost:1420 | 否 |
| `OSS_PROVIDER` | 存储提供商 | local | 否 |

### 云存储配置

支持多种云存储服务：

- **阿里云 OSS**
- **腾讯云 COS**
- **七牛云 Kodo**
- **华为云 OBS**
- **MinIO**
- **AWS S3**

## 🤝 贡献指南

我们欢迎所有形式的贡献！

### 如何贡献

1. **Fork 本仓库**
2. **创建特性分支** (`git checkout -b feature/AmazingFeature`)
3. **提交更改** (`git commit -m 'feat: Add some AmazingFeature'`)
4. **推送到分支** (`git push origin feature/AmazingFeature`)
5. **提交 Pull Request**

### 贡献建议

- 🐛 报告 Bug
- 💡 提出新功能建议
- 📝 改进文档
- 🎨 优化 UI/UX
- ⚡ 性能优化
- 🧪 添加测试

### 开发路线图

- [ ] **移动端适配** - 响应式设计优化
- [ ] **智能客服机器人** - 接入 AI 模型自动回复
- [ ] **客服自动分配** - 基于负载和技能的智能分配
- [ ] **消息模板** - 丰富的消息模板库
- [ ] **数据分析** - 更详细的数据分析报表
- [ ] **视频通话** - WebRTC 视频客服
- [ ] **多语言支持** - 国际化 i18n
- [ ] **Docker 一键部署** - 完整的 Docker Compose 方案
- [ ] **插件系统** - 支持第三方插件扩展
- [ ] **白标定制** - 支持界面品牌定制

---

## 📄 许可证

本项目采用 [MIT](LICENSE) 许可证。

---

## 💬 社区与支持

### 获取帮助

- 📖 [文档中心](https://github.com/yourusername/ai-customer-service/wiki)
- 🐛 [提交 Issue](https://github.com/yourusername/ai-customer-service/issues)
- 💬 [讨论区](https://github.com/yourusername/ai-customer-service/discussions)

## 🔗 商业化合作 / 定制开发

如果你需要 **定制版本、私有化部署、功能扩展或商用授权**，欢迎联系。

**联系信息：**

- Telegram：@nv_exchange
- 邮箱：nvcha3901@proton.me

---

## 💝 请博主喝茶

如果这个项目对你有帮助，欢迎支持捐赠。你的支持将用于持续维护与功能优化。

**捐赠信息：**

- USDT(TRC20)：TQLmVxmAGXCqm6t2ab1P3rjehB8SuuUuUu
- USDT(ERC20)：0x9Da59FD96Bcb8700727038267de56380BeA72fb3
- BTC：bc1pzykrwx2xa84h04evtx0tp85a897kxvqdq98x07u50ejg57jeae4q3x4y8x
- ETH：0x9Da59FD96Bcb8700727038267de56380BeA72fb3
- BSC(BEP20)：0x9Da59FD96Bcb8700727038267de56380BeA72fb3

---

## ⭐ Star History

如果这个项目对你有帮助，请给我们一个 Star ⭐️

[![Star History Chart](https://api.star-history.com/svg?repos=yourusername/ai-customer-service&type=Date)](https://star-history.com/#yourusername/ai-customer-service&Date)

---

## 🙏 致谢

感谢所有为这个项目做出贡献的开发者！

<a href="https://github.com/yourusername/ai-customer-service/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=yourusername/ai-customer-service" />
</a>

---

## 系统截图

![系统截图](screenshots/image.png)

<div align="center">

**[⬆ 回到顶部](#-ai-customer-service---智能多坐席客服系统)**

Made with ❤️ by [Your Name](https://github.com/yourusername)

</div>
