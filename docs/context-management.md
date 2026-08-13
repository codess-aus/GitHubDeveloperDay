# Context management and optimization

<figure class="gdd-hero">
  <img src="../img/placeholder-hero.svg" alt="Placeholder hero graphic — the context management slide deck hasn't been added to this site yet">
  <figcaption>Hero image placeholder — swap in the real slide once it's available.</figcaption>
</figure>

**11:30am – 12:15pm**

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

## Bringing it back to today's other sessions

The same governance and surface choices from [Getting hands on with the
GitHub App and CLI](github-app-cli.md) directly affect context behaviour —
for example, `/worktree` isolates a session's context from the rest of your
work, and session history can be shared across surfaces via the same
runtime. When you get to [GitHub SDK and Microsoft Foundry](sdk-foundry.md),
you'll see that building your own harness means these context decisions
become *your* responsibility as the app developer, not something a
pre-built surface handles for you.
