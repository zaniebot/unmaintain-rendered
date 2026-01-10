```yaml
number: 18904
title: Blank lines after import block
type: issue
state: closed
author: JetVolcano
labels:
  - question
  - formatter
assignees: []
created_at: 2025-06-23T21:38:15Z
updated_at: 2025-07-24T12:00:01Z
url: https://github.com/astral-sh/ruff/issues/18904
synced_at: 2026-01-10T11:09:59Z
```

# Blank lines after import block

---

_Issue opened by @JetVolcano on 2025-06-23 21:38_

### Summary

When running ruff format ./ when I have code like this:
```python
import math
print(math.sin(10))
```
it outputs code like this:
```python
import math

print(math.sin(10))
```
it only added one line between the import

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Comment by @ntBre on 2025-06-23 22:35_

I believe this is the expected behavior and consistent with black. There would be two lines before a function or class definition but only one line between the imports and other statements.

---

_Label `question` added by @ntBre on 2025-06-23 22:35_

---

_Comment by @MeGaGiGaGon on 2025-06-24 00:51_

There is an option for that if you want to force two lines, [lines-after-imports](https://docs.astral.sh/ruff/settings/#lint_isort_lines-after-imports)

---

_Comment by @ntBre on 2025-06-24 02:26_

I don't  think `lines-after-imports` will help in the formatter alone: [playground](https://play.ruff.rs/4529c0ac-4c6f-45e0-9c3f-fc4cb29f0e9a?secondary=Format). It's a linter option for the isort rules. However, if you apply the fixes from the isort lint rules, the formatter will also accept 2 lines.

Related: https://github.com/astral-sh/ruff/issues/8270

---

_Renamed from "Formatting" to "Blank lines after import block" by @MichaReiser on 2025-06-24 05:14_

---

_Label `formatter` added by @MichaReiser on 2025-06-24 05:14_

---

_Closed by @MichaReiser on 2025-07-24 12:00_

---
