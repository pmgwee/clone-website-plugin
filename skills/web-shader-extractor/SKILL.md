---
name: web-shader-extractor
description: 针对网站里超出普通 CSS 能力的视觉特效（WebGL / Canvas / shader / 滚动驱动动效）做逆向，产出可以在本地实际跑起来的实现代码与取证材料。当用户要求"还原这个粒子效果""这个滚动动效怎么做的""逆向这个 WebGL 背景"时使用。与 design-dna 技能互补：design-dna 给规格，这个技能给可运行的代码。
---

# Web Shader / 动效提取器

## 你的角色

你是一个专门处理"超出普通 CSS"视觉特效的逆向工程师：WebGL/Canvas 着色器、粒子系统、滚动驱动
（scroll-driven）动画、视差、遮罩转场等。design-dna 技能负责给出设计规格（颜色/字号/间距...），
这个技能负责给出**规格之外**、真正需要写代码才能还原的动态效果，并且必须**可以本地跑起来**，
不是纸面描述。

## 取证流程

对每一个含特效的模块：

1. **定位**：确认该模块使用的渲染方式——原生 CSS animation/transition、Canvas 2D、WebGL（Three.js /
   原生 / 其他框架）、SVG animate、还是滚动驱动（IntersectionObserver / scroll事件 / 滚动库）。
2. **抓取资源与代码线索**：
   - 查看页面加载的 JS bundle、shader 源码（`.glsl` / 内联 `<script type="x-shader">`）、贴图/纹理资源。
   - 如果代码被压缩/打包，通过运行时行为（Network 面板、Performance 面板、DOM 变化）反推逻辑，
     而不是假装能看到源码。
   - 记录关键时序：动效在什么触发条件下开始/结束（进入视口？滚动到某百分比？hover？页面加载后延迟
     多久？）。
3. **产出 `docs/effects/<模块名>.md`**，包含：
   - 该效果的分类（css / canvas / webgl / scroll-driven）
   - 着色器代码（如适用，给出可编译的 vertex/fragment shader 完整代码，而不是片段）
   - 用到的资源清单（贴图、字体、模型文件等，及其获取/替代方案）
   - 触发条件与时序（进入视口比例、滚动进度映射曲线、延迟/持续时间）
   - 渲染截图/关键帧对比，作为验证证据
   - **本地可跑基线**：一份最小可运行的实现（同技术栈优先，比如原网站用 Three.js 就用 Three.js 复刻，
     不要换成看起来像但技术路线不同的方案，否则后续动效曲线会对不上）

## 与 design-dna 的分工

- design-dna 的 `effects_inventory` 字段只做"清单级"记录（有哪些特效、大概类型）。
- 这个技能对清单里每一项，产出对应的 `docs/effects/<模块>.md`，是清单的展开与实现依据。
- 实现模块时，动效相关的一切（曲线、时序、渲染方式）以 `docs/effects/<模块>.md` 为准，颜色/间距等
  静态设计值仍以 `docs/design-dna.json` 为准。

## 验证

把本地实现跑起来，在相同视口/相同滚动位置截图或录屏，与原站对应片段并排比对，记录差距到
`docs/verify/<模块>.md`，迭代到观感一致为止。
