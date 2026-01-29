# 本博客同款Gmeek静态博客
我的博客:[blog.20210701.xyz](https://blog.20210701.xyz)

前期准备
- [x] 一个Github账号
- [x] 可以正常访问到Github网页版
- _以下为非必须_
- [ ] Github手机版（如果需要方便发文，手机版可以直接用）
- [ ] Markor手机软件（类似的有很多能编辑Markdown的。可以先反复修改，完成之后再复制到Github发文）
- [ ] 自己的域名和cloudflared，如果需要自定义域名
（不自定义也行，Github.io国内访问30到60也是非常快的）

下面是官方搭建教程
## Gmeek

一个博客框架，超轻量级个人博客模板。完全基于`Github Pages` 、 `Github Issues` 和 `Github Actions`。不需要本地部署，从搭建到写作，只需要18秒，2步搭建好博客，第3步就是写作。

- [Demo页面](http://meekdai.github.io/)
- [Gmeek更新日志](https://meekdai.github.io/post/Gmeek-geng-xin-ri-zhi.html)
- [Gmeek快速上手](https://blog.meekdai.com/post/Gmeek-kuai-su-shang-shou.html)

![](https://github.com/Meekdai/Gmeek/blob/main/img%2Flight.jpg)

### 安装

1. 【创建仓库】点击[通过模板创建仓库](https://github.com/new?template_name=Gmeek-template&template_owner=Meekdai)，建议仓库名称为`XXX.github.io`，其中`XXX`为你的github用户名。

2. 【启用Pages】在仓库的`Settings`中`Pages->Build and deployment->Source`下面选择`Github Actions`。

3. 【开始写作】打开一篇issue，开始写作，并且**必须**添加一个`标签Label`（至少添加一个），再保存issue后会自动创建博客内容，片刻后可通过https://XXX.github.io 访问（可进入Actions页面查看构建进度）。

4. 【手动全局生成】这个步骤只有在修改`config.json`文件或者出现奇怪问题的时候，需要执行。

`通过Actions->build Gmeek->Run workflow->里面的按钮全局重新生成一次
`