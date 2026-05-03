# Docker 部署说明

## 快速开始

### 1. 配置环境变量

复制 `.env.example` 为 `.env` 并填写配置：

```bash
cp .env.example .env
```

编辑 `.env` 文件，填入你的 API Key：
- `OPENROUTER_API_KEY`: (必需) 从 https://openrouter.ai 获取
- `TWITTER_API_KEY`: (可选) 从 https://twitterapi.io 获取

### 2. 构建并启动服务

```bash
docker-compose up -d --build
```

### 3. 访问应用

- 前端: http://localhost:5173
- 后端 API: http://localhost:3001

## 常用命令

| 命令 | 说明 |
|------|------|
| `docker-compose up -d` | 后台启动服务 |
| `docker-compose down` | 停止并删除服务 |
| `docker-compose logs -f` | 查看实时日志 |
| `docker-compose logs server` | 查看后端日志 |
| `docker-compose logs client` | 查看前端日志 |
| `docker-compose restart` | 重启所有服务 |
| `docker-compose restart server` | 重启后端服务 |

## 数据持久化

- 数据库文件自动保存在 `./server/data/` 目录
- 数据会在容器重启后保留

## 开发模式

如果需要在开发环境运行（热更新），可以修改 docker-compose.yml，或者直接在本地运行：

```bash
# 后端
cd server
npm install
npx prisma generate
npx prisma db push
npm run dev

# 前端 (另一个终端)
cd client
npm install
npm run dev
```
