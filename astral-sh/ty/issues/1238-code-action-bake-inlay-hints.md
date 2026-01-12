```yaml
number: 1238
title: "Code Action: Bake inlay hints"
type: issue
state: closed
author: fightingdreamer
labels:
  - feature
  - server
assignees: []
created_at: 2025-09-22T23:45:57Z
updated_at: 2025-09-23T08:15:02Z
url: https://github.com/astral-sh/ty/issues/1238
synced_at: 2026-01-12T15:54:24Z
```

# Code Action: Bake inlay hints

---

_@fightingdreamer_

This feature exists in TypeScript language server, but I've never seen it in Python tooling.

The idea is that the LSP exposes a code action to make discovered types persistent. For example:

```py
def foo():
    return "bar"
```

Putting the cursor on foo:

```py
def foo<CURSOR>():
    return "bar"
```

Enables generating the discovered return type, in this case str. The tool can extend this by providing multiple options with different levels of precision, in this case Literal["bar"] and str.

Similarly for variables:

```py
foo = "bar"
```

Putting the cursor on foo:

```py
foo<CURSOR> = "bar"
```

Should provide code actions for adding annotation as Literal["bar"] or str.

---

_Label `server` added by @carljm on 2025-09-23 00:06_

---

_Label `feature` added by @carljm on 2025-09-23 00:06_

---

_Closed by @MichaReiser on 2025-09-23 08:15_

---
