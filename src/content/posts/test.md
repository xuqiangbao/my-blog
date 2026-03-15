---
title: 我的第一篇文章
published: 2026-03-15
description: 这是我的第一篇博客
tags: [生活, 记录]
category: 日常
draft: false
author: "许襁褓"   # 添加这一行
---
# 使用 GitHub Pages 免费部署 Astro + Fuwari 博客（完整指南）

本文档将引导你**在本地电脑**完成博客的搭建、配置和发布，最终通过 GitHub Pages 免费托管，并绑定自己的域名。全程无需云服务器，告别 Nginx 配置烦恼。

## 📦 第一步：本地环境准备

### 1.1 安装 Git
- **下载**：访问 [Git 官网](https://git-scm.com/downloads)，选择对应操作系统的版本。
- **安装**：一路默认，所有选项保持默认即可。
- **验证**：打开命令行（Windows 按 `Win+R` 输入 `cmd`，Mac 打开终端），输入：
  ```bash
  git --version
应显示类似 git version 2.40.0 的版本号。

1.2 安装 Node.js（LTS 版本）
下载：访问 Node.js 官网，点击左侧绿色的 LTS 按钮下载安装包。

安装：一路默认，务必勾选“自动安装必要的工具”（如果有）。

验证：

bash
node --version   # 应显示 v20.x.x
npm --version    # 应显示 10.x.x
1.3 安装 pnpm（更快、更省空间的包管理器）
在命令行中执行：

bash
npm install -g pnpm
pnpm --version   # 验证
🚀 第二步：获取 Fuwari 博客模板
方法 A：使用 Git 克隆（推荐，方便后续更新）
bash
# 切换到你想存放博客的目录，例如桌面
cd ~/Desktop
git clone https://github.com/saicaca/fuwari.git my-blog
cd my-blog
注意：如果 git clone 因网络问题失败，请使用方法 B。

方法 B：手动下载 ZIP（网络不稳定时使用）
打开 Fuwari GitHub 仓库。

点击绿色 Code 按钮 → 选择 Download ZIP。

解压到本地文件夹（例如 ~/Desktop/my-blog）。

打开命令行，进入该文件夹：

bash
cd ~/Desktop/my-blog
🛠️ 第三步：安装依赖并本地预览
bash
# 安装项目依赖
pnpm install

# 本地预览（会启动一个开发服务器）
pnpm dev
命令行会显示 Local: http://localhost:4321/。打开浏览器访问该地址，即可看到博客初始效果。按 Ctrl+C 可停止预览。

🎨 第四步：个性化配置
4.1 修改博客基本信息
用文本编辑器（如 VS Code、记事本）打开 src/config.ts 文件，修改以下字段：

ts
title: "你的博客标题",      // 例如 "许强的博客"
subtitle: "你的副标题",     // 例如 "记录生活与技术"
lang: "zh-CN",              // 改为中文
themeColor: "blue",         // 可选颜色：blue、green、pink 等
保存文件。

4.2 清理示例文章（可选）
删除 src/content/posts/ 目录下的所有 .md 文件（这些是别人的示例文章）。

4.3 修改构建配置（为绑定自定义域名做准备）
打开 astro.config.mjs 文件，修改 site 字段：

javascript
export default defineConfig({
  site: 'https://你的用户名.github.io',  // 如果你暂不绑定域名，可先保持这样
  // 如果你有自定义域名，请改为你的域名，例如 'https://blog.xuqiangbao.xyz'
  base: '/',
  // ... 其他配置不要动
});
保存文件。

📦 第五步：构建静态网站
bash
pnpm build
构建完成后，会生成一个 dist 文件夹，里面就是你的整个博客的静态文件（后续要上传到 GitHub）。

🌐 第六步：创建 GitHub 仓库
登录 GitHub。

点击右上角 + → New repository。

Repository name 输入一个名字，例如 my-blog（注意：不要勾选 “Add a README file”、“Add .gitignore” 或 “Choose a license”，保持仓库完全为空）。

选择 Public（公开仓库，免费）。

点击 Create repository。

🔧 第七步：初始化本地 Git 并推送代码
7.1 如果之前未初始化 Git 仓库
bash
# 在 my-blog 目录下执行
git init
git add .
git commit -m "Initial commit"
7.2 设置远程仓库地址
bash
git remote add origin https://github.com/你的用户名/my-blog.git
注意：如果提示 remote origin already exists，可先执行 git remote remove origin 再重新添加。

7.3 推送代码
bash
# 如果你的本地分支名为 master，将其重命名为 main（GitHub 默认分支为 main）
git branch -M main

# 推送
git push -u origin main
网络问题处理：如果推送失败（如 Failed to connect to github.com port 443），可尝试：

配置代理（假设代理端口为 7890）：

bash
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
或者使用 SSH 方式（需先配置 SSH Key，参考 GitHub 文档）：

bash
git remote set-url origin git@github.com:你的用户名/my-blog.git
git push -u origin main
推送成功后，刷新 GitHub 仓库页面，应该能看到你的代码。

⚙️ 第八步：配置 GitHub Pages 自动部署
8.1 创建 GitHub Actions 工作流
在本地项目根目录创建文件夹 .github/workflows，然后创建文件 deploy.yml：

bash
mkdir -p .github/workflows
用文本编辑器打开 .github/workflows/deploy.yml，粘贴以下内容：

yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - uses: pnpm/action-setup@v3
        with:
          version: 8
          run_install: false
      - run: pnpm install
      - run: pnpm build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
8.2 提交并推送工作流文件
bash
git add .github
git commit -m "Add GitHub Actions workflow"
git push
8.3 在 GitHub 上启用 Pages
进入 GitHub 仓库，点击 Settings → Pages。

在 Source 下选择 GitHub Actions（通常会自动检测到）。

等待几分钟，第一次部署完成后，你会看到一条信息：Your site is live at https://你的用户名.github.io/my-blog。

此时访问该地址，即可看到你的博客！

🔗 第九步：绑定自定义域名（可选）
假设你的域名是 blog.xuqiangbao.xyz，且已解析到服务器 IP（现在需要改为 CNAME 记录）。

9.1 在 GitHub 仓库设置中添加域名
进入仓库 Settings → Pages。

在 Custom domain 输入框中输入 blog.xuqiangbao.xyz，点击 Save。

GitHub 会自动在仓库根目录创建一个 CNAME 文件（包含你的域名），请确保该文件被提交到 main 分支（工作流构建时会包含它）。

9.2 修改域名 DNS 解析
登录你的域名服务商（如阿里云、腾讯云、Cloudflare 等），为 blog.xuqiangbao.xyz 添加一条 CNAME 记录：

主机记录：blog

记录类型：CNAME

记录值：你的用户名.github.io（例如 xuqiangbao.github.io）

TTL：默认即可

如果之前有 A 记录指向服务器 IP，记得删除或暂停。

9.3 等待生效
DNS 解析可能需要几分钟到几小时生效。生效后访问 https://blog.xuqiangbao.xyz，就能看到你的博客，且 GitHub 会自动颁发 SSL 证书。

✍️ 第十步：日常更新文章
在本地 src/content/posts/ 下创建新的 Markdown 文件，文件名任意（如 hello-world.md），内容格式如下：

markdown
---
title: 我的第一篇文章
published: 2026-03-15
description: 这是我的第一篇博客
tags: [生活, 记录]
category: 日常
draft: false
---
这里是正文，用 **Markdown** 写。
可选：本地预览（检查效果）：

bash
pnpm dev
提交并推送：

bash
git add .
git commit -m "新文章：xxx"
git push
等待几分钟，GitHub Actions 会自动构建并部署，之后访问你的域名就能看到新文章。

❓ 常见问题排查
Q1：git push 时提示 remote origin already exists
bash
# 先查看当前远程地址
git remote -v
# 如果地址不对，更新为新地址
git remote set-url origin https://github.com/你的用户名/my-blog.git
# 或者删除后重新添加
git remote remove origin
git remote add origin https://github.com/你的用户名/my-blog.git
Q2：git push 时提示 Repository not found
确保你已经在 GitHub 上创建了同名的仓库（my-blog），并且远程地址中的用户名和仓库名完全正确。

Q3：推送时遇到 Failed to connect to github.com port 443
网络问题，尝试：

配置代理（见第七步）。

使用 SSH 方式（需配置 SSH Key）。

切换网络（如手机热点）重试。

Q4：GitHub Actions 部署失败
检查 deploy.yml 文件是否完整、正确。

查看 Actions 日志（在仓库的 Actions 标签页），根据错误信息调整。

Q5：自定义域名访问显示 404
确认 DNS 解析已生效（可用 ping blog.xuqiangbao.xyz 查看解析到的 IP）。

确认 GitHub 仓库的 Settings → Pages 中已正确填写自定义域名，且 CNAME 文件存在于仓库根目录。