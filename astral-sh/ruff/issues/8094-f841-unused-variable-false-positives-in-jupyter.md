```yaml
number: 8094
title: "`F841` (unused-variable) false positives in Jupyter notebooks"
type: issue
state: open
author: tdulcet
labels:
  - bug
  - wish
  - notebook
assignees: []
created_at: 2023-10-20T15:31:23Z
updated_at: 2024-02-27T06:47:56Z
url: https://github.com/astral-sh/ruff/issues/8094
synced_at: 2026-01-12T15:54:47Z
```

# `F841` (unused-variable) false positives in Jupyter notebooks

---

_@tdulcet_

* A minimal code snippet that reproduces the bug.
```jupyter
var = "World"
!echo "Hello $var"
```
or
```jupyter
var = "World"
!echo "{'Hello ' + var}"
```
(Note that one can put arbitrary Python expressions/code in the above brackets, which is not currently linted.)

Ruff actual output:
```
test.ipynb:cell 1:1:3: F841 Local variable `var` is assigned to but never used
```
Ruff expected output: None
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
```
ruff --select F841 --isolated test.ipynb
```
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
None
* The current Ruff version (`ruff --version`).
```
ruff 0.1.1
```

There is a [public notebook here](https://github.com/tdulcet/Distributed-Computing-Scripts/blob/master/google-colab/GoogleColabGPU.ipynb) that currently triggers three of these false positives, if you want some real world examples.

Possibly related to #5188 and #1079.

---

_Label `bug` added by @charliermarsh on 2023-10-20 21:24_

---

_Label `wish` added by @charliermarsh on 2023-10-20 21:24_

---

_Comment by @dhruvmanila on 2023-10-30 02:41_

Thanks for opening this issue. Yes, this is the current limitation of linting Notebooks because Ruff considers everything past the IPython escape token (`!` in this case) as string. The reason being that there's no defined grammar on what can occur in that position and there could be any non-Python syntax as well. This is seen in your first example where bash syntax is being used instead (`!echo "Hello $var"`).

---

_Comment by @dhruvmanila on 2023-10-31 03:28_

Closing in favor of #8354 

---

_Closed by @dhruvmanila on 2023-10-31 03:28_

---

_Comment by @tdulcet on 2024-01-04 09:38_

Perhaps this issue should be reopened since #8354 has now been closed, but this bug still exists in Ruff.

---

_Reopened by @charliermarsh on 2024-01-04 16:32_

---

_Label `notebook` added by @dhruvmanila on 2024-02-12 05:42_

---

_Comment by @dhruvmanila on 2024-02-12 05:48_

@tdulcet Can you help me understand the mentioned code? Is it that there can be _any_ valid Python code between the curly braces?

```python
!echo "{<any valid Python code can go here>}"
```

And, does that need to be wrapped in quotes?

```python
var = "/path/to/foo"
# Does this need to be wrapped in quotes?
!ls {var}
```

---

_Comment by @dhruvmanila on 2024-02-12 05:49_

Ok, I just tried this and it seems that it does need to be quoted which makes me wonder if it's treated as format expression with the global scope passed in:

```python
"!ls {var}".format(globals())
```

I'll look at the source code later to check this.

---

_Comment by @tdulcet on 2024-02-12 10:40_

> Is it that there can be _any_ valid Python code between the curly braces?

Yes, any valid Python code can be between the braces, except code that contains more braces, so no f-strings or format strings.

> And, does that need to be wrapped in quotes?

This specific example does not need to be wrapped in quotes, but in general it does if the variable could contain whitespace or other special characters, to prevent globbing and word splitting by the shell:
```python
var = "/path/to/foo bar" # Variable with whitespace
# This needs to be wrapped in quotes
!ls "{var}"
```
Note that this is obviously not a robust solution against potenchally malicious inputs, where the string contains quote characters itself:
```python
var = '/path/to/foo"; rm -rf /"'
# Oh no!
!ls "{var}"
```
In this case, one would likely need to use [shlex.quote](https://docs.python.org/3/library/shlex.html#shlex.quote):
```python
import shlex
var = "/path/to/foo; rm -rf /"
# No quotes needed
!ls {shlex.quote(var)}
```

---

_Comment by @jpmckinney on 2024-02-27 06:47_

The same issue can can `ARG001` e.g. 

```
def foo(var="World"):
    !echo "Hello $var"
```

---
