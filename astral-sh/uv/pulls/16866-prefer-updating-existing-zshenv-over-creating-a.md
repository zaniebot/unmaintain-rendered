```yaml
number: 16866
title: "Prefer updating existing `.zshenv` over creating a new one in `tool update-shell`"
type: pull_request
state: merged
author: benberryallwood
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2025-11-26T19:41:50Z
updated_at: 2025-11-29T23:03:29Z
url: https://github.com/astral-sh/uv/pull/16866
synced_at: 2026-01-10T05:49:14Z
```

# Prefer updating existing `.zshenv` over creating a new one in `tool update-shell`

---

_Pull request opened by @benberryallwood on 2025-11-26 19:41_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update the logic in `uv_shell::Shell.configuration_files` for Zsh to match [rustup's rc file detection](https://github.com/rust-lang/rustup/blob/fede22fea7b160868cece632bd213e6d72f8912f/src/cli/self_update/shell.rs#L197) for the case when `ZDOTDIR` is set, but there is an existing `~/.zshenv` file.

Now instead of creating a new file at `ZDOTDIR/.zshenv`, it will update the existing `~/.zshenv` file.

Fixes: #16865

## Test Plan

Tested locally following the minimal example in #16865, as well as a couple of other scenarios to ensure that behaviour hasn't changed.

---

_Label `bug` added by @konstin on 2025-11-27 09:56_

---

_@zsol reviewed on 2025-11-27 10:29_

It would be helpful to have some test cases capturing the behavior in these cases:
- `ZDOTDIR` set and `$ZDOTDIR/.zshenv` exists
- `ZDOTDIR` set and only `$HOME/.zshenv` exists (no `$ZDOTDIR/.zshenv`)
- `ZDOTDIR` set and no `.zshenv` anywhere is found
- `ZDOTDIR` is unset

---

_Assigned to @zsol by @konstin on 2025-11-27 12:03_

---

_@zsol approved on 2025-11-29 15:11_

Thank you! 

---

_Comment by @benberryallwood on 2025-11-29 15:15_

No worries, thanks for the quick reviews!

---

_Merged by @zsol on 2025-11-29 22:13_

---

_Closed by @zsol on 2025-11-29 22:13_

---

_Review comment by @samypr100 on `crates/uv-shell/src/lib.rs`:351 on 2025-11-29 23:01_

nit: I'd use `EnvVars::USERPROFILE` and `EnvVars::HOME` here.

---

_Review comment by @samypr100 on `crates/uv-shell/src/lib.rs`:360 on 2025-11-29 23:02_

nit: I'd use `EnvVars::ZDOTDIR` here.

---

_Review comment by @samypr100 on `crates/uv-shell/src/lib.rs`:394 on 2025-11-29 23:02_

nit: I'd use `EnvVars::ZDOTDIR` here.

---

_Review comment by @samypr100 on `crates/uv-shell/src/lib.rs`:407 on 2025-11-29 23:02_

nit: I'd use `EnvVars::ZDOTDIR` here.

---

_Review comment by @samypr100 on `crates/uv-shell/src/lib.rs`:373 on 2025-11-29 23:02_

nit: I'd use `EnvVars::ZDOTDIR` here.

---

_Review comment by @samypr100 on `crates/uv-shell/src/lib.rs`:428 on 2025-11-29 23:03_

nit: I'd use `EnvVars::ZDOTDIR` here.

---

_@samypr100 reviewed on 2025-11-29 23:03_

---
