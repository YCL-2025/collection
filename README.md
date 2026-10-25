# 临床住院患者资料收集系统 (Cloudflare Pages + Supabase + GitHub 部署版)

本工程专为 **GitHub + Supabase + Cloudflare Pages** 静态托管与云端后端无服务器架构设计，无需自建服务器即可实现全球访问、住院号准入审批及 7 大类病历图片上传归档。

## 目录文件说明

- `index.html`：前端单页完整代码（内置 `@supabase/supabase-js` 云端数据库与 Storage 图片桶读写逻辑，同时兼容未配置时的本地演示模式）。
- `supabase_init.sql`：Supabase 数据库建表、RLS 策略及 `medical-images` 图片存储桶一键初始化 SQL。
- `_headers`：Cloudflare Pages 安全响应头配置文件。

## 三步完成上线流程

### 第一步：初始化 Supabase 云端数据库与图片存储桶
1. 登录 [Supabase Dashboard](https://supabase.com/dashboard) 创建新项目。
2. 在左侧菜单打开 **SQL Editor** -> **New query**，将 `supabase_init.sql` 的内容完整粘贴进去，点击 **Run**。
3. 在左侧菜单打开 **Project Settings** -> **API**，获取：
   - `Project URL`（形如 `https://xxxxxx.supabase.co`）
   - `anon public` Key（`eyJhbGci...` 开头的公开密钥）

### 第二步：填入密钥并上传到 GitHub 仓库
1. 打开 `index.html`（或部署后直接点击网页右上角 **「云端配置 & 部署向导」** 按钮在线填入），找到代码顶部的 `DEFAULT_SUPABASE_CONFIG`：
   ```javascript
   const DEFAULT_SUPABASE_CONFIG = {
     url: 'https://您的项目ID.supabase.co',
     anonKey: '您的_SUPABASE_ANON_PUBLIC_KEY',
     bucketName: 'medical-images'
   };
   ```
2. 在 GitHub 创建新仓库，将 `index.html`、`supabase_init.sql`、`_headers`、`README.md` 推送至仓库根目录。

### 第三步：使用 Cloudflare Pages 绑定 GitHub 自动发布
1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/) -> **Workers & Pages** -> **Create** -> **Pages** -> **Connect to Git**。
2. 选择您的 GitHub 仓库，构建参数保持默认（Framework preset: `None`，Build command 留空，Build output directory 留空或填 `/`）。
3. 点击 **Save and Deploy**，数秒后即可通过分配的 `.pages.dev` 域名或自定义域名访问。以后每次修改 GitHub 代码都会自动触发更新部署。
