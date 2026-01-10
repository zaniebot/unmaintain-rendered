```yaml
number: 9008
title: Add ignore noqa support for doc lines
type: issue
state: closed
author: amrit110
labels:
  - question
assignees: []
created_at: 2023-12-05T15:27:15Z
updated_at: 2023-12-05T23:46:55Z
url: https://github.com/astral-sh/ruff/issues/9008
synced_at: 2026-01-10T11:09:51Z
```

# Add ignore noqa support for doc lines

---

_Issue opened by @amrit110 on 2023-12-05 15:27_

Hi, when I use ruff on docstring lines, and get errors such as `W505 Doc line too long`, adding a `# noqa: W505` at the end of the line doesn't seem to work. Could we please add support for that. I believe `flake8` supports it.


---

_Comment by @charliermarsh on 2023-12-05 17:52_

@amrit110 -- For errors within multi-line strings, we require that the `# noqa` be placed _after_ the string. Otherwise, it ends up being included within the content of the string itself, which is generally not intended.

For example:

```python
"""Some docstring.

Pretend this line is too long.
"""  # noqa: W505
```

---

_Label `question` added by @charliermarsh on 2023-12-05 17:52_

---

_Comment by @charliermarsh on 2023-12-05 17:53_

(There's a note on this in docs [here](https://docs.astral.sh/ruff/linter/#error-suppression).)

---

_Closed by @charliermarsh on 2023-12-05 17:53_

---

_Comment by @amrit110 on 2023-12-05 18:16_

thanks @charliermarsh !

---
