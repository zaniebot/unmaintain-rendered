```yaml
number: 20913
title: Ruff noqa feature and PGH004
type: issue
state: closed
author: Morgadoooo
labels:
  - question
assignees: []
created_at: 2025-10-16T08:44:32Z
updated_at: 2026-01-19T08:55:43Z
url: https://github.com/astral-sh/ruff/issues/20913
synced_at: 2026-01-19T09:26:50Z
```

# Ruff noqa feature and PGH004

---

_@Morgadoooo_

### Question

Hello,

Quick question, I was wondering if a behaviour is "intended" regarding the `# ruff: noqa` feature

Basically, let's say that i want to ignore a file.
I then do this at the top level of my file.
```python
# ruff: noqa
```
I would expect it to ignore the file, but i get a `ruff check` error with 
```bash
  PGH004 Use specific rule codes when using `ruff: noqa`
   --> /app.py:1:1
    |
  1 | # ruff: noqa
    | ^^^^^^^^^^^^
  2 | import logging
  3 | from collections import defaultdict
    |

  Found 1 error.
```

It seems odd to have to disable `PGH004` to be able to use this disabling feature.

I also know that, one can achieve this exclusion by providing the file in my ruff config but I didn't "wanted" to add it in the file.

Using ruff pre-commit hooks here
```
-   repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.14.0
    hooks:
    -   id: ruff-check
    -   id: ruff-format
```
and 
```
❯ uv self version
uv 0.9.3 (83635a6c4 2025-10-15)
```

### Version

0.14.0

---

_Label `question` added by @Morgadoooo on 2025-10-16 08:44_

---

_Comment by @MichaReiser on 2025-10-16 12:28_

I think the diagnostic message here is a bit confusing. For file level suppresesions, you always have to specify a rule code like this:

```
# ruff: noqa: PGH004
```

It doesn't support disabling all rules within a file. To ignore a file entirely, add it to `extend-exclude` in your ruff or pyproject.toml

---

_Comment by @flashcode on 2025-12-06 13:40_

I've hit the same issue when I use `--select ALL`, and I think it's a bug: the ruff doc says such line ignores **all** violations ([link](https://docs.astral.sh/ruff/linter/#error-suppression)):

```
To ignore all violations across an entire file, add the line # ruff: noqa anywhere in the file, preferably towards the top, like so:

# ruff: noqa
```

In my opinion, such line should really disable all errors, including PGH004.

PGH004 should only apply on such lines:

```python
pass  # noqa
```

Having to exclude the file with `extend-exclude` should not be needed.

Also using `--ignore PGH004` is not a solution either, because I want to keep this error on lines ending with `# noqa` (my example above).

---

_Comment by @MichaReiser on 2025-12-06 14:56_

You're right. `# ruff: noqa` works.

I don't think it makes sense to suppress `PGH004` with `ruff: noqa`. The point of `PGH004` is to find blanked ignores. 

You can do the following if you want to suppress `PGH004` in that file too:

```py
# ruff: noqa: PGH004
# ruff: noqa
```

Note that Ruff will still show syntax errors even when disabling all rules. The only way to make Ruff skip a file entirely is to exclude it.

---

_Closed by @MichaReiser on 2026-01-19 08:55_

---
