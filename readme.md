# Unlimited Claude + Gemini + OpenCode with Bifrost

![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Mac-blue?style=for-the-badge)
![Agents](https://img.shields.io/badge/Agents-3-orange?style=for-the-badge)
![Providers](https://img.shields.io/badge/Providers-3-teal?style=for-the-badge)
![Cost](https://img.shields.io/badge/Cost-Under_$1-brightgreen?style=for-the-badge)

> **Three coding agents. Three inference providers. One gateway. Triple fallback routing. Provider success rate dropped to 43% — user success rate stayed at 100%.**

---
## 🎓 Complete AI Gateway Masterclass (Free)

This repository is part of a **3-part AI Gateway series**. Each video has a matching GitHub repository with all commands, configs, and source files.

| Part | Topic | Links |
| :--- | :--- | :--- |
| 🚀 **Part 1** | Claude Desktop Without Anthropic (AI Gateway Basics) | [▶️ YouTube](https://www.youtube.com/watch?v=4DlNb2weD_s) • [📂 GitHub](https://github.com/network-tocoder/Claude-Desktop-Without-Anthropic-AI-Gateway-Basics) |
| ⚡ **Part 2** | Unlimited Claude + Gemini + OpenCode with Bifrost | [▶️ YouTube](https://www.youtube.com/watch?v=LOrjVQnA4EY) • [📂 GitHub](https://github.com/network-tocoder/Run-Claude-Code-Gemini-CLI-OpenCode-One-Gateway) |
| 🔥 **Part 3** | Complete $0 AI Coding Stack (Bifrost + Claude + NVIDIA + VS Code) | [▶️ YouTube](https://www.youtube.com/watch?v=LzPvQAkQ8tw) • [📂 GitHub](https://github.com/network-tocoder/Run-Any-AI-Model-in-VS-Code-CLI-One-Gateway) |

> 📚 **Recommended:** Start with **Part 1** if you're new to AI Gateways, then continue through Parts 2 and 3 to build the complete production-ready stack.


---

## ⚡ What Is This?

Run **Claude Code**, **Gemini CLI**, and **OpenCode** through a single **Bifrost AI Gateway** using open tier models. When one provider hits a rate limit, the gateway auto-switches to the next.

| Tool | Role |
|------|------|
| 🌉 **Bifrost Gateway** | Open-source AI gateway — routing, logging, fallback |
| 🔌 **Bifrost CLI** | Launches agents through the gateway with one command |
| 🟠 **Claude Code** | Anthropic's CLI coding agent |
| 🟣 **Gemini CLI** | Google's CLI coding agent |
| 🔵 **OpenCode** | Open-source multi-provider coding agent |
| 🌐 **OpenRouter** | 400+ models, open tier available |
| ⚡ **Groq** | Blazing fast inference (~1.5s) |
| 🧠 **Cerebras** | Fast inference, GPT-OSS 120B |

---

## 🎯 How It Works

```
Agent (Claude Code / Gemini / OpenCode)
    ↓
Bifrost CLI (auto-detects agents, picks model)
    ↓
Bifrost Gateway (routing rules, fallback chain)
    ↓
Provider 1: Cerebras → ❌ Rate Limit
    ↓ fallback
Provider 2: Groq → ❌ Format Rejected
    ↓ fallback
Provider 3: OpenRouter → ✅ Success ($0)
```

Each agent thinks it's running its own model. The gateway handles everything behind the scenes.

---

## 🖥️ Requirements

| Requirement | Detail |
|------------|--------|
| OS | Windows / Mac |
| Node.js | v18+ |
| GPU | ❌ Not required |
| Anthropic Account | ❌ Not required |
| API Keys | Free — OpenRouter, Groq, Cerebras |

---

## 🚀 Setup

### Step 1 — Install Bifrost Gateway

```bash
npx -y @maximhq/bifrost
```

Open `http://localhost:8080` in your browser.

### Step 2 — Add Three Providers

Get free API keys from:
- [openrouter.ai](https://openrouter.ai) → API Keys → Create
- [console.groq.com](https://console.groq.com) → API Keys → Create
- [cloud.cerebras.ai](https://cloud.cerebras.ai) → API Keys → Create

In Bifrost: **Model Providers** → **Add Provider** → paste key → save. Repeat for all three.

**Result:** 3 providers, 400+ models, $0.

### Step 3 — Install Coding Agents

```bash
npm install -g @anthropic-ai/claude-code
npm install -g @google/gemini-cli
npm install -g opencode-ai
```

Verify:
```bash
claude --version
gemini --version
opencode --version
```

### Step 4 — Install Bifrost CLI

```bash
npx -y @maximhq/bifrost-cli
```

Add to PATH (Windows):
```powershell
$env:PATH += ";C:\Users\Admin\.bifrost\bin"
```
```powershell
[Environment]::SetEnvironmentVariable("PATH", $env:PATH + ";C:\Users\Admin\.bifrost\bin", "User")
```

Close terminal → open new terminal.

### Step 5 — Launch an Agent

```bash
# Set dummy key for Claude Code auth
$env:ANTHROPIC_API_KEY = "dummy-key"

# Launch Bifrost CLI
bifrost
```

- Enter base URL: `http://localhost:8080`
- Skip Virtual Key
- Choose agent → Choose model → Start

---

## 🧪 Tested Results

| Agent | Provider | Model | Speed |
|-------|----------|-------|-------|
| Claude Code | OpenRouter | Nemotron 120B | 1.85s |
| Gemini CLI | Groq | Llama Scout 17B | 1.35s |
| OpenCode | Cerebras | GPT-OSS 120B | 1.22s |

| Agent | System Prompt | Tools |
|-------|--------------|-------|
| Claude Code | 44 tokens | 6 |
| Gemini CLI | 7,118 tokens | 13 |
| OpenCode | 2,584 tokens | 11 |

---

## 🔄 Triple Fallback Setup

### Create Routing Rule

Bifrost → **Routing Rules** → **New Rule**

| Field | Value |
|-------|-------|
| Name | `Route to Open Models` |
| Enable | ON |
| Scope | Global |
| Priority | 0 |
| Conditions | Leave empty (catches all) |

**Targets:**

| Priority | Provider | Model |
|----------|----------|-------|
| Primary | Cerebras | `zai-glm-4.7` |
| Fallback 1 | Groq | `meta-llama/llama-4-scout-17b-16e-instruct` |
| Fallback 2 | OpenRouter | `google/gemma-4-31b-it:free` |

### What Happens

```
Request → Cerebras (rate limit) → Groq (format rejected) → OpenRouter (success)
```

| Metric | Value |
|--------|-------|
| Provider Success Rate | 43% |
| User Success Rate | **100%** |
| Total Tokens | 55K |
| Total Cost | < $1 |

The agent built a full Flask landing page across three provider failures without noticing.

---

## ⚠️ Known Limitations

| Issue | Detail |
|-------|--------|
| Claude Code + Groq | ❌ `reasoning_effort` parameter rejected |
| Claude Code + Cerebras | ❌ Same parameter issue |
| Gemini CLI + Groq 70B | ❌ 12K TPM limit exceeded (use Scout 17B) |
| Groq multi-turn fallback | ❌ `reasoning_content` in history rejected |
| Codex CLI | Requires paid ChatGPT subscription |
| All providers | Open tier has daily rate limits |

**Rule of thumb:**
- Claude Code → OpenRouter only
- Gemini CLI → All three providers work
- OpenCode → OpenRouter + Cerebras work

---

## 🔧 Cleanup / Reset

```powershell
# Uninstall agents
npm uninstall -g @anthropic-ai/claude-code
npm uninstall -g @google/gemini-cli
npm uninstall -g opencode-ai

# Delete Bifrost data
Remove-Item -Recurse -Force "$env:APPDATA\bifrost"
Remove-Item -Recurse -Force "$env:LOCALAPPDATA\bifrost"
Remove-Item -Recurse -Force "$env:USERPROFILE\.bifrost"

# Clear cache
npx clear-npx-cache

# Clear env vars
Remove-Item Env:ANTHROPIC_API_KEY -ErrorAction SilentlyContinue
```

---

## 🔗 Resources

| Resource | Link |
|----------|------|
| Bifrost GitHub | [github.com/maximhq/bifrost](https://github.com/maximhq/bifrost) |
| Bifrost CLI Docs | [docs.getbifrost.ai/quickstart/cli](https://docs.getbifrost.ai/quickstart/cli/getting-started) |
| OpenRouter | [openrouter.ai](https://openrouter.ai) |
| Groq | [console.groq.com](https://console.groq.com) |
| Cerebras | [cloud.cerebras.ai](https://cloud.cerebras.ai) |
| Claude Code | [docs.anthropic.com](https://docs.anthropic.com/en/docs/claude-code) |
| Gemini CLI | [github.com/google/gemini-cli](https://github.com/google/gemini-cli) |
| OpenCode | [opencode.ai](https://opencode.ai) |

---

## 🔗 Links

[![YouTube](https://img.shields.io/badge/YouTube-NetworkCoder-red?style=for-the-badge&logo=youtube)](https://www.youtube.com/@NetworkCoder)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=for-the-badge&logo=github)](https://github.com/network-tocoder)

---

<p align="center">
  <strong>43% Provider Fail Rate → 100% User Success Rate</strong><br/>
  <em>Three agents. Three providers. One gateway. Zero downtime.</em>
</p>

