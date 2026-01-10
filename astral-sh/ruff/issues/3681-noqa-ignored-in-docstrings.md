```yaml
number: 3681
title: "# noqa ignored in docstrings"
type: issue
state: closed
author: staticf0x
labels:
  - suppression
assignees: []
created_at: 2023-03-23T09:20:27Z
updated_at: 2023-03-23T13:59:15Z
url: https://github.com/astral-sh/ruff/issues/3681
synced_at: 2026-01-10T11:09:46Z
```

# # noqa ignored in docstrings

---

_Issue opened by @staticf0x on 2023-03-23 09:20_

The `# noqa` seems to be ignored in docstrings, e.g.:

Let's say I have some long string:

```py
"""
URL = "https://github.com/pulp/pulp_docker/blob/master/pulp_docker/app/tasks/recursive_remove.py"
"""

URL = "https://github.com/pulp/pulp_docker/blob/master/pulp_docker/app/tasks/recursive_remove.py"
```

Run `ruff test.py` and I get this:

```
$ ruff test.py
test.py:2:89: E501 Line too long (97 > 88 characters)
test.py:5:89: E501 Line too long (97 > 88 characters)
Found 2 errors.
```

which is correct, but if I add `# noqa` to both lines:

```py
"""
URL = "https://github.com/pulp/pulp_docker/blob/master/pulp_docker/app/tasks/recursive_remove.py"  # noqa
"""

URL = "https://github.com/pulp/pulp_docker/blob/master/pulp_docker/app/tasks/recursive_remove.py"  # noqa
```

And again run `ruff test.py`, I get this:

```
$ ruff test.py
test.py:2:89: E501 Line too long (105 > 88 characters)
Found 1 error.
```

So now the actual code line is ignored as expected, but the one in docstring is not.

For comparison, here's `flake8 test.py`

Without `# noqa`:

```
$ flake8 test.py
test.py:2:89: E501 line too long (97 > 88 characters)
test.py:5:89: E501 line too long (97 > 88 characters)
```

With `# noqa` (on both lines):

```
$ flake8 test.py
```

no errors.

---

_Renamed from "# noqa causes error in docstrings" to "# noqa ignored in docstrings" by @staticf0x on 2023-03-23 09:20_

---

_Comment by @JonathanPlasse on 2023-03-23 09:45_

I guess you do not want a `noqa` in the text of your docstring.
You need to put a `noqa` after the docstring like below.
```python
"""
URL = "https://github.com/pulp/pulp_docker/blob/master/pulp_docker/app/tasks/recursive_remove.py"
"""  # noqa: E501

URL = "https://github.com/pulp/pulp_docker/blob/master/pulp_docker/app/tasks/recursive_remove.py"  # noqa: E501
```

---

_Comment by @onerandomusername on 2023-03-23 10:27_

For what it's worth, this IS different behavior from flake8, but it's arguably much better than what flake8 requires. 

---

_Comment by @staticf0x on 2023-03-23 10:30_

Thanks @JonathanPlasse that makes sense

---

_Comment by @charliermarsh on 2023-03-23 13:59_

Ah yeah, this is by design. Obligatory link to the docs for future readers: https://beta.ruff.rs/docs/configuration/#error-suppression

---

_Closed by @charliermarsh on 2023-03-23 13:59_

---

_Label `noqa` added by @charliermarsh on 2023-03-23 13:59_

---
