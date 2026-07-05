# Clone Website Plugin

**English | [中文](./README.zh.md)**

1:1 restoration of a live website's visual design and animations: full-site evidence gathering → Design DNA extraction → animation reverse-engineering → modular rebuild → closed-loop verification.

Includes three skills (`skills/`) and one one-click workflow command (`commands/clone-site.md`):

- **design-dna** — Reverse-engineers a website into a structured Design DNA JSON (colors/fonts/spacing/layout/animation curves...), serving as the single source of design truth for the whole cloning project.
- **web-shader-extractor** — Reverse-engineers WebGL/Canvas/shader/scroll-driven animations, producing a locally runnable implementation.
- **duck** — A self-talk-through step before starting each module, to surface hidden assumptions.
- **`/clone-site`** — Chains the three skills above into a complete workflow; when triggered, it first confirms which website you want to clone.

⚠️ **Usage notice**: Please only use this tool on websites you own the copyright to, or have been granted permission for (e.g. your own portfolio or a company website you're responsible for).
Do not use it to clone someone else's copyrighted commercial website and then publish or monetize it.

---

## Installation Method 1: Claude Cowork (Recommended for non-technical users)

Requires Claude Pro / Max + the desktop App.

1. Download `dist/clone-website-plugin.plugin` (included in this repo, direct link below).<br>
👉 [Click to download clone-website-plugin.plugin](https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/dist/clone-website-plugin.plugin)
2. Open the Claude desktop App → Cowork → left sidebar **Customize** → **Plugins** tab.
3. Click **Upload plugin**, and select the `.plugin` file you just downloaded.
4. A "not controlled by Anthropic" security notice will pop up — this is the standard notice shown for all third-party plugins; confirm you trust the source and continue.
5. Start a new Cowork task, and type:
   ```
   /clone-site
   ```
   Claude will first ask which website you want to clone, and once you answer, it will automatically start the whole process.

## Installation Method 2: Claude Code / Claude Cowork (via Marketplace)

⚠️ **Important**: Please run the following commands in an **actual terminal** (a standalone terminal, or VS Code's **Terminal** panel, both work).
**Do not** type the slash-based `/plugin ...` in Claude Code's chat/plugin sidebar — that's an interactive UI command, and in some
environments (e.g. certain IDE plugin chat panels) it will throw `"/plugin isn't available in this environment"`, which is a
known limitation. Use the plain terminal commands below instead — they work in both places.

Run them one line at a time; confirm the first line succeeded before running the second:
```
claude plugin marketplace add pmgwee/clone-website-plugin
```
```
claude plugin install clone-website-plugin@clone-website-plugin
```

Once installed, you can use `/clone-website-plugin:clone-site`, or just `/clone-site` depending on your Claude Code version.

**Don't want to run the commands yourself?** Open `claude` in your terminal, and send it this message directly to let it execute for you:
```
Help me install clone-website-plugin: in the terminal, run
claude plugin marketplace add pmgwee/clone-website-plugin
and
claude plugin install clone-website-plugin@clone-website-plugin
in order, confirming each one succeeded before running the next — don't paste both at once.
```

## Installation Method 3: Codex CLI / Gemini CLI and other non-Claude agents

These tools don't recognize Claude's Plugin format. Please use the "bootstrap-style" installation instructions in `docs/install.md` instead —
just send this message to your Agent:
```
Help me install clone-website-plugin: https://raw.githubusercontent.com/pmgwee/clone-website-plugin/main/docs/install.md
```

---

## Directory Structure

```
clone-website-plugin/
├── .claude-plugin/
│   ├── plugin.json        # Plugin metadata
│   └── marketplace.json   # Lets this repo itself be installed via /plugin marketplace add
├── skills/
│   ├── design-dna/SKILL.md
│   ├── web-shader-extractor/SKILL.md
│   └── duck/SKILL.md
├── commands/
│   └── clone-site.md       # One-click workflow command (master prompt)
├── .mcp.json                # Auto-wires up Context7
├── docs/
│   └── install.md           # Bootstrap-style install instructions for agents like Codex CLI / Gemini CLI
└── dist/
    └── clone-website-plugin.plugin   # Packaged plugin file, for direct upload in Cowork
```
