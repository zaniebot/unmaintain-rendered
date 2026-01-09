---
number: 12096
title: Ruff extension for Visual Studio Code not show error E501 (line-too-long)
type: issue
state: closed
author: alejandro-kid
labels:
  - needs-info
assignees: []
created_at: 2024-06-28T19:48:38Z
updated_at: 2024-07-26T01:23:48Z
url: https://github.com/astral-sh/ruff/issues/12096
synced_at: 2026-01-07T13:12:15-06:00
---

# Ruff extension for Visual Studio Code not show error E501 (line-too-long)

---

_Issue opened by @alejandro-kid on 2024-06-28 19:48_

I am using Visual Studio Code Insider, version 1.85, in Fedora Linux version 39.
Don't matter what config I set, never shows when the line exceeds de configured limit. The other features in RUFF work well.


---

_Comment by @charliermarsh on 2024-06-28 19:59_

We actually don't enable `E501` by default. Are you enabling it in your configuration file?

---

_Label `needs-info` added by @dhruvmanila on 2024-07-01 04:33_

---

_Comment by @Denis-Alexeev on 2024-07-10 20:22_

@charliermarsh I have found 1 case when ruff does not raise E501. Pls tell me if I should create new issue for that.
This is steps to reproduce.

I have 2 modules: `other.py` and `test_new.py`

`other.py`
```python
def very_very_very_long_function_name(attr: int):
    pass

```

`test_new.py`
```python
"""docstring."""

import logging

import other

logging.info("first long")
other.very_very_very_long_function_name(9)
logging.info("second long")

```

`ruff.toml`
```toml
target-version = "py312"
line-length = 17

[lint]
select = ["ALL"]

[lint.pydocstyle]
convention = "google"
```

```bash
ruff --version                                                       
ruff 0.5.1

python --version   
Python 3.12.2
```

`command`
```bash
ruff check --config ./ruff.toml --output-format concise ./test_new.py
```

`output`
```bash
test_new.py:7:18: E501 Line too long (26 > 17)
test_new.py:9:18: E501 Line too long (27 > 17)
Found 2 errors.
```

`Expected behavior`
3 violations of E501

`Actual behavior`
2 violations of E501

---

_Comment by @MichaReiser on 2024-07-10 20:32_

@Denis-Alexeev The problem here is that the line 8 contains no whitespace ([see the rules documentation](https://docs.astral.sh/ruff/rules/line-too-long/#why-is-this-bad)). 

I think we could do better here now that is kind of cheap to get the tokens at a specific offset by only applying the exception rules if the range falls into a string (wdyt @charliermarsh )?

---

_Comment by @Denis-Alexeev on 2024-07-10 20:41_

@MichaReiser Oh, I guess I've misread the first exception. I always thought, that it is about exactly one word. Now I see that the word "word" has quotation marks 😄 

---

_Closed by @charliermarsh on 2024-07-26 01:23_

---
