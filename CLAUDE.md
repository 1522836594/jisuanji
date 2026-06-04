# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个纯静态的 HTML 单页应用——"博雅未来高中 · 艺术签名明信片"。用户输入名字后，随机选择一款中文字体生成书法签名，展示在正反面明信片上，支持下载 PNG 和打印。

## 核心文件

| 文件 | 用途 |
|------|------|
| `signature-postcard.html` | 主文件：HTML 结构 + 内嵌 CSS + 内嵌 JS（约 450 行） |
| `images.js` | 三个图片变量的 base64 Data URI（`IMG_EMBLEM`, `IMG_CAMPUS1`, `IMG_CAMPUS2`），由本地图片文件编码生成 |
| `1.png` | 校徽原始图片 |
| `微信图片_*.jpg` | 校园照片原始图片（2 张） |

## 运行方式

无需构建。直接在浏览器中打开 `signature-postcard.html` 即可：

```powershell
Start-Process "E:\AI\jisuanji\signature-postcard.html"
```

## 外部依赖（CDN）

- **Tailwind CSS** — 样式框架
- **Google Fonts** — 7 款中文书法字体（Ma Shan Zheng, Liu Jian Mao Cao, ZCOOL KuaiLe 等）
- **html2canvas** — DOM 截图生成 PNG 下载

## 关键逻辑

签名生成的核心算法在 `trace()` 函数中：用 Canvas 离屏渲染文字，逐列扫描像素获取笔画的垂直区间，转换为 SVG `<path>` 元素，从而实现文字转矢量笔画路径的效果。

`trace()` 生成的每个 `<path>` 初始 `opacity="0"`，真实透明度存储在 `data-opacity` 属性中。`animateWriting()` 函数用 `requestAnimationFrame` 按时间比例依次显示路径，模拟手写从左到右的笔顺效果。签名和名言同时书写，都完成后才启用下载/打印按钮。

`images.js` 是纯数据文件。如果需要更换图片，修改本地图片文件后重新编码为 base64 替换该文件中的三个变量即可。

## Git 远程

- 仓库地址：`https://github.com/1522836594/jisuanji`
- 推送前需确保 GitHub 认证已配置（推荐 GitHub CLI `gh auth login` 或 Git Credential Manager）
