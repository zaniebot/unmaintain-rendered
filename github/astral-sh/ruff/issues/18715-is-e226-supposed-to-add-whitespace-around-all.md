---
number: 18715
title: Is E226 supposed to add whitespace around all plus signs, even in list slices?
type: issue
state: closed
author: kaddkaka
labels:
  - question
assignees: []
created_at: 2025-06-17T07:26:49Z
updated_at: 2025-07-24T12:01:08Z
url: https://github.com/astral-sh/ruff/issues/18715
synced_at: 2026-01-07T13:12:16-06:00
---

# Is E226 supposed to add whitespace around all plus signs, even in list slices?

---

_Issue opened by @kaddkaka on 2025-06-17 07:26_

### Question

The rules E226 https://docs.astral.sh/ruff/rules/missing-whitespace-around-arithmetic-operator/ seem to make this code change which I think make the code less readable compare to before:

```diff
-time[offset+1:offset+2]
+time[offset + 1:offset + 2]
```

is this intended? Is there any configuration for this rule? 

### Version

_No response_

---

_Label `question` added by @kaddkaka on 2025-06-17 07:26_

---

_Comment by @ntBre on 2025-06-17 18:28_

Hmm, I'm not sure about this one. [PEP 8](https://peps.python.org/pep-0008/#pet-peeves) includes at least two different "Correct" formatting examples for related cases:

```python
ham[lower+offset : upper+offset]
ham[lower + offset : upper + offset]
```

`pycodestyle` flags your first case, but running `autopep8` fixes it to the strange-looking

```python
time[offset +1:offset +2]
```

which apparently satisfies `pycodestyle`.

So I think our E226 results are reasonable, but I'd prefer the results from our formatter or from black, which match the second example from PEP 8:

```python
time[offset + 1 : offset + 2]
```

---

_Comment by @MichaReiser on 2025-06-18 08:15_

Yes, I think we changed this so to make the rule compatible with our formatter.

---

_Closed by @MichaReiser on 2025-07-24 12:01_

---
