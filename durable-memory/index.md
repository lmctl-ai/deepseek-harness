# durable-memory index: lmctl-ai fork of deepseek-harness

Knowledge base for the lmctl team about `dsh` (DeepSeek Harness) as used by `lmctl`'s `dsh` provider. Plain Markdown, one
durable fact per file, newest knowledge last. Updated 2026-10-08. Format: https://lmctl.com/skills/durable-memory.md

| File | What it records |
|---|---|
| [why-this-fork.md](why-this-fork.md) | Why this fork exists, license, naming/trademark note, what we change (nothing in code yet) |
| [how-lmctl-uses-dsh.md](how-lmctl-uses-dsh.md) | How `lmctl` starts and talks to dsh: sidecar, environment, state directory, models |
| [rpc-protocol-0.1-vs-0.2.md](rpc-protocol-0.1-vs-0.2.md) | The wire differences between dsh 0.1.x and 0.2.x that broke `lmctl` (issue 017) |
| [arm64-npm-crash-0.1.1-rc.2.md](arm64-npm-crash-0.1.1-rc.2.md) | The published 0.1.1-rc.2 package crashing on arm64; what was ruled out; 0.2.0-rc.2 is fine |
| [building-from-source.md](building-from-source.md) | Exact steps that worked on the awsspot arm64 box (pnpm via corepack), with the pitfalls |
| [upstream-reports.md](upstream-reports.md) | Everything we reported upstream, with the upstream id, and upstream's contribution policy |
