```yaml
number: 2206
title: "new check: suggest to iterate over dict.values() instead if items() if "
type: issue
state: closed
author: spaceone
labels:
  - rule
assignees: []
created_at: 2023-01-26T17:36:10Z
updated_at: 2023-07-10T01:30:46Z
url: https://github.com/astral-sh/ruff/issues/2206
synced_at: 2026-01-12T15:54:42Z
```

# new check: suggest to iterate over dict.values() instead if items() if 

---

_@spaceone_

B007 unused-loop-control-variable revealed and fixed code like:

```
-               for k, v in self.validation_errors.items():
+               for _k, v in self.validation_errors.items():
                        num += v.number_of_errors()
```

a new checker could make it even better:
```
-               for k, v in self.validation_errors.items():
+               for v in self.validation_errors.values():
                        num += v.number_of_errors()
```

---

_Label `rule` added by @charliermarsh on 2023-01-26 17:39_

---

_Comment by @andersk on 2023-01-26 22:29_

Some classes such as [`xml.etree.elementtree.Element`](https://docs.python.org/3/library/xml.etree.elementtree.html#xml.etree.ElementTree.Element) support `items` but not `values`.

---

_Comment by @JonathanPlasse on 2023-01-27 09:02_

It would be an aggressive rule.

---

_Comment by @dhruvmanila on 2023-06-10 16:36_

This is being implemented as part of `perflint` (#4789)

---

_Comment by @charliermarsh on 2023-07-10 01:30_

I believe we now support this via `PERF102`.

---

_Closed by @charliermarsh on 2023-07-10 01:30_

---
