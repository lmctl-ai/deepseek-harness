# The published @deepseek-ai/dsh 0.1.1-rc.2 crashes on arm64

Found 2026-10-08 on the awsspot box (AWS t4g.large, Graviton = ordinary arm64, Ubuntu 24.04, Node 24.15). lmctl issue 015.

- **Symptom:** `dsh web --no-open --port 0` prints its URL and then dies every time (6 of 6 runs):
  `Error: dsh: user patch-layer watching requires the Cordis HMR service` (at `watchUserPatches`, dsh-app-boot `lib/index.js:764`,
  called from `runProfile`). `lmctl` sees it as `transport error: fetch failed`.
- **Where the message comes from:** `runProfile` creates the `@deepseek-ai/cordis-plugin-hmr` plugin with `ctx.loader.create` and then
  reads `ctx.get("hmr")`; here that service is still undefined. The hmr plugin throws "--expose-internals is required for HMR service"
  when `ctx.loader.internal` is missing; whether that is the reason here was not proven (the plugin's own log output is not shown).
- **Not the cause (tested on the box):** the DeepSeek key and network (curl and node fetch return 200), missing profile files, Node
  version (24.15 and 24.18 behave the same), inotify limits (0 of 128 used), `node-addon-require-builtin` (loads; the arm64 package
  `-linux-arm64-gnu` is installed), `internal/modules/esm/loader` access (works), the working directory, debug env switches.
  `NODE_OPTIONS=--expose-internals` is rejected by node, so it is not a workaround.
- **What works (verified):**
  - the same version **built from source** (tag `dsh-v0.1.1-rc.2`) on the same box: sidecar stays up; lmctl's dsh smoke test 6/6;
  - the newer **npm package 0.2.0-rc.2** (the npm `latest`) on the same box: sidecar starts without the crash;
  - the source build of main (0.2.1-alpha.1) starts too.
- **Conclusion:** the defect is in the published 0.1.1-rc.2 package on arm64 (a packaging/bundling difference from the source build),
  not in the hardware and not in lmctl. Not root-caused, and no longer needed to chase: 0.2.0+ works. Practical rule: on arm64 use
  0.2.0-rc.2 or later (requires an lmctl that includes the 017 fix) or build 0.1.1-rc.2 from source (building-from-source.md).
