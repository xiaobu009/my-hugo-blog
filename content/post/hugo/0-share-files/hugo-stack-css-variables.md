---
title: "Stack 4.0 主题 CSS 变量完整参考表"
date: 2026-06-23
lastmod: 2026-06-23
draft: false
description: "Stack 4.0 主题常用 CSS 变量速查表，按颜色、圆角间距、字体、阴影分类整理，配合 custom.scss 自定义样式使用。"
keywords: ["Hugo", "Stack主题", "CSS变量", "custom.scss", "暗色模式"]
categories:
  - Hugo建站
tags:
  - Hugo
  - Stack主题
url: "hugo-stack-css-variables"
series: ["Hugo建站指南"]
---

## 写在前面

这篇是[《主题视觉定制——custom.scss 覆盖样式、主题色、暗色模式》](/hugo-stack-custom-style/)的配套参考表，把 Stack 4.0 主题能改的 CSS 变量按类型整理在一起，方便照着改的时候随手查。

所有变量都写在 `myblog/assets/scss/custom.scss` 里的 `:root { }` 内。要让暗色模式单独生效，把对应变量放进 `&[data-scheme="dark"] { }` 嵌套块里，写法见本文第五部分。

变量名以 Stack 主题官方仓库（CaiJimmy/hugo-theme-stack）主分支源码为准，个别变量在不同版本之间可能有调整。改完没反应时，打开浏览器开发者工具，点选目标元素，在样式面板里确认实际用的变量名最靠谱。

------

## 一、颜色类

| 变量名 | 作用 |
|---|---|
| `--accent-color` | 强调色，影响链接、标题左侧色条、目录当前项高亮、锚点图标 |
| `--body-background` | 页面整体背景色 |
| `--card-background` | 卡片背景色（文章卡片、侧边栏小部件、目录框） |
| `--card-text-color-main` | 卡片内正文文字颜色 |
| `--card-text-color-secondary` | 次要文字颜色，比如图片说明文字 |
| `--card-text-color-tertiary` | 第三级文字颜色，比如分割线、目录小标题 |
| `--card-separator-color` | 卡片内分隔线颜色（引用块左侧边线等） |
| `--code-background-color` | 行内代码背景色 |
| `--code-text-color` | 行内代码文字颜色 |
| `--pre-background-color` | 多行代码块背景色 |
| `--pre-text-color` | 多行代码块文字颜色 |
| `--blockquote-background-color` | 引用块背景色 |
| `--table-border-color` | 表格边框颜色 |
| `--tr-even-background-color` | 表格偶数行背景色（斑马纹效果） |
| `--kbd-border-color` | 键盘按键样式元素（`<kbd>`）的边框颜色 |
| `--alert-note-color` / `--alert-note-background` | 提示框 note 类型的文字色与背景色 |
| `--alert-tip-color` / `--alert-tip-background` | 提示框 tip 类型 |
| `--alert-important-color` / `--alert-important-background` | 提示框 important 类型 |
| `--alert-warning-color` / `--alert-warning-background` | 提示框 warning 类型 |
| `--alert-caution-color` / `--alert-caution-background` | 提示框 caution 类型 |

------

## 二、圆角与间距类

| 变量名 | 作用 |
|---|---|
| `--card-border-radius` | 卡片整体圆角 |
| `--tag-border-radius` | 标签、行内代码等小圆角元素 |
| `--card-padding` | 卡片内边距 |
| `--heading-border-size` | 标题左侧色条、目录高亮项左边线的粗细 |
| `--blockquote-border-size` | 引用块左侧边线粗细 |
| `--spacing-xs` / `--spacing-sm` / `--spacing-md` / `--spacing-lg` / `--spacing-2xl` | 主题内部通用间距尺度，多处布局复用，数值越大间距越宽 |
| `--main-top-padding` | 页面顶部留白（社区实测可用，部分版本可能需要确认） |
| `--section-separation` | 卡片与卡片之间的间距（社区实测可用，部分版本可能需要确认） |

------

## 三、字体类

| 变量名 | 作用 |
|---|---|
| `--article-font-size` | 文章正文字号 |
| `--article-line-height` | 正文行高 |
| `--article-font-family` | 正文字体 |
| `--code-font-family` | 代码字体（行内代码 + 代码块） |
| `--base-font-family` | 全局基础字体，脚注等位置使用 |

------

## 四、阴影类

| 变量名 | 作用 |
|---|---|
| `--shadow-l1` | 一级阴影，最浅，常用于卡片默认状态 |
| `--shadow-l2` | 二级阴影，常用于悬浮/展开状态 |
| `--shadow-l3` | 三级阴影，最明显 |

------

## 五、暗色模式写法示例

```scss
:root {
    // 亮色模式下的值
    --accent-color: #2b6cb0;
    --body-background: #f7f7f7;
    --card-background: #ffffff;

    // 暗色模式专属，只在暗色模式下生效
    &[data-scheme="dark"] {
        --accent-color: #63b3ed;
        --body-background: #0f172a;
        --card-background: #1a2332;
        --code-background-color: #1e293b;
        --code-text-color: #fbbf24;
    }
}
```

没有在 `&[data-scheme="dark"]` 里单独写的变量，暗色模式下会继续使用 `:root` 里的默认值。

------

## 六、排查小技巧

1. **改了某个变量没反应**：先确认文件路径是站点根目录的 `assets/scss/custom.scss`，不是 `themes/hugo-theme-stack/` 里面那个。
2. **不确定某个元素具体用了哪个变量**：浏览器按 `F12` 打开开发者工具，点选该元素，在右侧样式面板搜索 `var(--` 即可看到实际引用的变量名。
3. **想整体复原**：清空 `custom.scss` 里自己写的内容即可，主题文件本身没被改动过。

------

返回：[《主题视觉定制——custom.scss 覆盖样式、主题色、暗色模式》](/hugo-stack-custom-style/)
