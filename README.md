# 我的个人博客

这是一个基于 GitHub Pages、Hexo 和 Icarus 主题的静态个人博客。

## 本地开发

```bash
npm install
npm run dev
```

默认访问地址为 `http://localhost:4000`。

## 生成静态文件

```bash
npm run build
```

生成结果位于 `public/`，该目录不会提交到 Git。

## 发布到 GitHub Pages

1. 在 GitHub 创建仓库，推荐个人站点仓库名使用 `<your-github-username>.github.io`。
2. 将 `_config.yml` 中的 `url` 改成你的 Pages 地址。
3. 推送到 `main` 分支。
4. 在仓库 `Settings -> Pages` 中将 `Build and deployment` 的 `Source` 设置为 `GitHub Actions`。

如果你使用项目站点仓库，例如 `MyBlog`，请把 `_config.yml` 中的地址改为：

```yml
url: https://your-github-username.github.io/MyBlog
root: /MyBlog/
```

## 写文章

```bash
npx hexo new "文章标题"
```

文章会生成在 `source/_posts/`。
