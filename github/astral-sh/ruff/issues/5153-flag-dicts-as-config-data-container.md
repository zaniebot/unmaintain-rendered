---
number: 5153
title: Flag dicts as config/data container
type: issue
state: closed
author: jankatins
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-06-16T22:50:16Z
updated_at: 2023-09-21T01:17:13Z
url: https://github.com/astral-sh/ruff/issues/5153
synced_at: 2026-01-07T13:12:15-06:00
---

# Flag dicts as config/data container

---

_Issue opened by @jankatins on 2023-06-16 22:50_

In a lot of cases, using dicts as "config" or "data" is an anti-pattern, as it doesn't allow for type inference and it pushes fallback handling to the user and not the declarer of the data structure. With config/data dicts I mean pattern like this:

```python
d = {
"key1": "something",
"key2": some_var, 
}

func(d) # bad, should be func(key1="something", key2=some_var) or taking a dataclass/... as input 
whatever = d["key2"].get("another_key", {})
```

I identify such "config/data" dicts by:

- Keys are strings
- Lookups via static keys, not via computed keys or iterations
- A lot of the time: dicts of dicts: `{"a": {"B": {"c": var,...},...},...}`
- (A lot of (duplicated) validation on the user side: `dict.get("key", fallback)` -> usually missing and so buggy)

Better alternatives:

- Use a dataclass / pydantic class / TypedDict class as that allows both validating the keys and the type of the values
- Define fallbacks during class declarations
- Make each value dict it's own class so that it's a composed object, not a nested dict
- If you call a func: add typed arguments (with defaults, if needed/possible)

It would be nice if ruff could flag such cases and suggest better alternatives.

---

_Label `question` added by @charliermarsh on 2023-06-17 14:40_

---

_Label `rule` added by @charliermarsh on 2023-06-17 14:40_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:11_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:11_

---

_Comment by @charliermarsh on 2023-09-21 01:17_

Gonna close for now as I think this will be hard to rigorously define and enforce without better type inference.

---

_Closed by @charliermarsh on 2023-09-21 01:17_

---
