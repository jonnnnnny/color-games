# 彩虹小课堂 · 颜色大冒险

一个无需后端和外部资源的单页颜色与形状认知小游戏，支持手机、平板和桌面浏览器，可部署到云端随时随地访问。

## 玩法介绍

游戏共有 **11 种模式**，覆盖颜色学习、形状学习、自由探索和触摸游戏：

### 颜色游戏

| 模式 | 玩法 | 特色 |
| --- | --- | --- |
| 🐣 颜色启蒙 | 2 种颜色、1 选 1，点一点学认色 | 2 岁友好，无失败无惩罚 |
| 🌈 经典闯关 | 4 个关卡（颜色启蒙 → 颜色探险 → 颜色大师 → 颜色魔法），看物体选颜色，共 18 题 | 关卡递进，选项从 2 个增至 4 个 |
| 🧠 记忆挑战 | 先看 3 秒颜色物体，藏起来后凭记忆选出刚才的颜色，共 6 题 | 锻炼儿童颜色记忆力 |
| ⚡ 限时冲刺 | 60 秒内快速答题，答对 +1 秒、答错 -2 秒 | 挑战手速与反应 |
| 👀 找不同 | 4 个颜色块中有 1 个与众不同，把它找出来，共 6 题 | 训练视觉辨别力 |

### 形状游戏

| 模式 | 玩法 | 特色 |
| --- | --- | --- |
| 🐣 形状启蒙 | 2 种形状、1 选 1，点一点学认形 | 2 岁友好，无失败无惩罚 |
| 🔷 形状闯关 | 3 关递进（形状启蒙 → 形状探险 → 形状大师），认识 6 种基础形状 | 从 2 选 1 到 4 选 1 |

### 探索与触摸游戏

| 模式 | 玩法 | 特色 |
| --- | --- | --- |
| ✨ 自由探索 | 点一点颜色和形状，听声音看动画 | 无压力自由认知 |
| 🎈 气球戳戳乐 | 戳破飘动的彩色气球，听颜色名 | 纯触摸探索，无计分 |
| 🖍️ 颜色涂鸦板 | 用手指在画布上自由涂鸦 | 11 种颜色可选，锻炼手眼协调 |
| 🫧 气泡纸 | 戳破彩色气泡，啵啵啵 | 解压式触摸游戏 |

### 得分与激励

- **连击加分**：连续答对触发 🔥 连击，每 3 连击额外 +1 分
- **生命值**：闯关/记忆/找不同模式共 3 颗心，答错扣 1 颗
- **星级评价**：结算按失误情况给 1–3 星
- **成就徽章**：🏆 全对大师、💥 连击高手、🧠 过目不忘、🔍 火眼金睛、⚡ 极速闪电、🌈 彩虹勇士、🔷 形状达人、✨ 三星得主

### 其他特性

- 认识 **11 种颜色**（红、橙、黄、绿、青、蓝、紫、粉、棕、灰、白）与 **6 种基础形状**（圆、方、三角、星、心、菱形）
- 30+ 种可爱物体造型（苹果、小鱼、蝴蝶、雪人、冰淇淋等）
- 全程中文语音朗读（Web Speech API）与合成音效（Web Audio），可一键静音
- 答对粒子迸发、通关彩带、连击弹字等动效反馈
- **PWA 支持**：可添加到 iPad/手机主屏幕，离线可用
- 适配手机竖屏/横屏、iPad 大屏与桌面，支持 `prefers-reduced-motion`
- 触控区域 ≥ 88px，安全区域适配，防止幼儿误触

## 本地预览

直接打开 `index.html`，或在项目目录启动任意静态文件服务器：

```bash
python3 -m http.server 8090
```

打开 `http://localhost:8090`。

> **注意**：Service Worker 和 Web Speech API 需要 HTTPS 环境（localhost 除外）。本地预览时 PWA 功能可能受限，建议通过部署后的 HTTPS 地址体验完整功能。

## 云环境部署

### 方式一：Docker 自部署

项目包含 `Dockerfile` 和 `docker-compose.yml`，容器监听 `8090` 端口，提供 `/healthz` 健康检查。

```bash
# 构建并运行
docker build -t color-games .
docker run --rm -p 8090:8090 color-games

# 或使用 Docker Compose
docker compose up -d
```

部署后访问 `http://你的服务器IP:8090`。

### 方式二：一键部署到 Render

1. 将本项目上传到 GitHub、GitLab 或 Bitbucket。
2. 打开 `https://render.com/deploy?repo=你的仓库地址`，例如：

```text
https://render.com/deploy?repo=https://github.com/你的用户名/color-games
```

3. 登录 Render，确认服务配置后点击 **Deploy**。
4. 部署完成后，Render 会生成一个可直接访问的 `onrender.com` 地址（自动 HTTPS）。

项目根目录中的 `render.yaml` 已包含静态站点配置，无需填写构建目录、启动命令或端口。该一键部署配置默认关闭自动部署；如需代码推送后自动更新，可在 Render 服务设置中将 Auto-Deploy 改为 On Commit。

### 方式三：Nginx 反向代理

如果已有 Nginx 服务器，可将构建产物（`index.html`、`manifest.json`、`sw.js`）复制到网站根目录，参考 `nginx.conf` 配置：

```nginx
server {
    listen 443 ssl http2;
    server_name your-domain.com;

    # SSL 证书配置（Let's Encrypt 或自有证书）
    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    root /var/www/color-games;
    index index.html;

    # PWA 资源 MIME 类型
    types {
        application/manifest+json manifest.json;
        application/javascript sw.js;
    }

    location / {
        try_files $uri $uri/ /index.html;
    }

    # Service Worker 禁止缓存
    location = /sw.js {
        add_header Cache-Control "no-cache, no-store, must-revalidate";
    }

    location = /manifest.json {
        add_header Cache-Control "max-age=3600";
    }
}

# HTTP → HTTPS 跳转
server {
    listen 80;
    server_name your-domain.com;
    return 301 https://$server_name$request_uri;
}
```

### 方式四：其他静态托管平台

项目无构建依赖，可直接上传到 Vercel、Netlify、GitHub Pages、Cloudflare Pages 等平台：
- **发布目录**：项目根目录（`.`）
- **构建命令**：无
- **Node 版本**：无要求

> AI生成
