```yaml
number: 10810
title: "new feat: Jump to source "
type: issue
state: closed
author: me-v2
labels:
  - question
assignees: []
created_at: 2024-04-07T02:48:59Z
updated_at: 2024-04-18T00:47:06Z
url: https://github.com/astral-sh/ruff/issues/10810
synced_at: 2026-01-10T11:09:53Z
```

# new feat: Jump to source 

---

_Issue opened by @me-v2 on 2024-04-07 02:48_

```
❯ ruff format .  --line-length 120 
error: Failed to parse graph/node.py:26:5: Expected 'Indent', but got 'llm'
2 files reformatted, 11 files left unchanged
```

click  "graph/node.py:26:5" go to source,  use can fix it instead  find file and location line no.


---

_Renamed from "new feat:  click file  go to source " to "new feat: Jump to source " by @me-v2 on 2024-04-07 02:57_

---

_Comment by @charliermarsh on 2024-04-07 03:34_

I think this would be a feature of your terminal rather than anything that Ruff could do. Like, in my terminal, I can right-click on `graph/node.py` and it will open the file in my default editor.

Do you know of similar tools that expose this, where you're seeing different behavior than for Ruff?

---

_Label `question` added by @charliermarsh on 2024-04-07 03:34_

---

_Comment by @me-v2 on 2024-04-08 01:10_

one case:
` ruff check .  
haiku/api/endpoints/chat/rag.py:74:37: W292 [*] No newline at end of file
`
can jump to code,

```
ruff format .  --line-length 120 
error: Failed to parse graph/node.py:26:5: Expected 'Indent', but got 'llm'
2 files reformatted, 11 files left unchanged

``` 
not do

---

_Comment by @CommanderStorm on 2024-04-12 00:44_

Currently, there is really not enough context to look if there is something that can be improved.
Lets get this into a reproducible direction:

- What directory are you in?
- What terminal emulators are you using? What happens in other terminal emulators?
- What is the minimal, reproducible output?

---

_Comment by @charliermarsh on 2024-04-18 00:47_

Closing for now as it doesn't feel actionable.

---

_Closed by @charliermarsh on 2024-04-18 00:47_

---
