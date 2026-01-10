```yaml
number: 11931
title: "Different behavior between servers for \"fixAll\" with syntax errors"
type: issue
state: closed
author: dhruvmanila
labels:
  - server
assignees: []
created_at: 2024-06-19T01:50:47Z
updated_at: 2024-07-04T04:07:18Z
url: https://github.com/astral-sh/ruff/issues/11931
synced_at: 2026-01-10T11:09:54Z
```

# Different behavior between servers for "fixAll" with syntax errors

---

_Issue opened by @dhruvmanila on 2024-06-19 01:50_

The behavior between `ruff server` and `ruff-lsp` is different when a source action is applied in a file with syntax errors.

`ruff server` returns an error stating "A parsing error occurred during `fix_all`: ..." and does not apply any fixes.

`ruff-lsp` does not return any error and applies whatever fixes are available.

Use the following code snippet to test this out with `source.fixAll` code action:
```py
foo;
y =
```

`ruff-lsp` will remove the semicolon after `foo` while `ruff server` will give an error.

For context, even if the source code contains syntax errors, Ruff can produce diagnostics for token-based rules and can generate fixes for some of them.

I'm not sure if we want to change this behavior but I don't think the new server should return an error if there's a syntax error. The diagnostics already point out the syntax error so this seems like a noise.


---

_Label `server` added by @dhruvmanila on 2024-06-19 01:50_

---

_Comment by @dhruvmanila on 2024-06-19 01:50_

cc @snowsignal 

---

_Assigned to @snowsignal by @snowsignal on 2024-06-19 03:30_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-07-02 11:13_

---

_Unassigned @snowsignal by @dhruvmanila on 2024-07-02 11:13_

---

_Added to milestone `Ruff Server: Stable` by @dhruvmanila on 2024-07-02 11:15_

---

_Closed by @dhruvmanila on 2024-07-04 04:07_

---

_Closed by @dhruvmanila on 2024-07-04 04:07_

---
