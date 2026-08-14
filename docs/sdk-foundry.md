# GitHub SDK and Microsoft Foundry

<figure class="gdd-hero">
  <img src="../img/4-SDKAndFoundry.png" alt="Title slide: GitHub SDK and Microsoft Foundry">
</figure>

<div class="gdd-speaker">
  <div>
    <strong>Graeme Foster</strong>
    <span>Apps &amp; AI Global Black Belt, Microsoft</span>
  </div>
</div>

```bash
dotnet add package GitHub.Copilot.SDK
```

## GitHub Copilot SDK: a reusable agent harness

Every surface in the [previous session](github-app-cli.md) - CLI, app, cloud
agent - is really the same agent runtime wearing a different front end. The
Copilot SDK gives you a programmable way to put that same agent loop **inside
your own app, tool, workflow, or service.**

<figure class="gdd-hero">
  <img src="../img/4.2-GHCPSDK.png" alt="Slide: GitHub Copilot SDK - reusable agent harness, GA, 6 SDKs, agent runtime">
</figure>

### What it is

A thin developer surface over the same GitHub Copilot agent runtime used by
Copilot CLI. Your app defines the goal, tools, permissions and context.
Copilot does the messy agent bits: planning, tool use, file edits, streaming
and multi-turn work.

- **App** - your UX, workflow, or service.
- **SDK** - session, auth, tools, hooks.
- **Runtime** - plan, act, observe, iterate.

Shortcut version: you stop building the harness and start building the
useful part. The SDK is generally available with **6 language SDKs** -
TypeScript, Python, Go, .NET, Rust and Java - all backed by the same agent
runtime.

## Why use it: a harness for real work, not just code

The interesting bit isn't "Copilot can write code." It's "Copilot can drive
a governed, tool-using workflow."

<div class="gdd-feature-grid">
  <div class="gdd-feature">
    <h4>Non-coding task automation</h4>
    <p>Summarise documents, populate decks, clean spreadsheets, research pages, draft reports, or run repeatable back-office steps.</p>
  </div>
  <div class="gdd-feature">
    <h4>Bring-your-own tools</h4>
    <p>Register APIs, MCP servers, custom tools, skills and plugins so the agent can call the right capability at the right point.</p>
  </div>
  <div class="gdd-feature">
    <h4>Control the harness</h4>
    <p>Use permissions, hooks and prompt customisation to shape what the agent can do before, during and after tool calls.</p>
  </div>
  <div class="gdd-feature">
    <h4>Deploy where users work</h4>
    <p>Embed the agent loop into apps, services, desktop experiences, internal tools, CI/CD assistants, or customer-facing workflows.</p>
  </div>
</div>

<figure class="gdd-hero">
  <img src="../img/4.3-Harness.png" alt="Slide: why use it - a harness for real work, not just code">
</figure>

!!! tip "Good fit signal"
    Reach for the SDK when the task has: **context + tools + judgement +
    iteration.** If a task is a single deterministic transform, a plain
    function is still the right tool - save the agent loop for work that
    genuinely benefits from planning and tool use.

## Why it matters: the same pattern is showing up across Microsoft

The Copilot SDK isn't only a developer toy - it's becoming *the* agent
harness pattern across Microsoft's products and platforms:

- **GitHub Copilot SDK** - build the harness into your own app or service.
  Programmatic access to planning, tools, files and sessions.
- **Copilot Studio** - the GitHub Copilot harness powers reasoning-heavy
  agents and workflows in the new experience.
- **Microsoft Scout** - a desktop agent experience that acts across files,
  shell, browser and M365 data (treat as a product proof point, not a public
  SDK claim).
- **Foundry / hosted agents** - a useful way to think about the split: the
  SDK gives you the agent loop; hosted/managed platforms like Microsoft
  Foundry give you runtime and enterprise management choices - model
  routing, observability, compliance controls, and scaling - around that
  loop.

<figure class="gdd-hero">
  <img src="../img/4.4-Patterns.png" alt="Diagram: Copilot-style harness pattern connecting GitHub Copilot SDK, Copilot Studio, Microsoft Scout, and Foundry / hosted agents">
</figure>

!!! quote
    If you need an agentic harness inside an experience, start here before
    inventing your own orchestration.

## Where this leaves you

Put together, today's four sessions form a straight line: understand what an
agent actually is (keynote), use the surfaces GitHub already ships
([App & CLI](github-app-cli.md)), manage the resource that limits all of them
well ([context](context-management.md)), and - when an existing surface
doesn't fit - build your own with the SDK and a managed runtime like
Microsoft Foundry.

Head to [**Resources**](resources.md) for links to get started with the SDK,
Copilot CLI, Spec Kit, and more.
