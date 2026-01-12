```yaml
number: 803
title: Add CI filter on rust and python files
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
draft: true
base: main
head: add-ci-filter
created_at: 2022-11-18T09:49:39Z
updated_at: 2022-12-01T17:15:30Z
url: https://github.com/astral-sh/ruff/pull/803
synced_at: 2026-01-12T15:55:05Z
```

# Add CI filter on rust and python files

---

_@JonathanPlasse_

This avoids running the CI for nothing when the files checked (Python and Rust) are unchanged.
Maybe there are more `paths` to add, like `Cargo.toml`.

---

_Comment by @andersk on 2022-11-19 21:27_

This seems dubious. Many other files affect how CI runs: `**/Cargo.toml`, `**/Cargo.lock`, `rust-toolchain`, `rustfmt.toml`, `*.snap`, and `ci.yaml` itself, for example. Even if we manage to list all of them, we might add more later and forget to update the list.

Adding `paths-ignore` instead might be reasonable, but I don’t think we actually have many commits where it would be worthwhile.

---

_Comment by @charliermarsh on 2022-11-20 00:23_

Yeah I appreciate the intent here but I think I'd rather leave this one out.

---

_Closed by @JonathanPlasse on 2022-11-20 08:26_

---

_Branch deleted on 2022-12-01 17:15_

---
