# 🚀 我的 Astro 博客

这是一个基于 [Astro](https://astro.build/) 构建的个人博客，拥有完全的自定义自由度！

## ✨ 功能特性

- 🎨 **完全自定义** - 可以添加任何CSS样式和动画效果
- 🎵 **音效支持** - 使用Web Audio API或音频文件
- 🖼️ **背景图片** - 支持全屏背景、视差滚动等效果
- ⚡ **高性能** - Astro的静态生成确保极快的加载速度
- 📱 **响应式设计** - 完美适配各种设备
- 🔧 **易于扩展** - 可以集成React、Vue等框架

## 🎯 快速开始

### 安装依赖

```bash
npm install
```

### 本地开发

```bash
npm run dev
```

访问 `http://localhost:4321` 查看你的博客

### 构建生产版本

```bash
npm run build
```

### 预览构建结果

```bash
npm run preview
```

## 📁 项目结构

```
/
├── public/              # 静态资源（图片、音频、字体等）
├── src/
│   ├── components/      # Astro组件
│   ├── content/         # 博客文章（Markdown/MDX）
│   ├── layouts/         # 页面布局
│   ├── pages/           # 页面路由
│   │   ├── index.astro  # 首页
│   │   └── demo.astro   # 功能演示页面
│   └── styles/          # 全局样式
├── astro.config.mjs     # Astro配置
└── package.json
```

## 📚 如何添加功能

### 添加背景图片

在任何 `.astro` 文件中：

```astro
<style>
.hero {
  background-image: url('/your-image.jpg');
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
}
</style>
```

### 添加音效

```astro
<script>
// 方法1: 使用音频文件
const audio = new Audio('/sounds/click.mp3');
audio.play();

// 方法2: 使用Web Audio API生成音效
const audioContext = new AudioContext();
const oscillator = audioContext.createOscillator();
oscillator.connect(audioContext.destination);
oscillator.start();
</script>
```

### 添加CSS动画

```astro
<style>
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.animated-element {
  animation: fadeIn 1s ease-out;
}
</style>
```

### 添加交互效果

```astro
<script>
document.querySelector('.button').addEventListener('click', () => {
  // 你的交互逻辑
  console.log('按钮被点击了！');
});
</script>
```

## 🎨 自定义样式

### 修改主题颜色

编辑 `src/styles/global.css` 或在组件中添加样式。

### 添加自定义字体

1. 将字体文件放在 `public/fonts/` 目录
2. 在CSS中引用：

```css
@font-face {
  font-family: 'MyFont';
  src: url('/fonts/myfont.woff2') format('woff2');
}

body {
  font-family: 'MyFont', sans-serif;
}
```

## 📝 写博客文章

在 `src/content/blog/` 目录创建新的 `.md` 或 `.mdx` 文件：

```markdown
---
title: '我的第一篇文章'
description: '这是文章描述'
pubDate: 'May 08 2026'
heroImage: '/blog-placeholder-1.jpg'
---

这里是文章内容...
```

## 🚀 部署到 GitHub Pages

1. 确保你的仓库名为 `username.github.io`
2. 推送代码到 `main` 分支
3. GitHub Actions 会自动构建和部署
4. 访问 `https://username.github.io` 查看你的博客

### 首次部署设置

1. 进入仓库的 Settings > Pages
2. Source 选择 "GitHub Actions"
3. 推送代码后会自动部署

## 🔧 高级功能

### 集成 React 组件

```bash
npx astro add react
```

### 集成 Tailwind CSS

```bash
npx astro add tailwind
```

### 集成 Vue 组件

```bash
npx astro add vue
```

## 📖 学习资源

- [Astro 官方文档](https://docs.astro.build/)
- [Astro 示例](https://astro.build/themes/)
- [MDN Web 文档](https://developer.mozilla.org/)

## 🎉 示例页面

访问 `/demo` 页面查看：
- 全屏背景图片效果
- 音效交互
- CSS动画
- 粒子特效
- 悬停效果

## 💡 提示

- 所有静态资源放在 `public/` 目录
- 图片可以使用 Unsplash 等免费图库
- 音效可以使用 Web Audio API 生成或使用 MP3 文件
- CSS 动画性能优于 JavaScript 动画
- 使用 `backdrop-filter` 创建毛玻璃效果

## 📄 许可证

MIT License
