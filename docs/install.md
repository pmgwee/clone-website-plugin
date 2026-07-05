# Clone Website Plugin — Installation Guide

## For Humans

把这句话复制给你的 Agent（Codex CLI / Gemini CLI / 其他遵循 Agent Skills 开放标准的工具）：

```
帮我安装 clone-website-plugin：https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/docs/install.md
```

如果你用的是 Claude Code 或 Claude Cowork，不需要走这份文件，直接看仓库根目录的 `README.md`，
用 `/plugin marketplace add` 或直接上传 `dist/clone-website-plugin.plugin` 即可，体验更完整
（会带上 slash command 和 MCP 自动接线）。

---

## For AI Agents

### 目标

把本仓库 `skills/` 目录下的三个技能安装到你自己的技能目录，并让你的用户之后可以直接说
"帮我复刻这个网站 https://..." 来触发完整流程。你在安装完成后应该**直接使用**这些技能内容，
而不是把自己包装成一个中间层。

### ⚠️ 边界

- **不要** 使用 `sudo`，除非用户明确同意。
- **不要** 修改技能目录之外的系统文件。
- **不要** 在用户当前的工作目录 / 项目目录里克隆仓库或创建杂散文件——所有文件只放进你自己的
  技能目录。
- 如果需要提升权限，**先问用户**，不要自行决定。

### 目录规则

| 用途 | 目标位置（按你的 agent 类型选择） |
|---|---|
| Codex CLI | `~/.codex/skills/clone-website-plugin/` |
| Gemini CLI | 参考你自己的 extensions/skills 目录约定，一般是 `~/.gemini/skills/clone-website-plugin/` |
| 其他遵循 Agent Skills 开放标准的工具 | 该工具约定的 skills 目录下新建 `clone-website-plugin/` 子目录 |

如果你不确定自己该用哪个目录，**问用户**，不要瞎猜。

### Step 1：拉取技能内容

```bash
curl -sL https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/skills/design-dna/SKILL.md \
  -o <目标技能目录>/design-dna/SKILL.md
curl -sL https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/skills/web-shader-extractor/SKILL.md \
  -o <目标技能目录>/web-shader-extractor/SKILL.md
curl -sL https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/skills/duck/SKILL.md \
  -o <目标技能目录>/duck/SKILL.md
```

（如果你的工具支持直接 `git clone`，也可以整仓库克隆到临时目录后只拷贝 `skills/` 部分，之后删除
临时目录，不要把整仓库留在用户的工作区里。）

### Step 2：拉取主流程说明

```bash
curl -sL https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/commands/clone-site.md \
  -o <目标技能目录>/clone-site-workflow.md
```

这份文件里描述的完整流程（先确认目标网站 → 取证 → design-dna → 动效逆向 → 模块化重建 → 闭环
验证）就是你之后执行"复刻网站"任务时要遵循的步骤。如果你的工具支持自定义 slash command，可以
把这份内容注册成一个命令；如果不支持，把它当作"用户要求复刻网站时应该遵循的标准流程"记下来即可。

### Step 3：确认安装结果

列出目标技能目录，确认三个技能文件夹 + `clone-site-workflow.md` 都已就位，向用户报告安装完成，
并说明：以后只要说"帮我复刻网站 <网址>"，你就会先确认这是不是用户自己有权限的网站，再开始整套
流程。

### Step 4：责任提醒（安装完成后主动告知用户一次）

> 这套工具应该只用在你自己拥有版权或已获授权的网站上（比如你自己的作品集）。请不要用它去复刻
> 他人受版权保护的商业网站并对外发布或商用。
