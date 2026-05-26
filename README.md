# oc-cc-universal

A Go CLI proxy that lets you use your [OpenCode Go](https://opencode.ai/docs/go/) subscription with [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

`oc-cc-universal` sits between Claude Code and OpenCode Go, intercepting Anthropic API requests, transforming them to OpenAI format, and forwarding them to OpenCode Go's endpoint. Claude Code thinks it's talking to Anthropic — but your requests go to affordable open models instead.

## Why?

OpenCode Go gives you access to powerful open coding models for **$5/month** (then $10/month). This proxy makes those models work seamlessly with Claude Code's interface — no patches, no forks, just set two environment variables and go.

## Features

- **Transparent Proxy** — Claude Code sends Anthropic-format requests, proxy transforms to OpenAI format and back
- **Model Routing** — Automatically routes to different models based on context (default, thinking, long context, background)
- **Fallback Chains** — If a model fails, automatically tries the next one in your configured chain
- **Circuit Breaker** — Tracks model health and skips failing models to avoid latency spikes
- **Real-time Streaming** — Full SSE streaming with live OpenAI -> Anthropic format transformation
- **Tool Calling** — Proper Anthropic tool_use/tool_result <-> OpenAI function calling translation
- **Token Counting** — Uses tiktoken (cl100k_base) for accurate token counting and context threshold detection
- **JSON Configuration** — Flexible config file with environment variable overrides and `${VAR}` interpolation
- **Hot Reload** — Watch config file for changes and reload automatically (off by default)
- **Background Mode** — Run as daemon detached from terminal
- **Auto-start on Login** — Launch on system startup via launchd (macOS)

## Quick Start

### 1. Install

```bash
# macOS / Linux
brew tap samueltuyizere/tap && brew install oc-cc-universal

# Windows
scoop bucket add oc-cc-universal https://github.com/samueltuyizere/scoop-bucket && scoop install oc-cc-universal
```

Or see [INSTALLATION.md](INSTALLATION.md) for more options.

### 2. Initialize Configuration

```bash
oc-cc-universal init
```

Creates a default config at `~/.config/oc-cc-universal/config.json`. Edit it to add your API key, or set the environment variable:

```bash
export OC_CC_UNIVERSAL_API_KEY=sk-opencode-your-key-here
```

### 3. Start the Proxy

```bash
oc-cc-universal serve
```

### 4. Configure Claude Code

```bash
export ANTHROPIC_BASE_URL=http://127.0.0.1:3456
export ANTHROPIC_AUTH_TOKEN=unused
```

### 5. Run Claude Code

```bash
claude
```

## CLI Commands

```
oc-cc-universal serve              Start the proxy server
oc-cc-universal serve -b           Start in background (detached from terminal)
oc-cc-universal serve --port 8080  Start on a custom port
oc-cc-universal stop               Stop the running proxy server
oc-cc-universal status             Check if the proxy is running
oc-cc-universal init               Create default configuration file
oc-cc-universal validate           Validate configuration file
oc-cc-universal models             List available OpenCode Go models
oc-cc-universal autostart enable   Enable auto-start on login
oc-cc-universal autostart disable  Disable auto-start on login
oc-cc-universal autostart status   Check autostart status
oc-cc-universal --version          Show version
```

## Documentation

| Document | Description |
| -------- | ----------- |
| [INSTALLATION.md](INSTALLATION.md) | Homebrew, Scoop, build from source, release binaries |
| [CONFIGURATION.md](CONFIGURATION.md) | Config file reference, env vars, model routing, fallback chains |
| [MODELS.md](MODELS.md) | Model capabilities, costs, and routing recommendations |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Development setup, architecture, how it works |
| [TROUBLESHOOTING.md](TROUBLESHOOTING.md) | Common issues and debug mode |

## License

[AGPL-3.0](LICENSE)
