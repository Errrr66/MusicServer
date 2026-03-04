# 前后端启动注意事项：
# 运行 MinIO 服务器的命令（在 minio/bin 目录下运行）
# minio.exe server "项目路径\minio\data"
# 启动Redis服务。
# admin管理端启动：npx pnpm dev
# 前端启动：npm run dev
# 后端Spring Boot启动：运行VibeMusicApplication的main方法
# 前端访问地址：http://localhost:8090
# 管理端访问地址：http://localhost:8089
后端 
集成 DeepSeek AI 服务
新增配置: 在 application.yml 中添加了 DeepSeek API 的 Key、Base URL 和模型名称配置。
新增服务: 创建 DeepSeekService 类，负责封装与 DeepSeek 接口的 HTTP 通信逻辑（OpenAI 兼容格式）。
新增控制器: 创建 ChatController，开放 /chat/ask 接口，接收前端用户消息并返回 AI 回复。
数据模型更新
DTO: 新增 ChatRequestDTO 用于接收前端请求参数。
API 模型: 新增 ChatCompletionRequest 和 ChatCompletionResponse 及其内部类，用于映射 DeepSeek API 的 JSON 请求和响应结构。
基础设施优化
配置类: 新增 RestTemplateConfig，配置并注入 RestTemplate Bean 用于发送 HTTP 请求。
工具类优化: 修改 Result<T> 统一返回类，增加了对泛型方法的支持，修复了部分类型转换告警，确保接口返回类型安全。

前端
新增 AI 问答功能模块
新增页面: 创建 src/pages/chat/index.vue，实现了类似聊天软件的交互界面，包含：
消息列表自动滚动。
区分用户（蓝色）和 AI（灰色）的气泡样式。
“思考中...”的加载状态反馈。
API 封装: 在 src/api/chat.ts 中封装了 sendChatMessage 方法，用于调用后端聊天接口。
路由与导航更新
侧边栏菜单: 在 src/layout/components/aside/data.ts 中新增了 "AI 问答" 菜单项，配置了图标和路由跳转。
路由配置: 在 src/routers/index.ts 中注册了 /chat 路由路径。