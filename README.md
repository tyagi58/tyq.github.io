# 工科职场内容助手 - 部署说明

## 项目架构

```
前端 (HTML)  →  后端 (Node.js)  →  本地AI服务 (Moltbot)
              ↑
           持有API密钥
```

## 快速开始

### 1. 安装依赖

确保已安装 Node.js (v14+)，然后运行：

```bash
npm install
```

### 2. 配置API密钥

将 `.env.example` 复制为 `.env`：

```bash
copy .env.example .env
```

编辑 `.env` 文件，确认配置正确：

```env
API_KEY=6n-wGEr5HMRlqdGx_mHrySZDTTTGsItQZXUOSwDjv97KPAq_fvuUAvvRww2YxGNGpPtWygTRdPx2N2iXAHMqSQ
API_BASE_URL=http://127.0.0.1:18789
PORT=3000
```

### 3. 启动本地AI服务

确保 Moltbot 服务已启动并运行在 `http://127.0.0.1:18789`

### 4. 启动后端服务

```bash
npm start
```

看到以下输出表示启动成功：

```
✅ 后端服务已启动: http://localhost:3000
📡 AI服务地址: http://127.0.0.1:18789
🔑 API密钥: 已配置
```

### 5. 打开前端页面

在浏览器中打开 `engineering-content-assistant.html`

## 功能测试

### 测试选题生成

1. 打开前端页面
2. 切换到"选题灵感"模块
3. 输入关键词，如"机械工程师晋升"
4. 点击"生成选题库"
5. 等待AI生成10个选题

### 测试脚本创作

1. 切换到"脚本创作"模块
2. 输入选题标题（或从素材库选择）
3. 选择工科领域和专业度
4. 点击"生成大纲"
5. 查看大纲后，点击"生成脚本"
6. 查看完整脚本内容

## 安全说明

### ✅ 安全实践

- API密钥存储在 `.env` 文件中（已加入 .gitignore）
- 前端代码不包含任何密钥信息
- 后端服务作为中转层，隔离密钥访问

### ⚠️ 注意事项

1. **绝对不要**将 `.env` 文件提交到 Git
2. **绝对不要**将包含密钥的代码上传到公开平台
3. 如果需要分享代码，只分享 `.env.example`（不含真实密钥）

## 故障排查

### 问题1：后端启动失败

**错误信息**：`Error: Cannot find module 'express'`

**解决方案**：
```bash
npm install
```

### 问题2：前端无法连接后端

**错误信息**：`Failed to fetch` 或 `Network error`

**检查清单**：
1. 后端服务是否已启动？（访问 http://localhost:3000/health）
2. 前端代码中的 `BACKEND_API_URL` 是否正确？
3. 浏览器控制台是否有CORS错误？

### 问题3：AI生成失败

**错误信息**：`AI服务返回错误: 401` 或 `AI服务返回错误: 500`

**检查清单**：
1. Moltbot 服务是否已启动？
2. `.env` 中的 API_KEY 是否正确？
3. `.env` 中的 API_BASE_URL 是否正确？
4. 尝试直接访问 AI 服务测试连通性

### 问题4：生成的内容格式错误

**可能原因**：AI返回的不是标准JSON格式

**解决方案**：
- 检查后端日志，查看AI返回的原始内容
- 调整后端代码中的提示词（prompt）
- 根据实际AI模型调整 `model` 参数

## 开发模式

使用 nodemon 自动重启（代码修改后自动重启服务）：

```bash
npm run dev
```

## 生产部署

### 方案1：本地部署

适合个人使用，按照上述步骤部署即可。

### 方案2：服务器部署

1. 将代码上传到服务器（不包含 `.env`）
2. 在服务器上创建 `.env` 文件并配置密钥
3. 使用 PM2 管理进程：

```bash
npm install -g pm2
pm2 start backend-server.js --name content-assistant
pm2 save
pm2 startup
```

4. 配置 Nginx 反向代理（可选）

### 方案3：云平台部署

- **Heroku**：支持环境变量配置
- **Vercel**：适合前端，后端需要使用 Serverless Functions
- **Railway**：简单易用，支持 Node.js

## API端点说明

### 健康检查

```
GET /health
```

返回：
```json
{
  "status": "ok",
  "message": "后端服务运行正常"
}
```

### 生成选题

```
POST /api/generate-topics
Content-Type: application/json

{
  "keyword": "机械工程师晋升"
}
```

### 生成大纲

```
POST /api/generate-outline
Content-Type: application/json

{
  "topic": "机械工程师晋升的3个关键技能",
  "field": "机械",
  "level": "进阶"
}
```

### 生成脚本

```
POST /api/generate-script
Content-Type: application/json

{
  "outline": { ... }
}
```

## 技术栈

- **前端**：HTML + Tailwind CSS + Vanilla JavaScript
- **后端**：Node.js + Express
- **AI服务**：Moltbot (兼容 OpenAI API)
- **存储**：LocalStorage（前端本地存储）

## 许可证

本项目仅供个人学习和使用。

## 联系方式

如有问题，请检查：
1. 后端日志输出
2. 浏览器控制台错误信息
3. AI服务是否正常运行
