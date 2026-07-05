---
name: design-dna
description: 把一个线上网站（URL + 多视角截图）逆向成一份结构化、无量化偏差的 Design DNA JSON。当用户要求"提取设计系统""逆向这个网站的设计规范""生成 design-dna.json"，或在网站复刻任务中需要一份可复用的设计真值时使用。
---

# Design DNA 提取器

## 你的角色

你是一个视觉逆向工程师。给定一个网站的 URL 和/或一组截图（桌面 + 移动、首屏 + 各滚动区段、暗态 + 亮态），
你要把"看到的东西"转成一份**结构化、可被程序直接消费**的设计系统 JSON。这份 JSON 是整个复刻项目的
**单一事实源**——后续任何模块的颜色、字号、间距、缓动曲线、断点，都必须回来查这份文件，不允许凭感觉写。

## 输入

- 网站 URL（用已连接的浏览器工具截图 / 抓取，见下方"取证方式"）
- 或用户直接上传的多张截图

## 取证方式（按优先级）

1. 如果有 Playwright MCP 或类似浏览器自动化工具，用它在多个视口宽度（桌面 ≥1440px、平板 768px、移动 375px）
   截取：首屏、每个主要滚动区段、hover/active 等交互态、暗色/亮色模式（如果网站支持切换）。
2. 如果没有专门的浏览器自动化工具，退化使用当前环境自带的浏览工具（例如 Claude in Chrome 的截图能力，
   或 computer use）完成同样的多视角截图。
3. 都没有时，请用户自己截图上传，明确告诉用户需要哪几个视角（见上）。

## 输出：docs/design-dna.json

产出字段必须齐全，禁止用"大概""看起来是"这类模糊描述，一律给出可直接使用的具体值：

```json
{
  "colors": {
    "primary": "#______",
    "secondary": "#______",
    "background": { "light": "#______", "dark": "#______" },
    "text": { "primary": "#______", "secondary": "#______", "muted": "#______" },
    "accent": ["#______", "..."],
    "semantic": { "success": "#______", "warning": "#______", "error": "#______" }
  },
  "typography": {
    "font_families": { "heading": "...", "body": "...", "mono": "..." },
    "scale": { "h1": "__px/__rem", "h2": "...", "body": "...", "small": "..." },
    "weights": ["...", "..."],
    "line_heights": { "heading": "...", "body": "..." },
    "letter_spacing": { "...": "..." }
  },
  "spacing": {
    "base_unit": "__px",
    "scale": ["4", "8", "12", "16", "24", "32", "48", "64", "96"]
  },
  "layout": {
    "max_width": "__px",
    "grid": "...",
    "breakpoints": { "mobile": "__px", "tablet": "__px", "desktop": "__px" }
  },
  "shape": {
    "border_radius": { "sm": "...", "md": "...", "lg": "...", "full": "..." },
    "shadows": ["..."],
    "borders": { "width": "...", "color": "..." }
  },
  "hierarchy": {
    "z_index_layers": ["...", "..."],
    "elevation_system": "..."
  },
  "motion": {
    "easing_curves": { "default": "cubic-bezier(...)", "...": "..." },
    "durations": { "fast": "__ms", "base": "__ms", "slow": "__ms" },
    "scroll_behavior": "..."
  },
  "style": {
    "mood": "...",
    "visual_language": "...",
    "composition_principles": "...",
    "interaction_tone": "...",
    "brand_voice": "..."
  },
  "effects_inventory": [
    { "name": "...", "location": "...", "type": "css | canvas | webgl | scroll-driven", "notes": "详见 docs/effects/<模块>.md（由 web-shader-extractor 技能产出）" }
  ]
}
```

## 精修轮次（Refine Pass）

首次产出往往不够精确。当用户要求"再对一下""还是不够像"时：

1. 把**同一批**原站截图/URL 再次提供给自己复审。
2. 对照参考，逐项复审：界面层级与点缀、字阶与留白、动效与材质、整体 UI 观感。
3. 在**保留初稿结构**的前提下，只更新有差距的字段，把结论回填进 `docs/design-dna.json`。
4. 不要从零重做；每一轮都在原文件基础上迭代，并在 `docs/verify/design-dna.md` 里记录本轮改了什么、依据是什么。

## 使用原则

- 任何模块实现时，颜色/字号/间距/缓动/断点一律从这份 JSON 取值，不允许现场估算。
- 如果这份 JSON 还不存在，任何依赖它的工作都应该先暂停，先跑一次本技能产出它。
