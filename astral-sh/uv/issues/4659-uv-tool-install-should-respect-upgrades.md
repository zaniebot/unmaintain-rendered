```yaml
number: 4659
title: "`uv tool install` should respect upgrades"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-06-30T17:06:57Z
updated_at: 2024-07-02T20:46:32Z
url: https://github.com/astral-sh/uv/issues/4659
synced_at: 2026-01-12T15:58:51Z
```

# `uv tool install` should respect upgrades

---

_@charliermarsh_

After running `cargo run tool install black==24.1.0`, both: (1) `cargo run tool install black --upgrade` and (2) `cargo run tool install black==24.2.0` are no-ops ("Tool is already installed").

---

_Label `bug` added by @charliermarsh on 2024-06-30 17:07_

---

_Label `preview` added by @charliermarsh on 2024-06-30 17:07_

---

_Comment by @charliermarsh on 2024-06-30 17:08_

I can do this to familiarize myself with the code.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-30 17:08_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-06-30 22:10_

---

_Comment by @charliermarsh on 2024-06-30 22:37_

Ok this needs a bit of thought with respect to the semantics of `uv tool upgrade`, `uv tool install --upgrade`, and `uv tool install --reinstall`.

---

_Comment by @charliermarsh on 2024-06-30 22:48_

Okay, here's a proposal just to give us something to workshop.

Given `uv tool install foo<1.0.0 --with bar<2.0.0`...

- `uv tool install foo --reinstall` removes `bar` and ignores `>=1.0.0` (i.e., it's as-if the tool was not installed at all).
- `uv tool install foo --reinstall-package foo` has the same sematics.
- `uv tool install foo --reinstall-package bar` reinstalls `bar` but respects the `>=2.0.0` constraint.
- `uv tool install foo==2.0.0` upgrades `foo` and removes `bar` (unsure on this one).
- `uv tool install foo --with bar==2.0.0` upgrades `bar`.
- `uv tool install foo --with baz` removes `bar` and adds `baz`.
- `uv tool install foo --upgrade` upgrades `foo` and `bar`, but respects the constraints.
- `uv tool install foo==2.0.0 --upgrade` upgrades `foo` and `bar`, replacing the constraint on `foo` (unsure on this one, should `bar` be removed like in `uv tool install foo==2.0.0`).

(`uv tool install foo --upgrade` and `uv tool upgrade foo` could have the same behavior.)


---

_Comment by @charliermarsh on 2024-06-30 22:58_

Maybe the simplest framing is: if you don't pass `--reinstall`... everything is additive? So using the above example `uv tool install foo --with baz` would keep `foo` and `bar` and add `baz`.

---

_Comment by @charliermarsh on 2024-07-01 19:24_

Another:

- `uv tool install foo<0.5.0` (so, a compatible but different requirement) would be equivalent to `uv tool install foo==2.0.0` (so, as soon as you include a requirement, it's like a reinstall).


---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-01 22:19_

---

_Comment by @charliermarsh on 2024-07-02 13:07_

I'm now tempted to change the behavior to: as soon as you specify a `from` or `with` that doesn't match the receipt, we completely ignore and remove the existing receipt.

---

_Comment by @charliermarsh on 2024-07-02 14:11_

Another framing:
```rust
// If the user specified `--force`, we ignore the existing receipt entirely.
//
// Otherwise, combine the requested requirements with the tool receipt. If the requirements
// changed, we allow replacement of any entrypoints.
//
// If the user specified `--reinstall` or `--upgrade`, we pass those flags to the resolver and
// also allow replacement of any entrypoints.
```

---

_Closed by @charliermarsh on 2024-07-02 20:46_

---
