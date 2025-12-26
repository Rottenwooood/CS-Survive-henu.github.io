# CLAUDE.md

本文档为 Claude Code (claude.ai/code) 在此代码库中工作时提供指导。

## 项目概述

这是一个基于 VitePress 的文档网站，面向**河南大学计算机生存指北**（Henan University Computer Science Survival Guide）。该项目为河南大学计算机科学专业学生提供开源的生存指南和学习资源。网站部署在 GitHub Pages 上，并在推送到 main 分支时自动构建。

**在线站点**: https://cs-survive-henu.github.io

## 常用命令

### 开发
```bash
# 安装依赖（需要 Node.js 18+）
npm install

# 启动本地开发服务器 (http://localhost:5173)
npm run dev

# 构建静态站点
npm run build

# 本地预览构建结果 (http://localhost:4173)
npm run preview
```

### 部署
- **自动部署**: 推送到 `main` 分支会触发 GitHub Actions 部署
- **手动部署**: 使用便捷脚本:
  - `deploy-vitepress.bat dev` - 启动开发服务器
  - `deploy-vitepress.bat build` - 构建站点
  - `deploy-vitepress.bat preview` - 预览构建结果
  - `deploy-vitepress.bat deploy` - 部署到 GitHub Pages

### 遗留脚本（来自 Jekyll 迁移）
- `./deploy.sh install` - 安装依赖
- `./deploy.sh dev` - 启动开发服务器 (Jekyll)
- `./deploy.sh build` - 构建站点 (Jekyll)
- `./deploy.sh deploy` - 部署 (Jekyll)

## 架构与结构

### 核心技术
- **VitePress** - 静态站点生成器（Vue 3 + Vite）
- **Node.js 18+** - 所需运行时环境
- **KaTeX** - LaTeX 数学公式渲染
- **markdown-it-mathjax3** - Markdown 数学插件
- **GitHub Actions** - CI/CD 部署

### 目录结构
```
CS-Survive-henu.github.io/
├── docs/                          # 主内容目录
│   ├── .vitepress/
│   │   ├── config.ts             # VitePress 配置
│   │   └── dist/                 # 构建输出（生成）
│   ├── public/                   # 静态资源（图片等）
│   ├── *.md                      # 内容页面
│   ├── LaTeX公式使用指南.md       # LaTeX 使用指南
│   └── 数学公式测试.md            # 数学公式测试页面
├── .github/workflows/
│   └── vitepress-deploy.yml     # GitHub Actions 部署
├── package.json                  # 依赖和脚本
├── theme.yml                     # Jekyll 主题配置（遗留）
└── index.md                      # 站点首页
```

### 内容组织

主内容位于 `docs/*.md` 文件中：

**主要章节**（在导航中引用）：
- `序.md` - 前言
- `新生工具箱.md` - 新生工具箱
- `AI方向.md` - AI 方向指南
- `竞赛指北.md` - 竞赛指南
- `Linux基础.md` - Linux 基础
- `升学（保研，考研，or出国）.md` - 升学指南
- `实验室.md` - 实验室指南
- `学会提问.md` - 如何提问
- `如何使用AI.md` - 如何使用 AI
- `试着去成为一个有梦想的CS人.md` - 成为一个有梦想的 CS 人

**特殊章节**:
- `LaTeX公式使用指南.md` - 数学符号指南
- `数学公式测试.md` - 公式渲染测试页面
- `Wiki史.md` - 项目历史

## 关键配置

### VitePress 配置 (`docs/.vitepress/config.ts`)
- 基础路径: `/` (根目录)
- 语言: `zh-CN` (简体中文)
- 主题: 使用河南大学颜色的自定义主题
- 数学渲染: 通过 KaTeX 启用
- 导航: 在 `nav` 和 `sidebar` 部分配置
- 搜索: 启用本地提供者
- 编辑链接: 指向 GitHub 仓库

### GitHub Actions 工作流
- **触发条件**: 推送到 `main` 分支
- **Node 版本**: 20
- **构建步骤**:
  1. 完整历史检出
  2. 设置 Node.js 20 和 npm 缓存
  3. 安装依赖（带回退策略）
  4. 构建 VitePress 站点
  5. 创建 `.nojekyll` 文件（用于 GitHub Pages）
  6. 复制公共资源和图标
  7. 部署到 GitHub Pages

### LaTeX 数学支持
网站支持使用 LaTeX 数学公式：
- **行内公式**: `$E = mc^2$`
- **块级公式**: `$$\int_{a}^{b} f(x) dx$$`
- **完整 KaTeX 支持** 包括矩阵、方程、希腊字母

## 重要注意事项

### 部署
- 站点在推送到 `main` 时自动部署到 GitHub Pages
- GitHub Pages 配置为使用 GitHub Actions（而非 Pages 服务）
- 工作流创建 `.nojekyll` 以防止 Jekyll 处理 VitePress 输出
- 部署通常在推送后 1-2 分钟内完成

### 内容指南
- 所有内容均为**简体中文**
- 使用带有 VitePress 增强功能的 Markdown
- 支持并鼓励在技术内容中使用 LaTeX 数学
- 图片应放置在 `docs/public/` 中，并使用 `/image-name.jpg` 引用
- 文件名应具有描述性，适当使用中文字符

### 构建配置
- **构建输出**: `docs/.vitepress/dist/`（git 忽略）
- **基础 URL**: `/`（根域名）
- **简洁 URL**: 启用（无 `.html` 扩展名）
- **最后更新**: 通过 git 历史记录跟踪

### 浏览器兼容性
站点使用现代 Web 标准（ES2020+）。如果要添加新功能：
- 在最新的 Chrome/Firefox/Safari 中测试
- 为较旧浏览器考虑渐进增强
- 使用 CSS Grid 和 Flexbox 进行布局

## 添加新内容

### 新页面
1. 在 `docs/` 中创建新的 `.md` 文件
2. 在 `docs/.vitepress/config.ts` → `themeConfig.nav` 或 `themeConfig.sidebar` 中添加导航条目
3. 使用适当的前言开始内容（VitePress 支持 YAML 前言）
4. 使用 `npm run dev` 在本地测试

### 图片和资源
- 放置在 `docs/public/`
- 使用绝对路径引用: `/image-name.jpg`
- 添加前优化图片（尽可能使用 WebP）

### 数学公式
- 使用 `$...$` 表示行内数学
- 使用 `$$...$$` 表示块级数学
- 参考 `docs/LaTeX公式使用指南.md` 了解语法示例

## 遗留迁移说明

本项目从 Jekyll 迁移到 VitePress。遗留的 Jekyll 文件保留作为参考：
- `theme.yml`、`Gemfile`、`_config.yml`（在 VitePress 中不使用）
- 旧部署脚本（`deploy.sh`、`deploy.bat`）仍然存在，但首选 VitePress 脚本
- 迁移过程记录在 `VITEPRESS_MIGRATION_COMPLETE.md` 中

## 故障排除

### 构建问题
- 确保安装了 Node.js 18+
- 删除 `node_modules` 和 `package-lock.json`，然后运行 `npm install`
- 检查 Markdown 语法错误
- 验证 `docs/.vitepress/config.ts` 语法

### 部署问题
- 检查仓库中的 GitHub Actions 日志
- 确保 `.github/workflows/vitepress-deploy.yml` 具有正确权限
- 验证 GitHub Pages 设置为 "GitHub Actions" 源（而非 Pages 服务）

### 本地开发问题
- 如果端口 5173 被占用，VitePress 将自动使用下一个可用端口
- 使用 `npm run preview` 在本地测试生产构建
- 如果样式不更新，清除浏览器缓存

## 仓库信息

- **仓库**: https://github.com/CS-Survive-henu/CS-Survive-henu.github.io
- **许可证**: MIT
- **维护者**: 河南大学计算机科学与软件工程专业学生/校友
- **主要语言**: 中文（简体）
- **网站**: https://cs-survive-henu.github.io
