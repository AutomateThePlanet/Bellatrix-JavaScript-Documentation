---
layout: default
title:  "Use with AI Agents"
excerpt: "Let an AI coding agent write BELLATRIX TypeScript web tests with the bellatrix CLI and scaffolded skills."
date:   2026-10-27 06:50:17 +0200
permalink: /use-with-ai-agents/
anchors:
  overview: Overview
  install-the-cli: Install the CLI
  scaffold-the-typescript-skills: Scaffold the TypeScript Skills
  set-up-the-npm-project: Set Up the npm Project
  learn-more: Learn More
---
Overview
--------
The BELLATRIX AI agent toolkit lets an AI coding agent (Claude Code, GitHub Copilot, Cursor, Devin, Codex CLI, Gemini CLI for enterprise, Antigravity CLI, Kiro CLI or OpenCode) write BELLATRIX tests for you. It has two parts:
- the **`bellatrix` CLI**, which drives a live browser one action at a time (`bellatrix web <verb>`) and runs no AI model itself;
- **skills and subagents** that `bellatrix init` writes into your repository, in your agent's native layout. They teach the agent the `@bellatrix/web` components, page objects and test structure, and how to resolve stable locators.

The TypeScript skills cover **web**. BELLATRIX-JavaScript has no desktop or mobile component and page-object layer yet; use the C# or Java version for desktop and mobile.

Install the CLI
---------------
The CLI is a .NET global tool, so you need the .NET 8 SDK even for TypeScript projects:
```
dotnet tool install -g Bellatrix.Cli
bellatrix --version
bellatrix help
```
For web, the CLI starts and stops a local browser driver itself (Chrome, Edge or Firefox); no Selenium server is needed.

Scaffold the TypeScript Skills
------------------------------
Run this in the root of your test repository:
```
bellatrix init --agent claude --language typescript --domains web --json
```
Replace `claude` with `copilot`, `cursor`, `devin`, `codex`, `gemini`, `antigravity`, `kiro` or `opencode`, or list several (`--agent claude,cursor`). With `package.json` and `tsconfig.json` in the root folder, `--language typescript` is detected automatically. Add `--dry-run` to preview the files.

Set Up the npm Project
----------------------
The `@bellatrix/*` packages are not published to npm and ship TypeScript source. Reference a BELLATRIX-JavaScript checkout with `file:` dependencies:
```
"devDependencies": {
  "@bellatrix/core": "file:../BELLATRIX-JavaScript/@bellatrix/core",
  "@bellatrix/web": "file:../BELLATRIX-JavaScript/@bellatrix/web",
  "@bellatrix/extras-web": "file:../BELLATRIX-JavaScript/@bellatrix/extras/web"
}
```
and add this line to `.npmrc` so npm copies the packages instead of linking them:
```
install-links=true
```

Learn More
----------
The full guide — workflow, CLI reference, support matrix per AI agent and known limits — is at [bellatrix.solutions/agent](https://bellatrix.solutions/agent/).
The CLI and the skill templates are open source: [BELLATRIX-Agent on GitHub](https://github.com/AutomateThePlanet/BELLATRIX-Agent).
