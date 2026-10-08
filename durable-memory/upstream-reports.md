# Upstream reports

Upstream policy (CONTRIBUTING.md, read 2026-10-08): no external pull requests; report problems in GitHub Discussions of
https://github.com/deepseek-ai/deepseek-harness ; plugins are shared by tagging a repo with the `dsh-plugin` topic.

| Our issue | Upstream id | Filed | Status |
|---|---|---|---|
| (none yet) | | | |

Candidates, not filed (decide before filing; each needs a reproducible case):
- The published 0.1.1-rc.2 arm64 crash: already gone in 0.2.0-rc.2, so probably not worth reporting. If reported, include the six
  checks in arm64-npm-crash-0.1.1-rc.2.md.
- The 0.1.x to 0.2.x RPC change was not announced in a place we found; a short note in their docs or a changelog entry would help
  integrators (route style, auth cookie, `requestId`, versioned session file). This is a documentation request, not a defect.

When something is filed: add a row above with the Discussion id/URL and copy the id into the README section "Upstream reports".
