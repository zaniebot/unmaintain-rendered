```yaml
number: 10234
title: "Feature Request: Rule for Flagging Missing Required Parameters in Function Calls"
type: issue
state: closed
author: gtupak
labels:
  - type-inference
assignees: []
created_at: 2024-03-05T03:11:23Z
updated_at: 2024-10-24T14:34:12Z
url: https://github.com/astral-sh/ruff/issues/10234
synced_at: 2026-01-10T11:09:52Z
```

# Feature Request: Rule for Flagging Missing Required Parameters in Function Calls

---

_Issue opened by @gtupak on 2024-03-05 03:11_

Would it be possible to add a rule in Ruff to detect and flag missing required parameters when calling a function? 

This feature would enhance error detection, especially in complex projects, by ensuring that all necessary arguments are provided during function calls. Implementing such a rule could prevent runtime errors related to missing arguments and improve code robustness. 

Here's an example:

```python
def test(required_param: str):
    print(required_param)

test() # This should raise an error
```

---

_Comment by @MichaReiser on 2024-03-05 09:06_

Agree, that would be useful. Although I'm not sure if that would be covered by a type checker instead #3893 

---

_Comment by @zanieb on 2024-03-11 17:23_

Let's track this there as it would otherwise require multi-file analysis to be useful.

---

_Closed by @zanieb on 2024-03-11 17:23_

---

_Label `type-inference` added by @zanieb on 2024-03-11 17:24_

---

_Label `multifile-analysis` added by @zanieb on 2024-03-11 17:24_

---

_Comment by @JohannesBuchner on 2024-03-25 14:42_

@gtupak you can try [pystrict3](https://github.com/JohannesBuchner/pystrict3/)

---

_Comment by @gtupak on 2024-03-25 22:59_

@JohannesBuchner I'm already using pylance but it would be great to have this in ruff.

I'll keep following the progress on this in https://github.com/astral-sh/ruff/issues/3893

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---
