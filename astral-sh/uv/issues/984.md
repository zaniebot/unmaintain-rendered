```yaml
number: 984
title: Should we allow relative URLs in direct path dependencies?
type: issue
state: closed
author: charliermarsh
labels:
  - question
assignees: []
created_at: 2024-01-19T00:14:25Z
updated_at: 2024-01-22T14:20:32Z
url: https://github.com/astral-sh/uv/issues/984
synced_at: 2026-01-10T05:40:31Z
```

# Should we allow relative URLs in direct path dependencies?

---

_Issue opened by @charliermarsh on 2024-01-19 00:14_

E.g., in your requirements file:

```
black @ ../black
```

We allow these with editable installs, and we allow this via `black @ file://${PROJECT_ROOT}/../black`. I'd kind of like it to be consistent between editable installs and non-editable installs though...

See: https://discuss.python.org/t/what-is-the-correct-interpretation-of-path-based-pep-508-uri-reference/2815. pip isn't sure how to interpret the relative path.


---

_Label `question` added by @charliermarsh on 2024-01-19 00:14_

---

_Comment by @konstin on 2024-01-19 10:10_

I like that syntax, and i prefer it over `black @ file:../black` (a nightmare with RFCs) and strongly over `black @ file://${PROJECT_ROOT}/../black`

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-19 16:28_

---

_Comment by @charliermarsh on 2024-01-19 16:28_

Okay, I'll change it today. It won't be compatible with `pip` but it will be a superset...

---

_Closed by @charliermarsh on 2024-01-22 14:20_

---
