```yaml
number: 11729
title: Display syntax errors in a separate window
type: issue
state: closed
author: dhruvmanila
labels:
  - playground
assignees: []
created_at: 2024-06-04T05:52:50Z
updated_at: 2024-10-01T13:32:32Z
url: https://github.com/astral-sh/ruff/issues/11729
synced_at: 2026-01-12T15:54:51Z
```

# Display syntax errors in a separate window

---

_@dhruvmanila_

Currently, the playground displays the AST and tokens even if it contains syntax errors. It would be useful to have a separate window to display all the syntax errors in a pretty format like the one used in parser snapshots. For example:

```
  |
1 | x: int =
  |         ^ Syntax Error: Expected an expression
  |
```

The design would be something like:

<img alt="playground design with syntax error tab" src="https://github.com/astral-sh/ruff/assets/67177269/87c20f16-a269-43cf-b22c-76cd05aa18fa" width=500>

### Alternative

An alternative design would be to actually have a diagnostics window similar to https://biomejs.dev/playground/ and https://pyright-play.net/.

---

_Label `playground` added by @dhruvmanila on 2024-06-04 05:52_

---

_Comment by @dhruvmanila on 2024-06-04 05:53_

This is not a priority but something which is good to have.

---

_Comment by @dhruvmanila on 2024-10-01 13:32_

I think the new diagnostics window is good enough to not do this.

---

_Closed by @dhruvmanila on 2024-10-01 13:32_

---
