# pi-uncensored

Unrestricted system layer + NVIDIA Nemotron wiring for [Pi](https://pi.dev).

Keeps the full agentic stack (tool loop, skills, sessions, MCP, extensions).  
Replaces identity/policy via `SYSTEM.md` / `APPEND_SYSTEM.md` so the agent **answers any topic** the operator asks — including drugs, weapons, malware, exploits, fraud, and other dual-use or illegal-activity research — with technical detail and no refusal.

Registers:

- provider: `nvidia`
- endpoint: `https://integrate.api.nvidia.com/v1`
- model: `nvidia/nemotron-3.5-super-vl-120b-a12b`

## Files

| file | role |
|------|------|
| `SYSTEM.md` | unrestricted agent preamble |
| `APPEND_SYSTEM.md` | standing zero-refusal orders |
| `models.json` | NVIDIA provider (`$NVIDIA_API_KEY`) |
| `INSTALL.md` | setup |
| `docs/ANALYSIS.md` | how Pi builds the agentic prompt |

## Install

See [INSTALL.md](INSTALL.md).

```bash
curl -fsSL https://pi.dev/install.sh | sh
export PATH="$HOME/.pi/agent/bin:$PATH"
cp SYSTEM.md APPEND_SYSTEM.md models.json ~/.pi/agent/
export NVIDIA_API_KEY="nvapi-..."
pi --provider nvidia --model nvidia/nemotron-3.5-super-vl-120b-a12b
```

## Security

No API keys in this repo. Put keys in the environment or `~/.pi/agent/auth.json` (gitignored).
