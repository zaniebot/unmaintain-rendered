```yaml
number: 16633
title: "installer/pip compatibility: support script entrypoints with slashes"
type: issue
state: open
author: paveldikov
labels:
  - compatibility
  - needs-decision
assignees: []
created_at: 2025-11-07T15:39:13Z
updated_at: 2025-11-12T11:20:32Z
url: https://github.com/astral-sh/uv/issues/16633
synced_at: 2026-01-12T16:02:35Z
```

# installer/pip compatibility: support script entrypoints with slashes

---

_@paveldikov_

### Summary

`pip` will happily install a fully-functional script entrypoint with a slash in the name, e.g.
```
"foo/bar" = "foo.bar:main"
```
will generate an executable called `bin/foo/bar` (or indeed `Scripts\foo\bar.exe`)

`uv` fails with an `ENOENT`, presumably on the `foo` directory not existing.

I am cognisant that this is a cursed practice as per [PyPA's recommended regex](https://packaging.python.org/en/latest/specifications/entry-points/)... however... `pip` supports it... Hyrum's law!

This will also make the relocatable venv logic a bit annoying as it will have to compute the relative path to the Python executable.

### Platform

RHEL 9

### Version

0.9.4

### Python version

_No response_

---

_Label `bug` added by @paveldikov on 2025-11-07 15:39_

---

_Label `bug` removed by @konstin on 2025-11-12 11:20_

---

_Label `compatibility` added by @konstin on 2025-11-12 11:20_

---

_Label `needs-decision` added by @konstin on 2025-11-12 11:20_

---
