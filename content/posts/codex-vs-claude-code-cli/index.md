---
title: "OpenAI Codex vs Claude Code: A CLI-First Comparison"
date: 2026-03-10T13:15:00-04:00
draft: true
tags: ["ai tooling", "developer tools", "cli", "openai", "anthropic"]
description: "A deep comparison of Codex CLI and Claude Code CLI, focused on terminal workflows, safety controls, extensibility, and automation rather than model branding."
---

Software engineers do not use coding agents in the abstract. They use them in real repositories, with real shell history, real test suites, real secrets, and real cleanup costs when something goes wrong.

That is why the most useful comparison between OpenAI Codex and Claude Code is not "which company has the better model?" It is "which CLI gives me the better operating model for day-to-day engineering?"

That question has become more interesting in 2026. Claude Code still appears to be the default choice for many engineers. Codex, especially paired with [GPT-5.4](https://openai.com/index/introducing-gpt-5-4/), is much closer than it was when the early comparison was mostly about model taste. If you stay inside the terminal and ignore the branding war, the gap is narrower and the tradeoffs are clearer.

This draft focuses on the `codex` CLI and the `claude` CLI specifically. It does not try to rank the underlying models. It compares what the tools expose in the terminal: safety controls, repo memory, extension surfaces, review flows, and automation paths.

I also want to separate two kinds of evidence in this piece:

- documented product capabilities
- personal usage experience

The first is easier to verify. The second is still useful, especially when it captures what the tools feel like over a week of real work instead of a single benchmark prompt.

## Scope

This comparison uses official documentation reviewed on March 10, 2026.

The rules for the comparison are simple:

- CLI only
- Prefer documented capabilities over anecdotes
- Treat popularity as context, not evidence
- Separate direct facts from interpretation

## The Short Version

If you want the shortest honest answer, it looks like this:

| Question | Codex CLI | Claude Code CLI |
| --- | --- | --- |
| What does it feel like? | An opinionated coding agent with explicit session controls | A programmable terminal agent with a broader policy surface |
| Best early strength | Clear approval and sandbox model | Rich extension and memory model |
| Best fit | Engineers who want a strong default operating model with less setup | Engineers and teams who want deeper customization and policy control |
| Biggest built-in advantage | Direct planning, diff, review, and automation flow | Hooks, plugins, subagents, and richer repo memory |

That summary is an interpretation, not a vendor claim. The rest of the post is the argument for it.

## My Usage Experience

Before getting into the feature-by-feature comparison, I should add one piece of anecdotal evidence from using both Pro plans at roughly the same consumer price point: $20/month in the US.

In my own usage, OpenAI Codex allowed for a lot more actual work before I ran into usage limits than Claude Code did. That was true even while using Codex with the latest GPT-5.4 model. I cannot generalize that into a universal claim about quota policy, because usage limits depend on timing, account state, and product changes. Still, as a user experience point, it mattered.

I also noticed two tradeoffs:

- Codex tended to be more verbose than Claude Code.
- Codex responses sometimes felt more sluggish.

That combination created an interesting split in practice. Codex often felt more available, while Claude often felt more concise and faster to read. If you spend hours a day in these tools, that difference affects how tiring they feel, not just how capable they are.

### Anecdotal experience table

| Area | My experience with Codex | My experience with Claude Code | Caveat |
| --- | --- | --- | --- |
| Usage headroom on the $20 US plan | Higher | Lower | Personal experience only, not a platform-wide claim |
| Response verbosity | More verbose | More concise | Subjective, but consistent in my usage |
| Perceived responsiveness | Sometimes more sluggish | Often snappier | Subjective and likely sensitive to task shape and load |

This section should stay in the final post, but it should remain clearly labeled as usage experience rather than documented fact.

{{< figure src="./two-buttons-usage-vs-speed.jpg" alt="Two buttons meme about choosing between higher usage limits and faster, terser replies" title="The practical tradeoff I felt most often on the $20 plans." >}}

## A Useful Mental Model

The easiest way to compare these tools is to ask four questions:

| Question | Codex CLI | Claude Code CLI |
| --- | --- | --- |
| How do I steer autonomy? | Approval policy plus sandbox mode | Permission rules, permission modes, and sandboxing |
| How do I teach repo conventions? | `AGENTS.md` and local overrides | `CLAUDE.md`, imports, rules, and auto memory |
| How do I extend it? | MCP, skills, and multi-agents | MCP, skills, hooks, plugins, and subagents |
| How do I automate it? | `codex exec`, JSON events, schema output | `claude -p`, JSON output, stream-json, resume/continue |

That framing matters because most engineering work with these tools reduces to those four tasks:

- keep the agent inside safe boundaries
- give it the right local context
- connect it to your systems
- script repeatable workflows

## Where They Are Clearly Similar

Both tools have moved well beyond "chat bot in a terminal."

| Area | Codex CLI | Claude Code CLI | Why it matters |
| --- | --- | --- | --- |
| Terminal-native workflow | Yes | Yes | Both are designed for repository work, not browser chat |
| MCP support | Yes | Yes | Both can reach external systems through MCP |
| Repo instruction files | `AGENTS.md` | `CLAUDE.md` | Both support persistent project guidance |
| Non-interactive mode | `codex exec` | `claude -p` / `--print` | Both can be used in scripts and CI |
| Session continuation | `/resume`, `/fork` | `--continue`, `--resume`, `/resume` | Both can reuse context across runs |
| Specialized agents | Multi-agents | Subagents / `--agents` | Both support task-specific delegation |
| Local file references | `/mention` | `@path` autocomplete | Both reduce copy-paste friction |

If you have only used web chat products, this shared baseline is already a big shift. Both tools assume the important unit of work is a repository and an execution environment, not a single prompt.

## The First Real Difference: How They Handle Trust

The first serious question with a coding agent is not intelligence. It is trust.

You want to know:

- Can this tool inspect the repo without editing it?
- Can it edit files but stop before risky commands?
- Can I let it run unattended in CI without accidentally giving it the keys to everything?

Codex and Claude both answer those questions, but they do it differently.

### Codex: compact and legible

Codex puts approval policy and sandbox mode near the center of the product. The docs present a compact matrix: read-only, workspace-write, and danger-full-access style modes, combined with approval behavior such as asking for untrusted or risky actions.

That gives Codex a clean mental model. You can usually explain the current operating state in one sentence:

"This run can edit the workspace, cannot roam freely outside it, and must ask before risky commands."

That clarity matters in practice. Engineers are more likely to use safety controls consistently when they can understand them quickly.

### Claude Code: deeper policy surface

Claude Code exposes a more layered model. The docs describe permission rules such as `allow`, `ask`, and `deny`, separate permission modes such as `default`, `acceptEdits`, `plan`, `dontAsk`, and `bypassPermissions`, and separate sandboxing controls.

That is more expressive. It also means the mental model is heavier.

For an individual engineer, Codex may be easier to reason about. For a team that wants to shape policy precisely, Claude Code gives more knobs.

### Safety comparison table

| Area | Codex CLI | Claude Code CLI | Practical takeaway |
| --- | --- | --- | --- |
| Primary safety frame | Approval policy + sandbox mode | Permission rules + permission modes + sandboxing | Codex is simpler to explain; Claude is more configurable |
| Preset operating modes | Strongly emphasized | Present, but wrapped in a larger permissions system | Codex feels more opinionated |
| Fine-grained allow/ask/deny rules | Not the main documented surface in the reviewed docs | First-class documented concept | Claude has more explicit policy depth |
| Best fit | Fast local reasoning about what the agent may do | Team-shaped policy and more explicit restrictions | Choose based on how much control detail you need |

This is one of the main places where the tools express different product philosophies.

## Repo Memory: Simple Instructions vs Layered Context

The next question is not "what can the tool do?" It is "what will it remember about how this repo works?"

Both tools support repository-local instructions. That is table stakes now. The difference is how far they take it.

### Codex: file-based and explicit

In the material reviewed for this post, Codex uses `AGENTS.md`, `AGENTS.override.md`, and nested overrides near the current work area. That is a straightforward model:

- put global repo instructions near the root
- add more specific instructions near subdirectories
- let the closer instruction win

That is easy to teach and easy to audit. If a repo behaves strangely, you can usually find the relevant instruction file without much digging.

### Claude Code: layered and long-lived

Claude Code also supports repo instruction files, but the documented system is broader. The docs describe `CLAUDE.md`, imports using `@path/to/import`, `.claude/rules/` files with `paths` frontmatter, and auto memory tied to a project or working tree.

That makes Claude Code more capable in large, uneven repos where different areas need different operating rules. It also means the context system has more moving parts.

### Memory comparison table

| Area | Codex CLI | Claude Code CLI | Practical takeaway |
| --- | --- | --- | --- |
| Base repo instructions | `AGENTS.md` | `CLAUDE.md` | Both support shared project guidance |
| Local overrides | Yes | Yes | Both can narrow context by directory |
| Imported instruction files | Not documented in the reviewed Codex sources | Yes | Claude supports more modular rule composition |
| Path-pattern rules | Not documented in the reviewed Codex sources | Yes | Claude supports more targeted scoping |
| Auto memory | Not documented in the reviewed Codex sources | Yes | Claude can accumulate more working context over time |

If I were standardizing a small or medium repo quickly, I would expect Codex to have the lower setup burden. If I were building a heavily customized team workflow across several codebases, Claude Code's memory system looks stronger on paper.

## Extensibility: Both Are Serious, But One Is Wider

This is the part where Claude Code still looks ahead.

Both tools support MCP. Both support specialized skills. Both support some notion of multiple agents or delegated work. That already covers a large portion of what serious users want.

Claude Code goes further in the reviewed docs by exposing hooks, plugins, plugin marketplaces, and subagents as first-class concepts.

Codex, by contrast, presents a tighter extension story:

- MCP for external systems
- skills for reusable guidance
- multi-agents for delegation

That is a good set of primitives. It is just not as broad.

### Extension comparison table

| Area | Codex CLI | Claude Code CLI | Practical takeaway |
| --- | --- | --- | --- |
| MCP | Yes | Yes | Shared baseline |
| Skills | Yes | Yes | Shared baseline |
| Multi-agent or delegated workflows | Yes | Yes | Shared baseline |
| Hooks | Not documented in the reviewed Codex sources | Yes | Claude can intercept or augment tool lifecycle events |
| Plugins | Not documented in the reviewed Codex sources | Yes | Claude has a larger extension surface |
| Marketplace model | Not documented in the reviewed Codex sources | Yes | Claude is closer to a platform model |

This is one of the clearest documented differences in the current generation of the tools.

The interpretation I would keep in the final version is narrow:

- Claude Code has the wider extension surface.
- Codex has the tighter and easier-to-survey extension story.

That is enough. There is no need to inflate it into a universal winner.

## Interactive Workflow: Codex Is More Direct About Review

One place where Codex looks stronger than many engineers may expect is the built-in command surface inside the CLI itself.

The Codex docs expose a broad set of interactive commands around planning, permissions, diffs, reviews, process inspection, MCP, and agent switching. The list matters less than the shape of it. Codex treats the CLI as a control room.

Claude Code also has substantial built-in commands. The difference is that more of its power appears to route through the larger ecosystem of skills, plugins, rules, and subagents.

That becomes clearest in code review.

### Review flow comparison table

| Area | Codex CLI | Claude Code CLI | Practical takeaway |
| --- | --- | --- | --- |
| Built-in working tree review | `/review` | `/review` deprecated in current docs | Codex is more direct here |
| Diff-oriented command flow | Yes | Yes | Both support inspection |
| Security-specific review | Not the emphasis of the reviewed surface | `/security-review` documented | Claude has a more explicit security review entry point |
| Plugin or skill-based review | Less emphasized in reviewed docs | More emphasized in current docs | Claude review is more ecosystem-shaped |

If your workflow is "make changes, inspect diff, run review, then decide what to fix," Codex currently looks more straightforward.

If your workflow is "compose custom review flows from plugins, subagents, and repo policy," Claude Code looks more flexible.

## Automation and CI: Both Are Real Tools, Not Toys

This is another place where the conversation is easy to oversimplify.

Codex is strong in automation, but it is not alone there.

Claude Code also supports structured outputs, schema-guided responses, and non-interactive execution. The more accurate comparison is about the shape of automation, not whether one tool "can script" and the other cannot.

### Automation comparison table

| Area | Codex CLI | Claude Code CLI | Practical takeaway |
| --- | --- | --- | --- |
| Main automation entry point | `codex exec` | `claude -p` / `--print` | Both support headless workflows |
| Structured output | `--json`, JSONL events, `--output-schema`, `-o` | `--output-format json`, `stream-json`, `--json-schema` | Both can feed downstream tooling |
| Session reuse in scripts | Present | Strongly emphasized with continue/resume flags | Claude leans harder into interactive-to-script continuity |
| Final artifact output | Explicit file output is foregrounded | Structured print output is foregrounded | Different ergonomics, same general class of capability |

My current read is this:

- Codex looks strong when you want "agent as command."
- Claude Code looks strong when you want "interactive agent that can also drop into script mode without changing tools."

That is a real difference in feel, even though both are capable.

## So Why Does Claude Code Still Feel Like the Default?

I would not write this section as a hard claim about market share unless I had outside usage data. What I would say is narrower:

Claude Code has several characteristics that make it easy to become the default tool inside a technical team:

- broader policy controls
- richer memory model
- hooks and plugins
- subagents as a first-class pattern
- more obvious path to deep customization

{{< figure src="./distracted-boyfriend-default-choice.jpg" alt="Distracted boyfriend meme about engineers sticking with the familiar default while another tool offers better day-to-day usage headroom" title="Default choice and daily value are not always the same thing." >}}

That is a compelling package for teams that expect their coding agent to become infrastructure.

Codex is more competitive than that reputation suggests because it is strong in exactly the areas that matter once you actually live in the terminal:

- clear safety controls
- direct planning and review workflow
- solid automation path
- less cognitive overhead in the core operating model

My own usage experience adds one more reason to take Codex seriously: I was able to get more work out of the Pro plan before running into limits, even while using GPT-5.4. That does not erase Claude Code's strengths, but it does change the cost-to-actual-usage equation in day-to-day practice.

That is why this has become a real comparison rather than a one-line dismissal.

## Which Tool Fits Which Engineering Culture?

This is the section where a lot of comparisons go wrong. They try to pick one universal winner. That misses the point.

### Codex CLI is a better fit when:

- you want a strong built-in workflow with less configuration
- you care a lot about a clear approval and sandbox model
- you want planning, diffing, review, and automation to feel like one coherent product
- you want to explain the tool's operating state quickly to another engineer

### Claude Code CLI is a better fit when:

- you want deeper policy shaping
- you want richer repository memory and path-specific rules
- you expect hooks, plugins, and subagents to become part of your standard engineering workflow
- you are treating the coding agent as an extensible platform, not only a terminal assistant

### Team fit table

| Team or use case | Better first look | Why |
| --- | --- | --- |
| Individual engineer in one repo | Codex CLI | Lower cognitive overhead, direct built-in controls |
| Small team with shared conventions | Codex CLI | Easier to explain and standardize quickly |
| Large repo with uneven local rules | Claude Code CLI | Richer memory and path-specific rule model |
| Team building custom agent workflows | Claude Code CLI | Hooks, plugins, and broader extension surface |
| CI-heavy structured automation | Tie | Both have strong machine-readable output paths |
| Diff and review centered local workflow | Codex CLI | Built-in review flow is more direct in current docs |

## What I Would Show With Images Later

No images are needed for this draft, but the final post would benefit from a few diagrams.

1. A split terminal screenshot showing Codex on one side with planning, permissions, and review commands, and Claude Code on the other with permissions, sandboxing, agents, and plugin-oriented controls.
2. A repo tree showing `AGENTS.md` overlays compared with `CLAUDE.md`, imports, and path-scoped rules.
3. A matrix with two axes: "easy to reason about" and "highly configurable."
4. A small CI diagram comparing `codex exec` and `claude -p` as structured automation steps.
5. A meme break after the anecdotal section using a "Two buttons" template.
6. A meme break near the "default choice" section using a "Distracted Boyfriend" template.

## Final Take

Claude Code still looks stronger if you care most about extensibility, policy depth, and turning the CLI into a programmable platform.

Codex looks stronger than its reputation if you care most about having a clean, explicit operating model for real repository work. Its approval controls, sandboxing story, review flow, and automation path make it a serious option rather than a distant second choice.

My own experience pushes that conclusion a bit further. If you are paying for the consumer-tier Pro plans and actually living in the CLI every day, usage headroom matters a lot. Codex felt more available to me. Claude felt terser and faster. That tradeoff is not visible in feature tables, but it is very visible in daily use.

If I had to compress the comparison into one line, it would be this:

Claude Code currently looks broader. Codex currently looks tighter.

That sounds small, but it captures a lot. In terminal tools, breadth and tightness are different virtues. Which one matters more depends on whether you want your coding agent to behave like a well-designed power tool or like an extensible operating environment.

## Sources

Primary documentation reviewed for this draft:

- OpenAI Codex CLI: https://developers.openai.com/codex/cli
- OpenAI Codex slash commands: https://developers.openai.com/codex/cli/slash-commands
- OpenAI Codex CLI reference: https://developers.openai.com/codex/cli/reference
- OpenAI Codex approvals and security: https://developers.openai.com/codex/agent-approvals-security
- OpenAI Codex `AGENTS.md`: https://developers.openai.com/codex/guides/agents-md
- OpenAI Codex MCP: https://developers.openai.com/codex/mcp
- OpenAI Codex multi-agent: https://developers.openai.com/codex/multi-agent
- OpenAI Codex non-interactive mode: https://developers.openai.com/codex/noninteractive
- OpenAI Codex skills: https://developers.openai.com/codex/skills
- Claude Code overview: https://code.claude.com/docs/en/overview
- Claude Code interactive mode: https://code.claude.com/docs/en/interactive-mode
- Claude Code settings: https://code.claude.com/docs/en/settings
- Claude Code memory: https://code.claude.com/docs/en/memory
- Claude Code hooks: https://code.claude.com/docs/en/hooks
- Claude Code MCP: https://code.claude.com/docs/en/mcp
- Claude Code programmatic usage: https://code.claude.com/docs/en/programmatic-usage
- Claude Code subagents: https://code.claude.com/docs/en/sub-agents
- Claude Code plugins reference: https://code.claude.com/docs/en/plugins-reference
