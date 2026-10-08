# RPC protocol: dsh 0.1.x versus 0.2.x

Found 2026-10-08 (lmctl issue 017 "lmctl dsh adapter incompatible with dsh 0.2.x"). Symptom with an unchanged lmctl 0.2.141:
`session.create failed: dsh RPC session.create failed: "Method Not Allowed"` against dsh 0.2.0-rc.2 (npm `latest`) and
0.2.1-alpha.1 (this repo, tag `dsh-v0.2.1-alpha.1`). Works against 0.1.1-rc.2.

| | dsh 0.1.1-rc.x | dsh 0.2.x |
|---|---|---|
| boot URL printed by `dsh web` | `http://127.0.0.1:<port>` | `http://127.0.0.1:<port>/?token=<secret>` |
| endpoint | `POST /api/session.create` (dot notation) | `POST /api/session/create` (slash: `<channel>/<endpoint>`) |
| body | `{type:"client-request", rpcId, method, payload}` | same envelope; request fields wrapped as `payload: {args: {request: {...}}}` |
| auth | none | cookie `dsh_session_<authority>`, obtained by a GET on the token boot URL (redirect not followed); sent on every RPC |
| `session.prompt` | no request id | non-empty `requestId` required |
| session log file | `session.jsonl.zstd` | versioned: `session.v4.jsonl.zstd` |

Evidence: the envelope and the route rule `${channel}/${endpoint}` are in this repo, `packages/client/connection/src/client/rpc.ts`
(and `rpc-schema.ts`). The cookie exchange, the `requestId` rule and the `v4` file name were observed live against 0.2.0-rc.2 by the
lmctl Coder (not read from source by us). Why the old adapter got 405: it built `${baseUrl}/api/session.create` by string
concatenation, and with a `?token=` in the boot URL that turns the whole path into a query string for `/`.

Fix: lmctl branch `fix/dsh-0.2-rpc` (commit 1dfbc62 at the time of writing; not merged yet): strip the boot URL to its origin, do the
cookie exchange, choose the 0.1 or 0.2 request shape, find `session.v*.jsonl.zstd`. Verified by a fake-sidecar unit test (fails on the
old code) and by the live smoke test against 0.1.1-rc.2 and 0.2.0-rc.2. Update this file with the final commit when merged.

Security notes: the token in the boot URL and the cookie are credentials; never log them, never put them in teamfiles or errors.
