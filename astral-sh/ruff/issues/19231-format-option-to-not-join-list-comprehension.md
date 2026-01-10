```yaml
number: 19231
title: format option to not join list-comprehension multilines into one single line
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2025-07-09T12:12:43Z
updated_at: 2025-08-04T12:00:38Z
url: https://github.com/astral-sh/ruff/issues/19231
synced_at: 2026-01-10T11:09:59Z
```

# format option to not join list-comprehension multilines into one single line

---

_Issue opened by @spaceone on 2025-07-09 12:12_

I am generally allowing a lower line length: 120 chars because the code base has function calls with many parameters.
On the other side I format certain list-comprehension code blocks for readability into multiple lines.

one example:
```diff
-    return [
-        idx * 8 + bit
-        for idx, value in enumerate(octets)
-        for bit in range(8)
-        if value & (1 << bit)
-    ]
+    return [idx * 8 + bit for idx, value in enumerate(octets) for bit in range(8) if value & (1 << bit)]
```

It would be nice if the formatter would allow a setting, that already multiline-splitted list/set/dict comprehensions are not joined into one single line again.
Maybe a line-length for comprehensions may differ from a general line length for function calls / the rest.

Basically that is the number one blocking issue for using the formatter at all.

---

_Closed by @MichaReiser on 2025-07-09 12:17_

---

_Comment by @Germanki on 2025-08-04 12:00_

Seems also to be a duplicate of #11753 

---
