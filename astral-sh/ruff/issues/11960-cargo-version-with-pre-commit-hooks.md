```yaml
number: 11960
title: Cargo version with pre-commit hooks
type: issue
state: closed
author: wyardley
labels:
  - question
assignees: []
created_at: 2024-06-21T06:30:55Z
updated_at: 2024-06-21T14:29:37Z
url: https://github.com/astral-sh/ruff/issues/11960
synced_at: 2026-01-12T15:54:51Z
```

# Cargo version with pre-commit hooks

---

_@wyardley_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
“fmt”

From a quick look, the docs don’t seem to mention a specific version of cargo (unless I missed it). The latest version seems to use `cargo fix` vs `cargo fmt` which breaks the `cargo-fmt` pre-commit command unless you skip it.

---

_Comment by @MichaReiser on 2024-06-21 11:24_

By "latest version", do you mean the rust nightly? I just tried running `cargo fmt` and that runs just fine locally. 

We do specify a cargo version in the `rust-toolchain.toml` which is the version that cargo should pick up by default, unless you have an active override.

---

_Label `question` added by @MichaReiser on 2024-06-21 11:24_

---

_Comment by @wyardley on 2024-06-21 14:29_

Ah, looks like if you install via homebrew, you need to install `rustfmt` as well.

```
% cargo --version
cargo 1.79.0
% cargo fmt
error: no such command: `fmt`

	Did you mean `fix`?

	View all installed commands with `cargo --list`
	Find a package to install `fmt` with `cargo search cargo-fmt
```

---

_Closed by @wyardley on 2024-06-21 14:29_

---
