```yaml
number: 7488
title: "`DTZ007` false-positive when using time format from const"
type: issue
state: closed
author: aw1cks
labels: []
assignees: []
created_at: 2023-09-18T10:00:13Z
updated_at: 2023-09-18T13:10:17Z
url: https://github.com/astral-sh/ruff/issues/7488
synced_at: 2026-01-12T15:54:47Z
```

# `DTZ007` false-positive when using time format from const

---

_@aw1cks_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi there, first of all just wanted to say thanks for this awesome project.

I noticed a bug with the `DTZ007` rule - it seems like it's not reading my const value and falsely telling me that I'm not using `%z`.
### Configuration

```shell
$ ruff --version
ruff 0.0.290
$ cat pyproject.toml 
[tool.ruff]
select = [
  "DTZ",
]
$ 
```

### Code example

This works fine:

```python
import datetime

my_date = "Thu, 14 Sep 2023 07:45:07 +0000"
mydt = datetime.datetime.strptime(my_date, "%a, %d %b %Y %H:%M:%S %z")
```

```shell
$ ruff .
$
```

However, when using a const:

```python
import datetime

MY_DATE_FORMAT = "%a, %d %b %Y %H:%M:%S %z"

my_date = "Thu, 14 Sep 2023 07:45:07 +0000"
mydt = datetime.datetime.strptime(my_date, MY_DATE_FORMAT)
```

```shell
$ ruff .
test.py:6:8: DTZ007 The use of `datetime.datetime.strptime()` without %z must be followed by `.replace(tzinfo=)` or `.astimezone()`
Found 1 error.
$
```



---

_Renamed from "DTZ007 false-positive when using time format from const" to "`DTZ007` false-positive when using time format from const" by @aw1cks on 2023-09-18 10:01_

---

_Comment by @aw1cks on 2023-09-18 10:14_

Whoops - I missed https://github.com/astral-sh/ruff/issues/1306 - closing

---

_Closed by @aw1cks on 2023-09-18 10:14_

---

_Comment by @charliermarsh on 2023-09-18 13:10_

Thanks for the kind words, the thorough issue, and the follow-up!

---
