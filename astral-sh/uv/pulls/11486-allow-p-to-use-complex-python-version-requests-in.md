```yaml
number: 11486
title: "Allow `-p` to use complex Python version requests in `uv pip compile`"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: tracking/060
head: zb/compile-p
created_at: 2025-02-13T18:30:01Z
updated_at: 2025-02-13T21:44:03Z
url: https://github.com/astral-sh/uv/pull/11486
synced_at: 2026-01-10T11:10:38Z
```

# Allow `-p` to use complex Python version requests in `uv pip compile`

---

_Pull request opened by @zanieb on 2025-02-13 18:30_

Closes #11285
Closes https://github.com/astral-sh/uv/pull/11437

This changes `-p` from an alias of `--python-version` to `--python` while retaining backwards compatibility for `--python-version`-like fallback behavior when the requested version, e.g., `-p 3.12`, cannot be found.

This was initially implemented with a hidden `--python-legacy` flag which allows us to special case the short `-p` flag — unlike the implementation in #11437. However, after further discussion, we decided the behavior difference between `-p` and `--python` would be confusing so now `-p` is an alias for `--python` and `--python` is special-cased when a version is used.

Additionally, we now respect the `UV_PYTHON` environment variable, but it is ignored when `--python-version` is set. If you want different `--python-version` and `--python` values, you must do so explicitly. I considered banning this, but it is valid for e.g. `--python pypy --python-version 3.12`



---

_Label `breaking` added by @zanieb on 2025-02-13 18:30_

---

_Added to milestone `v0.6.0` by @zanieb on 2025-02-13 18:31_

---

_Review requested from @Gankra by @zanieb on 2025-02-13 18:51_

---

_Review requested from @charliermarsh by @zanieb on 2025-02-13 18:51_

---

_@charliermarsh reviewed on 2025-02-13 20:04_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/compile.rs`:121 on 2025-02-13 20:04_

Should this be enforced in Clap? Separately, I believe we omit periods if it's a single sentence.

---

_@charliermarsh reviewed on 2025-02-13 20:05_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/compile.rs`:135 on 2025-02-13 20:05_

So the net effect here is that we don't error if the version doesn't exist, right?

---

_@charliermarsh approved on 2025-02-13 20:05_

---

_@charliermarsh reviewed on 2025-02-13 20:05_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1185 on 2025-02-13 20:05_

Why can't this just be done via `-p`? So `--python 3.7` and `-p 3.7` now behave differently?

---

_@charliermarsh reviewed on 2025-02-13 20:05_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1185 on 2025-02-13 20:05_

_Should_ `--python 3.7` error if 3.7 isn't installed or available?

---

_@zanieb reviewed on 2025-02-13 20:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:121 on 2025-02-13 20:13_

Uhh.. I guess it can be since I moved the environment variable reading out of there. That's nice.

---

_@zanieb reviewed on 2025-02-13 20:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:135 on 2025-02-13 20:14_

Yes

---

_@zanieb reviewed on 2025-02-13 20:16_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1185 on 2025-02-13 20:16_

It's complicated.

`--python 3.7` continues to work as it does today — it errors if it's not found. I don't know if I want to break that behavior because it _does_ matter when you're not just doing version requests, e.g., `--python pypy` needs to fail if we can't find that implementation or the tags will be wrong. Similarly, `--python <path>` needs to fail.

We could special case `--python <version>` in the future if we want, which (as you said) would let us drop this legacy flag. However, then there would be no way for users to enforce that we actually find that interpreter.

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1190 on 2025-02-13 20:55_

this help heading is presumably pointless but i guess it's future-proof

---

_@Gankra approved on 2025-02-13 21:03_

Always setting a great example with your test coverage :)

---

_Comment by @zanieb on 2025-02-13 21:12_

Thanks @Gankra :)

For reference, I talked to @charliermarsh offline and made some more changes in https://github.com/astral-sh/uv/pull/11486/commits/24be46cb27b2a1d13040fdf7436960b874e3ad40 to special-case `--python` further so it just allows versions to be missing as mentioned in https://github.com/astral-sh/uv/pull/11486#discussion_r1955158236

---

_@zanieb reviewed on 2025-02-13 21:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/compile.rs`:243 on 2025-02-13 21:21_

Avoids warning if `--python` intentionally requested a different interpreter than `--python-version`.

---

_@charliermarsh approved on 2025-02-13 21:30_

---

_Merged by @zanieb on 2025-02-13 21:44_

---

_Closed by @zanieb on 2025-02-13 21:44_

---

_Branch deleted on 2025-02-13 21:44_

---
