```yaml
number: 990
title: How does this differ from pyright?
type: issue
state: closed
author: benatshippabo
labels:
  - question
assignees: []
created_at: 2025-08-14T16:38:33Z
updated_at: 2025-08-14T18:29:30Z
url: https://github.com/astral-sh/ty/issues/990
synced_at: 2026-01-12T15:54:24Z
```

# How does this differ from pyright?

---

_@benatshippabo_

### Question

Just wanted to see a comparison between [pyright](https://github.com/microsoft/pyright) and ty, how do the goals, priorities, and vision compare to each other?

What would be some reasons someone using pyright would switch to ty aside from performance?

### Version

_No response_

---

_Label `question` added by @benatshippabo on 2025-08-14 16:38_

---

_Comment by @carljm on 2025-08-14 17:08_

Ultimately ty and pyright are quite similar: both are Python type checkers and LSP servers that aim to understand your codebase and offer useful diagnostic feedback and IDE features.

There are a few differing type system decisions; ty puts a bit more emphasis than other Python type checkers on avoiding making assumptions that can result in false positives on working untyped code.

But ultimately I think the goals and vision are much more similar than different.

---

_Comment by @benatshippabo on 2025-08-14 18:29_

This makes sense, thank you @carljm !

---

_Closed by @benatshippabo on 2025-08-14 18:29_

---
