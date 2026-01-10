```yaml
number: 12320
title: "Possible False Positive: too-many-arguments (PLR0913) considers `self` and `cls`"
type: issue
state: closed
author: Bibo-Joshi
labels:
  - rule
assignees: []
created_at: 2024-07-14T17:31:04Z
updated_at: 2024-07-17T19:36:26Z
url: https://github.com/astral-sh/ruff/issues/12320
synced_at: 2026-01-10T11:09:54Z
```

# Possible False Positive: too-many-arguments (PLR0913) considers `self` and `cls`

---

_Issue opened by @Bibo-Joshi on 2024-07-14 17:31_

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.

  * "PRL0913"
  * "too-many-arguments"
  * "self"
  * "cls"
  * "false positve"

* A minimal code snippet that reproduces the bug.

    ```python
    class DemoClass:
        def __init__(self, one, two, three, four, five):
            pass

        @classmethod
        def bar(cls, one, two, three, four, five):
            pass

        @staticmethod
        def foo(one, two, three, four, five):
            pass
    ```

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

    ```shell
    ruff check foo.py --isolated --select PLR0913
    ```
    This produles the following output:

    ```shell
    (.venv) ~\PycharmProjects\test git:[dev]
    ruff check foo.py --isolated --select PLR0913
    foo.py:2:9: PLR0913 Too many arguments in function definition (6 > 5)
      |
    1 | class DemoClass:
    2 |     def __init__(self, one, two, three, four, five):
      |         ^^^^^^^^ PLR0913
    3 |         pass
      |

    foo.py:6:9: PLR0913 Too many arguments in function definition (6 > 5)
      |
    5 |     @classmethod
    6 |     def bar(cls, one, two, three, four, five):
      |         ^^^ PLR0913
    7 |         pass
      |

    Found 2 errors.
    ```

    Apparently, ruff considers `self` and `cls` when computing the number of arguments. However, these arguments are mandatory anyway, are not visible to the consumer of these methods and don't introduce any maintenance overhead. I propose that ruff ignores them.
 
* ~The current Ruff settings (any relevant sections from your `pyproject.toml`).~ used `--isolated` flag above
* The current Ruff version (`ruff --version`): `ruff 0.5.1`


---

_Comment by @MichaReiser on 2024-07-15 06:28_

I'm not arguing whether the behavior is correct. But what I understand from Pylint's source code is that it also counts `self` and `cls`

https://github.com/pylint-dev/pylint/blob/a48cd4c6a872b6565bc58030b74585812f327f36/pylint/checkers/design_analysis.py#L536-L553

One important difference is that pylint [supports a configuration](https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#ignored-argument-names) (Regex) to ignore arguments. 

---

_Label `rule` added by @MichaReiser on 2024-07-15 06:28_

---

_Comment by @dhruvmanila on 2024-07-15 07:56_

FWIW, Pylint considers this as false-positive in https://github.com/pylint-dev/pylint/issues/8675 and I think that makes sense. The implementation should also consider decorators like `@staticmethod` and `@classmethod` when determining whether to count `self` / `cls`.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 14:21_

---

_Closed by @charliermarsh on 2024-07-17 14:49_

---

_Closed by @charliermarsh on 2024-07-17 14:49_

---

_Comment by @Bibo-Joshi on 2024-07-17 19:36_

That was lightning fast. Awesome, thanks :)

---
