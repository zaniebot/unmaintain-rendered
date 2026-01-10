```yaml
number: 1798
title: PIE794 should only remove simple code?
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2023-01-11T23:22:57Z
updated_at: 2023-01-16T08:20:47Z
url: https://github.com/astral-sh/ruff/issues/1798
synced_at: 2026-01-10T11:09:44Z
```

# PIE794 should only remove simple code?

---

_Issue opened by @spaceone on 2023-01-11 23:22_

```
class Foo(object):
   bar = baz()
   bar = foo(bar)
```

`bar` uses `bar`. The fixer removes that to (or something else since #1794):
```
class Foo(object):
   bar = baz()
```

Even if the bar reference is not used I would say the code should not be changed as it makes function calls which might be necessary.


---

_Comment by @charliermarsh on 2023-01-11 23:32_

Hmm. It's hard to think of logic that would work here, yet would still make the autofix behavior even marginally useful. The primary use-case is redefined fields in ORMs, which necessitate function calls. I guess in theory we could try to omit any call that relies on a variable bound within the same scope, but that will take a bit of time.


---

_Label `question` added by @charliermarsh on 2023-01-12 01:02_

---

_Comment by @charliermarsh on 2023-01-12 01:03_

Do you have any real-world examples of where this is triggering?

---

_Comment by @charliermarsh on 2023-01-12 01:10_

Actually, here's what I would propose (which would also resolve the other issue you filed around this rule): we only autofix if the _values_ are the same. (Right now, we use that same logic for duplicate dictionary keys, so it feels very consistent.)

---

_Closed by @charliermarsh on 2023-01-16 08:20_

---
