# Getting hands on with the GitHub App and CLI

<figure class="gdd-hero">
  <img src="img/3-GHCPAppAndCLI.png" alt="Title slide: Getting hands on with the GitHub App and CLI">
</figure>

<div class="gdd-speaker">
  <div>
    <strong>Scott Holden</strong>
    <span>Solution Engineer Manager, Microsoft</span>
  </div>
</div>

**10:15 – 11:00am**

## Six surfaces, one Copilot subscription

GitHub Copilot isn't a single tool anymore — it's a runtime that shows up in
six places, all backed by the same subscription and the same underlying agent:

<div class="gdd-feature-grid">
  <div class="gdd-feature">
    <h4>In the editor</h4>
    <p>VS Code, Visual Studio, JetBrains, Xcode, Eclipse and Neovim. Completions, chat and agent mode.</p>
  </div>
  <div class="gdd-feature">
    <h4>On GitHub.com</h4>
    <p>Copilot Chat, code review, issue and pull request help.</p>
  </div>
  <div class="gdd-feature">
    <h4>Copilot cloud agent</h4>
    <p>Assign an issue or delegate a task. The agent works in a hosted environment and raises a pull request.</p>
  </div>
  <div class="gdd-feature">
    <h4>Copilot CLI</h4>
    <p>Terminal-native agent for Linux, macOS and Windows. Interactive or scripted.</p>
  </div>
  <div class="gdd-feature">
    <h4>GitHub Copilot app</h4>
    <p>Desktop control centre for parallel agent sessions on macOS, Windows and Linux. Built on the CLI.</p>
  </div>
  <div class="gdd-feature">
    <h4>Copilot SDK</h4>
    <p>Build your own tools on the same runtime in TypeScript, Python, Go, .NET, Rust and Java.</p>
  </div>
</div>

<figure class="gdd-hero">
  <img src="img/3.1-WaysToUse.png" alt="Diagram: Ways to use GitHub Copilot across six surfaces">
</figure>

## Where the two newest surfaces fit

The Copilot CLI and the GitHub Copilot app are the newest additions, and
they share **one runtime**: custom instructions, agent skills, MCP servers,
custom agents, hooks, plugins and session history are configured once and
shared across surfaces.

- **Editor** — inline authoring and agent mode.
- **Copilot CLI** — terminal agent, scriptable.
- **Copilot app** — desktop control centre.
- **Cloud agent** — async work on GitHub-hosted VMs.
- **Copilot SDK** — your own tools and automations.

Why this matters: a developer can plan in the terminal, hand the same
session to the desktop app with `/app`, delegate the long tail to the cloud
agent, and review everything back on GitHub. Configuration follows them.

<figure class="gdd-hero">
  <img src="img/3.2-Surface.png" alt="Diagram: Where the two newest surfaces fit — one runtime across editor, CLI, app, cloud agent and SDK">
</figure>

## The governance model

Policy flows down. Enterprise settings win, organisation settings fill the
gaps, and each surface has its own switch:

1. **Enterprise policies** — set once for every organisation. An enterprise
   choice cannot be overridden lower down.
2. **Organisation policies** — enable or disable each surface and feature for
   licensed members where the enterprise allows it.
3. **Enterprise-managed settings** — `managed-settings.json` distributed
   server-side, by MDM, or as a file. Controls client behaviour such as
   plugins, permissions and sandbox floors.
4. **Repository and user configuration** — custom instructions, skills, MCP
   servers and plugins committed to the repo, or held per user.

<figure class="gdd-hero">
  <img src="img/3.3-Governance.png" alt="Diagram: the governance model, from enterprise policy down to repository configuration">
</figure>

### How to enable each surface

| Surface | Where it's controlled | Notes |
| --- | --- | --- |
| Copilot CLI | Organisation → Copilot → Copilot CLI policy | Must be enabled in at least one organisation granting the seat |
| GitHub Copilot app | GitHub Copilot app policy | Enabled by default, governed separately from the CLI |
| Copilot cloud agent | Copilot cloud agent policy | Enable per organisation, then per repository |
| Cloud sandboxes | Cloud sandbox access policy | Disabled by default; inherits cloud agent controls such as firewall rules |
| Third-party agents | Partner agents toggle | Claude and Codex enabled per organisation once the enterprise allows them |

<figure class="gdd-hero">
  <img src="img/3.4-enable.png" alt="Table: how to enable each Copilot surface via organisation settings and policies">
</figure>

## GitHub Copilot CLI

A terminal-native coding agent that reads your repository, runs commands
with your approval, and talks to GitHub.com. Generally available since
February 2026, on every Copilot plan.

- **Two interfaces** — interactive with `copilot`, or scripted with `copilot -p`.
- **Three modes** — standard, plan and autopilot, cycled with `Shift+Tab`.
- **GitHub built in** — the GitHub MCP server ships preconfigured.
- **Infinite sessions** — auto-compaction near 95% of the context limit.
- **Approval controls** — per tool, per session, or with allow/deny flags.
- **Extensible** — instructions, skills, MCP, hooks, plugins, custom agents.

```bash
npm install -g @github/copilot
winget install GitHub.Copilot
brew install --cask copilot-cli

copilot
copilot -p "list my open PRs" --allow-tool='shell(git)'
```

Available on Linux, macOS and Windows, with PowerShell 6 or WSL.

<figure class="gdd-hero">
  <img src="img/3.5-GHCPCLI.png" alt="Slide: GitHub Copilot CLI install and run instructions">
</figure>

## GitHub Copilot app

A desktop control centre for agent-driven development, built on the Copilot
CLI and connected natively to GitHub. Generally available since June 2026,
on every Copilot plan or with your own key.

- **My work** — issues, pull requests, CI status and reviews in one inbox.
- **Parallel sessions** — each in its own git worktree and branch.
- **Session modes** — Interactive, Plan and Autopilot, plus model and
  reasoning controls.
- **Canvases** — shared work surfaces in the right-side panel.
- **Automations** — recurring agent tasks, locally or in the cloud.
- **Agent merge** — drives a pull request through checks to merge.

Available on macOS, Windows and Linux.

<figure class="gdd-hero">
  <img src="img/3.6-GHCPApp.png" alt="Slide: GitHub Copilot app sidebar and availability">
</figure>

## What has landed recently

Both surfaces ship weekly. A few changes that are most likely to affect how
your team works today:

<div class="gdd-feature-grid">
  <div class="gdd-feature">
    <h4>Copilot CLI</h4>
    <p>Sessions sidebar for concurrent sessions · <code>/worktree</code> for an isolated tree and conversation · <code>/rewind</code> restores conversation and files without Git · live tool-call durations in the timeline.</p>
  </div>
  <div class="gdd-feature">
    <h4>Copilot app</h4>
    <p>Canvases, cloud automations and bring-your-own-model · <code>/side</code> and Ask in Side chat for parallel questions · Auto shows which model handled each request · <code>/pr-stack</code> builds stacked pull requests.</p>
  </div>
  <div class="gdd-feature">
    <h4>Platform and admin</h4>
    <p>Stacked pull requests in public preview · Copilot app usage attributed per user in the usage metrics API · <code>remoteControl</code> managed setting limits which devices can host remotely controlled sessions.</p>
  </div>
</div>

<figure class="gdd-hero">
  <img src="img/3.7-New.png" alt="Slide: What has landed recently across Copilot CLI, Copilot app, and platform/admin">
</figure>

## Choosing between them

The app is built on the CLI, so this is a question of workflow shape rather
than capability tiers.

**Reach for the CLI when** you're already in the terminal, need scripting/CI
or headless automation, are working across several repositories at once,
want the newest features first behind `/experimental`, or you're on a remote
box over SSH or in a container.

**Reach for the app when** you're steering several agents at once, your work
starts from issues and ends at merged pull requests, you want diffs,
terminal, browser preview and review in one window, you want scheduled
automations without writing a workflow, or you want a shared visual surface
through canvases.

<figure class="gdd-hero">
  <img src="img/3.8-Which.png" alt="Slide: choosing between the Copilot CLI and the Copilot app">
</figure>

## Where to start

**Copilot CLI** — the agent where you already work. Interactive or scripted,
on every platform, across many repositories, with sandboxing and approval
controls you set.

```bash
npm install -g @github/copilot
# then run `copilot` in a project folder
```

**GitHub Copilot app** — the control centre when several pieces of work are
moving at once. Issues in, isolated sessions running, pull requests
reviewed and merged, all in one window. Get it from
[github.com/features/ai/github-app](https://github.com/features/ai/github-app)
(macOS, Windows and Linux).

Learn more: [Copilot CLI docs](https://docs.github.com/copilot/concepts/agents/copilot-cli) ·
[CLI best practices](https://docs.github.com/copilot/how-tos/copilot-cli/cli-best-practices) ·
[Copilot app docs](https://docs.github.com/copilot/how-tos/github-copilot-app) ·
[Policies by surface](https://docs.github.com/copilot/reference/supported-surfaces-for-policies) ·
[Enterprise-managed settings](https://docs.github.com/copilot/how-tos/administer-copilot)

<figure class="gdd-hero">
  <img src="img/3.9-WhereToStart.png" alt="Slide: where to start with Copilot CLI and the GitHub Copilot app, plus documentation links">
</figure>
