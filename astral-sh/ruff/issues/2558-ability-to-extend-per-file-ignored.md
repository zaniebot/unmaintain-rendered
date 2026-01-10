```yaml
number: 2558
title: Ability to extend per-file-ignored
type: issue
state: closed
author: Alphasite
labels:
  - configuration
assignees: []
created_at: 2023-02-03T22:33:06Z
updated_at: 2023-05-19T16:17:15Z
url: https://github.com/astral-sh/ruff/issues/2558
synced_at: 2026-01-10T11:09:45Z
```

# Ability to extend per-file-ignored

---

_Issue opened by @Alphasite on 2023-02-03 22:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi i have a similar issue to !2446, in our mono repo we have a global `ruff.toml` file which has per file ignores specified in it and we would like to add sub-project specific ignores in the child `pyproject.toml` file, but currently that will override those provided in the parent toml. 

So i'd like to propose the addition of an `extend.per-file-ignores` so that we can add extra ignores without removing the ones provided in the parent config file. 

---

_Renamed from "[feature request] Ability to extend per-file-ignored" to "Ability to extend per-file-ignored" by @Alphasite on 2023-02-03 22:33_

---

_Label `configuration` added by @charliermarsh on 2023-02-04 00:17_

---

_Comment by @charliermarsh on 2023-05-19 16:17_

This will exist in the next release.

---

_Closed by @charliermarsh on 2023-05-19 16:17_

---
