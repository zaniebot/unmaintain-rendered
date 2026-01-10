---
number: 16348
title: "Provide a way for `uv lock` to ignore and replace existing and/or broken `uv.lock` files"
type: issue
state: open
author: namurphy
labels:
  - bug
assignees: []
created_at: 2025-10-17T19:58:10Z
updated_at: 2025-12-18T16:27:10Z
url: https://github.com/astral-sh/uv/issues/16348
synced_at: 2026-01-10T01:26:05Z
---

# Provide a way for `uv lock` to ignore and replace existing and/or broken `uv.lock` files

---

_Issue opened by @namurphy on 2025-10-17 19:58_

### Summary

There are occasions when `uv.lock` breaks, in particular when there is a git merge conflict (#5633, #11185).  When `uv.lock` is not a valid TOML file, running `uv lock` or `uv lock --upgrade` results in an error.  Potential workarounds for lockfile merge conflicts are ~~to manually delete `uv.lock` before running `uv lock` again, or~~ to check out `uv.lock` from one of the branches before running `uv.lock`.  

It would be helpful if there was a graceful way to recreate `uv.lock` that works even when there is a merge conflict or problem with `uv.lock`.  Two possibilities would be to:

1. Create a new option for `uv lock` to ignore and replace existing lockfiles (perhaps `--replace`, `--recreate`, or `--redo`?)
2. ~~Have `uv lock --upgrade` create a fresh new `uv.lock` instead of failing when the current `uv.lock` is invalid, possibly while issuing a warning.~~

Doing `uv lock --replace` (option 1) wouldn't change the behavior of existing options, would be more deterministic, and would be intuitive with the right name.

~~Enabling `uv lock --upgrade` to replace broken `uv.lock` files (option 2) seems intuitive since my mental model with this command is to upgrade the environment to include the ≈newest allowed versions of dependencies, but would change the behavior of existing options.~~ ← Replacing `uv.lock` would unpin versions, so this approach would likely cause things to break.

Thank you as always!

### Example

Start by creating a project with a broken `uv.lock`.

```bash
$ uv init
$ uv lock
$ echo "reminder: adding text to uv.lock will break it!" >> uv.lock
```

Running `uv lock` or `uv lock --upgrade` when `uv.lock` then gives the following error (with uv 0.9.3):
```bash
$ uv lock --upgrade
Using CPython 3.13.3
error: Failed to parse `uv.lock`
  Caused by: TOML parse error at line 9, column 11
  |
9 | reminder: adding text to uv.lock will break it!
  |           ^
key with no value, expected `=`
```

My desired behavior would be something like:
```bash
$ uv lock --replace
Using CPython 3.13.3
Resolved 1 package in 1ms
```

---

_Label `enhancement` added by @namurphy on 2025-10-17 19:58_

---

_Referenced in [astral-sh/uv#11185](../../astral-sh/uv/issues/11185.md) on 2025-10-17 20:03_

---

_Comment by @zanieb on 2025-10-17 20:19_

The `--force` flag seems like a good candidate for this behavior, xref #15757 

---

_Comment by @zanieb on 2025-10-17 20:21_

> Potential workarounds for lockfile merge conflicts are to manually delete uv.lock before running uv lock again, or to check out uv.lock from one of the branches before running uv.lock.

Only the latter is appropriate for merge conflicts. The former will unpin any versions, and should generally not be used unless there's no recourse.

---

_Comment by @namurphy on 2025-10-20 17:21_

> > Potential workarounds for lockfile merge conflicts are to manually delete uv.lock before running uv lock again, or to check out uv.lock from one of the branches before running uv.lock.
> 
> Only the latter is appropriate for merge conflicts. The former will unpin any versions, and should generally not be used unless there's no recourse.

That is helpful to know! Given that replacing `uv.lock` would unpin any versions, it seems very likely that having `uv lock --upgrade` replace a `uv.lock` with a merge conflict (option 2) would cause problems. I'll adjust the issue accordingly. Thank you!

---

_Comment by @konstin on 2025-10-20 18:39_

Should we skip reading the lockfile altogether with `--upgrade`? This circumvents this problem and it's faster.

---

_Label `enhancement` removed by @konstin on 2025-12-18 16:27_

---

_Label `bug` added by @konstin on 2025-12-18 16:27_

---
