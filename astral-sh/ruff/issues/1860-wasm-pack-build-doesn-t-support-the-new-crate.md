---
number: 1860
title: "`wasm-pack build` doesn't support the new crate structure"
type: issue
state: closed
author: not-my-profile
labels: []
assignees: []
created_at: 2023-01-14T03:18:26Z
updated_at: 2023-01-14T08:21:18Z
url: https://github.com/astral-sh/ruff/issues/1860
synced_at: 2026-01-10T01:22:40Z
---

# `wasm-pack build` doesn't support the new crate structure

---

_Issue opened by @not-my-profile on 2023-01-14 03:18_

The new structure introduced in #1816 is now unfortunately causing the "[Playground] Release" workflow to fail. This is because the `wasm-pack build --target web --out-dir playground/src/pkg` command defined in `playground.yaml` is attempting to also build `ruff_cli` (which no longer supports wasm).

The fix for that would be to append `-p ruff` to the command, so that the command only builds the `ruff` crate ... only that `wasm-pack build` apparently doesn't support that yet for our a bit unconventional structure of having the `src` in the top-level directory as opposed to `ruff/src`. I just reported this to wasm-pack as https://github.com/rustwasm/wasm-pack/issues/1211 ... I'll try to see if I can contribute a fix there.

---

_Referenced in [astral-sh/ruff#1816](../../astral-sh/ruff/pulls/1816.md) on 2023-01-14 03:20_

---

_Comment by @charliermarsh on 2023-01-14 03:20_

Thanks, let me know if I can be helpful.

---

_Referenced in [astral-sh/ruff#1861](../../astral-sh/ruff/pulls/1861.md) on 2023-01-14 03:43_

---

_Closed by @charliermarsh on 2023-01-14 03:46_

---

_Comment by @not-my-profile on 2023-01-14 08:21_

While it did take me another commit to actually fix the playground CI 7b1ce72f862f0967df1924c583288ccc8b7cb81c and wasm-pack does in fact support our crate structure and I was wrong to assume that wasm-pack had such a limitation, looking further into it not only was the initial `wasm-pack` error  message very confusing but the  `wasm-pack build` CLI is apparently quite poorly designed with an optional positional argument that suddenly becomes required when you want to pass options to `cargo` ... and even worse `wasm-pack build` and `wasm-pack test` treat the same arguments differently ... I reported this as https://github.com/rustwasm/wasm-pack/issues/1213 ... I would contribute a fix to wasm-pack but their tests are failing on master and I am not interested in debugging that.

---
