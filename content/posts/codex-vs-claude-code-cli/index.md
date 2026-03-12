---
title: "OpenAI Codex vs Claude Code: A CLI-First Comparison"
featuredImage: "posts/codex-vs-claude-code-cli/featured.png"
date: 2026-03-10T13:15:00-04:00
draft: true
tags: ["ai tooling", "developer tools", "cli", "openai", "anthropic"]
description: "A deep comparison of Codex CLI and Claude Code CLI, focused on terminal workflows, safety controls, extensibility, and agentic behavior."
---

## Agentic Software Engineering 

The pace of progress in Agentic Software Engineering over the past year has been incredible, with new tooling and capabilities being released on an almost daily basis. It isn't just the models that are advancing at a rapid pace, with innovation happening at the tooling & harness level that is redefining the entire Software Development Lifecycle. Everyone seemed to have noticed this at around the same time at the beginning of 2026 and as ["Claude Code is the Inflection Point"](https://newsletter.semianalysis.com/p/claude-code-is-the-inflection-point) lays out clearly it doesn't look like it is slowing down anytime soon. For a while it looked like Anthropic had hit an inflection point with Opus 4.5+ and Claude Code but the recent [release of GPT 5.4](https://openai.com/index/introducing-gpt-5-4/) by OpenAI feels like it leveled the playing field in terms of model capabilities causing me to give Codex a new look.

First a quick detour to understand what I mean by "Agentic" Software Engineering, and why this is such a profound, possibly even generational shift:

Since the release of Github Copilot in 2021-2022, and ChatGPT in 2022-2023, much of the tasks that software engineers had traditionally been responsible for have undergone various levels of automation. [Bassim Eledath's "The 8 Levels of Agentic Engineering"](https://www.bassimeledath.com/blog/levels-of-agentic-engineering) does an excellent job of explaining the levels of maturity Software Engineers can achieve with the current generation of tooling. His argument is that the frontier is no longer basic autocomplete or IDE chat, but the jump into Level 5 and beyond: skills, MCP, harnesses, feedback loops, background agents, and eventually agent teams. That is the level where these tools stop feeling like clever assistants and start affecting the shape of everyday engineering work. `codex` and `claude` both now operate in that territory, which makes this comparison less about raw model ranking and more about operating model: how they handle trust, memory, delegation, review, and automation when the work is happening in a real repository, with real consequences.


## Comparing Claude Code and Codex CLI

{{< figure src="./two-buttons-usage-vs-speed.jpg" alt="Two buttons meme about choosing between higher usage limits and faster, terser replies" >}}

Before getting into the feature-by-feature comparison. I compared both tools with their respective Pro plans at roughly the same consumer price point: $20/month USD.

In my own usage, OpenAI Codex let me get a lot more real work done before I hit usage limits than Claude Code did. That was true even while I was using Codex with the latest GPT-5.4 model. I do not want to stretch that into a universal claim about quota policy, because limits can change and account state matters. But as a day-to-day product experience, it mattered a lot.

I also noticed two tradeoffs:

- Codex tended to be more verbose than Claude Code.
- Codex responses sometimes felt more sluggish.

I didn't try `fast` mode on either tool as the choice to do so felt wasteful given that the token costs were 1.5-2x and I wasn't in a rush.


## Convergent Evolution

Both Claude Code and Codex CLI go far beyond a "chat bot in the terminal" and both take slightly different approaches to answer the following needs:

- How do you keep agents inside safe boundaries
- Can you give them the right local context
- How does it connect to your external systems & context sources
- How can they be used to script repeatable workflows and long form work

In order to accomplish this each tool has a slightly different approach, but overall the experience is nearly identical

| Feature | Codex CLI | Claude Code CLI |
| --- | --- | --- |
| MCP support | `~/.claude.json` | `~/.codex/config.toml` |
| Skills | `.agents/skills` | `.claude/skills` |
| Repo instruction files | `AGENTS.md` | `CLAUDE.md` |
| Non-interactive mode | `codex exec` | `claude -p` / `--print` |
| Session continuation | `/resume`, `/fork` | `--continue`, `--resume`, `/resume` |
| Specialized agents | multi-agents | subagents / `--agents` |
| Local file references | `/mention` | `@path` autocomplete |

## The First Real Difference: How They Handle Trust

The first serious question with a coding agent is trust and this is also one of the clearest product philosophy differences between the two tools.

You want to know:

- Can this tool inspect the repo without editing it?
- Can it edit files but stop before risky commands?
- Can I let it run unattended in CI without accidentally giving it the keys to everything?

Codex and Claude both answer those questions, but they do it differently.

### Codex: compact and legible

Codex puts approval policy and sandbox mode near the center of the product. The docs present a [compact matrix](https://developers.openai.com/codex/agent-approvals-security#common-sandbox-and-approval-combinations): read-only, workspace-write, and danger-full-access style modes, combined with approval behavior such as asking for untrusted or risky actions.

This gives Codex a clean mental model. You can usually explain the current operating state in one sentence:

"This run can edit the workspace, cannot roam freely outside it, and must ask before risky commands."

### Claude Code: deeper policy surface

Claude Code exposes a more layered model. The [docs](https://code.claude.com/docs/en/permissions#permission-modes) describe permission rules such as `allow`, `ask`, and `deny`, separate permission modes such as `default`, `acceptEdits`, `plan`, `dontAsk`, and `bypassPermissions`, and separate sandboxing controls.

It is more expressive and customizable but it also means the mental model is heavier.

## Context Customizability

After trust, the next question is what each tool will remember about how the repo works, and what context it loads when it starts.

Both tools support repository-local instructions since that is a table stakes feature. The difference is how far they take it.

### Codex: file-based and explicit

Codex uses the [AGENTS.md](https://agents.md/) standard, with `AGENTS.md`, `AGENTS.override.md`, and nested overrides near the current work area. That is a straightforward model:

- put global repo instructions near the root
- add more specific instructions near subdirectories
- let the closer instruction win

It is easy to teach and easy to audit. This also means that id a prompt behaves strangely, you can usually find the relevant instruction file without much digging.

### Claude Code: layered and long-lived

Claude Code also supports repo instruction files, but the documented system is broader. The docs describe `CLAUDE.md`, imports using `@path/to/import`, `.claude/rules/` files with `paths` frontmatter, and [auto memory](https://code.claude.com/docs/en/memory#claude-md-vs-auto-memory) tied to a project or working tree.

That makes Claude Code more capable in large, uneven repos where different areas need different operating rules and the approach is bespoke and non-standard.

### Memory comparison table

| Area | Codex CLI | Claude Code CLI | Practical takeaway |
| --- | --- | --- | --- |
| Base repo instructions | `AGENTS.md` | `CLAUDE.md` | Both support shared project guidance |
| Local overrides | ✅ | ✅ | Both can narrow context by directory |
| Imported instruction files | ❌ | ✅ | Claude supports more modular rule composition |
| Path-pattern rules | ❌ | ✅ | Claude supports more targeted scoping |
| Auto memory | ❌ | ✅ | Claude can accumulate more working context over time |

This seems like a killer feature for Claude Code to have that Codex doesn't support. Workarounds like resuming a session aren't as clean since they can pollute a new conversation/task with unnecessary context. I stumbled across the [Locus](https://github.com/Magnifico4625/locus) project which attempts to fix this across agent harnesses but didn't get a chance to test it myself.

## Extensibility: Both Are Serious, But One Is Wider

Both tools support MCP. Both support specialized skills. Both support some notion of multiple agents or delegated work. That already covers a large portion of what serious users want.

Claude Code goes further in the reviewed docs by exposing hooks, plugins, plugin marketplaces, and subagents as first-class concepts.

Codex, by contrast, presents a tighter extension story:

- MCP for external systems
- skills for reusable guidance
- multi-agents for delegation

That is a good set of primitives. It is just narrower.

### Extension comparison table

| Area | Codex CLI | Claude Code CLI | Practical takeaway |
| --- | --- | --- | --- |
| MCP | ✅ | ✅ | Shared baseline |
| Skills | ✅ | ✅ | Shared baseline |
| Multi-agent or delegated workflows | ✅ | ✅ | Shared baseline |
| Hooks | ❌ | ✅ | Claude can intercept or augment tool lifecycle events |
| Plugins | ❌ | ✅ | Claude has a larger extension surface |
| Marketplace model | ❌ | ✅ | Claude is closer to a platform model |

## What About Codex App Server as a Hooks alternative?

One nuance worth adding here: if you are comparing Codex to Claude hooks, the closest OpenAI feature is probably [Codex App Server](https://developers.openai.com/codex/app-server), but only if you are willing to build your own client around it.

Claude hooks are a first-class CLI feature. You configure hook events like `PreToolUse`, `PostToolUse`, `PermissionRequest`, `Stop`, `SessionStart`, and others in Claude Code, and then run shell commands, HTTP handlers, prompts, or agent hooks automatically inside that lifecycle.

Codex App Server is different. OpenAI describes it as the protocol Codex uses to power rich clients like the Codex VS Code extension. It exposes a bidirectional JSON-RPC interface over `stdio` or WebSocket, streamed agent events, approval requests, thread and turn lifecycles, dynamic tool calls, and skill discovery. The docs are explicit that if you are automating jobs or running Codex in CI, you should use the Codex SDK instead.

With Claude Code, you stay in the CLI and configure hooks.

With Codex App Server, you are closer to building the thing that hosts Codex. That can be more powerful if you want your own app, your own approval UX, your own event handling, or your own client-side orchestration. It is also more work, and it is not the right comparison if the question is simply "which CLI gives me built-in hooks out of the box?"

Given all of that, according to an OpenAI team comment, [native hooks are planned for Codex](https://github.com/openai/codex/issues/2109#issuecomment-3806569090), so this comparison may age quickly if that lands in the CLI itself.

## Automation and CI: Both Are Built for Extensibility

Automation is another part of the comparison that gets flattened too easily.

Codex is strong here, but it is not alone.

Claude Code also supports structured outputs, schema-guided responses, and non-interactive execution. The better comparison is about the shape of automation, not whether one tool can script and the other cannot.

### Automation comparison table

| Area | Codex CLI | Claude Code CLI | Practical takeaway |
| --- | --- | --- | --- |
| Main automation entry point | `codex exec` | `claude -p` / `--print` | Both support headless workflows |
| Structured output | `--json`, JSONL events, `--output-schema`, `-o` | `--output-format json`, `stream-json`, `--json-schema` | Both can feed downstream tooling |
| Session reuse in scripts | ✅ | ✅ | Claude leans harder into interactive-to-script continuity |
| Final artifact output | file-first | stdout-first | Different ergonomics, same general class of capability |

## Is Claude Code still the clear winner?

Given everything I saw its clear to me that this race is tighter than it looks initially.

Claude Code has several characteristics that make it easy to become the default tool inside a technical team like:

- broader policy controls
- richer memory model
- hooks and plugins
- subagents as a first-class pattern
- more obvious path to deep customization

{{< figure src="./distracted-boyfriend-default-choice.jpg" alt="Distracted boyfriend meme about engineers sticking with the familiar default while another tool offers better day-to-day usage headroom" title="Default choice and daily value are not always the same thing." >}}

That is a compelling package for teams that expect their coding agent to become part of their infrastructure.

Codex is similar and in some ways it feels more competitive than that reputation suggests because it is strong in exactly the areas that matter for full time use like clear safety controls, adoption of the AGENTS.md standard, and less cognitive overhead in the core operating model.

My own usage experience adds one more reason to take Codex seriously: I was able to get more work out of the Pro plan before running into limits, even while using GPT-5.4. That does not erase Claude Code's strengths, but it does change the cost-to-actual-usage equation in day-to-day practice.

## Final Take

Claude Code still looks stronger if you care most about extensibility, policy depth, and turning the CLI into a programmable platform.

Codex looks better than its reputation if you care most about having a clean, explicit operating model for real repository work. Its approval controls, sandboxing story, review flow, and automation path make it feel like a serious option rather than a distant second choice.

My own experience pushes that conclusion a bit further. If you are paying for the consumer-tier Pro plans and actually living in the CLI every day, usage headroom matters a lot. Codex felt more available to me. Claude felt terser and faster. That tradeoff is not visible in feature tables, but was very visible in daily use.


## Sources

Primary documentation reviewed for this post:

- OpenAI Codex CLI: https://developers.openai.com/codex/cli
- Claude Code: https://code.claude.com/docs
