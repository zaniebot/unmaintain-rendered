---
number: 12040
title: "Consider using `-C` as a shortcut for `--project`"
type: issue
state: open
author: nils-werner
labels:
  - enhancement
assignees: []
created_at: 2025-03-07T11:13:16Z
updated_at: 2025-03-12T12:32:56Z
url: https://github.com/astral-sh/uv/issues/12040
synced_at: 2026-01-10T01:25:14Z
---

# Consider using `-C` as a shortcut for `--project`

---

_Issue opened by @nils-werner on 2025-03-07 11:13_

### Summary

It's a pretty established pattern that the option `-C` is a shortcut for what `uv` calls `--project`:

 - `make -C dir/`
 - `git -C dir/`
 - `poetry -C dir/`
 - `cargo -C dir/`
 - `meson -C dir/`

and probably many may more.

It would be great if `uv` could follow that pattern and also use `-C` as a shortcut for `--project`.

Right now it is [unfortunately used as a shortcut for `--config-settings`](https://github.com/astral-sh/uv/issues/1460) so this would likely have to go through a deprecation period.

### Example

I would like to use

```
uv -C project/ run python
```

like I could with Poetry

```
poetry -C project/ run python
```

and like I am used to from many other tools.

---

_Label `enhancement` added by @nils-werner on 2025-03-07 11:13_

---

_Renamed from "Consider using `-C` as a shortcut for `--directory`" to "Consider using `-C` as a shortcut for `--project`" by @nils-werner on 2025-03-12 12:32_

---

_Referenced in [astral-sh/uv#15250](../../astral-sh/uv/pulls/15250.md) on 2025-08-13 09:24_

---
