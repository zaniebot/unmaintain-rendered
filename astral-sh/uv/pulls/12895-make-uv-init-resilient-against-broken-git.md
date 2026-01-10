```yaml
number: 12895
title: "Make `uv init` resilient against broken git"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/resilient-uv-init
created_at: 2025-04-15T10:48:14Z
updated_at: 2025-04-17T15:45:26Z
url: https://github.com/astral-sh/uv/pull/12895
synced_at: 2026-01-10T11:10:40Z
```

# Make `uv init` resilient against broken git

---

_Pull request opened by @konstin on 2025-04-15 10:48_

Currently, `uv init` works without a `git` executable, and with a working `git` executable, but not with a broken `git`, be it from GitHub Action's Windows CI or from the shim we insert.

`uv init` calls git twice: Once `git rev-parse` to check whether a git repo already exists, and then `git init` (if there is no git repository yet and no `--vcs none`).

By separately handling the cases where git failed during `git rev-parse` doesn't work vs. where the is no repository when checking for an existing repo work tree, we can avoid calling `git init` for broken git and erroring. We have to hardcode the expected git command outputs to be able to check.

---

_Label `enhancement` added by @konstin on 2025-04-15 10:48_

---

_@zanieb reviewed on 2025-04-16 13:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:1212 on 2025-04-16 13:27_

```suggestion
            "`git rev-parse --is-inside-work-tree` failed for `{}`",
```

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:1212 on 2025-04-16 13:27_

Actually, I might prefer it as-is 🤔 do what you prefer!

---

_@zanieb reviewed on 2025-04-16 13:28_

---

_Comment by @konstin on 2025-04-16 13:28_

The main questions here are: a) Are we ok with parsing git text output? b) Do we want to ignore broken git installation? c) Does that interact properly with the `git` test feature?

---

_@zanieb approved on 2025-04-16 13:30_

---

_@zanieb reviewed on 2025-04-16 13:30_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:3778 on 2025-04-16 13:30_

What's the behavior if without `--vcs`?

---

_Comment by @Gankra on 2025-04-16 14:34_

> GitHub Action's Windows CI 

...what on earth is happening here??

---

_@Gankra approved on 2025-04-16 14:37_

I'm definitely suspicious of this but I doubt these outputs change much and other tools are liable to freakout if they do.

And this is pretty narrowly scoped so if it's an issue it's unlikely to cause massive breakage for people.

---

_@konstin reviewed on 2025-04-16 23:25_

---

_Review comment by @konstin on `crates/uv/src/commands/project/init.rs`:1212 on 2025-04-16 23:25_

I like the current versions for pointing out that this isn't case of a non-successful exist status, but one where `git` failed to spawn as a subprocess.

---

_@konstin reviewed on 2025-04-16 23:27_

---

_Review comment by @konstin on `crates/uv/tests/it/init.rs`:3778 on 2025-04-16 23:27_

That's tested above with `context.init().arg("working").assert().success();`

---

_Comment by @konstin on 2025-04-16 23:28_

Since we seem to be aligned on doing this I've polished the PR into a mergeable state

---

_Merged by @konstin on 2025-04-17 15:45_

---

_Closed by @konstin on 2025-04-17 15:45_

---

_Branch deleted on 2025-04-17 15:45_

---
