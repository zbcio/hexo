# ZhangBaiCheng's Programming Blog

基于 [Hexo](https://hexo.io/) 与 [NexT](https://theme-next.js.org/) 主题搭建的个人技术博客。

- **博客地址**：[https://zbcio.github.io](https://zbcio.github.io)
- **自动化构建**：由 GitHub Actions 驱动，代码推送到 `master` 分支后自动编译并部署至 GitHub Pages。

---

## 环境要求

- **Node.js**：推荐 `v20.x` 或 `v22.x`（LTS 版本）
- **npm**：与 Node.js 配套版本

> 若本地使用 [nvm](https://github.com/nvm-sh/nvm)，项目根目录包含 `.nvmrc`：
> ```bash
> nvm install    # 首次使用可根据 .nvmrc 安装对应版本
> nvm use        # 切换至对应 Node 版本（若已有 Node 20/22 也可直接使用）
> ```

---

## 快速上手

### 1. 安装依赖

```bash
npm install
```

### 2. 启动本地预览

```bash
npx hexo server
```
或使用简写命令：
```bash
npx hexo s
```

启动成功后，在浏览器中打开：[http://localhost:4000/](http://localhost:4000/)

> **常用参数**：
> - 启动并自动在浏览器打开：`npx hexo s -o`
> - 指定端口（如 5000）：`npx hexo s -p 5000`

---

## 常用命令

| 命令 | 说明 |
| :--- | :--- |
| `npx hexo new post "文章标题"` | 新建博客文章，源文件位于 `source/_posts/` |
| `npx hexo clean` | 清理生成缓存文件 (`db.json`) 及 `public/` 静态目录 |
| `npx hexo generate` (或 `npx hexo g`) | 渲染并生成静态站点文件至 `public/` 目录 |
| `npx hexo server` (或 `npx hexo s`) | 启动本地开发预览服务器，支持热重载 |

---

## 目录结构概览

```text
hexo/
├── .github/workflows/ci.yml # GitHub Actions 自动化构建部署配置
├── .nvmrc                   # 推荐 Node.js 版本基准
├── _config.yml              # 站点主配置文件
├── package.json             # 项目依赖配置
├── scaffolds/               # 文章生成模板
├── source/                  # 博客内容源文件
│   ├── _posts/              # Markdown 文章正文
│   ├── categories/          # 分类页
│   ├── tags/                # 标签页
│   ├── images/              # 图片资源
│   └── robots.txt           # 搜索引擎抓取规则
└── themes/                  # 站点主题（当前启用 themes/next）
```

---

## 部署流程与 CI/CD 配置

项目配置了 GitHub Actions 自动化部署流水线：

### 1. 日常发布文章
本地撰写文章并完成调试后，提交并推送至当前仓库的 `master` 分支即可自动触发构建部署：
```bash
git add .
git commit -m "feat: 发布新文章"
git push origin master
```
GitHub Actions 将自动拉取代码、安装依赖、编译静态站点，并推送到 `zbcio.github.io` 仓库完成发布。

### 2. GitHub 部署凭证（GH_TOKEN）配置

由于工作流需要跨仓库推送到 `zbcio/zbcio.github.io`，必须在当前仓库的 **Settings -> Secrets and variables -> Actions** 中配置名为 **`GH_TOKEN`** 的 Secret。

可选择以下任一方式生成 Token：

#### 方案 A：Classic Token（推荐，最不易踩坑）
1. 访问 [Personal access tokens (classic)](https://github.com/settings/tokens)；
2. 点击 **Generate new token (classic)**；
3. **Select scopes** 中务必勾选 **`repo`**（包含所有子项，确保仓库完整读写权限）；
4. 生成后复制 Token，添加到当前仓库的 `secrets.GH_TOKEN`。

#### 方案 B：Fine-grained Token（细粒度令牌，安全性更高）
1. 访问 [Fine-grained personal access tokens](https://github.com/settings/personal-access-tokens/new)；
2. **Resource owner**：必须切换为目标仓库的归属方（如 `zbcio`）；
3. **Repository access**：选择 **Only select repositories**，搜索并选中 **`zbcio.github.io`**；
4. **Permissions**：
   - 展开 **Repository permissions** -> 找到 **Contents**，将权限调整为 **`Access: Read and write`**；
   - 确认 **Metadata** 为 `Read-only`（通常默认已勾选）；
5. 生成后若处于组织中，请确保状态非 Pending 审批中，然后将其添加到当前仓库的 `secrets.GH_TOKEN`。
