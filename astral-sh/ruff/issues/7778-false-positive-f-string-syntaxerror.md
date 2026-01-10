```yaml
number: 7778
title: False positive f-string SyntaxError
type: issue
state: closed
author: youknowone
labels:
  - bug
  - parser
assignees: []
created_at: 2023-10-03T13:21:45Z
updated_at: 2023-10-03T14:08:04Z
url: https://github.com/astral-sh/ruff/issues/7778
synced_at: 2026-01-10T11:09:50Z
```

# False positive f-string SyntaxError

---

_Issue opened by @youknowone on 2023-10-03 13:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```python
# x.py
f"{1:{{1}.pop()}{f'n'}}"
```

```sh
$ ruff x.py
error: Failed to parse x.py:1:16: f-string: single '}' is not allowed
x.py:1:16: E999 SyntaxError: f-string: single '}' is not allowed
Found 1 error.
$ ruff --version
ruff 0.0.292
```


---

_Comment by @dhruvmanila on 2023-10-03 13:33_

Thanks for the bug report! It seems to be caused by the two `{{` inside the format spec part. It seems that the rules for escaping `{`/`}` are different in and out of a format spec.

---

_Label `bug` added by @dhruvmanila on 2023-10-03 13:34_

---

_Label `parser` added by @dhruvmanila on 2023-10-03 13:34_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-10-03 13:34_

---

_Closed by @dhruvmanila on 2023-10-03 14:08_

---
