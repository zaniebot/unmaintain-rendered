---
number: 5471
title: SIM102 Warning when using multiple if-statements with walrus operator
type: issue
state: closed
author: macintacos
labels:
  - question
assignees: []
created_at: 2023-07-03T04:06:59Z
updated_at: 2023-07-17T17:21:41Z
url: https://github.com/astral-sh/ruff/issues/5471
synced_at: 2026-01-07T13:12:15-06:00
---

# SIM102 Warning when using multiple if-statements with walrus operator

---

_Issue opened by @macintacos on 2023-07-03 04:06_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I have some nested walrus operator code that is making this warning show up. Code looks something like this:

```python
if foo := bar():
	if baz := buz():
		# use 'foo' and 'baz' here
```

There is no way for me to combine these two if statements into one, since that's not (to my knowledge) supported in Python. Could this warning be silenced in these situations?


---

_Label `bug` added by @charliermarsh on 2023-07-03 04:32_

---

_Comment by @charliermarsh on 2023-07-03 04:34_

Hmm -- does this work, or am I misunderstanding the code?

```python
def bar():
    return 1

def buz():
    return 2

if (foo := bar()) and (baz := buz()):
    print(foo, baz)  # 1 2
```

---

_Label `bug` removed by @charliermarsh on 2023-07-05 02:12_

---

_Label `question` added by @charliermarsh on 2023-07-05 02:12_

---

_Comment by @macintacos on 2023-07-05 14:11_

Ah you're totally right - closing!

---

_Closed by @macintacos on 2023-07-05 14:11_

---

_Comment by @tylerlaprade on 2023-07-17 17:21_

I also ran into this problem and had to search Google for this answer. Could we please add the example to the [documentation](https://beta.ruff.rs/docs/rules/collapsible-if/) for this rule?

---
