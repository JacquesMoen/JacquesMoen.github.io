# JacquesMoen.github.io

这是一个使用 **Hugo + CleanWhite + Sveltia CMS + GitHub Pages** 搭建的个人博客。构建、发布和日常写作都可以在浏览器中完成，不要求在电脑上安装 Git、Hugo、Node.js 或 Sveltia CMS。

## 常用入口

- 博客：https://jacquesmoen.github.io/
- 在线写作后台：https://jacquesmoen.github.io/admin/
- 仓库：https://github.com/JacquesMoen/JacquesMoen.github.io
- 构建与发布：https://github.com/JacquesMoen/JacquesMoen.github.io/actions

搜索按钮目前保留，但 `/search/` 只是“搜索功能暂未启用”的占位页。

## 免费与纯线上边界

当前方案使用公开 GitHub 仓库、GitHub Pages、GitHub Actions 免费额度和 `github.io` 域名，不依赖付费服务器、数据库、OAuth 服务或其他部署平台。日常使用只需要浏览器、GitHub 账号，以及用于 Sveltia CMS 登录的最小权限 Fine-grained personal access token。

GitHub 的免费额度、产品条款和服务可用性可能调整；超出免费额度、自行购买域名或接入其他付费服务不属于本方案的免费范围。仓库和发布后的网页均为公开内容，请只提交适合公开的信息，绝不要把 token、密码或其他密钥写入仓库。

## 日常发布文章

1. 打开 https://jacquesmoen.github.io/admin/。
2. 选择 GitHub token 登录，只在 Sveltia 的登录界面粘贴 token，不要写进文章或配置文件。
3. 进入“文章”，新建文章并填写标题、时间、标签、分类和正文。
4. 正文图片可直接上传；单个文件应不超过 1.5 MB。
5. 准备公开时关闭“草稿”，然后保存。
6. 打开仓库的 Actions 页面，确认最新的 **Build and deploy Hugo site** 运行成功。
7. 等待一两分钟后刷新博客。如果仍是旧内容，可进行强制刷新。

Sveltia 会把文章提交到 `content/post/`，把正文图片提交到 `static/img/uploads/`。文章 Front Matter 的 `image` 固定为不带前导斜杠的 `img/home-bg.jpg`；正文上传图片可以使用 `/img/uploads/...`，Hugo 会在构建时转换成正确的部署路径。

## 修改博客基础信息

在 GitHub 网页中编辑 `hugo.toml`，可修改：

- `title`、`SEOTitle`：博客标题；
- `description`、`slogan`：简介和口号；
- `header_image`、`image_404`：头图；
- `sidebar_avatar`：头像；
- `additional_menus`：导航菜单；
- `dark_mode_toggle`：深色模式切换。

头图位于 `static/img/home-bg.jpg`，头像位于 `static/img/avatar.png`，可在 GitHub 网页中用同名文件替换。菜单 `href` 保持站点相对路径，例如 `archive` 和 `about`，不要擅自改成带域名的硬编码地址。

建议所有配置变更都新建分支并提交 Pull Request；Actions 检查通过后再合并到 `main`。

## 更新 CleanWhite 主题

主题以 Git submodule 固定到指定 commit。更新时请让 Codex 或熟悉 Git submodule 的维护者：

1. 创建 `chore/update-cleanwhite` 分支；
2. 只更新 `themes/hugo-theme-cleanwhite` 的 gitlink；
3. 创建 Pull Request；
4. 等待 Hugo 构建、根路径/项目路径检查通过；
5. 检查首页、文章、About、Archive、搜索页、移动端和深浅色模式后再合并。

不要直接修改 submodule 内的主题文件；站点级样式放在 `static/css/custom.css`。

## 更新 Sveltia CMS

1. 先从 Sveltia CMS 官方发布渠道确认要使用的已发布版本；
2. 创建独立更新分支；
3. 同时更新 `static/admin/index.html` 中的脚本 URL 和 `static/admin/config.yml` 顶部的 schema URL；
4. 创建 Pull Request，等待 Actions 通过；
5. 在分支预览或合并前检查 `/admin/` 能正确加载，再合并。

## 回滚

- 普通配置或代码变更：在 GitHub 对相应 Pull Request 或 commit 使用 **Revert**，再等待 Actions 重新部署。
- 主题更新：创建新分支，把 gitlink 恢复到上一个确认可用的 commit，经 Pull Request 合并。
- Sveltia 更新：恢复 `static/admin/index.html` 和 `static/admin/config.yml` 到上一个可用版本，经 Pull Request 合并。
- 文章或图片误改：在 GitHub 的文件历史或 commit 历史中找到正确版本并恢复。删除公开内容后仍需注意 Git 历史可能保留旧版本；敏感信息一旦误提交，应立即撤销对应凭据，而不只是删除文件。

## 固定版本与发布机制

- Hugo：`0.164.0`
- Sveltia CMS：`0.172.4`
- CleanWhite：`f1266b575eb8627c4dc310a672a06387484c1d2e`
- 默认分支：`main`
- Pages 地址：`https://jacquesmoen.github.io/`
- Pages 基础路径：空（用户主页仓库）
- 工作流：`.github/workflows/hugo.yaml`

推送到 `main` 后，GitHub Actions 构建 Hugo、执行路径检查并把产物部署到 GitHub Pages。功能分支和 Pull Request 只执行构建与检查，不发布正式网站。

## 密钥扫描

仓库使用 `.github/workflows/secret-scan.yaml` 运行 TruffleHog OSS。

- Pull Request：扫描 PR 新增的提交；
- 推送 `main`：扫描本次推送；
- 手动运行：扫描完整仓库历史；
- 权限：仅 `contents: read`；
- 不需要 PAT、仓库 Secret、Artifact 或本地安装。

扫描失败时，不要在 Issue、PR 或聊天中复制疑似密钥。若确认泄露，应先撤销或轮换凭据，再删除文件并评估 Git 历史清理。删除文件本身不能使已经公开的凭据恢复安全。

TruffleHog 版本更新必须使用独立分支和 PR，同时更新 Action 的完整 commit SHA、`version` 输入和版本注释，并重新执行 PR、`main` 和手动全历史扫描。
