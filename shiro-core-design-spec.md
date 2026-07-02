# Shiro Core Design Specification

> **Shiro Core** — "淬炼设计本质，回归纯粹核心"  
> 一个极简主义的个人网站主题，基于 TailwindCSS v4 + DaisyUI v5 + Motion 构建。  
> 原项目: [github.com/Innei/Shiro](https://github.com/Innei/Shiro)

---

## 1. 设计哲学

| 原则 | 说明 |
|---|---|
| **极简主义** | 内容优先，大量留白，去除多余装饰 |
| **毛玻璃质感** | 导航栏、卡片、FAB 均使用 `backdrop-blur` + 半透明背景 |
| **日系美学** | 柔和色彩（薄荷青 / 樱花粉），CJK 字体优先 |
| **弹簧物理动画** | 所有动效使用 Spring 物理曲线，非 CSS ease |
| **微噪纹理** | `html.noise` 类添加伪元素噪点叠加层（浅色 4%，深色 1%） |
| **💡 暗黑模式必备** | **使用本主题必须实现暗黑模式，默认跟随系统自动切换** |

---

## 2. 色彩系统

### 2.1 浅色主题 (Light Theme)

| Token | 色值 | 用途 |
|---|---|---|
| `--color-primary` | `#33a6b8` | 主色（薄荷青） |
| `--color-accent` | `#33a6b8` | 强调色（同主色） |
| `--color-base-100` | `#ffffff` | 基础表面 |
| `--color-base-content` | `#000000` | 正文颜色 |
| `--color-info` | `#007aff` | 信息 |
| `--color-success` | `#34c759` | 成功 |
| `--color-warning` | `#ff9500` | 警告 |
| `--color-error` | `#ff3b30` | 错误 |
| `--bg-opacity` | `rgba(255,255,255,0.72)` | 毛玻璃背景 |
| `--color-border` | `rgba(24,24,27,0.1)` | 边框 |

### 2.2 深色主题 (Dark Theme)

| Token | 色值 | 用途 |
|---|---|---|
| `--color-primary` | `#f596aa` | 主色（樱花粉） |
| `--color-accent` | `#f596aa` | 强调色（同主色） |
| `--color-base-100` | `#1c1c1e` | 基础表面 |
| `--color-base-content` | `#ffffff` | 正文颜色 |
| `--color-info` | `#0a84ff` | 信息 |
| `--color-success` | `#30d158` | 成功 |
| `--color-warning` | `#ff9f0a` | 警告 |
| `--color-error` | `#ff453a` | 错误 |
| `--bg-opacity` | `rgba(29,29,31,0.72)` | 毛玻璃背景 |
| `--color-border` | `#3f3f46` | 边框 |

### 2.3 动态强调色池

每次会话随机选取一个强调色：

| 浅色模式 | 深色模式 |
|---|---|
| `#33A6B8` (默认薄荷) | `#F596AA` (默认樱花) |
| `#FF6666` (珊瑚) | `#A0A7D4` (薰衣草) |
| `#26A69A` (深青) | `#ff7b7b` (鲑鱼) |
| `#fb7287` (粉色) | `#99D8CF` (薄荷) |
| `#69a6cc` (天蓝) | `#838BC6` (长春花) |

### 2.4 Muted 灰度色阶

```
50:  #fafafa      100: #f5f5f5      200: #e5e5e5
300: #d4d4d4      400: #a3a3a3      500: #737373
600: #525252      700: #404040      800: #262626
900: #171717      950: #0a0a0a
```

---

## 3. 字体排版

### 3.1 字体栈

**Sans（无衬线，默认正文）:**
```
var(--app-font-sans), system-ui, -apple-system,
'PingFang SC', 'Microsoft YaHei', 'Segoe UI',
Roboto, Helvetica, 'Hiragino Sans GB', sans-serif
```

**Serif（衬线，文章正文）:**
```
'Noto Serif CJK SC', 'Noto Serif SC',
'Source Han Serif SC', 'Source Han Serif',
SongTi SC, SimSun, 'Hiragino Sans GB', serif
```

**Mono（等宽，代码）:**
```
'OperatorMonoSSmLig Nerd Font', 'Cascadia Code PL',
'FantasqueSansMono Nerd Font', JetBrainsMono,
'Fira Code', Consolas, Monaco, monospace
```

### 3.2 Google Fonts

- **Sans 主字体:** Manrope (300, 400, 500) → `--app-font-sans`
- **Serif 主字体:** Noto Serif SC (400) → `--app-font-serif`

### 3.3 字号层级

| 层级 | 规格 | 用途 |
|---|---|---|
| `html` 基准 | `14px` / `1.5` | 全局基准 |
| Hero 标题 | `text-3xl` (28px) | 首页大标题 |
| 文章标题 | `text-2xl` / `font-medium` | 文章列表标题 |
| 文章摘要 | `text-sm` / `leading-relaxed` | 文章卡片摘要 |
| 正文 | `text-zinc-800/90` | 文章正文 |
| 小字 | `text-xs` | 标签、元数据 |
| Prose | `1.1rem` (~15.4px) | 文章内容区 |

---

## 4. 布局与网格

### 4.1 页面整体结构

```
┌──────────────────────────────────────────────────────────┐
│  Header (fixed, h-4.5rem, z-9, backdrop-blur-md)         │
│  ┌──────────────┬──────────────────────┬──────────────┐  │
│  │ Logo (左)    │   Nav Pill (中)      │  Actions (右) │  │
│  │              │ [文章][思考][时间线] │              │  │
│  └──────────────┴──────────────────────┴──────────────┘  │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  main (fill-content, z-1, pt-[4.5rem])                   │
│  ┌────────────────────────────────────────────────────┐  │
│  │                                                    │  │
│  │  [Hero 区域 - 两栏布局]                             │  │
│  │  ┌──────────────────┐  ┌──────────────────┐       │  │
│  │  │ 文字介绍 (左1/2) │  │ 头像 (右1/2)     │       │  │
│  │  │ max-w-2xl        │  │ size-[200/300px] │       │  │
│  │  └──────────────────┘  └──────────────────┘       │  │
│  │                                                    │  │
│  │  [文章列表 - 居中单栏]                              │  │
│  │  ┌──────────────────────────────────┐              │  │
│  │  │         max-w-3xl (768px)        │              │  │
│  │  │  文章卡片 x N (py-8 each)        │              │  │
│  │  └──────────────────────────────────┘              │  │
│  │                                                    │  │
│  └────────────────────────────────────────────────────┘  │
│                                                          │
├──────────────────────────────────────────────────────────┤
│  Footer (mt-32, py-6, border-t)                          │
│  ┌───────┬───────┬───────┬───────┐                      │
│  │ Links │ Links │ Links │ Copy  │                      │
│  └───────┴───────┴───────┴───────┘                      │
└──────────────────────────────────────────────────────────┘
│                                                          │
│  FAB (fixed bottom-right) ───► [↑] [🔍]                 │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### 4.2 关键尺寸

| 元素 | 值 |
|---|---|
| 基准字号 | `14px` |
| Header 高度 | `4.5rem` (72px) |
| 内容最大宽（文章） | `max-w-3xl` (768px) |
| 内容最大宽（大屏） | `2xl:max-w-4xl` (896px) |
| Hero/全宽内容 | `max-w-7xl` (1280px) / `max-w-[1800px]` |
| 内容水平内边距 | `px-4 md:px-0` |
| 内容最小高度 | `calc(100vh - 17.5rem)` |
| Hero 区域 | `mt-20 lg:mt-[-4.5rem] lg:h-dvh lg:min-h-[800px]` |
| Footer 上边距 | `mt-32` |
| FAB 位置 | `bottom-[calc(2rem+env(safe-area-inset-bottom))]` |

### 4.3 Nav Pill 布局细节

```
┌─────────────────────────────────────────────────────┐
│     ┌──────────────────────────────────────────┐    │
│     │  [ 文章 ]  [ 思考 ]  [ 时间线 ]  [ 关于 ] │    │
│     │     ▲                                     │    │
│     │  text-accent (active)                     │    │
│     │  ───────────────────── (渐变下划线)        │    │
│     └──────────────────────────────────────────┘    │
│     bg-gradient-to-b from-zinc-50/70 to-white/90    │
│     shadow-lg shadow-zinc-800/5 backdrop-blur-md    │
│     ring-1 ring-zinc-900/5                          │
│     rounded-full                                    │
└─────────────────────────────────────────────────────┘
```

### 4.4 Hero 两栏布局

```
max-w-[1800px], flex-col lg:flex-row
┌───────────────────────┬───────────────────────┐
│ Left Column (1/2)     │ Right Column (1/2)    │
│                       │                       │
│ ┌─────────────────┐   │                ┌────┐ │
│ │ Title (text-3xl) │   │                │    │ │
│ │ Subtitle         │   │                │ 🖼 │ │
│ │ Description      │   │                │    │ │
│ │ Social Links     │   │                └────┘ │
│ └─────────────────┘   │           Avatar      │
│ max-w-2xl             │  size-[200px] lg:[300]│
│ lg:h-[15rem]/h-1/2    │  rounded-full         │
└───────────────────────┴───────────────────────┘
                          ↓
                    [⇓ 滚动指示]
                   animate-bounce
```

---

## 5. 组件规范

### 5.1 按钮

#### Primary Button

```css
/* 样式 */
bg-accent text-zinc-100
font-semibold
rounded-lg py-2 px-3 text-sm
hover:contrast-[1.10] active:contrast-125
disabled:bg-accent/40 disabled:opacity-80
dark:text-neutral-800

/* 交互 (Motion) */
whileFocus: { scale: 1.02 }
whileHover: { scale: 1.02 }
whileTap: { scale: 0.95 }
```

```
┌──────────────┐
│  按 钮 文 字  │  ← rounded-lg, bg-accent, text-sm
└──────────────┘
  hover → contrast(1.10)
  tap  → scale(0.95), contrast(1.25)
```

#### Secondary Button

```css
rounded-full
bg-gradient-to-b from-zinc-50/50 to-white/90
shadow-lg shadow-zinc-800/5 ring-1 ring-zinc-900/5 backdrop-blur
dark:from-zinc-900/50 dark:to-zinc-800/90 dark:ring-white/10 dark:hover:ring-white/20
```

#### 图标圆形按钮

```css
rounded-full bg-accent p-2
hover:opacity-90
size-10 centered
```

### 5.2 文章卡片

```
┌─────────────────────────────────────────────────────────┐
│  before:-inset-x-6 (hover 区域扩展)                      │
│                                                         │
│  ┌──────────────────────────────────┐  ┌────────────┐  │
│  │                                 │  │            │  │
│  │  文章标题 (text-2xl font-medium) │  │  封面图片   │  │
│  │                                 │  │  size-[5.5  │  │
│  │  📅 日期  📂 分类  👁 阅读       │  │  rem]       │  │
│  │                                 │  │  rounded-md │  │
│  │  ┌──────────────────────────┐   │  │            │  │
│  │  │ 摘要文本 (text-sm,        │   │  └────────────┘  │
│  │  │ leading-relaxed,         │   │                  │
│  │  │ ring-1 ring-accent/10)   │   │                  │
│  │  └──────────────────────────┘   │                  │
│  │                                 │                  │
│  │  [阅读更多 →] (text-accent)     │                  │
│  └──────────────────────────────────┘                  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

```css
/* 卡片容器 */
relative flex flex-col py-8 before:-inset-x-6 cursor-pointer

/* 标题 */
text-2xl font-medium

/* 正文预览 */
leading-loose text-zinc-800/90 dark:text-zinc-200/90

/* 摘要框 */
rounded-md px-4 py-2 text-sm leading-relaxed
text-zinc-900 ring-1 ring-accent/10

/* 封面图 */
size-[5.5rem] rounded-md

/* 卡片阴影 */
card-shadow: 0 0 0 1px rgba(0,0,0,0.08), 0 4px 6px rgba(0,0,0,0.04)
card-shadow:hover: 0 0 0 1px rgba(0,0,0,0.08), 0 6px 14px rgba(0,0,0,0.08)

/* 完美阴影 (perfect-md) */
box-shadow:
  0 0 0 1px var(--base),
  0 2px 2px -1px var(--shade),
  0 6px 6px -3px var(--shade),
  0 12px 12px -6px var(--shade),
  0 24px 24px -12px var(--base),
  0 48px 48px -24px var(--base)
```

### 5.3 导航栏 Header

```css
/* Header 容器 */
fixed top-0 z-[9] h-[4.5rem]
max-w-7xl
grid-cols-[4.5rem_auto_4.5rem]  /* 三列网格: Logo | Nav | Actions */

/* Nav Pill */
rounded-full
bg-gradient-to-b from-zinc-50/70 to-white/90
shadow-lg shadow-zinc-800/5 ring-1 ring-zinc-900/5 backdrop-blur-md
dark:from-zinc-900/70 dark:to-zinc-800/90 dark:ring-zinc-100/10

/* Spotlight 效果 (跟随鼠标) */
--spotlight-color: oklch(from var(--color-accent) l c h / 0.12)
/* 鼠标悬停时，径向渐变从鼠标位置辐射 */

/* Nav 项 */
px-4 py-2 font-medium
text-accent (active 时)

/* Active 指示器 */
bg-gradient-to-r from-accent/0 via-accent/70 to-accent/0
/* 底部渐变下划线，仅 active 项显示 */
```

### 5.4 页脚 Footer

```
┌─────────────────────────────────────────────────────────────┐
│ border-t bg-[var(--footer-bg)] py-6 text-base-content/80    │
│                                                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ Links    │  │ Links    │  │ Social   │  │ © Name   │   │
│  │ · Link1  │  │ · Link2  │  │ · GitHub │  │ RSS      │   │
│  │ · Link1  │  │ · Link2  │  │ · Twtr   │  │ Sitemap  │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│                                                             │
│  [🌐 EN]  ← Locale Switcher                                 │
│  [☀ ○ ☾] ← Theme Switcher                                  │
└─────────────────────────────────────────────────────────────┘
```

```css
/* Footer 背景 */
/* 通过 chroma-js 动态计算: 强调色 + 基础背景混合 */
/* 浅色: accent 5% + white 95% */
/* 深色: accent 12% + dark 88% */

/* 样式 */
mt-32 border-t py-6 text-base-content/80
```

### 5.5 主题切换器

```
┌───────────────────────────────────┐
│  rounded-full border              │
│  ┌─────┬─────┬─────┐             │
│  │ ☀️  │  🖥  │  🌙  │             │
│  │     │ ████│     │ ← Active 指示器 (滑动) │
│  └─────┴─────┴─────┘             │
│  border-zinc-200 dark:border-zinc-700  │
└───────────────────────────────────┘
```

```css
inline-flex rounded-full border border-zinc-200 p-[3px] dark:border-zinc-700

/* 按钮 */
size-8 (32x32px)
inline-flex items-center justify-center

/* Active 指示器 */
rounded-full bg-base-100 shadow
/* left 位置随 active tab 动画过渡 */
transition-all duration-200

/* SVG 图标 */
strokeWidth: 1.5, 24x24 viewBox
```

### 5.6 FAB 浮动按钮

```
                          ┌──────┐
                          │  ↑   │ ← Back to Top
                          └──────┘
                          ┌──────┐
                          │  🔍  │ ← Search
                          └──────┘
```

```css
/* 单个 FAB */
size-12 text-lg md:size-10 md:text-base
rounded-xl border border-zinc-400/20 backdrop-blur-lg
dark:border-zinc-500/30
bg-zinc-50/80 shadow-lg dark:bg-neutral-900/80

/* 容器位置 */
fixed bottom-[calc(2rem+env(safe-area-inset-bottom))]
right-[calc(100vw-3rem-1rem)]

/* 入场/退场动画 */
animate: scale(0,0)→(1,1)
transition: duration: 0.2, ease: 'easeInOut'
```

### 5.7 链接

```css
/* 下划线链接 (.shiro-core-link--underline) */
color: currentColor
text-decoration: underline
text-underline-offset: 3px
background-image: linear-gradient(var(--color-accent), var(--color-accent))
background-size: 0% 1.5px       /* 默认隐藏 */
background-repeat: no-repeat
background-position: left 1.2em
transition: all 500ms ease

/* Hover */
background-size: 100% 1.5px     /* 展开 */
text-shadow: 0.05em 0 var(--color-base-100),
             -0.05em 0 var(--color-base-100)
transition: all 250ms ease

/* 视觉效果: 文字下划线从左侧滑入，文字两侧有遮罩切割效果 */
```

```
Before hover: 阅读更多
              ========= (underline hidden)

After hover:  阅读更多
              ========= (underline slides in from left)
```

### 5.8 引用块 Blockquote

```
┌─────────────────────────────────────────────┐
│ ██                                          │
│ ██  这是一段引用的文字内容。                  │
│ ██  这是第二行引用文字。                      │
│ ██                                          │
│ ██  — 引用来源                              │
└─────────────────────────────────────────────┘
   ↑
   3px 宽, accent 色, border-radius: 1em
```

```css
/* 样式 */
relative border-0

/* 左侧强调线 */
::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 3px;
  height: 100%;
  border-radius: 1em;
  background-color: var(--color-accent);
}
```

### 5.9 毛玻璃层级 (Material UI)

| 层级 | Class | 浅色背景 | 深色背景 | 模糊度 |
|---|---|---|---|---|
| 厚重 | `uk-material-thick` | `rgba(153,153,153,0.97)` | `rgba(37,37,37,0.9)` | 20px / 50px |
| 默认 | `uk-material-default` | `rgba(179,179,179,0.82)` | `rgba(37,37,37,0.82)` | 17.5px / 50px |
| 轻薄 | `uk-material-thin` | `rgba(166,166,166,0.7)` | `rgba(37,37,37,0.7)` | 15px / 50px |
| 极薄 | `uk-material-ultrathin` | `rgba(191,191,191,0.44)` | `rgba(37,37,37,0.44)` | 15px / 50px |

### 5.10 滚动条

```css
/* Webkit 浏览器 */
*::-webkit-scrollbar {
  width: 6px !important;
  height: 6px !important;
}
*::-webkit-scrollbar-thumb {
  background: var(--color-muted-500);
  border-radius: 0.75rem;
}
*::-webkit-scrollbar-thumb:hover {
  background: oklch(from var(--color-muted-500) l c h / 0.8);
}

/* Firefox */
* {
  scrollbar-color: var(--color-muted-500);
  scrollbar-width: thin;
}
```

---

## 6. 动效系统

### 6.1 Spring 弹性动画预设

所有动画基于 **Motion (Framer Motion)** Spring 物理引擎：

| 预设名称 | 类型 | Stiffness | Damping | Duration | Bounce | 用途 |
|---|---|---|---|---|---|---|
| `softBouncePreset` | spring | 100 | 10 | — | — | Hero 入场，通用过渡 |
| `softSpringPreset` | spring | 120 | 20 | 0.35s | — | 从下往上过渡 |
| `reboundPreset` | spring | 140 | 8 | — | 10 | 抽屉内容，强回弹 |
| `microDampingPreset` | spring | — | 24 | — | — | 微交互 |
| `microReboundPreset` | spring | 300 | 20 | — | — | 快速微回弹 |
| `smooth` | spring | — | — | 0.4s | 0 | 平滑无弹跳 |
| `snappy` | spring | — | — | 0.4s | 0.15 | 敏捷小弹跳 |
| `bouncy` | spring | — | — | 0.4s | 0.3 | 弹性弹跳 |

### 6.2 入场动画

```css
/* Scale In */
.animate-scale-in {
  animation: scale-in 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
/* 0%: scale(0.9) opacity:0 → 100%: scale(1) opacity:1 */

/* Bottom-to-Up Transition */
/* 等效 Motion 参数 */
initial: { y: 50, opacity: 0.001 }
animate: { y: 0, opacity: 1 }
transition: softBouncePreset

/* Text Up (逐字) */
/* 每个字符 stagger: 0.05s */
eachDelay: 0.05
initial: { y: 20, opacity: 0 }
animate: { y: 0, opacity: 1 }

/* Timeline Reveal */
animation: timeline-reveal 3s ease-in-out forwards
/* Mask 渐变从左到右揭示内容 */
```

### 6.3 装饰动效

```
┌────────────────────────────────────────┐
│  blob-entrance (2-2.2s)               │
│  ┌──┐                                  │
│  │██│ 350x350px, blur 60px             │
│  │██│ 渐变圆形从 0 放大至 1              │
│  └──┘                                  │
│                                        │
│  blob-float (20-25s infinite)          │
│        ┌──┐                             │
│   ┌──┐ │  │                             │
│   │  │ │  │ ← 4 个方向漂移              │
│   └──┘ └──┘                             │
└────────────────────────────────────────┘
```

- **签名动画:** `stroke-dasharray: 2400`, 8s 循环绘制 SVG 路径
- **Ping:** `scale(1.4)` + opacity 0, 2s 循环
- **Wave:** `rotate(0→20deg→0)`, 2s 循环
- **Shake:** 0.4s ease-in-out on hover

### 6.4 页面过渡 (View Transitions API)

```
浅色 → 深色: turnOff (800ms)
┌────────────┐
│ ████████████│ ← 从底部裁剪到顶部
│ ████████████│
└────────────┘

深色 → 浅色: turnOn (800ms)
┌────────────┐
│            │ ← 从顶部展开
│ ████████████│
│ ████████████│
└────────────┘
```

---

## 7. 深色 / 浅色模式 ⚠️ 必须实现

> **使用本主题务必同时实现暗黑模式。默认行为：跟随系统 (`prefers-color-scheme`) 自动切换，用户选择后持久化到 localStorage。**

| 属性 | 浅色模式 | 深色模式 |
|---|---|---|
| 根背景 | `#ffffff` | `#1c1c1e` |
| 毛玻璃背景 | `rgba(255,255,255,0.72)` | `rgba(29,29,31,0.72)` |
| 正文色 | `#000000` | `#ffffff` |
| 强调色 | `#33a6b8`(薄荷) | `#f596aa`(樱花) |
| 边框色 | `rgba(24,24,27,0.1)` | `#3f3f46` |
| 噪点透明度 | `0.04` | `0.01` |
| Selection | accent / 0.3 | accent / 0.3 |

**切换机制:**
- DaisyUI `data-theme` 属性 + `next-themes` 持久化
- 三态: Light / System / Dark，**默认 System（自动识别系统偏好）**
- 使用 View Transitions API 平滑过渡
- 通过 `window.matchMedia('(prefers-color-scheme: dark)')` 监听系统主题变化

---

## 8. 图标系统

| 图标库 | 使用方式 | 示例 |
|---|---|---|
| MingCute | `i-mingcute-*` CSS class | `i-mingcute-right-line` |
| 自定义 SVG | 组件内联 | SunIcon, DarkIcon, SystemIcon (24x24, stroke 1.5) |
| 社交图标 | SocialIcon 组件 | GitHub, Twitter 等 |

**图标尺寸模式:**
- 导航/按钮图标: `text-lg` (18px) 或 `h-4 w-4` (16px)
- FAB 图标: `text-2xl` (24px)
- 文章元信息图标: inline, `translate-y-[0.5px]`
- 社交图标: `size-[1.2em]` 或 `w-5 h-5`

---

## 9. 技术栈

| 层级 | 技术 |
|---|---|
| 框架 | NextJS 16 (App Router) |
| CSS | TailwindCSS v4 + DaisyUI v5 |
| 动画 | Motion (Framer Motion) |
| 状态管理 | Jotai |
| 无障碍 | Radix UI |
| 图标 | `@egoist/tailwindcss-icons` |
| 排版 | `@tailwindcss/typography` |
| 颜色处理 | chroma-js, colorjs.io |
| 实时通信 | Socket.IO |
| 国际化 | next-intl |
| 数据请求 | TanStack Query |

---

## 10. 速查表 (Quick Reference)

| 要做什么 | 用什么 |
|---|---|
| 页面整体背景 | `bg-white dark:bg-[#1c1c1e]` |
| 毛玻璃导航 | `backdrop-blur-md bg-white/70 shadow-lg` + `rounded-full` |
| 主按钮 | `bg-[#33a6b8] text-white rounded-lg` (浅色) / `bg-[#f596aa] text-black rounded-lg` (深色) |
| 卡片 | `rounded-lg border border-black/10 p-4 hover:shadow-md transition-shadow` |
| 链接悬停 | 底部渐变下划线动画 (500ms → 250ms) |
| 入场动画 | Spring: y:50→0, opacity:0→1 |
| 内容区最大宽 | `max-w-3xl mx-auto` |
| 引用块 | 左侧 3px accent 色竖线 |
| 文章标题 | `text-2xl font-medium` |
| 正文排版 | `prose`, 基准 `14px`, 内容 `1.1rem` |
| 字体 | Sans: Manrope + 系统字体; Serif: Noto Serif SC |
| 深色切换 | `data-theme='dark'` + View Transitions |

---

> 本文档基于 [Innei/Shiro](https://github.com/Innei/Shiro) 项目提取整理，供 AI 辅助开发时作为 UI 参考规范使用。

---

## 📋 AI 使用指南

让 AI 写软件时，直接复制以下内容发给 AI：

```
请使用以下 UI 设计规范来构建界面：

设计规范文档：https://github.com/ZXEB/shiro-core/blob/master/shiro-core-design-spec.md
HTML 展示页面：https://shiro-core.xp6.top/

要求：必须实现暗黑模式，默认跟随系统自动切换。
```

