```yaml
number: 5930
title: Can respect-gitignore respect gitignore when not in a git repo
type: issue
state: open
author: timj
labels:
  - configuration
  - accepted
assignees: []
created_at: 2023-07-20T21:21:37Z
updated_at: 2025-02-21T03:20:24Z
url: https://github.com/astral-sh/ruff/issues/5930
synced_at: 2026-01-10T11:09:48Z
```

# Can respect-gitignore respect gitignore when not in a git repo

---

_Issue opened by @timj on 2023-07-20 21:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

We sometimes create export tar files that include all the source files but do not include the `.git` directory. With `ruff` this leaves us in the situation where when a developer is using the git repo `ruff` ignores all the built files in the directory but when they are building from the export tar files it does not.

Is it possible for `ruff --respect-gitignore`  to respect the `.gitignore` even if there is no `.git` directory? I realize that ruff scans parent directories looking for a git repo but if it can't find one can it check the current directory for a `.gitignore` anyhow? Renaming the `.gitignore` to `.ignore` does "fix" the problem but adding a `.ignore` to every repo (with a symlink say) is not a great solution for us. Is there a command line switch I'm missing for specifying that I want `.gitignore` to be the `.ignore` file?

The current behavior makes the two scenarios inconsistent for us and we don't really want to add explicit ignores into the ruff configuration file for everything that's already listed in .gitignore. 


---

_Comment by @charliermarsh on 2023-07-20 21:24_

I think we can just turn on this flag: https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.require_git.

---

_Comment by @erykoff on 2023-07-20 21:27_

I think that the functionality is in the WalkBuilder crate here: https://docs.rs/ignore/latest/src/ignore/walk.rs.html#797-805 and this should be exposed to ruff, or set to `no` when `--respect-gitignore` is specified.

---

_Comment by @erykoff on 2023-07-20 21:27_

(Oh you responded while I was typing.  yes!)

---

_Comment by @charliermarsh on 2023-07-21 01:16_

It seems reasonable to enable, PR welcome if anyone's interested :)

---

_Label `accepted` added by @charliermarsh on 2023-07-21 01:16_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-07-21 02:15_

---

_Closed by @charliermarsh on 2023-07-21 02:35_

---

_Comment by @charliermarsh on 2023-08-05 19:38_

Unfortunately I ended up having to revert this as it led to some undesirable behavior (namely, that `.gitignore` files in parent directories now affect all child directories, even if the child directory is itself a git repo with its own `.gitignore` -- see https://github.com/astral-sh/ruff/issues/6335).

We could consider adding a `--no-require-git` flag similar to ripgrep, though it'd be nice if we could find a solution that didn't require adding new configuration options and new command-line flags, since every additional flag comes with some maintenance and cognitive cost. For now I would suggest trying to use `.ignore` in some capacity.

---

_Comment by @timj on 2025-02-18 18:12_

Should I file this as a new issue since this is closed but the exact issue is still a problem for us even if the fix described here didn't work. 

---

_Comment by @ntBre on 2025-02-18 18:32_

I think we can just reopen this one.

---

_Reopened by @ntBre on 2025-02-18 18:32_

---

_Label `configuration` added by @ntBre on 2025-02-18 18:33_

---

_Unassigned @dhruvmanila by @dhruvmanila on 2025-02-21 03:20_

---
