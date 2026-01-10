```yaml
number: 10357
title: E402 false positive with cell magics in Jupyter notebooks
type: issue
state: closed
author: jamesmyatt
labels:
  - rule
  - notebook
assignees: []
created_at: 2024-03-12T11:45:42Z
updated_at: 2024-03-24T10:50:01Z
url: https://github.com/astral-sh/ruff/issues/10357
synced_at: 2026-01-10T11:09:52Z
```

# E402 false positive with cell magics in Jupyter notebooks

---

_Issue opened by @jamesmyatt on 2024-03-12 11:45_

The E402 rule should permit cell magics as well as comments before the import statements. The cell magics must come first.

e.g.

```
%%time
import expensive_module
```

I suspect that line magics should also be permitted before import statements, like:

```
%time import expensive_module
import cheap_module
```

Error message is:

```
E402 Module level import not at top of cell
```

---

_Label `rule` added by @charliermarsh on 2024-03-12 14:29_

---

_Label `needs-decision` added by @charliermarsh on 2024-03-12 14:29_

---

_Comment by @dhruvmanila on 2024-03-13 08:04_

Yeah, I agree. Certain cell magics like the one you mentioned should come before the actual code.

I don't think we flag `E402` if there's a comment before the first import: https://play.ruff.rs/8d6055cc-be7c-41d9-aac3-ef991213116c

---

_Label `notebook` added by @dhruvmanila on 2024-03-13 08:04_

---

_Label `needs-decision` removed by @dhruvmanila on 2024-03-13 08:04_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-03-13 15:26_

---

_Closed by @dhruvmanila on 2024-03-24 10:50_

---
