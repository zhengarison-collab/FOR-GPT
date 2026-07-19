# PTG Web Heritage

PTG Web 是 PTG iOS 的独立网页附属版本，目标不是一比一复制原生端底层能力，而是留下一个**不依赖 App 签名、不依赖 Pro 订阅、可安装、可迁移、可离线进入**的个人工作台。

## 已实现

- 五栏：**首页、文件库、笔记、写作、设置**
- 安装为 PWA，缓存完成后可离线进入
- IndexedDB 本地数据库
- 文本、Markdown、JSON、CSV、HTML、PDF、图片导入
- 文件、笔记、写作加入当前对话上下文
- OpenAI Compatible / DeepSeek 流式调用
- 云端失败时自动回退到离线文本工具
- 写作自动保存、AI 九项操作、Markdown 导出
- 完整 JSON 备份与恢复
- 单文件 `pocket.html` 离线工具

## 本地打开

由于 Service Worker 和 ES Module 需要 HTTP 环境，不建议直接双击 `index.html`。在 `web` 目录运行：

```bash
python3 -m http.server 8080
```

然后打开 `http://localhost:8080`。

`pocket.html` 可以直接双击使用。

## GitHub Pages

仓库已附带 `.github/workflows/ptg-web-pages.yml`。在 GitHub 仓库：

1. Settings → Pages
2. Source 选择 **GitHub Actions**
3. 合并包含本目录的分支后，工作流会发布 `web/`

私有仓库是否可用 Pages 取决于 GitHub 账户方案。也可以把 `web/` 直接拖到 Netlify 或部署到 Cloudflare Pages。

## 云端 API

设置页默认使用：

- Endpoint：`https://api.deepseek.com/chat/completions`
- Model：`deepseek-chat`
- Max output tokens：`8192`

浏览器直连可能受 CORS 或密钥安全限制。推荐部署仓库附带的 `worker/` 代理，并在 PTG 设置中填写 Worker 地址，不在浏览器保存密钥。

## 数据安全

- 文件、笔记、写作、聊天和设置默认保存在当前浏览器 IndexedDB。
- 卸载浏览器、清理站点数据或更换域名可能导致本地数据丢失。
- 定期使用“设置 → 导出完整备份”。
- 不要在公共电脑中保存 API Key。

## 当前边界

- 网页端不能直接复用 iOS 内的 MLX 本地模型和 Keychain。
- PDF/图片可保存与预览，但当前版本不内置 OCR 或 PDF 文字提取。
- “本地模式”是可靠的离线文本工具与本地检索，不冒充大语言模型。
- 浏览器直连第三方 API 可能因服务商 CORS 策略失败，Worker 代理用于解决这一问题。
