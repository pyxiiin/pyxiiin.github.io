# pyxiiin.github.io

个人技术博客，基于 [Hugo](https://gohugo.io/) + [Stack 主题](https://github.com/CaiJimmy/hugo-theme-stack)，推送到 `main` 后由 GitHub Actions 自动发布到 <https://pyxiiin.github.io/>。

## 目录说明

| 路径 | 作用 | 是否修改 |
| --- | --- | --- |
| `content/post/` | 博客文章，每篇一个文件夹 | 日常写作就在这里 |
| `content/page/` | 独立页面（搜索、归档） | 一般不动 |
| `hugo.yaml` | 站点配置（标题、菜单、侧边栏等） | 可以改 |
| `assets/` | 头像等需要主题处理的资源 | 可以加 |
| `static/` | 原样复制到网站根目录的文件（如 favicon） | 可以加 |
| `layouts/`、`i18n/`、`data/` | 覆盖主题的模板、翻译、数据 | 需要定制主题时才用 |
| `archetypes/` | 新建文章时的模板 | 可以改 |
| `themes/hugo-theme-stack/` | 主题源码（git 子模块） | 不要改 |
| `.github/workflows/` | 自动部署流程 | 不要改 |
| `public/`、`resources/` | 构建产物，已被忽略 | 不要改 |

## 写新文章

```bash
hugo new content post/<英文名>/index.md   # 新建
hugo server -D                            # 本地预览 http://localhost:1313
```

写完把 `draft` 改为 `false`。图片和 `index.md` 放在同一文件夹，用 `![说明](图片.png)` 引用。

## 发布

```bash
git add . && git commit -m "新文章：xxx" && git push
```

> 克隆仓库时需带上主题子模块：`git clone --recurse-submodules <仓库地址>`
