---
number: 12833
title: Two-value environment specifiers evaluated incorrectly wrt boolean logic
type: issue
state: open
author: mgorny
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2025-04-11T12:09:42Z
updated_at: 2025-04-11T13:09:07Z
url: https://github.com/astral-sh/uv/issues/12833
synced_at: 2026-01-10T01:25:25Z
---

# Two-value environment specifiers evaluated incorrectly wrt boolean logic

---

_Issue opened by @mgorny on 2025-04-11 12:09_

### Summary

```console
$ uv pip install 'ipython; "x"=="y"'
Resolved 16 packages in 4ms
░░░░░░░░░░░░░░░░░░░░ [0/16] Installing wheels...                                                                                       warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 16 packages in 47ms
 + asttokens==3.0.0
 + decorator==5.2.1
 + executing==2.2.0
 + ipython==9.1.0
 + ipython-pygments-lexers==1.1.1
 + jedi==0.19.2
 + matplotlib-inline==0.1.7
 + parso==0.8.4
 + pexpect==4.9.0
 + prompt-toolkit==3.0.50
 + ptyprocess==0.7.0
 + pure-eval==0.2.3
 + pygments==2.19.1
 + stack-data==0.6.3
 + traitlets==5.14.3
 + wcwidth==0.2.13
```

However, `"x"=="y"` should be evaluated to false.

For comparison, pip throws a `KeyError` here, apparently misinterpeting `y` as a variable name.

### Platform

Gentoo Linux amd64

### Version

0.6.13

### Python version

Python 3.13.3

---

_Label `bug` added by @mgorny on 2025-04-11 12:09_

---

_Comment by @konstin on 2025-04-11 12:55_

uv ignores all marker expressions that don't make sense type-wise, such as string-string comparisons or extra-to-env-key comparisons, evaluating them as if they weren't there. We have to accept bad markers for legacy reasons, but we could change this behavior to evaluating them to false instead.

There is also bug report in packaging for the pip error: https://github.com/pypa/packaging/issues/632

---

_Label `needs-decision` added by @konstin on 2025-04-11 13:09_

---
