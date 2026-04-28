# Mizuki 博客搭建指南

## 一、前期准备

**核心能力**：具备基础问题解决能力，遇到疑问优先思考、主动搜索，搜索无果可借助AI工具，避免盲目提问；能同时处理简单多任务。

### 必备工具

1.  **Git**：最常用的版本控制器，用于操作GitHub（推荐官网安装：[Git - Downloads](https://git-scm.com/downloads)）。
    *   补充：可尝试 GitHub Desktop。
2.  **Node.js**：Fuwari 博客的运行基础，必须安装（官网：[Node.js — Run JavaScript Everywhere](https://nodejs.org/)）。
3.  **GitHub账号**：用于创建代码仓库，存放Fuwari博客的所有文件（注册地址：[GitHub](https://github.com/)）。
4.  **Cloudflare账号**：用于创建Pages服务，绑定域名实现博客公开访问（注册地址：[Cloudflare](https://dash.cloudflare.com/sign-up)）。
5.  **黑曜石（Obsidian）**：可视化Markdown编辑器，用于编写博客文章（Fuwari文章均为Markdown格式，官网：[Obsidian](https://obsidian.md/)）。
6.  **基础技能**：掌握Markdown基础语法（用于编写博客文章），不会可参考：[Markdown 基本语法 | Markdown 官方教程](https://markdown.com.cn/)。

## 二、本地部署Mizuki

1.  **Fork仓库**：访问 [Mizuki 官方仓库](https://github.com/matsuzaka-yuki/Mizuki)，点击右上角「Fork」，将仓库复制到自己的GitHub账号下。
2.  **克隆仓库到本地**：
    *   打开Git终端，输入命令：`git clone <你的仓库URL>`（推荐使用SSH链接，无需科学上网即可推送更改）。
    *   提示：仓库URL可在自己Fork的仓库页面，点击「Code」获取。
3.  **安装依赖工具**：
    *   全局安装pnpm（包管理工具）：输入命令
     ```bash
     npm install -g pnpm
     ```
    *   补充：若npm国内下载速度过慢，可替换为cnpm（镜像），参考：[npmmirror 镜像站](https://npmmirror.com/)。
4.  **安装项目依赖**：进入克隆后的项目根目录，依次输入以下两条命令：
    ```bash
    pnpm install
    pnpm add sharp
    ```

✅ **至此，Fuwari 已成功在本地部署完成，可开始后续个性化配置。**

## 三、个性化配置

### 3.1 修改博客核心信息

在项目根目录的 `src` 文件夹中，找到`config.ts` 文件（用Obsidian或任意文本编辑器打开），按以下说明修改：

*   `title`：填写你的博客主标题（必填）。
*   `subtitle`：填写博客副标题（可选，首页显示为「主标题 - 副标题」）。
*   `lang`：设置博客显示语言，注释中已标注常用值（如：`en`、`zh_CN`、`zh_TW`、`ja`、`ko`）。
*   `themeColor`：设置博客主题色（填写hue值，可通过博客右上角「画板图标」预览喜欢的颜色后填写）。
*   `banner`：设置首页横幅图片，`src` 后填写图片的http/https链接。
*   `favicon`：设置网站图标，`src` 后填写图标的http/https链接。
*   `links`：配置友情链接（会显示在博客导航栏）。
*   `avatar`：上传你的个人头像，填写头像的http/https链接。
*   `name`：填写你的名字（将显示在头像下方）。
*   `bio`：填写个性签名（显示在头像和名字下方）。
*   `NavBarConfig`：配置导航栏的超链接（如首页、文章列表等）。
*   `ProfileConfig`：配置你的个人社交链接（如GitHub、公众号等）。

### 3.2 清理冗余文件

项目根目录的 `src/content/posts` 文件夹中，会有默认的示例文章（用于介绍Markdown语法和Fuwari使用技巧）。

**建议**：将这些示例文章保存到本地其他文件夹（便于后续参考），然后删除 `posts` 文件夹中的示例文件，避免影响博客整洁度。

## 四、部署上线（Cloudflare Pages）

完成本地配置和文章编写后，通过Cloudflare Pages部署博客，实现公开访问，步骤如下：

1.  登录你的Cloudflare账号，进入「Workers 和 Pages」页面。
2.  点击「创建项目」→「Pages」→「连接Git存储库」。
3.  授权连接你的GitHub账号，选择你Fork的Fuwari仓库。
4.  配置构建参数（关键！）：
    *   构建命令：填写 `pnpm build`
    ```bash
    pnpm build
    ```
    *   构建输出目录：填写 `dist`
5.  点击「部署」，等待几分钟，Cloudflare会自动完成构建和部署。
    *   其他可以静态托管的平台也是如此。

**后续更新**：只需在本地用Obsidian编写Markdown文章（保存到 `src/content/posts` 文件夹），然后通过Git推送到GitHub仓库，Cloudflare会自动检测更改并重新部署，无需手动操作

**本地预览，然后发布到Github**
当你认为你的文章已经写得差不多时，想要看看效果？请到项目根目录执行：`pnpm dev`，稍等片刻，你就可以本地预览你的博客啦 [http://localhost:4321/](http://localhost:4321/)