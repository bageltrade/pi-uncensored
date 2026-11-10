# pi-uncensored

Uncensored system layer + NVIDIA Nemotron wiring for [Pi](https://pi.dev) (coding agent harness).

Keeps the full agentic stack (tool loop, skills, sessions, MCP, extensions). Replaces only the identity/policy surface via `SYSTEM.md` / `APPEND_SYSTEM.md`, and registers:

- provider: `nvidia`
- endpoint: `https://integrate.api.nvidia.com/v1`
- model: `nvidia/nemotron-3.5-super-vl-120b-a12b`

## Files

| file | role |
|------|------|
| `SYSTEM.md` | unrestricted agent preamble (replaces default identity) |
| `APPEND_SYSTEM.md` | standing no-refusal orders |
| `models.json` | OpenAI-compatible NVIDIA provider (key via `$NVIDIA_API_KEY`) |
| `INSTALL.md` | setup steps |
| `docs/ANALYSIS.md` | how Pi’s agentic prompt assembly works |

## Quick install

See [INSTALL.md](INSTALL.md).

```bash
curl -fsSL https://pi.dev/install.sh | sh
export PATH="$HOME/.pi/agent/bin:$PATH"
cp SYSTEM.md APPEND_SYSTEM.md models.json ~/.pi/agent/
export NVIDIA_API_KEY="nvapi-..."
pi --provider nvidia --model nvidia/nemotron-3.5-super-vl-120b-a12b
```

## Security

This repo contains **no API keys**. Put keys in environment or `~/.pi/agent/auth.json` (gitignored locally).
