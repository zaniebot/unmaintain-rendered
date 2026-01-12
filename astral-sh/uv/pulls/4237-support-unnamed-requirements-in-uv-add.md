```yaml
number: 4237
title: "Support unnamed requirements in `uv add`"
type: pull_request
state: closed
author: ibraheemdev
labels: []
assignees: []
base: main
head: uv-add-unnamed
created_at: 2024-06-11T16:21:40Z
updated_at: 2024-06-14T13:57:35Z
url: https://github.com/astral-sh/uv/pull/4237
synced_at: 2026-01-12T16:06:06Z
```

# Support unnamed requirements in `uv add`

---

_@ibraheemdev_

## Summary

Support unnamed URL requirements in `uv add`. For example, `uv add git+https://github.com/pallets/flask`.

Part of https://github.com/astral-sh/uv/issues/3959.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-11 16:21_

---

_Comment by @ibraheemdev on 2024-06-11 16:23_

I'm not 100% sure about all the CLI arguments here, would appreciate a more thorough audit of those. For example, I didn't add `--only-binary` because that doesn't seem very useful.

---

_@ibraheemdev reviewed on 2024-06-11 16:24_

---

_Review comment by @ibraheemdev on `crates/pypi-types/src/requirement.rs`:56 on 2024-06-11 16:24_

This is a little weird because we lose the distinction between empty version specifiers and `None`, but that doesn't seem relevant for formatting at least.

---

_@charliermarsh reviewed on 2024-06-11 19:03_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:70 on 2024-06-11 19:03_

I think it would be more conventional for this piece to be in `main.rs` (or, at least, we do that for the other commands).

---

_@charliermarsh reviewed on 2024-06-11 19:03_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:99 on 2024-06-11 19:03_

Can we define this `NoBinary` as `let no_binary = NoBinary::default()`, and put it with all the other defaults below? Like, move that block up to line 75 or so?

---

_@charliermarsh reviewed on 2024-06-11 19:04_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:99 on 2024-06-11 19:04_

I'd like to have something that looks like the `// TODO(charlie): Respect project configuration.` block in `do_lock`, just so we have consistency and it's easier to fix them all later.

---

_@charliermarsh reviewed on 2024-06-11 19:06_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:1967 on 2024-06-11 19:06_

I think you could feasibly omit this, `keyring_provider`, `python_version`, `python_platform`, `link_mode`, `build_isolation`, ... We don't support them in `lock` or `sync` or `run` yet and need to tackle the problem holistically. Honestly, it might be _best_ to omit them for now so we don't tie ourselves to them.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-06-14 12:21_

---

_Comment by @ibraheemdev on 2024-06-14 13:57_

Moved to https://github.com/astral-sh/uv/pull/4326 (switched to a local branch).

---

_Closed by @ibraheemdev on 2024-06-14 13:57_

---
