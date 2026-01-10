```yaml
number: 7594
title: "Formatter: handle comments, quotes, and expressions inside f-strings"
type: issue
state: closed
author: dhruvmanila
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-09-22T09:23:30Z
updated_at: 2024-02-16T14:58:13Z
url: https://github.com/astral-sh/ruff/issues/7594
synced_at: 2026-01-10T11:09:49Z
```

# Formatter: handle comments, quotes, and expressions inside f-strings

---

_Issue opened by @dhruvmanila on 2023-09-22 09:23_

This issue is to kickoff a discussion around the formatting for f-strings. With [PEP 701], f-strings changes the behavior with what kind of syntax is allowed:

1. Comments inside f-strings:
```python
# comment 0
f"""
{ # comment 1
    # comment 2
    foo # comment 3
    # comment 4
}"""  # comment 5
```

2. F-strings can be nested within f-strings
```python
f"first {f"second {f"third ..."} second"} first"
```

3. Quotes can be repeated inside nested f-strings (for both strings and f-strings)
```python
f"double quotes {"same quotes in strings" + f"same quotes in f-strings"}"
```

This means that the formatter needs to be updated to possibly handle f-strings on it's own. Currently, it's handled as part of string formatting.

[PEP 701]: https://peps.python.org/pep-0701/

---

_Label `formatter` added by @dhruvmanila on 2023-09-22 09:23_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-22 10:04_

---

_Assigned to @dhruvmanila by @MichaReiser on 2023-09-27 13:32_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-10-23 09:01_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-23 09:01_

---

_Renamed from "Formatter: handle comments and quotes for f-strings" to "Formatter: handle comments, quotes, and expressions inside f-strings" by @dhruvmanila on 2023-12-11 15:24_

---

_Label `preview` added by @MichaReiser on 2024-02-14 16:30_

---

_Closed by @dhruvmanila on 2024-02-16 14:58_

---
