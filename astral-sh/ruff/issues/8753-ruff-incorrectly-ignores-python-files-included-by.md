```yaml
number: 8753
title: Ruff incorrectly ignores Python files included by negated pattern in .gitignore
type: issue
state: open
author: henribru
labels:
  - bug
assignees: []
created_at: 2023-11-18T08:46:27Z
updated_at: 2023-11-30T16:31:21Z
url: https://github.com/astral-sh/ruff/issues/8753
synced_at: 2026-01-10T11:09:51Z
```

# Ruff incorrectly ignores Python files included by negated pattern in .gitignore

---

_Issue opened by @henribru on 2023-11-18 08:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Given the following .gitignore:
```
*
!/src/
!/src/**
```

and a file `src/foo.py`, `ruff src/` outputs `warning: No Python files found under the given path(s)`. Given that Git includes this file due to the negated pattern overriding the `*`, this seems like a bug.

This is with 0.1.6 and no Ruff configuration in `pyproject.toml`. 


---

_Comment by @charliermarsh on 2023-11-18 22:54_

@BurntSushi -- We use the `ignore` crate for `.gitignore` behavior. Would you expect `src/foo.py` to be included based on the above? (It could definitely be an error on our side and not in the `ignore` crate, but wanted to check with you first.)

---

_Label `question` added by @charliermarsh on 2023-11-18 22:54_

---

_Comment by @henribru on 2023-11-19 07:06_

Seems like this might be https://github.com/BurntSushi/ripgrep/issues/1050

---

_Comment by @BurntSushi on 2023-11-20 13:34_

It might also be this: https://github.com/BurntSushi/ripgrep/issues/1757 (A fix was merged recently but I believe it has not been released yet.)

But yes, this looks like a bug in the `ignore` crate to me.

---

_Comment by @eli-schwartz on 2023-11-22 23:24_

> @BurntSushi -- We use the `ignore` crate for `.gitignore` behavior. Would you expect `src/foo.py` to be included based on the above? (It could definitely be an error on our side and not in the `ignore` crate, but wanted to check with you first.)

I would expect files that are checked into git with `git add --force` to be checked, but maybe that is just me? :thinking: 

---

_Label `question` removed by @MichaReiser on 2023-11-27 03:12_

---

_Label `bug` added by @MichaReiser on 2023-11-27 03:12_

---

_Label `question` added by @MichaReiser on 2023-11-27 03:12_

---

_Comment by @BurntSushi on 2023-11-27 13:18_

The `ignore` crate only reads `.gitignore` files. It doesn't actually read the git repository state to determine what's tracked and what isn't. One possible work-around to this is to ensure that `.gitignore` files are in sync with your repository state.

---

_Label `question` removed by @charliermarsh on 2023-11-29 14:57_

---

_Comment by @charliermarsh on 2023-11-29 14:58_

@BurntSushi - Did that fix go out in the latest version of ignore?

---

_Comment by @BurntSushi on 2023-11-29 15:03_

Yeah if the issue here is https://github.com/BurntSushi/ripgrep/issues/1757 then that's in the latest `ignore` release.

But https://github.com/BurntSushi/ripgrep/issues/1050 remains unfixed.

---

_Comment by @charliermarsh on 2023-11-29 15:07_

I actually can't reproduce this on v0.1.6. @henribru - is that the exact pattern and directory structure you were using?

---

_Comment by @henribru on 2023-11-30 16:31_

> I actually can't reproduce this on v0.1.6. @henribru - is that the exact pattern and directory structure you were using?

Yes. But I discovered something curious: It only seems to reproduce on Windows.

---
