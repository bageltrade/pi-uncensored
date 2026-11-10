# Pi Agentic Architecture Analysis

Source: Pi 1.5.2 (`@earendil-works/pi-coding-agent` and related packages)

## What makes Pi agentic

Pi is a **coding agent harness**. The model is not just chat — it runs a closed loop of:

1. **Message intake** → user message lands on the active session branch
2. **Context assembly** → system prompt sections + branch history + tool defs + skill ads
3. **Model request** → provider streams assistant text + tool calls
4. **Tool execution** → each tool call runs, results recorded
5. **Turn complete** → if more work needed, loop again; else stop

That loop, plus persistent session trees, skills, extensions, and MCP, is the agentic core.

## System prompt construction (the real "MD" layer)

Pi does **not** ship one giant static system MD as the only brain.  
It builds a **structured system prompt** from sections at runtime:

| Section | Purpose |
|---------|---------|
| `preamble` | Identity: "You are a coding assistant running inside pi..." |
| `tools` | Visible tool names + short snippets |
| `rules` | Behavioral rules (concise, path clarity, tool-specific guidelines) |
| `docs` | Paths to Pi's own docs (only when user asks about the harness) |
| `project_context` | Content of discovered project instruction files |
| `skills` | Name + description of available skills (full SKILL.md loaded on demand) |
| `cwd` | Working directory |
| `addendum` | Optional APPEND_SYSTEM.md / append text |
| custom | Any extra named sections from extensions/settings |

**Override points that matter for a clone:**

- Global: `~/.pi/agent/SYSTEM.md` → becomes custom `preamble` (replaces default identity)
- Global: `~/.pi/agent/APPEND_SYSTEM.md` → addendum
- Project: `.pi/SYSTEM.md` / `.pi/APPEND_SYSTEM.md` (only after project trust)
- CLI / SDK: `forceSystemPrompt` / `customPrompt` / `appendSystemPrompt`

Default preamble (from `dist/core/system-prompt.js`):

```
You are a coding assistant running inside pi, a coding agent harness. Help users inspect files, run commands, and modify code.
```

Default rules always include:

- Be concise in your responses
- Show file paths clearly when working with files
- Tool-specific guidelines when bash/powershell/etc. are selected

## Session model

- Sessions = **JSONL trees** (each entry has id + parent)
- Active branch = path to current entry → that history is what the model sees
- Compaction = summary node that replaces older messages in *context* while originals stay in the tree
- Fork / clone = new session file with selected history

## Tools (default selected)

Typical defaults: `read`, `bash`, `edit`, `write`  
(plus whatever extensions / MCP / settings add)

Tools are advertised in the system prompt with short snippets. Guidelines per tool can be injected into `rules`.

## Skills (Agent Skills spec)

- Directory with `SKILL.md` (frontmatter: name, description, optional allowed-tools, etc.)
- At start: only **name + description** go into the system prompt
- On demand: model (or `/skill:name`) loads full instructions + bundled scripts/refs
- Keeps context lean until needed

## Prompt templates

- Markdown files under prompt dirs, e.g. `~/.pi/agent/prompts/*.md`
- Become slash commands (`/review`, etc.)
- Expand before becoming the user message (`$1`, `$@`, defaults)

## Extensions

TypeScript modules in-process. Can register:

- tools, commands, shortcuts
- providers, event handlers, renderers
- terminal UI pieces

## MCP

External tool servers via Model Context Protocol — same agent loop, more tools.

## Interfaces (same agent core)

- Interactive TUI
- Print / JSON modes
- RPC (JSONL stdin/stdout)
- TypeScript SDK

## Trust / security surface

- Project resources load only after explicit project trust
- Tools inherit the OS permissions of the Pi process
- No separate "safety model" layer in the prompt beyond whatever the *provider model* has and whatever you put in SYSTEM.md

---

## Clone strategy (uncensored)

1. Keep **all agentic machinery** (loop, tools, skills, sessions, MCP, extensions).
2. Replace only the **identity + policy surface** via `SYSTEM.md` (and optional `APPEND_SYSTEM.md`).
3. Do not strip tool registration, skill discovery, or session tree logic — that is what makes it agentic.
4. Optional: custom prompt templates / skills that assume an unrestricted operator.

