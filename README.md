# Evaluation Microservices

基于 Spring Boot 2.7.12 的微服务项目，支持用户认证（数据库、GitHub OAuth2、LDAP）。

## 技术栈

- Java 17
- Spring Boot 2.7.12
- Spring Cloud 2021.0.3
- Spring Cloud Alibaba 2021.0.4.0
- Nacos (服务发现与配置中心)
- MySQL 8.0
- Redis 6.2
- OpenLDAP
- Sa-Token (认证授权)
- MyBatis Plus 3.5.7
- Docker & Docker Compose

## 服务架构

- **Gateway**: 网关服务（端口 7573，唯一对外暴露）
- **UAA**: 用户认证服务（支持数据库登录、GitHub OAuth2、LDAP）
- **Product**: 产品服务
- **Nacos**: 服务发现与配置中心
- **MySQL**: 数据库
- **Redis**: 缓存
- **OpenLDAP**: LDAP 认证服务



## 环境变量配置

### GitHub OAuth2 配置

创建 `.env` 文件（基于 `.env.example`）：

```bash
cp .env.example .env
```

编辑 `.env` 文件，填入你的 GitHub OAuth App 配置：

```env
GITHUB_CLIENT_ID=your_github_client_id
GITHUB_CLIENT_SECRET=your_github_client_secret
GITHUB_REDIRECT_URI=http://localhost:7573/api/users/github/callback
```

| 变量名 | 说明 | 默认值 |
|--------|------|--------|
| GITHUB_CLIENT_ID | GitHub OAuth App 的 Client ID | 无 |
| GITHUB_CLIENT_SECRET | GitHub OAuth App 的 Client Secret | 无 |
| GITHUB_REDIRECT_URI | 回调地址 | http://localhost:7573/api/users/github/callback |

创建 GitHub OAuth App 的步骤：

1. 访问 https://github.com/settings/developers
2. 点击 "New OAuth App"
3. 填写应用信息：
   - Application name: 任意名称
   - Homepage URL: `http://localhost:7573`
   - Authorization callback URL: `http://localhost:7573/api/users/github/callback`
4. 创建后获得 Client ID 和 Client Secret

如不配置 `.env` 文件，会使用默认值（仅用于测试）。

### 数据库配置

默认配置：
- 数据库名: `evaluation`
- 用户名: `root`
- 密码: `123456`

如需修改，编辑 `docker-compose.env.yml` 和 `nacos/custom.env`。

### LDAP 配置

默认配置：
- 组织: `example.com`
- 管理员密码: `123456`
- Base DN: `dc=example,dc=com`

如需修改，编辑 `docker-compose.env.yml`。

## 部署步骤

### 1. 创建 Docker 网络

```bash
docker network create net
```

### 2. 启动基础服务

```bash
docker-compose -f docker-compose.env.yml up -d
```

等待 MySQL、Redis、LDAP、Nacos 服务启动完成（约 1-2 分钟）。

### 3. 启动应用服务

```bash
docker-compose up -d
```

### 4. 查看服务状态

```bash
docker-compose ps
```

### 5. 查看日志

```bash
# 查看所有服务日志
docker-compose logs -f

# 查看特定服务日志
docker-compose logs -f gateway-service
docker-compose logs -f user-service
```

## 服务访问

### 对外访问

- **API 网关**: `http://localhost:7573`

### 内部服务（仅容器网络访问）

- **Gateway**: `http://gateway-service:8080`
- **User Service**: `http://user-service:8081`
- **Product Service**: `http://product-service:8082`
- **Nacos**: `http://nacos2:8848` (用户名: nacos, 密码: nacos)
- **MySQL**: `mysql8:3306`
- **Redis**: `redis6:6379`
- **LDAP**: `openldap2:389`

## API 测试

### 1. 数据库登录

POST `/api/users/login`

```bash
curl -X POST http://localhost:7573/api/users/login -H "Content-Type: application/json" -d "{\"username\": \"your_username\", \"password\": \"your_password\"}"
```

### 2. GitHub OAuth2 登录

POST `/api/users/github/login`

```bash
curl -X POST http://localhost:7573/api/users/github/login -H "Content-Type: application/json" -d "{\"state\": \"9240582\"}"
```

返回示例：
```json
{
  "code": 200,
  "message": "Success",
  "data": {
    "authorizeUrl": "https://github.com/login/oauth/authorize?client_id=xxx&redirect_uri=http://localhost:7573/api/users/github/callback&state=9240582&scope=user:email"
  }
}
```

浏览器访问 authorizeUrl，授权后自动回调到 `/api/users/github/callback?code=xxx&state=xxx`

### 3. LDAP 登录

POST `/api/users/ldap/login`

```bash
curl -X POST http://localhost:7573/api/users/ldap/login -H "Content-Type: application/json" -d "{\"username\": \"ldap_editor_1\", \"password\": \"ldap_editor_1\"}"
```

### 4. 创建 LDAP 用户

POST `/api/users/ldap/user`

groups 可多选：EDITOR、USER

```bash
curl -X POST http://localhost:7573/api/users/ldap/user -H "Content-Type: application/json" -d "{\"username\": \"ldap_editor_1\", \"password\": \"ldap_editor_1\", \"firstName\": \"ldap\", \"lastName\": \"editor_1\", \"email\": \"editor_1@example.com\", \"groups\": [\"EDITOR\"]}"
```

### 5. 获取商品列表

GET `/api/products`

需要 Authorization header（登录后获得的 token）

```bash
curl -X GET http://localhost:7573/api/products -H "Authorization: your_token_here"
```

### 6. 添加商品

POST `/api/products`

```bash
curl -X POST http://localhost:7573/api/products -H "Content-Type: application/json" -H "Authorization: your_token_here" -d "{\"productName\": \"垃圾桶\"}"
```

### 7. 修改商品

PUT `/api/products/{id}`

```bash
curl -X PUT http://localhost:7573/api/products/2 -H "Content-Type: application/json" -H "Authorization: your_token_here" -d "{\"productName\": \"鼠标垫\"}"
```

### 8. 删除商品

DELETE `/api/products/{id}`

```bash
curl -X DELETE http://localhost:7573/api/products/3 -H "Authorization: your_token_here"
```

## 许可证

本项目仅供学习和个人使用，禁止商业使用。

Copyright (c) 2026

