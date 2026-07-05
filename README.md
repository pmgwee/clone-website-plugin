# Clone Website Plugin

1:1 还原线上网站的视觉与动效：全站取证 → Design DNA 提取 → 动效逆向 → 模块化重建 → 闭环验证。

包含三个技能（`skills/`）和一个一键流程命令（`commands/clone-site.md`）：

- **design-dna** — 把网站逆向成结构化 Design DNA JSON（颜色/字体/间距/版式/动效曲线...），作为
  整个复刻项目的设计真值。
- **web-shader-extractor** — 逆向 WebGL/Canvas/shader/滚动驱动动效，产出可本地运行的实现。
- **duck** — 每个模块开工前的自我讲解，暴露隐藏假设。
- **`/clone-site`** — 把以上三个技能串成完整流程，触发时会先确认你要复刻哪个网站。

⚠️ **使用须知**：请只对你自己拥有版权或已获授权的网站使用本工具（例如你自己的作品集/公司官网）。
不要用来复刻他人受版权保护的商业网站并对外发布或商用。

---

## 安装方式一：Claude Cowork（推荐非技术用户）

需要 Claude Pro / Max + 桌面版 App。

1. 下载 `dist/clone-website-plugin.plugin`（本仓库自带，见下方直链）。<br>
👉 [点击下载 clone-website-plugin.plugin](https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/dist/clone-website-plugin.plugin)
2. 打开 Claude 桌面 App → Cowork → 左侧 **Customize** → **Plugins** 标签。
3. 点击 **Upload plugin**，选择刚下载的 `.plugin` 文件。
4. 会弹出"未受 Anthropic 控制"的安全提示——这是所有第三方插件的标准提示，确认信任来源后继续。
5. 新开一个 Cowork 任务，输入：
   ```
   /clone-site
   ```
   Claude 会先问你要复刻哪个网站，回答后自动开始整套流程。
## 安装方式二：Claude Code / Claude Cowork（通过 Marketplace）

⚠️ **重要**：请在**真正的终端**里执行以下命令（独立终端、或 VS Code 的 **Terminal** 面板都可以），
**不要**在 Claude Code 的聊天/插件侧边栏里打带斜杠的 `/plugin ...`——那是交互式 UI 命令，在部分
环境（例如某些 IDE 插件的聊天面板）里会报 `"/plugin isn't available in this environment"`，这是
已知限制。下面这套是普通终端命令，两边通用：

一行一行执行，等第一行跑完确认成功，再跑第二行：

```
claude plugin marketplace add pmgwee/clone-website-plugin
```
```
claude plugin install clone-website-plugin@clone-website-plugin
```

安装后即可使用 `/clone-website-plugin:clone-site`，或视 Claude Code 版本而定直接用 `/clone-site`。

**懒得自己跑命令？** 打开终端里的 `claude`，把下面这句话直接发给它，让 Claude 自己执行：

```
帮我安装 clone-website-plugin：在终端依次运行
claude plugin marketplace add pmgwee/clone-website-plugin
和
claude plugin install clone-website-plugin@clone-website-plugin
每条跑完确认成功再跑下一条，不要一次性粘贴两条。
```


## 安装方式三：Codex CLI / Gemini CLI 等非 Claude 生态 Agent

这些工具不认识 Claude 的 Plugin 格式，请改用 `docs/install.md` 里的"自举式"安装说明——
把下面这句话发给你的 Agent 即可：

```
帮我安装 clone-website-plugin：https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/docs/install.md
```

---

## 目录结构

```
clone-website-plugin/
├── .claude-plugin/
│   ├── plugin.json        # 插件元数据
│   └── marketplace.json   # 让本仓库自身可以被 /plugin marketplace add 安装
├── skills/
│   ├── design-dna/SKILL.md
│   ├── web-shader-extractor/SKILL.md
│   └── duck/SKILL.md
├── commands/
│   └── clone-site.md       # 一键流程命令（master prompt）
├── .mcp.json                # 自动接好 Context7
├── docs/
│   └── install.md           # 给 Codex CLI / Gemini CLI 等 agent 用的自举安装说明
└── dist/
    └── clone-website-plugin.plugin   # 打包好的插件文件，供 Cowork 直接上传
```
