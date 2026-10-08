# Building dsh from source (arm64 awsspot box, 2026-10-08)

Worked as user `testuser` on Ubuntu 24.04 aarch64 (t4g.large, 8 GB, Node 24.15). About 5 minutes for install plus build.

```sh
mkdir -p ~/repos/lmctl-ai && cd ~/repos/lmctl-ai
git clone --depth 1 https://github.com/lmctl-ai/deepseek-harness.git deepseek-harness    # or the upstream URL
cd deepseek-harness && git fetch --depth 1 origin tag dsh-v0.1.1-rc.2                    # the tag lmctl 0.2.141 works with
git worktree add --detach ../deepseek-harness-0.1.1-rc.2 dsh-v0.1.1-rc.2 && cd ../deepseek-harness-0.1.1-rc.2
corepack enable --install-directory ~/.local/bin          # repo pins pnpm@11.7.0 (packageManager field)
pnpm install --frozen-lockfile && pnpm run build
chmod +x apps/cli/lib/bin.js && ln -sfn "$PWD/apps/cli/lib/bin.js" ~/.local/bin/dsh && dsh --version
```

Pitfalls seen:
- Run `pnpm` only from inside the checkout. From `/tmp` corepack picks the latest pnpm (12.x) and refuses:
  `ERR_PNPM_BAD_PM_VERSION ... configured to use 11.7.0`. `pnpm --dir <repo> ...` from another directory has the same problem.
- Without a global pnpm, run the CLI from the source tree with `node --import tsx/esm apps/cli/src/bin.ts web --no-open --port 0`
  (must be started inside the checkout because `tsx` is resolved from the current directory).
- `dsh web` prints a URL with `?token=...` in 0.2.x. Treat it as a secret; do not paste it into logs.
- The repo main (0.2.1-alpha.1) also builds, but lmctl without the 017 fix cannot talk to it ("Method Not Allowed").
- Keep a copy of the previous binary link (`dsh-npm`) so you can switch back.
