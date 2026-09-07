# GitHub Pages 部署操作文档

本项目是无需服务器的静态网站，适合直接托管在 GitHub Pages。工程内所有站内 CSS、JS、图片和 PDF 均使用相对路径，文件名统一使用小写。

## 1. 本地预览

直接双击 `index.html` 可以查看基础页面。为了让外部链接、下载和浏览器行为更接近线上环境，建议在项目根目录启动一个静态文件服务器：

```bash
# Python 3
python -m http.server 8000
```

然后打开 `http://localhost:8000`。也可以使用 VS Code 的 Live Server 等静态服务器插件。项目不需要 `npm install` 或构建步骤；Tailwind 通过 CDN 加载，联网时样式会正常生效。

## 2. 创建 GitHub 仓库并推送

### 浏览器上传

1. 登录 GitHub，点击右上角 `+` → `New repository`。
2. 创建一个仓库，例如 `tutor-t-resume`。公开仓库最适合直接展示求职主页。
3. 不要勾选自动生成 README（本地已经有文件）。
4. 进入仓库，点击 `Add file` → `Upload files`，把整个项目文件夹中的 `index.html`、`css/`、`js/`、`images/`、`assets/` 和 `github-pages-deploy.md` 拖入，点击 `Commit changes`。

### Git 命令行

```bash
git init
git add .
git commit -m "build: add personal product manager portfolio"
git branch -M main
git remote add origin https://github.com/你的用户名/tutor-t-resume.git
git push -u origin main
```

## 3. 开启 GitHub Pages

1. 打开仓库的 `Settings`。
2. 左侧进入 `Pages`。
3. 在 `Build and deployment` 的 `Source` 选择 `Deploy from a branch`。
4. Branch 选择 `main`，目录选择 `/ (root)`，点击 `Save`。
5. 等待几十秒到几分钟，刷新 Pages 页面，GitHub 会显示访问地址。
6. 默认地址通常是 `https://你的用户名.github.io/tutor-t-resume/`。如果仓库命名为 `你的用户名.github.io`，则会变成根域名 `https://你的用户名.github.io/`。

## 4. 自定义域名（可选）

### 方案 A：默认 github.io 域名

优点是免费、配置简单、HTTPS 自动签发；缺点是地址较长，品牌记忆度有限。适合先快速上线求职主页。

### 方案 B：自定义域名

优点是更专业、便于长期维护个人品牌；缺点是需要购买域名并维护 DNS。

1. 在仓库根目录创建一个文件，文件名必须是大写 `CNAME`，内容只写你的域名，例如 `resume.example.com`，不要带 `https://`。
2. 在域名服务商的 DNS 控制台添加解析：
   - 子域名（如 `resume.example.com`）：添加 `CNAME`，主机记录填 `resume`，记录值填 `你的用户名.github.io`。
   - 根域名（如 `example.com`）：添加 GitHub Pages 要求的 A 记录，指向 GitHub 官方公布的 Pages IP；同时建议再为 `www` 配置 CNAME。
3. 回到仓库 `Settings` → `Pages` → `Custom domain` 填入域名并保存。
4. DNS 生效后，勾选 `Enforce HTTPS`。若选项暂时不可用，等待证书签发后再刷新。

## 5. 常见坑排查

- **图片 404**：检查 `src` 是否写成 `images/avatar.jpg` 这样的相对路径；不要使用 `C:\`、`D:\` 或 `file:///` 本地绝对路径。
- **路径错误**：仓库项目页通常带有 `/仓库名/` 子路径，站内资源必须使用 `css/style.css`、`js/main.js`、`images/...` 这种相对路径，避免以 `/css/...` 开头。
- **大小写敏感**：GitHub Pages 的 Linux 环境区分大小写。`Images/Avatar.JPG` 和 `images/avatar.jpg` 不是同一个文件；建议目录和文件全部使用小写。
- **强制 HTTPS**：开启 `Enforce HTTPS` 后，图片、外部作品集和表单链接也尽量使用 `https://`，否则浏览器可能拦截混合内容。
- **页面没有更新**：检查是否推送到 Pages 设置的分支，等待部署完成；必要时用无痕窗口或强制刷新清理缓存。
- **PDF 无法下载**：确认文件确实叫 `assets/resume.pdf`，并且已经提交到仓库；文件名必须和 `index.html` 中的相对路径完全一致。

## 6. 上线前清单

- 确认 `images/` 中的头像、校园照片、产品截图、金融信贷系统截图和 `picture.jpg` 均已提交；图片文件名与 `index.html` 中的相对路径保持完全一致。
- 将脱敏后的管理员端数据截图放入页面，遮盖账号、用户隐私和内部域名。
- 把 `assets/resume.pdf` 换成正式简历。
- 补充飞书文档 / Axure 原型公开链接。
- 用手机宽度检查导航、卡片和联系方式按钮。
