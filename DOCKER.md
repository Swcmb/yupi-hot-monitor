# Docker 部署指南

本项目支持 Docker Compose 一键部署，包含后端服务和前端应用。

## 快速开始

### 1. 配置环境变量

```bash
cp .env.example .env
```

编辑 `.env` 文件，填入你的 API Key：

```env
OPENROUTER_API_KEY=your_openrouter_api_key_here
TWITTER_API_KEY=your_twitter_api_key_here  # 可选
```

### 2. 构建并启动

```bash
docker-compose up -d --build
```

### 3. 访问应用

| 服务 | 地址 |
|------|------|
| 前端 | http://localhost:5173 |
| 后端 API | http://localhost:3001 |

## 常用命令

```bash
# 启动服务（后台运行）
docker-compose up -d

# 查看日志
docker-compose logs -f
docker-compose logs -f server   # 仅后端
docker-compose logs -f client    # 仅前端

# 重启服务
docker-compose restart

# 停止服务
docker-compose down

# 完全重建
docker-compose down -v --rmi local
docker-compose up -d --build
```

## 数据持久化

数据库文件保存在 `./server/data/` 目录：
- `dev.db` - SQLite 数据库文件

容器删除后数据不会丢失，重新启动会自动加载。

如需重置数据库，删除 `server/data/` 目录后重启服务即可。

## 自定义配置

### 修改端口

编辑 `docker-compose.yml` 中的端口映射：

```yaml
services:
  server:
    ports:
      - "3002:3001"  # 修改后端端口
  client:
    ports:
      - "8080:80"    # 修改前端端口
```

### 环境变量说明

| 变量 | 说明 |
|------|------|
| `OPENROUTER_API_KEY` | OpenRouter API Key（必需） |
| `TWITTER_API_KEY` | Twitter API Key（可选） |
| `SMTP_*` | 邮件通知配置（可选） |

## 生产环境部署

对于生产环境，建议：

1. 使用反向代理（如 Nginx）处理 HTTPS
2. 使用环境变量或 Docker Secret 管理敏感信息
3. 配置定期数据库备份
4. 考虑使用 Docker Swarm 或 Kubernetes 进行编排

## 清理

```bash
# 删除容器和镜像
docker-compose down -v --rmi all

# 清理未使用的 Docker 资源
docker system prune -f
```
