---
number: 10273
title: Format breaks f-string with literal strings inside (versions 0.3.0 and 0.3.1)
type: issue
state: closed
author: mvaled
labels:
  - formatter
  - preview
  - style
assignees: []
created_at: 2024-03-07T11:03:53Z
updated_at: 2024-03-07T16:11:21Z
url: https://github.com/astral-sh/ruff/issues/10273
synced_at: 2026-01-07T13:12:15-06:00
---

# Format breaks f-string with literal strings inside (versions 0.3.0 and 0.3.1)

---

_Issue opened by @mvaled on 2024-03-07 11:03_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The f-string in the code is broken by the formater (version 0.3.1):

```python
import re
def build(extensions):
    regex = re.compile(f"({'|'.join(extensions)})$", re.IGNORECASE)
```

```
$ cat src/bug.py
import re
def build(extensions):
    regex = re.compile(f"({'|'.join(extensions)})$", re.IGNORECASE)


$ rye run ruff --version
ruff 0.3.1

$ rye run ruff format src/bug.py                                                                                                                                         1 file reformatted

$ cat src/bug.py                                                                                                                                                                   
import re


def build(extensions):
    regex = re.compile(f"({"|".join(extensions)})$", re.IGNORECASE)
```

Note: if using `--isolated` doesn't break the f-string:

```
import re


def build(extensions):
    regex = re.compile(f"({'|'.join(extensions)})$", re.IGNORECASE)
```

My `pyproject.toml` has little configuration for the formater:

```
[tool.ruff]
required-version = "0.3.1"
line-length = 100
target-version = "py312"

[tool.ruff.lint]
preview = true
ignore = [
  "E501", # line too long
  "B011", # 'assert False' is fine.
  "E741", # do not use variables named ‘l’, ‘O’, or ‘I’
  "C901", # fn is too complex
  "ISC001",
]
select = [
  "C",
  "E",
  "F",
  "W",
  "B",
  "INT",
  "ISC",    # Implicit string concat
  "G",      # f-string, %, and format in loggers is not logger-friendly.
  "RUF008",
  "W191",   # Indentation contains tabs
  "W605",   # Invalid escape sequence
  "W291",   # Trailing whitespace
  "W292",   # No newline at end of file
  "W293",   # Blank line contains whitespace
  "SIM118", # avoid 'key in dict.keys()'
  "SIM911", # use 'for k, v in dict.items()'.
  "SIM210", # use 'bool(a)' instead of 'True if a else False'
  "SIM211", # use 'not a' instead of 'False if a else True'
  "SIM401", # use 'd.get(k, default)' instead of 'd[k] if k in d else default'
  "SIM201", # use 'a != b' instead of 'not a == b'
  "SIM202", # use 'a == b' instead of 'not a != b'
  "SIM110", # https://docs.astral.sh/ruff/rules/reimplemented-builtin/
  "SIM101", # use 'isinstance(x, (T1, T2))' instead of two checks.
]

[tool.ruff.format]
preview = true
exclude = ["**/migrations/*.py"]
```

---

_Renamed from "Format break f-string with literal strings extensions" to "Format breaks f-string with literal strings inside" by @mvaled on 2024-03-07 11:04_

---

_Renamed from "Format breaks f-string with literal strings inside" to "Format breaks f-string with literal strings inside (versions 0.3.0 and 0.3.1)" by @mvaled on 2024-03-07 11:14_

---

_Comment by @dhruvmanila on 2024-03-07 11:34_

Hi, thanks for providing all the necessary details regarding the issue you're facing. This is related to https://github.com/astral-sh/ruff/issues/10184.

What you're seeing is a preview style change specific to Python 3.12 (set by `target-version` in your config) which allows the formatter to re-use the same quote inside a f-string. The reason `--isolated` doesn't format it is because it ignores the config file which enables preview style.

The f-string formatting style is currently in preview and open to feedback: https://github.com/astral-sh/ruff/discussions/9785. We would appreciate if you can provide any feedback on the chosen style where the formatter can use the same quote if target version is 3.12 otherwise it would preserve the existing quote.

---

_Label `formatter` added by @MichaReiser on 2024-03-07 11:53_

---

_Label `preview` added by @MichaReiser on 2024-03-07 11:53_

---

_Comment by @MichaReiser on 2024-03-07 11:55_

@mvaled is there some tool that's telling you that the f-string is "broken" now? 

Note: This is the second time this has come up. We might want to consider flipping the quotes.

---

_Comment by @charliermarsh on 2024-03-07 14:02_

(Just tested myself -- PyCharm doesn't flag this as invalid, perhaps other editors do though.)

---

_Label `style` added by @dhruvmanila on 2024-03-07 14:14_

---

_Comment by @mvaled on 2024-03-07 16:11_

@charliermarsh 

You're right.  I was targeting using Python 3.12 which would allow this weird looking quotes inside `f"..."`.

Using `target-version = "py311"` corrects this.


---

_Closed by @mvaled on 2024-03-07 16:11_

---

_Referenced in [astral-sh/ruff#11056](../../astral-sh/ruff/issues/11056.md) on 2024-04-22 09:39_

---
