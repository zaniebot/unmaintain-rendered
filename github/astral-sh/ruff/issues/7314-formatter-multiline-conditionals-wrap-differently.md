---
number: 7314
title: "Formatter: multiline conditionals wrap differently"
type: issue
state: closed
author: DanCardin
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-09-12T19:29:59Z
updated_at: 2023-09-29T17:27:34Z
url: https://github.com/astral-sh/ruff/issues/7314
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: multiline conditionals wrap differently

---

_Issue opened by @DanCardin on 2023-09-12 19:29_

Black wants to split boolean operators across lines, where they're being forced (at least here due to the comment) onto multiple lines:
```python
some_example_var = ""
if not some_example_var or (
    # A comment in the middle
    some_example_var
    and some_example_var not in some_example_var
):
    pass
```

whereas ruff wants to combine them onto one line:
```python
some_example_var = ""
if not some_example_var or (
    # A comment in the middle
    some_example_var and some_example_var not in some_example_var
):
    pass
```

I dont really have a preference, it was just one of 3 total formatting diffs over 30k LOC (one being a documented intentional diff).

---

_Label `formatter` added by @charliermarsh on 2023-09-12 19:52_

---

_Label `needs-decision` added by @charliermarsh on 2023-09-12 19:52_

---

_Comment by @MichaReiser on 2023-09-13 07:30_

Thanks for reporting this deviation. It isn't specific to bool expression but more generally that Black expands the commented node whereas Ruff does not (because associate the comment with the outer most vs inner most node):

**Input**
```python
# A comment in the middle
some_example_var and some_example_var not in some_example_var

a = (
    # test
    a if b else c
)

a = x[
    #test
    a + b : b + c
]

a = (
    # A comment in the middle
    some_example_var and some_example_var not in some_example_var
)
```

**Black**
```python
# A comment in the middle
some_example_var and some_example_var not in some_example_var

a = (
    # test
    a
    if b
    else c
)

a = x[
    # test
    a
    + b : b
    + c
]

a = (
    # A comment in the middle
    some_example_var
    and some_example_var not in some_example_var
)
```
**Ruff**
```python
# A comment in the middle
some_example_var and some_example_var not in some_example_var

a = (
    # test
    a if b else c
)

a = x[
    # test
    a + b : b + c
]

a = (
    # A comment in the middle
    some_example_var and some_example_var not in some_example_var
)

```

I generally prefer Ruff's formatting because it avoids unnecessary line breaks. But eager to hear more opinions. 

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-13 07:31_

---

_Label `wontfix` added by @MichaReiser on 2023-09-13 07:31_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-13 07:32_

---

_Comment by @DanCardin on 2023-09-13 13:08_

Ultimately i think the effect is very marginal either way, but I think perhaps I agree.

Particularly with the deviations regarding inline comments, it seems like this gives more control over where the comment "targets", as you can insert more comments to arrive at the same formatting decision otherwise, if it's important to the block of code.

Mostly posted so it could get added to the list of intentional deviations, if you were going to keep it.

---

_Comment by @MichaReiser on 2023-09-14 07:21_

> Mostly posted so it could get added to the list of intentional deviations, if you were going to keep it.

Thank you for reporting it. It's extremely helpful for us to have a collection of all deviations because we should either:

* Document them or
* fix them. 

So thanks again :)

---

_Comment by @charliermarsh on 2023-09-27 15:02_

We're going to mark this as an intentional deviation for now. Can be closed once it's documented.

---

_Label `documentation` added by @MichaReiser on 2023-09-27 16:04_

---

_Label `wontfix` removed by @MichaReiser on 2023-09-27 16:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 18:44_

---

_Referenced in [astral-sh/ruff#7679](../../astral-sh/ruff/pulls/7679.md) on 2023-09-27 20:18_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---
