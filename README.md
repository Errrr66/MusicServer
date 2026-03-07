### 版本更新日志 (V1.0.1)
#### 更新日期：2026-03-04
#### 更新类型：功能新增 + 基础设施优化
#### 关联模块：后端（Spring Boot）、前端（Vue）、第三方服务集成

---

### 一、运行环境说明
| 组件         | 启动命令/方式                                                                 | 访问地址               |
|--------------|-----------------------------------------------------------------------------|----------------------|
| MinIO 服务器 | 在 minio/bin 目录执行：`minio.exe server "项目路径\minio\data"`              | -                    |
| Redis 服务   | 启动本地/部署环境Redis服务                                                   | -                    |
| 管理端       | 执行命令：`npx pnpm dev`                                                     | http://localhost:8089 |
| 前端         | 执行命令：`npm run dev`                                                      | http://localhost:8090 |
| 后端         | 运行 VibeMusicApplication 类的 main 方法                                     | -                    |

---

### 二、后端核心更新
#### 1. 第三方服务集成
- 新增 DeepSeek AI 服务集成能力，支持 OpenAI 兼容格式的 HTTP 通信
- 在 `application.yml` 中新增配置项：
  - DeepSeek API Key（接口鉴权）
  - DeepSeek Base URL（接口请求地址）
  - 模型名称（指定调用的AI模型）

#### 2. 核心类/接口新增
- 服务层：新增 `DeepSeekService` 类，封装与 DeepSeek API 的请求/响应处理逻辑
- 控制层：新增 `ChatController`，开放 `/chat/ask` 接口，接收前端消息并返回AI回复
- 配置类：新增 `RestTemplateConfig`，配置并注入 `RestTemplate` Bean，用于HTTP请求发送

#### 3. 数据模型更新
- DTO层：新增 `ChatRequestDTO`，标准化接收前端聊天请求参数
- API模型：新增 `ChatCompletionRequest`/`ChatCompletionResponse` 及其内部类，映射DeepSeek API的JSON结构

#### 4. 基础设施优化
- 优化 `Result<T>` 统一返回类：
  - 增加泛型方法支持，提升类型安全性
  - 修复类型转换告警，保证接口返回数据格式统一

---

### 三、前端核心更新
#### 1. 功能模块新增
- 新增 AI 问答功能模块，路径：`src/pages/chat/index.vue`
- 核心交互能力：
  - 消息列表自动滚动，保证最新消息可视
  - 区分用户（蓝色气泡）/AI（灰色气泡）消息样式
  - 新增“思考中...”加载状态反馈，提升用户体验

#### 2. 接口封装
- 在 `src/api/chat.ts` 中新增 `sendChatMessage` 方法，封装后端 `/chat/ask` 接口调用逻辑

#### 3. 路由与导航更新
- 侧边栏：在 `src/layout/components/aside/data.ts` 中新增“AI 问答”菜单项，配置图标及路由跳转规则
- 路由配置：在 `src/routers/index.ts` 中注册 `/chat` 路由，关联AI问答页面

---
