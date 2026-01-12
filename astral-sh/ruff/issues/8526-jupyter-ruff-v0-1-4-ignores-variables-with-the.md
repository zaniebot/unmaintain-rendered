```yaml
number: 8526
title: "jupyter: ruff v0.1.4 ignores variables with the same name as magic commands"
type: issue
state: closed
author: njzjz
labels:
  - bug
assignees: []
created_at: 2023-11-06T20:15:34Z
updated_at: 2024-01-29T12:55:45Z
url: https://github.com/astral-sh/ruff/issues/8526
synced_at: 2026-01-12T15:54:48Z
```

# jupyter: ruff v0.1.4 ignores variables with the same name as magic commands

---

_@njzjz_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

This is the previous Jupyter Notebook file, and it had no lint errors with ruff v0.1.3.

![image](https://github.com/astral-sh/ruff/assets/9496702/1556ef28-1846-45df-a330-6eddf9b89bdd)

When ruff was upgraded to v0.1.4 (https://github.com/deepmodeling/dpdata/pull/573/), `import dpdata` got removed,

![image](https://github.com/astral-sh/ruff/assets/9496702/b4ab9a90-c366-4418-8dbf-972d1c08ba0b)

Besides, F821 was complained:
```
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

docs/nb/try_dpdata.ipynb:cell 4:1:1: F821 Undefined name `system`
docs/nb/try_dpdata.ipynb:cell 5:1:1: F821 Undefined name `system`
docs/nb/try_dpdata.ipynb:cell 6:1:1: F821 Undefined name `system`
Found 3 errors.
```

It's obvious that ruff doesn't consider variables in the other cells.


---

_Label `bug` added by @zanieb on 2023-11-06 20:25_

---

_Comment by @charliermarsh on 2023-11-06 20:25_

In this case, it's because `system` is a Jupyter magic which can be used as an automagic. In Jupyter, you can do `system ls` in a cell (without any percent signs), and it will treat the command as an `ls`. We started ignoring cells that look like automagics, because it can be hard (impossible) to understand their behavior, since Jupyter will actually treat them differently depending on what other variables are defined, i.e., the order in which you run your cells.

If you use any other variable name, it will work as expected, but we'll consider how to make this more targeted.

---

_Comment by @charliermarsh on 2023-11-06 20:27_

That explanation is probably unclear, here's an image that demonstrates what I'm describing -- the first and third cells are identical, but their behaviors entirely depend on the other of execution:

<img width="395" alt="Screen Shot 2023-11-06 at 3 26 41 PM" src="https://github.com/astral-sh/ruff/assets/1309177/eaa61200-9239-468d-a52f-ff384731b606">



---

_Comment by @charliermarsh on 2023-11-06 20:35_

\cc @dhruvmanila since we discussed this a bit at the time

---

_Comment by @dhruvmanila on 2023-11-07 13:30_

Yeah, this is tricky. We could implement the simple solution that we discussed earlier about having support for only `pip` but then the same problem would appear.

The far fetched solution would be to not ignore such cells and perform some semantic analysis on the parsed code but then it could potentially raise a syntax error (`cd ..`). For any potential automagic command, the semantic model would probably check if the identifier (`system`, `cd`, etc.) is already defined in the scope. If it is, continue as Python code otherwise continue as an automagic.

I'd actually want to spend some time figuring out how IPython does it as it could lead to some insights.

---

_Comment by @dhruvmanila on 2023-11-07 13:58_

So, IPython is doing exactly that: https://github.com/ipython/ipython/blob/de97f0032dd96e0109780396e9219e8d73073e29/IPython/core/prefilter.py#L428-L467

For something like `pwd = "foo"`, the `AssignmentChecker` will run it normally as Python code and then for any other usage of `pwd`, the `AutoMagicChecker` checks if it's shadowed or not and runs it as a magic command accordingly.

---

_Comment by @charliermarsh on 2023-11-07 15:37_

Hmm yeah. We could apply some similar heuristics though they'd need to be more expansive... We could also try parsing, then fallback to automagic if the code doesn't parse, but that will also be wrong in some cases. I'd just really like to avoid doing semantic analysis on the code block to understand how to parse it. 



---

_Renamed from "jupyter: ruff v0.1.4 doesn't consider variables in the other cells" to "jupyter: ruff v0.1.4 ignores variables with the same name as magic commands" by @njzjz on 2023-11-08 18:28_

---

_Comment by @charliermarsh on 2024-01-26 16:41_

This is fixed by #9653.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-26 16:41_

---

_Closed by @charliermarsh on 2024-01-29 12:55_

---
