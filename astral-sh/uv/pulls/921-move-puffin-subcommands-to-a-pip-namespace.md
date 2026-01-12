```yaml
number: 921
title: Move Puffin subcommands to a pip namespace
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/cli
created_at: 2024-01-15T01:33:07Z
updated_at: 2024-01-15T16:36:46Z
url: https://github.com/astral-sh/uv/pull/921
synced_at: 2026-01-12T16:04:17Z
```

# Move Puffin subcommands to a pip namespace

---

_@charliermarsh_

## Summary

This makes the separation clearer between the legacy `pip` API and the API we'll add in the future for the package manager itself. It also enables seamless `puffin pip` aliasing for those that want it.

Closes #918.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-15 01:33_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-15 01:33_

---

_Review requested from @konstin by @charliermarsh on 2024-01-15 01:33_

---

_Label `enhancement` added by @charliermarsh on 2024-01-15 01:33_

---

_Review comment by @konstin on `crates/puffin-cli/tests/pip_compile.rs`:483 on 2024-01-15 09:10_

Needs rustfmt, is it bailing because it's in a macro?

---

_@konstin approved on 2024-01-15 09:11_

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_compile.rs`:483 on 2024-01-15 16:04_

Yeah `rustfmt` doesn't work inside of macros. Or at least, most macros. [I guess there are some cases where it does.](https://users.rust-lang.org/t/rustfmt-skips-macro-arguments/74807)

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_compile.rs`:2594 on 2024-01-15 16:05_

Formatting is messed up here?

---

_@BurntSushi approved on 2024-01-15 16:05_

Love it.

---

_Comment by @charliermarsh on 2024-01-15 16:11_

Oops, I was definitely relying on `rustfmt` for the macro formatting. Will fix, thanks!

---

_@charliermarsh reviewed on 2024-01-15 16:11_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:2594 on 2024-01-15 16:11_

Now you see how I normally write my code.

---

_Merged by @charliermarsh on 2024-01-15 16:36_

---

_Closed by @charliermarsh on 2024-01-15 16:36_

---

_Branch deleted on 2024-01-15 16:36_

---
