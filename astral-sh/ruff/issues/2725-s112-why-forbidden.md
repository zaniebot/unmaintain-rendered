```yaml
number: 2725
title: "S112: why forbidden?"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-02-10T18:15:28Z
updated_at: 2023-02-10T18:24:46Z
url: https://github.com/astral-sh/ruff/issues/2725
synced_at: 2026-01-10T11:09:45Z
```

# S112: why forbidden?

---

_Issue opened by @spaceone on 2023-02-10 18:15_

why is `S112` disallowing this? 
```
for foo in bar:
    try:
        if foo['bar'][0] == baz:
            continue
    except (KeyError, IndexError):
        continue
```

I can live with disallowing `except Exception:` but I don't see above makes sense.

---

_Comment by @charliermarsh on 2023-02-10 18:17_

I think that's a bug, we're not handling the tuple correctly.

---

_Label `bug` added by @charliermarsh on 2023-02-10 18:17_

---

_Comment by @edgarrmondragon on 2023-02-10 18:19_

Just to confirm, @spaceone do you have [`check-typed-exception`](https://github.com/charliermarsh/ruff#check-typed-exception) set to true?

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-10 18:20_

---

_Comment by @charliermarsh on 2023-02-10 18:20_

I _think_ it's unrelated to that setting -- we have `checker.resolve_call_path(type_).map_or(true, ...)`, so if the value isn't resolvable to a reference (which would be the case for any tuple), then we'll trigger the check. But, testing now...

---

_Closed by @charliermarsh on 2023-02-10 18:24_

---
