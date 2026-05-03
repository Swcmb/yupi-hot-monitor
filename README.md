# AI 热点监控工具

基于 **Node.js + Express + React + OpenRouter** 的 AI 热点监控工具，支持多信息源聚合抓取、AI 真假识别与相关性分析、WebSocket 实时推送、邮件通知，并提供 Docker 一键部署能力。

## 功能特性

- **多源聚合抓取**：支持 Twitter、Bing、HackerNews、搜狗、B 站等 8+ 信息源
- **AI 智能分析**：利用大模型进行查询扩展、真假识别、相关性分析和智能摘要
- **实时推送**：WebSocket 实时推送 + 邮件通知
- **多维度筛选**：按来源、重要性、时间范围筛选，支持多种排序方式
- **全网搜索**：输入关键词从多个数据源聚合搜索
- **Docker 部署**：一行命令即可完成部署

## 技术栈

| 层级 | 技术 |
|------|------|
| 前端 | React 19 + Vite + Tailwind CSS + Socket.io |
| 后端 | Express 5 + Prisma + SQLite |
| AI | OpenRouter (支持多种大模型) |
| 实时通信 | Socket.io + node-cron |
| 部署 | Docker + Docker Compose |

## 快速开始

### Docker 部署（推荐）

1. **克隆项目**
```bash
git clone https://github.com/Swcmb/yupi-hot-monitor.git
cd yupi-hot-monitor
```

2. **配置环境变量**
```bash
cp .env.example .env
# 编辑 .env，填入你的 API Key
```

3. **启动服务**
```bash
docker-compose up -d --build
```

4. **访问应用**
- 前端: http://localhost:5173
- 后端 API: http://localhost:3001

### 本地开发

#### 前置条件
- Node.js ≥ 18（推荐 20 LTS）
- [OpenRouter API Key](https://openrouter.ai/settings/keys)

#### 安装步骤

```bash
# 克隆项目
git clone https://github.com/Swcmb/yupi-hot-monitor.git
cd yupi-hot-monitor

# 后端
cd server
npm install
npx prisma generate
npx prisma db push
npm run dev

# 前端（新终端）
cd client
npm install
npm run dev
```

访问 http://localhost:5173

## 环境变量

| 变量 | 必需 | 说明 |
|------|------|------|
| `OPENROUTER_API_KEY` | 是 | OpenRouter API Key，用于 AI 分析 |
| `TWITTER_API_KEY` | 否 | Twitter API Key，用于 Twitter 数据源 |
| `SMTP_HOST` | 否 | 邮件服务器地址 |
| `SMTP_PORT` | 否 | 邮件服务器端口 |
| `SMTP_USER` | 否 | 邮件用户名 |
| `SMTP_PASS` | 否 | 邮件密码 |
| `NOTIFY_EMAIL` | 否 | 通知接收邮箱 |

## API 接口

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/health` | 健康检查 |
| GET | `/api/keywords` | 获取关键词列表 |
| POST | `/api/keywords` | 创建关键词 |
| GET | `/api/hotspots` | 获取热点列表（支持筛选、排序、分页） |
| POST | `/api/hotspots/search` | 全网搜索 |
| GET | `/api/hotspots/stats` | 热点统计 |
| GET | `/api/notifications` | 通知列表 |
| POST | `/api/check-hotspots` | 手动触发热点检查 |

详细 API 文档请参考 [API 文档](./docs/API_INTEGRATION.md)

## 项目结构

```
yupi-hot-monitor/
├── server/                 # 后端服务
│   ├── src/
│   │   ├── routes/        # API 路由
│   │   ├── services/      # 业务服务
│   │   ├── jobs/          # 定时任务
│   │   └── utils/         # 工具函数
│   ├── prisma/            # 数据库 schema
│   └── Dockerfile
├── client/                 # 前端应用
│   ├── src/
│   │   ├── components/    # React 组件
│   │   └── hooks/         # 自定义 Hooks
│   ├── Dockerfile
│   └── nginx.conf         # Nginx 配置
├── docker-compose.yml      # Docker 编排
├── .env.example           # 环境变量模板
└── DOCKER.md              # Docker 部署文档
```

## 数据持久化

使用 Docker 部署时，数据库文件保存在 `./server/data/` 目录，容器重启后数据保留。

## License

MIT
