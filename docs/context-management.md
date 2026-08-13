# Context management and optimization

<figure class="gdd-hero">
  <img src="../img/5-hero-context.png" alt="Title slide: Context management and optimization">
</figure>

<div class="gdd-speaker">
  <div>
    <strong>Xavier Morris</strong>
    <span>Software Solution Engineer, Microsoft</span>
  </div>
</div>

## Why context is the real bottleneck

Every agent surface you saw in the previous session — editor, CLI, app,
cloud agent, SDK — shares the same underlying constraint: a finite context
window. As a session runs longer, more of that window fills up with tool
output, file contents, and conversation history, leaving less room for the
model to actually reason well. Poorly managed context shows up as slower
responses, higher cost, and — worst of all — an agent that starts forgetting
earlier decisions or re-reading the wrong files.

Context management is the discipline of keeping an agent fast, affordable,
and accurate as a task grows in size and duration.

## Where context comes from

<div class="gdd-feature-grid">
  <div class="gdd-feature">
    <h4>System &amp; instructions</h4>
    <p>Custom instructions, agent skills, and persona/style guidance loaded at session start.</p>
  </div>
  <div class="gdd-feature">
    <h4>Repository content</h4>
    <p>File reads, search results, and directory listings pulled in as the agent explores the codebase.</p>
  </div>
  <div class="gdd-feature">
    <h4>Tool output</h4>
    <p>Command output, test results, API responses — often the largest and noisiest contributor.</p>
  </div>
  <div class="gdd-feature">
    <h4>Conversation history</h4>
    <p>Every prior turn, including intermediate reasoning, that the model may need to recall.</p>
  </div>
</div>

## Techniques that keep sessions healthy

- **Auto-compaction** — summarising older turns once the session nears a
  threshold (e.g. ~95% of the context limit) so the agent keeps a compressed
  memory of what happened rather than losing it outright.
- **Scoped tool calls** — asking for a targeted `grep`/file range instead of
  dumping whole files or directories into context.
- **Session isolation** — using a fresh worktree or session per task (rather
  than one long-running mega-session) so unrelated context doesn't leak in.
- **Retrieval over recall** — re-fetching the exact file or docs section
  needed at the point of use, instead of trying to keep everything "in mind"
  from the start.
- **Prompt and instruction hygiene** — keeping custom instructions and agent
  skills concise and specific; verbose boilerplate steals space from the
  parts of context that actually matter for the task at hand.
- **Rewind / checkpoint** — being able to roll a session's conversation and
  files back to an earlier point (without relying purely on Git) so a bad
  branch of exploration doesn't have to be paid for in tokens forever.

## Cost and performance follow context

Every extra token in context is a token that has to be processed on every
subsequent turn — it isn't free just because it was "already there." That
means context choices are also cost and latency choices:

!!! note "Rule of thumb"
    Treat context like a budget, not a bucket. Before letting an agent read
    a large file or paste in verbose tool output, ask whether a smaller,
    targeted read would do — the answer is usually yes.

## A real-world case study: optimizing GitHub's own agentic workflows

The theory above plays out at scale in GitHub's own repositories. GitHub runs
hundreds of agentic workflows for CI and maintenance — triaging issues,
reviewing pull requests, checking compiler quality — and in April 2026 the
team started systematically measuring and cutting their token usage. The
[full write-up](https://github.blog/ai-and-ml/github-copilot/improving-token-efficiency-in-github-agentic-workflows/)
is worth reading end to end, but three findings map directly onto the
techniques above.

<div class="gdd-feature-grid">
  <div class="gdd-feature">
    <h4>Unused tools are expensive to carry</h4>
    <p>Every registered MCP tool's name and JSON schema rides along on every single API call, whether it's used or not. A 40-tool GitHub MCP server can add 10–15 KB of schema per turn — pure overhead if the agent only ever calls two or three of them. Pruning unused tools cut 8–12 KB of per-call context with no change in behaviour.</p>
  </div>
  <div class="gdd-feature">
    <h4>The cheapest LLM call is the one you don't make</h4>
    <p>Deterministic data-gathering — fetching a PR diff, issue metadata, or file contents — doesn't need a model in the loop at all. Moving those reads into plain <code>gh</code> CLI calls before the agent starts (or via a lightweight proxy) removes a full reasoning round-trip per fetch. One workflow saw a sustained <strong>62% reduction</strong> in effective token cost this way.</p>
  </div>
  <div class="gdd-feature">
    <h4>Not all tokens cost the same</h4>
    <p>Output tokens are roughly 4x the weighted cost of input tokens, and cache-read tokens are a fraction of the cost of fresh input. GitHub uses an "Effective Tokens" metric — <code>ET = m × (1.0×I + 0.1×C + 4.0×O)</code> — to normalise savings across model tiers, so a 10% ET reduction reflects a genuine cost reduction rather than a model swap in disguise.</p>
  </div>
</div>

The broader lesson for any agent session — not just scheduled CI workflows —
is the same one from earlier in this page: **scope what the agent can see
and call**, prefer deterministic retrieval over agentic tool calls where the
answer is fixed, and measure before you optimise. A misconfigured rule or an
unused tool can silently double your cost without anyone noticing until
someone looks at the numbers.

Learn more: [Improving token efficiency in GitHub Agentic Workflows](https://github.blog/ai-and-ml/github-copilot/improving-token-efficiency-in-github-agentic-workflows/).

## Optimizing your own day-to-day AI usage

The GitHub case study above is about scheduled CI workflows, but the same
principles apply directly to your everyday Copilot sessions. GitHub's
[Optimizing your AI usage](https://docs.github.com/en/copilot/tutorials/optimize-ai-usage)
tutorial distils this into eight practical habits:

<div class="gdd-feature-grid">
  <div class="gdd-feature">
    <h4>1. Match the model to the task</h4>
    <p>Reasoning models for architecture and hard debugging, mid-tier models when the plan is already clear, lighter models for routine refactors and formatting. Auto model selection routes each prompt to a suitable model automatically — and gets you a 10% cost discount on paid plans.</p>
  </div>
  <div class="gdd-feature">
    <h4>2. Write clear, scoped prompts</h4>
    <p>A clear task definition, relevant context up front, and an explicit stopping condition all cut down on retries and scope drift — without meaningfully increasing token usage themselves.</p>
  </div>
  <div class="gdd-feature">
    <h4>3. Keep context lean</h4>
    <p>Start a new conversation (<code>/new</code>) when you switch problems, run <code>/compact</code> to shrink a long session you still need, check usage with <code>/context</code>, maintain a good <code>AGENTS.md</code> / <code>copilot-instructions.md</code>, and only enable the MCP toolsets you actually need.</p>
  </div>
  <div class="gdd-feature">
    <h4>4. Preserve the cache</h4>
    <p>Cached tokens are typically billed at ~10% of fresh input cost — but switching models, changing reasoning level, or toggling tools mid-session invalidates that cache. Pick your settings up front and leave them alone for the session.</p>
  </div>
  <div class="gdd-feature">
    <h4>5. Set AI credit session limits</h4>
    <p>Cap how much a single session can spend in Copilot CLI or the SDK — the agent stops cleanly and lets you decide whether to raise the limit, rather than silently running up a bill.</p>
  </div>
  <div class="gdd-feature">
    <h4>6. Research, plan, then implement</h4>
    <p>Doing all three in one session lets irrelevant context pile up. Plan with a strong reasoning model (<code>/plan</code> in the CLI), then implement with a cheaper one in a fresh session.</p>
  </div>
  <div class="gdd-feature">
    <h4>7. Turn learnings into instructions</h4>
    <p>Use <code>/chronicle tips</code> and <code>/chronicle cost-tips</code> to surface recurring inefficiencies from your own session history, then encode the fix directly into <code>copilot-instructions.md</code> so it applies to every future session.</p>
  </div>
  <div class="gdd-feature">
    <h4>8. Add deterministic guardrails</h4>
    <p>Unit tests, linters, and security scans give the agent a fast pass/fail signal, which stops small errors from compounding into long, expensive chains of incorrect changes.</p>
  </div>
</div>

Learn more: [Optimizing your AI usage to maximize efficiency and reduce cost](https://docs.github.com/en/copilot/tutorials/optimize-ai-usage).

## Bringing it back to today's other sessions

The same governance and surface choices from [Getting hands on with the
GitHub App and CLI](github-app-cli.md) directly affect context behaviour —
for example, `/worktree` isolates a session's context from the rest of your
work, and session history can be shared across surfaces via the same
runtime. When you get to [GitHub SDK and Microsoft Foundry](sdk-foundry.md),
you'll see that building your own harness means these context decisions
become *your* responsibility as the app developer, not something a
pre-built surface handles for you.
