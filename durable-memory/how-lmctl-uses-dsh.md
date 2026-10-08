# How lmctl uses dsh

Verified 2026-10-08 against lmctl 0.2.141 (`lmctl-src/src/providers/dsh-sidecar.ts`, `dsh.ts`, `adapters/dsh-adapter.ts`).

- A `provider=dsh` member is not run as one process per turn. `lmctl` starts one **sidecar**: `dsh web --no-open --port 0`
  (random free port; the first stdout line is `dsh web: http://127.0.0.1:<port>/...`). It keeps the sidecar for the whole team/batch
  and talks to it over HTTP on 127.0.0.1.
- Calls are `session.create` (caller-supplied session id and cwd), `session.selectModel`, then repeated `session.prompt`; `lmctl`
  waits for the turn by polling the session log file (zstd-compressed JSONL) under the dsh home.
- **Environment:** `DEEPSEEK_API_KEY` (DeepSeek API key; never print it). `DSH_HOME` overrides `~/.dsh` (use a temp directory for
  experiments so a real `~/.dsh` is not touched). Node `^22.19 || >=24`; the repo pins `pnpm@11.7.0`.
- **State:** `~/.dsh/settings.yaml` (default model: provider, model, reasoningEffort), `~/.dsh/profiles/{headless,web}` (small config
  files; dsh refreshes `profiles/node_modules` itself), `~/.dsh/sessions`, `~/.dsh/storages`.
- **Model:** `deepseek-flash` (and `deepseek-pro`) are server-resolved aliases: DeepSeek maps them to its current flash/pro model, so
  they are not pinned versions. Pinned names: `deepseek-v4-flash`, `deepseek-v4-pro`.
- **Smoke test:** `lmctl-mono-test/lmctl-test/tests/provider-smoke.mjs --providers dsh` (seed, ping, three-turn sum 3+7=10).
- Supported dsh versions: 0.1.1-rc.x always; 0.2.x after lmctl issue 017 is merged (see rpc-protocol-0.1-vs-0.2.md).
