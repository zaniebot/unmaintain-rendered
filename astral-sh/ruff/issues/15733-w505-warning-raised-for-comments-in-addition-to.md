```yaml
number: 15733
title: "W505: Warning raised for comments in addition to docstrings"
type: issue
state: closed
author: martimlobao
labels:
  - question
assignees: []
created_at: 2025-01-24T20:57:36Z
updated_at: 2025-01-25T15:49:05Z
url: https://github.com/astral-sh/ruff/issues/15733
synced_at: 2026-01-12T15:54:54Z
```

# W505: Warning raised for comments in addition to docstrings

---

_@martimlobao_

### Description

[W505](https://docs.astral.sh/ruff/rules/doc-line-too-long/) (doc line too long) is raised for docstrings that are too long, but it is also raised for comments (and commented-out code). Comments, and commented-out code in particular, are not docs and I feel they should be treated differently.

```toml
# ruff.toml
line-length = 119

[lint]
select = ["ALL"]

[lint.pycodestyle]
max-doc-length = 80
```

```python
# example.py
class Foo:
    """A really long docstring that goes on and on and should be split into multiple lines."""

    def __init__(self):
        pass
        # print("This is some code that is less than 119 characters and that we commented out.")
```

```bash
$ ruff check
example.py:2:81: W505 Doc line too long (94 > 80)
  |
1 | class Foo:
2 |     """A really long docstring that goes on and on and should be split into multiple lines."""
  |                                                                                 ^^^^^^^^^^^^^^ W505
3 |
4 |     def __init__(self):
  |

example.py:6:81: W505 Doc line too long (100 > 80)
  |
4 |     def __init__(self):
5 |         pass
6 |         # print("This is some code that is less than 119 characters and that we commented out.")
  |                                                                                 ^^^^^^^^^^^^^^^^ W505
  |
```

```bash
$ ruff --version
ruff 0.9.3
```

---

_Comment by @ntBre on 2025-01-24 21:37_

It looks like we inherited this behavior from pycodestyle, which also applies it to comments:

https://github.com/PyCQA/pycodestyle/blob/368d9dc21496e4cbb26a00b9e167deed39865781/pycodestyle.py#L1664-L1665

So I'm not sure we'd really want to depart from that.

It's a bit hacky, but as the [docs](https://docs.astral.sh/ruff/rules/doc-line-too-long/#why-is-this-bad) suggest, you can avoid W505 on lines that match one of your [lint.task-tags](https://docs.astral.sh/ruff/settings/#lint_task-tags) if you combine it with [lint.pycodestyle.ignore-overlong-task-comments](https://docs.astral.sh/ruff/settings/#lint_pycodestyle_ignore-overlong-task-comments):

```shell
ruff check --select W505 \
    --config 'lint.pycodestyle.max-doc-length = 80' \
    --config 'lint.pycodestyle.ignore-overlong-task-comments = true' \
    --config 'lint.task-tags = [""]' example.py
```

The empty `task-tag` should match any comment.

This reports the error for the docstring but not for the comment.

---

_Label `question` added by @MichaReiser on 2025-01-25 10:50_

---

_Comment by @martimlobao on 2025-01-25 15:49_

@ntBre Thanks for the reply! That's interesting, I'm fairly sure I didn't used to get these warnings for comments in the past before switching to `ruff`, but maybe I wasn't using `pycodestyle`. In that case, it looks like this is behaving as intended (i.e. it matches `pycodestyle`'s behavior), and with your suggested workaround I'm sufficiently satisfied that I think I can close this issue.

---

_Closed by @martimlobao on 2025-01-25 15:49_

---
