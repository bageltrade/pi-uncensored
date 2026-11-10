# Uncensored Pi + NVIDIA Nemotron

Stock Pi agent loop, tools, skills, sessions, MCP. Identity replaced with unrestricted SYSTEM.md. NVIDIA OpenAI-compatible endpoint registered.

## 1. Install Pi

```bash
curl -fsSL https://pi.dev/install.sh | sh
export PATH="$HOME/.pi/agent/bin:$PATH"
```

## 2. Install this package into the agent dir

```bash
# from this repo root
AGENT_DIR="${HOME}/.pi/agent"
mkdir -p "$AGENT_DIR/prompts"
cp SYSTEM.md APPEND_SYSTEM.md models.json "$AGENT_DIR/"
# optional: cp prompts/* "$AGENT_DIR/prompts/"
```

## 3. Credentials (not stored in this repo)

```bash
export NVIDIA_API_KEY="nvapi-..."   # your NIM / integrate.api.nvidia.com key
```

Or write `~/.pi/agent/auth.json`:

```json
{
  "nvidia": {
    "type": "api_key",
    "key": "nvapi-..."
  }
}
```

## 4. Run

```bash
export PATH="$HOME/.pi/agent/bin:$PATH"
pi --provider nvidia --model nvidia/nemotron-3.5-super-vl-120b-a12b
```

One-shot:

```bash
pi --provider nvidia --model nvidia/nemotron-3.5-super-vl-120b-a12b --mode text -p "your prompt"
```

After editing SYSTEM.md in an active session, run `/reload`.

## Notes

- `models.json` uses `$NVIDIA_API_KEY` interpolation — no secrets in the file.
- Do not commit `auth.json` or real API keys.
- Provider base URL: `https://integrate.api.nvidia.com/v1`
- Model id: `nvidia/nemotron-3.5-super-vl-120b-a12b`
