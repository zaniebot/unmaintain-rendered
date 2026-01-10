```yaml
number: 14608
title: "F-string formatting: Invalid syntax for format-spec with double quotes when targeting pre Python 3.12"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
  - preview
assignees: []
created_at: 2024-11-26T13:12:55Z
updated_at: 2024-11-27T07:49:35Z
url: https://github.com/astral-sh/ruff/issues/14608
synced_at: 2026-01-10T11:09:56Z
```

# F-string formatting: Invalid syntax for format-spec with double quotes when targeting pre Python 3.12

---

_Issue opened by @MichaReiser on 2024-11-26 13:12_

```py
f'{x:a{y=:{z:hy "user"}}} \'\'\''
```

Gets formatted to 

```py
f"{x:a{y=:{z:hy "user"}}} '''"
```

when preview mode is enabled and targeting a version < Python 3.12

This is related to https://github.com/astral-sh/ruff/pull/14493

---

_Label `bug` added by @MichaReiser on 2024-11-26 13:12_

---

_Label `formatter` added by @MichaReiser on 2024-11-26 13:12_

---

_Label `preview` added by @MichaReiser on 2024-11-26 13:12_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-11-27 05:46_

---

_Closed by @dhruvmanila on 2024-11-27 07:49_

---

_Closed by @dhruvmanila on 2024-11-27 07:49_

---
