---
number: 13621
title: Autofix or better error message/docs for PERF401 + walrus operator
type: issue
state: closed
author: jamesbraza
labels:
  - documentation
assignees: []
created_at: 2024-10-03T23:13:48Z
updated_at: 2024-10-07T15:41:03Z
url: https://github.com/astral-sh/ruff/issues/13621
synced_at: 2026-01-10T01:22:54Z
---

# Autofix or better error message/docs for PERF401 + walrus operator

---

_Issue opened by @jamesbraza on 2024-10-03 23:13_

With this code:

```python
my_list = []
for element in my_list:
    if walrus_element := element:
        my_list.append(walrus_element)
```

`ruff==0.6.7`'s [PERF401](https://docs.astral.sh/ruff/rules/manual-list-comprehension/) errors like so: `PERF401 Use a list comprehension to create a transformed list`

However, due to the walrus operator (also see https://github.com/astral-sh/ruff/issues/6056), the fix involves putting parenthesis around the walrus.

This is just confusing enough that at first one thinks it's a PERF401 false positive. The request here is to either:
- Document this special case in the PERF401 docs
- Emit a special and more specific PERF401 error message when a walrus operator is present
- Implement an autofix for PERF401 that knows about the parenthesis

---

_Comment by @zanieb on 2024-10-04 01:51_

There's never an autofix for PERF401 now, right? I'm a bit confused about why this case feels like a false positive, can you share a bit more?

---

_Comment by @jamesbraza on 2024-10-04 03:06_

Yeah there's no autofix, I was trying to say a possible solution is just doing an autofix.

Does this clarify? It's that what the PERF401 docs suggest in this case is a `SyntaxError`:

```python
my_list = []
for element in my_list:
    if walrus_element := element:
        my_list.append(walrus_element)

# What ruff's PERF401 docs say, which leads to: SyntaxError: invalid syntax
my_list = [
    walrus_element for element in my_list if walrus_element := element
]

# What you actually need to do is know this parenthesis trick
my_list = [
    walrus_element for element in my_list if (walrus_element := element)
]
```

---

_Label `question` added by @MichaReiser on 2024-10-07 08:03_

---

_Label `question` removed by @MichaReiser on 2024-10-07 08:06_

---

_Label `documentation` added by @MichaReiser on 2024-10-07 08:06_

---

_Comment by @MichaReiser on 2024-10-07 08:07_

I can see how the walrus case is confusing because it requires manual code adjusting to make it valid syntax. However, our goal is to keep the documentation concise and that doesn't leave room to document every possible syntax combination. 

> Yeah there's no autofix, I was trying to say a possible solution is just doing an autofix.

I understand that a fix is desirable, but implementing a code fix is often very involved, especially in cases like the one you outline above. We would accept a code fix implementation if someone is interested in working on it. 

I'm closing this as not planned. While I agree that finding the right fix in the walrus case is somewhat more involved, the rule documentation isn't the right place to teach this. An automatic fix would be nice but it can be added without a tracking issue

---

_Closed by @MichaReiser on 2024-10-07 08:07_

---

_Comment by @jamesbraza on 2024-10-07 15:41_

I follow your logic and sounds good 👍 

---
