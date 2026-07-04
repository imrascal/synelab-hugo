# SynE Lab Website — 合成电子学实验室

> 合成电子学(Synthetic Electronics, SynE) 课题组官方网站模板
> 基于 Hugo + GitHub Pages，域名 synelab.xyz

---

## 目录

1. [前置环境安装](#一前置环境安装)
2. [项目初始化](#二项目初始化)
3. [图片资源准备](#三图片资源准备)
4. [本地预览与修改](#四本地预览与修改)
5. [GitHub 仓库创建与推送](#五github-仓库创建与推送)
6. [域名 DNS 配置](#六域名-dns-配置)
7. [GitHub Pages 启用](#七github-pages-启用)
8. [GitHub Actions 自动部署](#八github-actions-自动部署)
9. [HTTPS 证书配置](#九https-证书配置)
10. [内容管理操作](#十内容管理操作)
11. [迁移检查清单](#十一迁移检查清单)
12. [常见问题排查](#十二常见问题排查)

---

## 一、前置环境安装

### 1.1 安装 Git

**macOS：**
```bash
# 通过 Homebrew 安装
brew install git

# 验证安装
git --version
```

**Windows：**
```bash
# 下载安装程序：https://git-scm.com/download/win
# 或使用 winget
winget install --id Git.Git -e --source winget
```

**Linux (Ubuntu/Debian)：**
```bash
sudo apt update && sudo apt install git -y
git --version
```

### 1.2 安装 Hugo Extended

**macOS：**
```bash
# 通过 Homebrew 安装
brew install hugo

# 验证安装（必须包含 Extended）
hugo version
# 输出应包含：extended
```

**Windows：**
```powershell
# 通过 Chocolatey 安装
choco install hugo-extended -confirm

# 或通过 Scoop
scoop install hugo-extended

# 验证
hugo version
```

**Linux (Ubuntu/Debian)：**
```bash
# 下载最新版 Hugo Extended（替换 X.XX.X 为最新版本号）
# 查看最新版本：https://github.com/gohugoio/hugo/releases/latest
wget https://github.com/gohugoio/hugo/releases/download/v0.147.0/hugo_extended_0.147.0_linux-amd64.deb
sudo dpkg -i hugo_extended_0.147.0_linux-amd64.deb

# 验证
hugo version
```

> 注意：本模板使用了 Hugo 的资源管道（SCSS/CSS 处理），必须使用 **Hugo Extended** 版本，标准版无法编译。

---

## 二、项目初始化

### 2.1 解压项目文件

```bash
# 在桌面或工作目录下创建项目文件夹
mkdir -p ~/projects
cd ~/projects

# 解压 synelab-hugo.zip（请将 zip 文件放到当前目录）
unzip synelab-hugo.zip

# 进入项目目录
cd synelab-hugo

# 查看项目结构
ls -la
```

### 2.2 初始化 Git 仓库

```bash
# 在项目根目录下初始化 Git
cd ~/projects/synelab-hugo
git init

# 添加所有文件到暂存区
git add .

# 提交初始版本
git commit -m "Initial commit: SynE lab website template"

# 查看提交状态
git status
```

---

## 三、图片资源准备

### 3.1 从 x-mol 下载图片

```bash
# 创建图片目录
mkdir -p static/images/people

# 下载 Lab Logo
curl -o static/images/logo.png "https://xpic.x-mol.com/20210920/1632118672176.png"

# 下载 PI 头像
curl -o static/images/pi.jpg "https://xpic.x-mol.com/20210920/1632120078278.png"

# 下载首页大图
curl -o static/images/hero.jpg "https://xpic.x-mol.com/20241113/1731492964383.jpg"

# 下载研究方向图片
curl -o static/images/research-1.png "https://www.x-mol.com/showImage/showUeImage?fileSource=upload:webue/20240302/94741709308914687.png"
curl -o static/images/research-2.png "https://www.x-mol.com/showImage/showUeImage?fileSource=upload:webue/20250104/41321735976181016.png"
```

### 3.2 下载组员头像

```bash
# 博士研究生
curl -o static/images/people/qiu-yuanyuan.jpg "https://xpic.x-mol.com/20221110/1668044597852.jpg"
curl -o static/images/people/luo-qijun.jpg "https://xpic.x-mol.com/20241113/1731489643232.jpg"
curl -o static/images/people/hu-fei.jpg "https://xpic.x-mol.com/20241113/1731489986268.jpg"
curl -o static/images/people/wu-xinyan.jpg "https://xpic.x-mol.com/20241114/1731548794108.jpg"

# 硕士研究生
curl -o static/images/people/li-aoran.jpg "https://xpic.x-mol.com/20241113/1731490444180.jpg"
curl -o static/images/people/zhu-ying.jpg "https://xpic.x-mol.com/20260110/1768035785033.jpg"
curl -o static/images/people/zeng-qingsheng.jpg "https://xpic.x-mol.com/20250327/1743044183103.jpg"
curl -o static/images/people/lu-yang.jpg "https://xpic.x-mol.com/20260110/1768036212912.png"
curl -o static/images/people/huang-jingwei.jpg "https://xpic.x-mol.com/20260110/1768036412331.jpg"
curl -o static/images/people/chen-junda.jpg "https://xpic.x-mol.com/20260110/1768036022598.jpg"
curl -o static/images/people/wang-hao.jpg "https://xpic.x-mol.com/20260110/1768036632034.jpg"

# 本科生
curl -o static/images/people/ma-xiushuo.jpg "https://xpic.x-mol.com/20250327/1743044493042.jpg"
curl -o static/images/people/xia-zeyu.jpg "https://xpic.x-mol.com/20250327/1743045665843.jpg"
curl -o static/images/people/zheng-lizhong.jpg "https://xpic.x-mol.com/20250327/1743045210710.jpg"
curl -o static/images/people/zeng-yinong.jpg "https://xpic.x-mol.com/20260110/1768019349672.jpg"
```

### 3.3 提交图片

```bash
git add static/images/
git commit -m "Add images from x-mol"
```

---

## 四、本地预览与修改

### 4.1 启动本地服务器

```bash
cd ~/projects/synelab-hugo

# 启动开发服务器（含草稿内容）
hugo server -D

# 或绑定到所有网络接口（局域网内其他设备可访问）
hugo server -D --bind 0.0.0.0

# 指定端口
hugo server -D --port 8080
```

浏览器访问：**http://localhost:1313/**

> 修改任何文件后，Hugo 会自动重新编译并刷新浏览器，无需手动重启。

### 4.2 本地构建（验证无错）

```bash
# 构建网站（输出到 public/ 目录）
hugo --minify

# 查看生成的文件
ls -la public/
```

### 4.3 修改站点配置

```bash
# 编辑站点主配置（Lab 名称、联系方式等）
# 使用任意文本编辑器打开 hugo.toml

# VS Code:
code hugo.toml

# Vim:
vim hugo.toml

# Nano:
nano hugo.toml
```

需要修改的关键字段：
```toml
baseURL = "https://synelab.xyz/"        # 您的域名
[params]
  email = "jiqq@shanghaitech.edu.cn"    # 联系邮箱
  phone = "(021)-20684747"               # 联系电话
  address = "上海市浦东新区..."           # 实验室地址
```

---

## 五、GitHub 仓库创建与推送

### 5.1 在 GitHub 创建仓库

1. 打开 https://github.com/new
2. 填写信息：
   - **Repository name**: `synelab-hugo`
   - **Description**: SynE Lab Website
   - **Visibility**: Public
   - 勾选 **Add a README file**: 否（已有）
   - 勾选 **Add .gitignore**: 否（已有）
3. 点击 **Create repository**

### 5.2 推送代码到 GitHub

```bash
cd ~/projects/synelab-hugo

# 添加远程仓库（将 YOUR_USERNAME 替换为您的 GitHub 用户名）
git remote add origin https://github.com/YOUR_USERNAME/synelab-hugo.git

# 验证远程仓库
git remote -v

# 推送到 main 分支
git branch -M main
git push -u origin main

# 输入 GitHub 用户名和个人访问令牌（Token）
# 如果没有 Token，在 https://github.com/settings/tokens 创建
```

### 5.3 后续代码更新

```bash
# 修改文件后
git add .
git commit -m "Update: 描述本次修改内容"
git push origin main
```

---

## 六、域名 DNS 配置

在您的域名注册商管理后台（购买 synelab.xyz 的平台）执行以下操作。

### 6.1 添加 CNAME 记录

| 类型 | 名称（Host） | 值（Value/Points to） | TTL |
|------|-------------|---------------------|-----|
| CNAME | www | YOUR_USERNAME.github.io | 默认/自动 |
| A | @ | 185.199.108.153 | 默认/自动 |
| A | @ | 185.199.109.153 | 默认/自动 |
| A | @ | 185.199.110.153 | 默认/自动 |
| A | @ | 185.199.111.153 | 默认/自动 |

> 将 `YOUR_USERNAME` 替换为您的 GitHub 用户名。

### 6.2 验证 DNS 解析

```bash
# 等待 5-10 分钟后验证
nslookup www.synelab.xyz
nslookup synelab.xyz

# 或使用 dig（macOS/Linux）
dig www.synelab.xyz CNAME +short
dig synelab.xyz A +short
```

---

## 七、GitHub Pages 启用

### 7.1 配置 GitHub Pages

1. 打开 `https://github.com/YOUR_USERNAME/synelab-hugo/settings/pages`
2. **Build and deployment** 区域：
   - Source: **GitHub Actions**（推荐，使用自动部署工作流）
   - 如果使用手动部署，选择 **Deploy from a branch** → Branch: `gh-pages` / `/(root)`
3. **Custom domain** 区域：
   - 输入：`synelab.xyz`
   - 点击 **Save**
   - 勾选 **Enforce HTTPS**（DNS 生效后可勾选）

### 7.2 创建 GitHub Actions 工作流

在项目根目录创建文件：

```bash
mkdir -p .github/workflows
```

创建 `.github/workflows/deploy.yml`：

```yaml
name: Deploy Hugo Site to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.147.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb \
            https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb
          sudo dpkg -i ${{ runner.temp }}/hugo.deb
          hugo version

      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo --gc --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

提交工作流文件：

```bash
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Actions deploy workflow"
git push origin main
```

### 7.3 查看部署状态

1. 打开 `https://github.com/YOUR_USERNAME/synelab-hugo/actions`
2. 等待 workflow 运行完成（约 1-2 分钟）
3. 绿色勾表示部署成功

---

## 八、HTTPS 证书配置

DNS 生效且网站可访问后：

1. 打开 `https://github.com/YOUR_USERNAME/synelab-hugo/settings/pages`
2. 在 **Custom domain** 区域，勾选 **Enforce HTTPS**
3. GitHub 会自动申请 Let's Encrypt 证书，等待 1-24 小时生效

验证 HTTPS：
```bash
curl -I https://synelab.xyz
# 应返回 HTTP/2 200 且包含 strict-transport-security 头
```

---

## 九、内容管理操作

### 9.1 添加新组员

```bash
cd ~/projects/synelab-hugo

# 使用 archetype 模板创建新组员
hugo new content people/zhang-san.md

# 编辑文件
# 修改 front matter 中的 role, photo, role_label 等字段
```

或直接创建文件：
```bash
cat > content/people/zhang-san.md << 'EOF'
---
title: 张三
role: phd
photo: /images/people/zhang-san.jpg
role_label: 博士研究生
date: 2025-09-01
---

个人简介（可选）。
EOF
```

### 9.2 添加新闻

```bash
hugo new content --kind news news/new-paper-published.md
```

或直接创建：
```bash
cat > content/news/new-paper-published.md << 'EOF'
---
title: Research | 新论文发表在 Nature Chemistry
date: 2026-07-04
tag: research
---

新闻详情内容...
EOF
```

### 9.3 添加论文

编辑 `data/publications.yaml`，在 `since_2021` 列表顶部添加：

```yaml
since_2021:
  - number: 40
    title: "Your New Paper Title"
    authors: "Author A, **Qingqing Ji***, Author C"
    journal: "Nature Chemistry"
    year: "2026"
    doi: "10.1038/s41557-026-xxxxx"
```

### 9.4 提交更新

```bash
git add .
git commit -m "Add new member: 张三; Update publications"
git push origin main

# 等待 GitHub Actions 自动部署（约 1-2 分钟）
# 访问 https://synelab.xyz 查看最新内容
```

---

## 十、迁移检查清单

从 x-mol 迁移到 synelab.xyz 的完整步骤：

- [ ] **环境准备**：安装 Hugo Extended + Git
- [ ] **图片下载**：从 x-mol 下载所有图片到 `static/images/`
- [ ] **图片路径检查**：确认所有 `content/people/*.md` 中的 `photo` 路径正确
- [ ] **论文补充**：完善 `data/publications.yaml` 中 Before 2021 的 45 篇论文
- [ ] **新闻补充**：在 `content/news/` 中添加剩余新闻条目
- [ ] **本地预览**：运行 `hugo server -D`，检查所有页面正常
- [ ] **本地构建**：运行 `hugo --minify`，确认无报错
- [ ] **GitHub 仓库**：创建仓库并推送代码
- [ ] **DNS 配置**：在域名注册商添加 CNAME 和 A 记录
- [ ] **GitHub Pages**：启用 Pages 并设置 Custom domain
- [ ] **GitHub Actions**：配置自动部署工作流
- [ ] **HTTPS**：等待证书签发，勾选 Enforce HTTPS
- [ ] **x-mol 公告**：在原页面添加迁移公告，指向 synelab.xyz
- [ ] **搜索引擎**：在 Google Scholar / ResearchGate 更新个人主页链接

---

## 十一、常见问题排查

### Q1: `hugo` 命令报错 "extended version needed"

```bash
# 检查 Hugo 版本
hugo version

# 如果不是 Extended 版，重新安装：
# macOS
brew reinstall hugo

# 或手动下载对应平台的 extended 版本
# https://github.com/gohugoio/hugo/releases
```

### Q2: 本地预览时样式不生效

```bash
# 确认使用的是 Hugo Extended
hugo version | grep extended

# 如果 CSS 仍不生效，尝试清除缓存
hugo server -D --gc

# 或完全重建
rm -rf resources/
hugo server -D
```

### Q3: GitHub Actions 部署失败

```bash
# 1. 检查 hugo.toml 中的 baseURL 是否正确
# 2. 确认使用的是 Hugo Extended 版本
# 3. 检查 Actions 日志：https://github.com/YOUR_USERNAME/synelab-hugo/actions
# 4. 常见原因：CNAME 文件缺失、baseURL 配置错误
```

### Q4: 域名访问显示 404

```bash
# 1. 检查 DNS 是否生效
nslookup synelab.xyz

# 2. 检查 GitHub Pages 设置中的 Custom domain 是否填写正确
# 3. 确认 CNAME 文件存在于仓库根目录
# 4. 等待 DNS 传播（最长 24 小时）
```

### Q5: HTTPS 证书申请失败

```bash
# 1. 确保 DNS 解析已生效（通过 nslookup 验证）
# 2. 在 GitHub Pages 设置中先取消勾选 "Enforce HTTPS"
# 3. 等待 1 小时后重新勾选
# 4. 如果仍失败，删除 Custom domain 重新添加
```

### Q6: 图片加载失败

```bash
# 1. 确认图片已放入 static/images/ 目录
# 2. 确认 content 文件中的路径以 /images/ 开头（不是 static/images/）
# 3. 检查图片文件名是否一致（区分大小写）
# 4. 重新构建并部署
```

---

## 项目结构速查

```
synelab-hugo/
├── archetypes/              # 内容创建模板
│   ├── default.md
│   ├── news.md
│   └── people.md
├── assets/
│   └── css/
│       └── style.css        # 主样式（~800行，响应式）
├── content/
│   ├── _index.md            # 首页内容
│   ├── research/
│   │   └── _index.md        # 研究方向
│   ├── people/              # 组员（21个文件）
│   ├── publications/
│   │   └── _index.md        # 论文页面入口
│   ├── news/                # 新闻（10个文件）
│   ├── positions/
│   │   └── _index.md        # 招聘信息
│   └── contact/
│       └── _index.md        # 联系方式
├── data/
│   └── publications.yaml    # 论文数据（39+5+6篇）
├── i18n/
│   ├── zh.toml              # 中文翻译
│   └── en.toml              # 英文翻译
├── layouts/
│   ├── _default/            # 基础模板
│   ├── index.html           # 首页模板
│   ├── partials/            # 组件（Header/Footer）
│   └── section/             # 各页面模板
├── static/
│   └── images/              # 图片资源（需自行下载）
├── .github/
│   └── workflows/
│       └── deploy.yml       # 自动部署配置
├── CNAME                    # 域名配置
├── hugo.toml                # Hugo 主配置
└── README.md                # 本文件
```

---

## 许可证

本模板仅供合成电子学实验室使用。
