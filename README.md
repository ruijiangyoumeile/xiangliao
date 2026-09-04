# 🌿 艾草 · 3D 模型展示网站

一个基于 [Google model-viewer](https://modelviewer.dev/) 的免后端静态网页：
扫码打开即可**拖动旋转、缩放、全屏**查看模型，手机端还支持 **AR 实景放置**。

```
aicao-viewer/
├── index.html        # 主页面（模型展示）
├── qr-code.html      # 二维码生成小工具（本地双击打开即可用）
├── README.md         # 本说明
└── models/
    └── aicao.glb      # 模型文件（已减面 + 纹理压缩至约 4.7MB）
```

---

## 一、本地预览

> ⚠️ 不要直接双击 `index.html` 用 `file://` 打开 —— 浏览器的安全限制会导致模型加载不出来。
> 需要起一个本地小服务器。

任选一种方式，在该文件夹内执行：

```bash
# 方式一：Python（若已安装）
python -m http.server 8000

# 方式二：Node.js（若已安装）
npx serve .

# 方式三：VS Code 安装 Live Server 插件后，右键 index.html → Open with Live Server
```

然后浏览器打开：<http://localhost:8000>

---

## 二、部署到 GitHub Pages（免费，公网可访问）

> 前提：你有 GitHub 账号，且电脑已安装 git（或用网页拖拽上传，见「免命令行方式」）。

### 命令行方式

```bash
# 1. 进入项目目录
cd aicao-viewer

# 2. 初始化并提交
git init
git add .
git commit -m "艾草 3D 展示站"

# 3. 在 GitHub 网页上新建一个仓库（New repository），名字建议：aicao-viewer
#    建好后把本地的仓库关联并推送：
git branch -M main
git remote add origin https://github.com/你的用户名/aicao-viewer.git
git push -u origin main
```

### 免命令行方式（网页拖拽上传）

1. GitHub 上新建仓库 `aicao-viewer`（不要勾选 README 初始化）。
2. 进入仓库页 → **Add file → Upload files** → 把 `index.html`、`qr-code.html`、`README.md`
   和 `models/` 文件夹整体拖进去 → Commit changes。

### 开启 Pages

1. 仓库页 → **Settings** → 左侧 **Pages**。
2. **Branch** 选 `main`，目录选 `/ (root)` → **Save**。
3. 等 1~2 分钟，页面顶部会出现你的网址：
   `https://你的用户名.github.io/aicao-viewer/`

> 💡 若推送时模型文件超过 **100 MB**，GitHub Pages 会拒绝。需要压缩模型，见文末「模型过大怎么办」。

---

## 三、生成二维码（让所有人扫码进入）

1. 部署完成后，把上面的网址填进 `qr-code.html`（本地双击打开即可），点「生成二维码」。
2. 右键保存二维码图片，打印或发到群里。

> 手机上体验更佳：微信/支付宝扫码后，**点右上角「在浏览器中打开」**，
> 才能使用 AR 功能（微信内置浏览器对 AR 支持不完整）。

---

## 四、自定义修改

| 想改什么 | 怎么做 |
|---|---|
| **页面标题 / 说明文字** | 编辑 `index.html` 里 `.title-card` 中的 `艾草` 和说明段落 |
| **替换模型** | 用新文件覆盖 `models/aicao.glb`（保持文件名不变最省事） |
| **模型自动旋转展示** | 在 `index.html` 的 `<model-viewer>` 标签里加上属性 `auto-rotate` |
| **关闭 AR 按钮** | 删掉 `<model-viewer>` 标签里的 `ar` 属性 |
| **背景颜色** | 改 `body` 的 `background`（深色渐变） |

### 关于 AR（iPhone 用户）

- **Android / 部分浏览器**：直接支持，点模型右下角的 AR 按钮即可。
- **iPhone (iOS Safari)**：Apple 要求使用 USDZ 格式，否则不显示 AR 按钮。
  处理办法：把 GLB 转成 USDZ（macOS 用免费的 Reality Converter；
  Windows 可用在线转换工具搜索 "glb to usdz online"），
  放入 `models/` 后，在 `index.html` 中取消 `ios-src` 那一行的注释即可。

---

## 五、常见问题

**扫码后模型一直在转圈 / 加载不出来 / 加载失败**
- 模型已压缩到约 4.7MB；页面内置**双通道加载**：GitHub 主通道 30 秒无进展会自动切换 jsDelivr 国内加速节点重试，无需手动操作；
- 若仍失败，多为网络问题：点「重新加载」，或换 WiFi / 流量、稍后再试；
- 按 F12 看 Console 是否有报错。

**模型过大怎么办（> 100 MB）**
用 [gltf-transform](https://gltf.report/)（网页版）或 Blender 导出时
勾选 **Draco 压缩**，通常能把体积压缩到 1/5~1/10。

**手机上看不到 AR 按钮**
- 微信内打开 → 点右上角「在浏览器中打开」；
- iPhone 需要先完成上方「关于 AR」的 USDZ 步骤。

---

技术栈：纯 HTML/CSS/JS + `<model-viewer>`（Google 官方 Web Component，基于 three.js），无需任何后端与密钥。
