# Shiro Core

> **淬炼设计本质，回归纯粹核心**

基于 [Innei/Shiro](https://github.com/Innei/Shiro) 提取整理的 UI 设计规范系统，源自全球 4.2k Star 极简博客框架。供 AI 辅助开发时作为 UI 参考。

[完整设计规范](./shiro-core-design-spec.md) | [HTML 展示页](./shiro-core-demo.html) | **用浏览器直接打开 `shiro-core-demo.html` 即可预览**

## 核心特性

- **极简主义** — 内容优先，大量留白
- **毛玻璃质感** — 导航栏、卡片、FAB 均使用 backdrop-blur
- **弹簧物理动画** — Spring 弹性曲线，非 CSS ease
- **暗黑模式必备** — 默认跟随系统自动切换，三态切换（浅色/系统/深色）
- **日系美学** — 柔和色彩（薄荷青/樱花粉），CJK 字体优先

## 技术栈

| 层级 | 技术 |
|---|---|
| 框架 | NextJS 16 (App Router) |
| CSS | TailwindCSS v4 + DaisyUI v5 |
| 动画 | Motion (Framer Motion) |
| 状态管理 | Jotai |
| 无障碍 | Radix UI |

## AI 使用指南

让 AI 写软件时，复制以下内容：

```
请使用以下 UI 设计规范来构建界面：

设计规范文档：https://github.com/ZXEB/shiro-core/blob/master/shiro-core-design-spec.md
HTML 展示页面：https://github.com/ZXEB/shiro-core/blob/master/shiro-core-demo.html

要求：必须实现暗黑模式，默认跟随系统自动切换。
```
