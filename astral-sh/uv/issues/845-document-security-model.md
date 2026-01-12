```yaml
number: 845
title: Document security model
type: issue
state: open
author: konstin
labels:
  - documentation
assignees: []
created_at: 2024-01-09T10:40:32Z
updated_at: 2025-02-13T16:14:40Z
url: https://github.com/astral-sh/uv/issues/845
synced_at: 2026-01-12T15:58:24Z
```

# Document security model

---

_@konstin_

We should document our assumptions wrt to security. Should be mostly obvious, but warrants being written down. This includes:

* **You need to trust all the indexes that you use.** We do not protect against malicious indexes. I think this point should be highlighted in the docs next to the index url option.
* You have to trust the package authors of the packages you use. (This could partially change for deployment/installation with hash checking mode, which ensures that you only use files you audited and no later upload trying to be smuggled in, this prevents against a previously known good package being overtaken between resolution and installation.)
* **Resolution and installation can run arbitrary code.** Since you generally only resolve package you plan to run, this shouldn't change the consideration much. Use `--no-build` to disable this, even though this is more a feature for reproducibility and "don't mess up my dev machine" than for security.
* Puffin currently uses no unsafe code, though some of our dependencies do and puffin itself might in the future.

---

_Label `documentation` added by @zanieb on 2024-01-22 23:16_

---

_Comment by @zanieb on 2025-02-13 15:44_

@konstin Should we put this in the `SECURITY.md` file or in the documentation?

---

_Comment by @konstin on 2025-02-13 16:14_

I prefer the documentation, it has all the information in one place without a context switch to github.

---
