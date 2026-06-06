# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

这是一个纯静态的 HTML 单页应用——"博雅未来高中 · 艺术签名明信片"。用户输入名字后，随机选择一款中文字体生成书法签名，展示在正反面明信片上，支持下载 PNG 和打印。

设计方向为"墨韵书香"——暖色纸底、墨色文字层级、朱砂红（#c4423b）点缀、编辑级留白排版。

## 核心文件

| 文件 | 用途 |
|------|------|
| `signature-postcard.html` | 主文件：HTML 结构 + 内嵌 CSS + 内嵌 JS（约 800+ 行） |
| `images.js` | 图片变量的 base64 Data URI（`IMG_EMBLEM`, `IMG_CAMPUS1`～`IMG_CAMPUS12`），由本地图片文件编码生成 |
| `1.png` | 校徽原始图片 |
| `微信图片_*.jpg` | 校园照片原始图片（12 张） |

## 运行方式

无需构建。直接在浏览器中打开 `signature-postcard.html` 即可：

```powershell
Start-Process "E:\AI\jisuanji\signature-postcard.html"
```

## 外部依赖（CDN）

- **Google Fonts** — 6 款中文书法字体（Ma Shan Zheng, Liu Jian Mao Cao, ZCOOL KuaiLe, ZCOOL QingKe HuangYou, Zhi Mang Xing, Long Cang）+ Noto Serif SC（UI 衬线体）
- **html2canvas** — DOM 截图生成 PNG 下载

## UI 样式

全部手写 CSS，不依赖 Tailwind。CSS 变量（`:root`）定义纸色、墨色、朱砂红、阴影层级。自带约 24 个精简工具类（flex/gap/text/spacing）替代 Tailwind。

背景图使用 `object-fit: contain` 完整展示照片。正面照片全透，背面照片 28% 透明度作水印。

## 关键逻辑

签名生成的核心算法在 `trace()` 函数中：用 Canvas 离屏渲染文字，逐列扫描像素获取笔画的垂直区间，转换为 SVG `<path>` 元素，从而实现文字转矢量笔画路径的效果。

`trace()` 生成的每个 `<path>` 初始 `opacity="0"`，真实透明度存储在 `data-opacity` 属性中。`animateWriting()` 函数用 `requestAnimationFrame` 按时间比例依次显示路径，模拟手写从左到右的笔顺效果。签名和名言同时书写，都完成后才启用下载/打印按钮。

背面签名位于右下角（`bottom:30px; right:20px`），无旋转，半透明（opacity:0.65）。

`images.js` 是纯数据文件。如果需要更换图片，修改本地图片文件后重新编码为 base64 替换该文件中的对应变量即可。

## Git 远程

- 仓库地址：`https://github.com/1522836594/jisuanji`
- 推送前需确保 GitHub 认证已配置（推荐 GitHub CLI `gh auth login` 或 Git Credential Manager）
