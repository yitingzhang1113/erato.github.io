# Erato 的个人博客

这是我的个人技术博客，基于 Jekyll 和 GitHub Pages 搭建，主要分享计算机科学学习笔记、前端开发经验和人工智能探索。

---

## 技术栈

- Jekyll：静态网站生成器  
- GitHub Pages：免费托管服务  
- Sass (SCSS)：样式预处理  
- Markdown：博客内容书写格式  

---

## 本地运行

1. 安装 Bundler（如果未安装）：

gem install bundler

markdown
Copy

2. 安装依赖：

```shell
bundle install
```




3. 启动本地服务器：
```shell
bundle exec jekyll serve
```

4. 打开浏览器访问：

http://localhost:4000



---

## 构建与部署

- 构建静态文件：
```shell
bundle exec jekyll build
```


- 构建文件输出至 `_site` 目录。  
- 推送代码到 GitHub，GitHub Pages 会自动部署。

---

## Sass 注意事项

- `@import` 已废弃，建议改用 `@use` 和 `@forward`。  
- 详情见：https://sass-lang.com/documentation/at-rules/use

---

## 注意事项

- 确保 `_posts` 目录内文件名唯一，避免页面冲突。  
- 维护良好的代码结构，方便后续维护。

---

## 目录结构

├── _posts/ # 博客文章
├── _layouts/ # 布局模板
├── assets/ # 静态资源
├── _config.yml # 配置文件
├── style.scss # 样式入口
└── README.md # 本文件

yaml
Copy

---

## 联系我

- GitHub：https://github.com/yourusername  
- 博客：https://yourusername.github.io

---

**Erato**