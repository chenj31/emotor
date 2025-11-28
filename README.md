# eMotor.top 网站

这是基于 [Hugo](https://gohugo.io/) 静态网站生成器构建的网站，使用了 [FixIt](https://github.com/hugo-fixit/FixIt) 主题。

## 网站信息

- 网站名称：eMotor.top
- 基础URL：https://www.eMotor.top
- 构建工具：Hugo v0.152.2
- 主题：FixIt v0.4.0-alpha.3

## 内容概述

该网站是一个技术博客，目前包含以下内容：
- Web 开发相关文章
- 建站经验分享

最新文章是 "[My First Post](./posts/583bc6c/)"，发布于 2025年11月28日。

## 技术特点

- 响应式设计，支持桌面和移动设备
- 支持暗色/亮色主题切换
- 包含文章分类和标签功能
- 支持文章归档
- SEO 友好

## 目录结构

```
├── archives/          # 文章归档页面
├── categories/        # 分类页面
├── posts/             # 文章内容
├── tags/              # 标签页面
├── index.html         # 首页
├── index.xml          # RSS 订阅
└── sitemap.xml        # 站点地图
```

## 构建说明

要重新构建此网站，需要安装 Hugo：

```bash
# 安装 Hugo（以 Windows 为例）
winget install Hugo.Hugo.Extended

# 在项目根目录执行构建命令
hugo

# 构建后的静态文件将输出到 public/ 目录
```

## 部署方式

该网站可以部署到任何支持静态文件托管的服务上，如：
- GitHub Pages
- Netlify
- Vercel
- 云服务器等

## 许可证

内容版权归 eMotor.top 所有，除非另有说明。