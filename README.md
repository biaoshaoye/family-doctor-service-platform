[README.md](https://github.com/user-attachments/files/27228727/README.md)
# family-doctor-service-platform
基于 `Spring Boot 3 + Vue 3` 的前后端分离项目，覆盖家庭签约、医生服务、业务审核和系统管理四类角色。
# 家庭医生签约服务平台

基于 `Spring Boot 3 + Vue 3` 的前后端分离项目，覆盖家庭签约、医生服务、业务审核和系统管理四类角色。

## 项目现状（按当前实现）

- 完整角色：`FAMILY`、`DOCTOR`、`BIZ_ADMIN`、`SYS_ADMIN`
- 核心流程：注册登录 -> 医生资质审核 -> 签约申请与模拟支付 -> 合同生效 -> 咨询/预约/服务记录 -> 履约监督与统计
- 实时能力：在线咨询支持 `WebSocket` 实时消息推送（`/ws/chat`）
- 文件能力：支持资质与咨询附件上传，访问前缀为 `/files/**`
- 数据支撑：`database/schema.sql` 与 `database/test-data.sql` 提供建表和演示数据

## 技术栈

### 后端

- Java 17
- Spring Boot 3.2.5
- Spring Security + JWT
- MyBatis-Plus 3.5.6
- MySQL 8.x
- Spring WebSocket
- SpringDoc OpenAPI（Swagger UI）

### 前端

- Vue 3 + Vite 5
- Vue Router 4
- Pinia
- Element Plus
- Axios
- ECharts

## 目录结构

```text
family-doctor-system/
├─ backend/                # Spring Boot 后端
├─ frontend/               # Vue 3 前端
├─ database/               # SQL 脚本（建表 + 测试数据）
├─ file-storage/           # 本地文件存储目录（上传文件）
├─ README.md
└─ ARCHITECTURE.md
```

## 角色能力概览

- `FAMILY`：家庭档案、成员与健康档案、签约申请、模拟支付、咨询、预约、查看服务记录
- `DOCTOR`：资料维护、资质上传、签约家庭管理、咨询处理、排班与预约管理、服务记录维护
- `BIZ_ADMIN`：医生审核、合同审核、服务包管理、履约监督、业务统计看板
- `SYS_ADMIN`：平台总览、账号管理、角色权限、操作日志

## 快速启动

### 1) 准备数据库

1. 创建数据库：`family_doctor`
2. 执行脚本（推荐顺序）：
   - `database/schema.sql`
   - `database/test-data.sql`

### 2) 启动后端

1. 检查配置文件 `backend/src/main/resources/application.yml`
   - 数据库连接：`spring.datasource.*`
   - 文件存储目录：`app.file.upload-dir`
2. 在 `backend` 目录执行：

```bash
mvn spring-boot:run
```

后端默认地址：`http://localhost:8080`

可用文档地址：

- Swagger UI: `http://localhost:8080/swagger-ui.html`
- OpenAPI: `http://localhost:8080/api-docs`

### 3) 启动前端

在 `frontend` 目录执行：

```bash
npm install
npm run dev
```

前端默认地址：`http://localhost:5173`

前端代理（已在 `frontend/vite.config.js` 配置）：

- `/api` -> `http://localhost:8080`
- `/files` -> `http://localhost:8080`
- `/ws` -> `ws://localhost:8080`

## 演示账号（来自测试数据）

- 系统管理员：`admin / 123456`
- 业务管理员：`bizadmin / 123456`
- 家庭用户示例：`zhangsan / 123456`、`lisi / 123456`
- 医生示例：`drwang / 123456`、`drchen / 123456`

更多账号见 `database/test-data.sql`。

## 关键实现说明（与代码一致）

- 当前为演示实现，密码在数据库中按明文样例存储，后端使用 `NoOpPasswordEncoder`
- JWT 鉴权用于 API 访问控制，路由与接口均按角色隔离
- Redis 配置类已预留但未启用（`RedisConfig` 为占位）
- 咨询消息支持文本/图片等内容类型，并落库到 `consultation_message`

## 相关文档

- 架构与链路：`ARCHITECTURE.md`
- 数据模型：`database/README.md`
