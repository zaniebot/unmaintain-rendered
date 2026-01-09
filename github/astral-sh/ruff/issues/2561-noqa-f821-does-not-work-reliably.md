---
number: 2561
title: "noqa: F821 does not work reliably"
type: issue
state: closed
author: FranzForstmayr
labels:
  - bug
assignees: []
created_at: 2023-02-04T00:39:48Z
updated_at: 2023-02-04T07:58:16Z
url: https://github.com/astral-sh/ruff/issues/2561
synced_at: 2026-01-07T13:12:14-06:00
---

# noqa: F821 does not work reliably

---

_Issue opened by @FranzForstmayr on 2023-02-04 00:39_

Hi, 

I have following issue linting a sphinx configuration file, which has a special variable `tags` without importing available. Don't ask me why, see point 6. https://www.sphinx-doc.org/en/master/usage/configuration.html

I wanted to ignore this specific line for ruff but `noqa: F821` does not work when the undefined variable is not the first entry.
See this reproducer:

```python
# Following is not marked as error
tags1  # noqa: F821

# Following line is marked as error
if 'versionwarning' in tags2:  #noqa: F821
    pass
```
gives 
```
test.py:5:24: F821 Undefined name `tags2`
Found 1 error.
```

```
ruff --version
ruff 0.0.240
```

---

_Referenced in [scipy/scipy#15929](../../scipy/scipy/pulls/15929.md) on 2023-02-04 00:41_

---

_Comment by @charliermarsh on 2023-02-04 01:10_

Will take a look at this tonight!

---

_Label `bug` added by @charliermarsh on 2023-02-04 01:10_

---

_Comment by @charliermarsh on 2023-02-04 01:10_

(On my phone but is it possible you need a space between the hash and the noqa in the second example? Just a guess before I debug properly in a bit.)

---

_Comment by @charliermarsh on 2023-02-04 01:28_

Yeah I think that solves the problem. Flake8 has the same behavior, so I think this is correct!

Specifically, you want this:

```py
# Following is not marked as error
tags1  # noqa: F821

# Following line is marked as error
if 'versionwarning' in tags2:  # noqa: F821
    pass
```

---

_Closed by @charliermarsh on 2023-02-04 01:28_

---

_Comment by @charliermarsh on 2023-02-04 01:28_

(Let me know if you're still seeing this behavior after that change and I'll be happy to reopen.)

---

_Referenced in [scipy/scipy#17878](../../scipy/scipy/pulls/17878.md) on 2023-02-04 02:04_

---

_Comment by @FranzForstmayr on 2023-02-04 07:58_

Thank's for your time and quick help :)
I usually don't dig around with whitespace variations in python, probably that's why I don't found the issue :)

---
