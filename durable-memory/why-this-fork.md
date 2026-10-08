# Why this fork exists

Date: 2026-10-08. Fork of https://github.com/deepseek-ai/deepseek-harness, kept by the lmctl team (lmctl-ai).

- **License:** MIT, Copyright (c) 2026 DeepSeek. The `LICENSE` file is unchanged; MIT lets us copy, modify and publish with the
  notice retained.
- **Purpose:** a source tree we control for (1) building `dsh` on machines where the published npm package does not work
  (the arm64 awsspot box, see arm64-npm-crash-0.1.1-rc.2.md), (2) reading the real RPC code when `lmctl` has to follow a protocol
  change (rpc-protocol-0.1-vs-0.2.md), and (3) keeping our research next to the code (this directory).
- **Code changes:** none so far. If we ever carry a patch it must be listed here with the reason and, where possible, an upstream
  report in upstream-reports.md.
- **Upstream does not take outside pull requests** (`CONTRIBUTING.md`: "we cannot accept external pull requests at the moment";
  problems go to GitHub Discussions). So "filing upstream" here means a Discussion; its id is recorded in upstream-reports.md and
  in the README.
- **Naming:** upstream's `BRAND_GUIDELINES.md` asks projects not to use the full "DeepSeek Harness" trademark in a project name and
  recommends "DSH"; it says unauthorized use "may involve trademark infringement". Describing the relationship ("a fork of / built on
  DeepSeek Harness") is allowed. The repository name is therefore a decision for the lmctl owner; this README only uses the
  allowed descriptive form.
- **Syncing:** `git remote add upstream https://github.com/deepseek-ai/deepseek-harness.git; git fetch upstream --tags`. Only the
  `README.md` fork section and `durable-memory/` differ from upstream, so merges stay trivial.
