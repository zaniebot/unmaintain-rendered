```yaml
number: 1020
title: home folder not detected and resolved in ty.toml (on a Mac)
type: issue
state: closed
author: andythomas
labels: []
assignees: []
created_at: 2025-08-16T13:59:40Z
updated_at: 2025-08-16T14:11:25Z
url: https://github.com/astral-sh/ty/issues/1020
synced_at: 2026-01-12T15:54:24Z
```

# home folder not detected and resolved in ty.toml (on a Mac)

---

_@andythomas_

### Summary

The bug is most apparent if I utilize `extra-paths` in `~/.config/ty/ty.toml` as in

```toml
[environment]
#python = "~/.venv/test/bin/python"
extra-paths = ["~/.venv"]
````

Now, I start `ty` and get

```
ty failed
  Cause: /Users/thomasa/~/.venv does not point to a directory
```

It seems that `~` is not resolved. If I set the `python` key it seems to be the same, just the error message ist not as descriptive, which is confusing at first.

```
 python = "~/.venv/test/bin/python"
  |          ^^^^^^^^^^^^^^^^^^^^^^^^^^^ does not point to a Python executable or a directory on disk
```

If I expand the `~` 'by hand' to `/Users/thomasa` it does work as expected. 

### Version

ty 0.0.1-alpha.18 (d697cc092 2025-08-14)

---

_Comment by @MichaReiser on 2025-08-16 14:01_

I'll merge this into https://github.com/astral-sh/ty/issues/914

---

_Closed by @MichaReiser on 2025-08-16 14:01_

---

_Comment by @andythomas on 2025-08-16 14:07_

Thank you for the insanely quick answer. I saw the linked issue, but it occurred to me, that it is not the same. I am using plain `~` directly form the [ty docs](https://docs.astral.sh/ty/reference/configuration/#environment).

---

_Comment by @MichaReiser on 2025-08-16 14:11_

Thanks for pointing this out. This is an error in the docs

---
