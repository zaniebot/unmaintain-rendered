```yaml
number: 10186
title: allow inline dummy_implementations (aka ellipsis on the same line) to be disabled
type: issue
state: open
author: toppk
labels:
  - formatter
  - style
assignees: []
created_at: 2024-03-01T16:35:41Z
updated_at: 2024-03-01T16:58:57Z
url: https://github.com/astral-sh/ruff/issues/10186
synced_at: 2026-01-12T15:54:50Z
```

# allow inline dummy_implementations (aka ellipsis on the same line) to be disabled

---

_@toppk_

while I do appreciate ruff following black's lead, and I do think that this default not the biggest annoyance in the world,  there are many eyes that are used to seeing:

```python
class Foo:
   ...
```
rather than:

```python
class Foo: ...
```
The latter seems somehow broken to me.  I would like a configuration setting for this and all things that inline code, which at least in much of the code I've seen isn't commonly done.

Hopefully we can use this issue to gauge sentiment on this new formatting style as introduced in: 
  https://github.com/astral-sh/ruff/issues/8357


---

_Label `formatter` added by @MichaReiser on 2024-03-01 16:40_

---

_Label `style` added by @MichaReiser on 2024-03-01 16:40_

---
