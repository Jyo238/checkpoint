# Checkpoint

**Never lose what your AI learned.**

Checkpoint is a persistent memory archival skill for AI coding assistants. It systematically captures breakthroughs, failed approaches, user preferences, and project context — so your AI never solves the same problem twice.

[繁體中文](README.zh-TW.md)

---

## The Problem

AI coding assistants are brilliant within a single conversation — but start a new session and everything is forgotten:

- **Session 1**: Spend 30 minutes discovering the API requires site-based paths
- **Session 2**: Spend 30 minutes rediscovering the same thing
- **Session 3**: You're frustrated. The AI has amnesia.

## The Solution

Checkpoint creates a structured, persistent memory system that survives across conversations:

```
You:  /checkpoint
AI:   Scanning conversation... Found 3 new discoveries:

      1. [SAVED] project_auth_config.md
         - Tenant blocks app consent, must use pre-authorized client ID
         - Type: project

      2. [SAVED] feedback_no_admin.md
         - Never suggest "ask your admin" — user has no admin rights
         - Type: feedback

      3. [UPDATED] reference_api_paths.md
         - Added newly discovered endpoint paths
         - Type: reference

      Memory index updated. 3 entries saved/updated.
```

## How It Works

Checkpoint operates on two layers:

### Active Layer: `/checkpoint` Command
Explicitly trigger a full knowledge archival. The AI will:
1. **Scan** the conversation for all discoveries
2. **Check** existing memory to avoid duplicates
3. **Save** new findings as structured memory files
4. **Verify** completeness

### Passive Layer: Anti-Amnesia Protocol
A set of rules added to your `CLAUDE.md` (or equivalent) that runs automatically:
- **Before work**: Check memory for prior discoveries
- **After breakthroughs**: Save immediately, don't wait
- **Before retries**: Check if already solved — don't rediscover

## Memory Types

| Type | Purpose | Example |
|------|---------|---------|
| **user** | Who you are | "Senior backend engineer, new to React" |
| **feedback** | What you corrected | "Don't mock the database in integration tests" |
| **project** | Context behind the work | "Auth rewrite is compliance-driven, not tech debt" |
| **reference** | Where to find things | "Pipeline bugs tracked in Linear project INGEST" |

## Installation

### Claude Code

```bash
claude install-skill Jyo238/checkpoint
```

This installs:
- `/checkpoint` slash command
- Skill definition for the AI to follow
- Reference docs for memory types and anti-amnesia protocol

**Optional**: Add the Anti-Amnesia Protocol to your project's `CLAUDE.md`:

```bash
cat skills/checkpoint/references/anti-amnesia-protocol.md >> CLAUDE.md
```

### Cursor

Copy the rule file to your project:

```bash
mkdir -p .cursor/rules
cp cursor/rules/checkpoint.mdc .cursor/rules/
```

### VSCode / GitHub Copilot

Copy the instruction and prompt files:

```bash
cp vscode/instructions/checkpoint.md .github/copilot-instructions.md
cp vscode/prompts/checkpoint.prompt.md .github/prompts/
```

### OpenAI Codex CLI

```bash
cp codex/checkpoint/SKILL.md your-project/
cp .codex/INSTALL.md your-project/.codex/
```

### Manual Installation (Any AI Tool)

1. Copy the content from `skills/checkpoint/SKILL.md` into your AI tool's system prompt or instruction file
2. Copy the Anti-Amnesia Protocol from `skills/checkpoint/references/anti-amnesia-protocol.md` into your project config
3. Create a `memory/` directory with an empty `MEMORY.md` index file

## File Structure

```
checkpoint/
├── skills/checkpoint/
│   ├── SKILL.md                          # Core skill definition
│   └── references/
│       ├── anti-amnesia-protocol.md      # Passive defense for CLAUDE.md
│       └── memory-types.md              # Detailed guide to 4 memory types
├── commands/
│   └── checkpoint.md                    # /checkpoint command trigger
├── cursor/rules/
│   └── checkpoint.mdc                   # Cursor AI rule
├── vscode/
│   ├── instructions/checkpoint.md       # Copilot instructions
│   └── prompts/checkpoint.prompt.md     # Copilot prompt
├── codex/checkpoint/
│   └── SKILL.md                         # Codex CLI version
├── .claude-plugin/
│   ├── plugin.json                      # Claude Code plugin manifest
│   └── marketplace.json                 # Marketplace metadata
├── .codex/
│   └── INSTALL.md                       # Codex install guide
├── .github/workflows/
│   └── release.yml                      # Auto-release on tag
├── LICENSE                              # MIT
├── README.md                            # This file
└── README.zh-TW.md                      # 繁體中文版
```

## Real-World Impact

This skill was born from real pain. During a multi-session project working with OneNote APIs, the AI repeatedly:
- Forgot that shared notebooks require site-based API paths (`/sites/{id}/onenote/`)
- Forgot that the tenant blocks app consent and needs a pre-authorized client ID
- Forgot the user has no admin rights and kept suggesting "ask your admin"

Each rediscovery wasted 20-30 minutes. After implementing Checkpoint, the AI reads its memory at the start of every session and skips straight to working solutions.

**Before Checkpoint**: 3 sessions x 30 min rediscovery = 90 min wasted
**After Checkpoint**: 0 min wasted. Memory is loaded in seconds.

## Quick Start (30 seconds)

```bash
# Claude Code
claude install-skill Jyo238/checkpoint

# Then in any conversation:
/checkpoint
```

That's it. Your AI will scan the conversation, extract key discoveries, and save them as persistent memory files. Next session, it reads them automatically.

## Use Cases

| Scenario | Without Checkpoint | With Checkpoint |
|----------|-------------------|-----------------|
| Multi-day debugging | Rediscover root cause every session | AI reads prior findings, picks up where it left off |
| Team onboarding | Each person's AI re-learns the same gotchas | Shared memory files capture tribal knowledge |
| Complex API integration | Forget auth quirks, endpoint patterns | All discoveries persisted and loaded automatically |
| User preferences | "I told you not to do that" (again) | Feedback saved once, respected forever |

## Contributing

Contributions are welcome! Feel free to:
- Add support for more AI coding tools
- Improve the memory type system
- Share your Anti-Amnesia Protocol variations
- Report issues or suggest improvements

## License

[MIT](LICENSE)
