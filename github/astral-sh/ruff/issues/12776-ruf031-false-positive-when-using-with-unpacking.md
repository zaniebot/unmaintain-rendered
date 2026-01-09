---
number: 12776
title: "RUF031: False positive when using with unpacking "
type: issue
state: closed
author: alexfikl
labels:
  - bug
  - rule
assignees: []
created_at: 2024-08-09T08:12:34Z
updated_at: 2024-08-09T15:30:30Z
url: https://github.com/astral-sh/ruff/issues/12776
synced_at: 2026-01-07T13:12:15-06:00
---

# RUF031: False positive when using with unpacking 

---

_Issue opened by @alexfikl on 2024-08-09 08:12_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The following run should not indicate any errors
```
>> ruff check --isolated --preview --select RUF031 --target-version py38 subscript-tuple.py
subscript-tuple.py:1:3: RUF031 [*] Avoid parentheses for tuples in subscripts.
|
1 | d[(*indices, 5)]
|   ^^^^^^^^^^^^^ RUF031
|
= help: Remove the parentheses.

Found 1 error.
[*] 1 fixable with the `--fix` option.
```
As far as I know, `d[*indices, 5]` is invalid syntax on Python 3.8, but should work for Python >=3.9 (with the new PEG parser, I think? definitely works on 3.12).

Expected result:
* No errors / fixes reported when setting `--target-version py38`.
* The recommended fix when setting `--target-version py39` and above.

xref: CI failure resulting from the fix: https://github.com/inducer/pymbolic/actions/runs/10315281929/job/28555159989?pr=144






---

_Label `bug` added by @MichaReiser on 2024-08-09 08:52_

---

_Label `rule` added by @MichaReiser on 2024-08-09 08:53_

---

_Comment by @MichaReiser on 2024-08-09 08:53_

Thanks for reporting this preview error!

@dylwil3  do you want to take this. Surprising how many edge cases there are to this rule :open_mouth: 

---

_Comment by @dylwil3 on 2024-08-09 11:51_

Sure!

This has been a good learning experience in both Rust and the intricacies of Python, so I'm happy to keep taking on RUF031 bugs 😄 

---

_Assigned to @dylwil3 by @MichaReiser on 2024-08-09 12:41_

---

_Referenced in [astral-sh/ruff#12784](../../astral-sh/ruff/pulls/12784.md) on 2024-08-09 13:01_

---

_Comment by @dylwil3 on 2024-08-09 13:27_

The CI link above seems to also show a syntax error in Python 3.9, unless I'm reading it wrong:

https://github.com/inducer/pymbolic/actions/runs/10315281929/job/28555160514?pr=144

I can confirm 3.11 is ok, but I gotta go check 3.10. Will update PR once I get a chance to do that.

---

_Comment by @alexfikl on 2024-08-09 13:31_

> The CI link above seems to also show a syntax error in Python 3.9, unless I'm reading it wrong:

Ah, yes! I didn't check that because it got cancelled, but it seems like 3.9 also doesn't support unpacking in the subscript. Sorry for the confusion.



---

_Comment by @dylwil3 on 2024-08-09 14:00_

Looks like this is a syntax error in 3.10 as well. It was allowed starting in 3.11, hidden in [PEP-646](https://peps.python.org/pep-0646/#change-1-star-expressions-in-indexes)

---

_Closed by @MichaReiser on 2024-08-09 15:30_

---
