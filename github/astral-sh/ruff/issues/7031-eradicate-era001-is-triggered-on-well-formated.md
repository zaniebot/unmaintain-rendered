---
number: 7031
title: Eradicate (ERA001) is triggered on well formated TODO
type: issue
state: closed
author: Cjkjvfnby
labels:
  - bug
  - help wanted
assignees: []
created_at: 2023-08-31T18:13:14Z
updated_at: 2023-09-28T23:13:13Z
url: https://github.com/astral-sh/ruff/issues/7031
synced_at: 2026-01-07T13:12:15-06:00
---

# Eradicate (ERA001) is triggered on well formated TODO

---

_Issue opened by @Cjkjvfnby on 2023-08-31 18:13_

`ruff 0.0.286`

When my TODO is bad (according to the TD rules), it's not reported as unused code
But when I fix TD warnings the code becomes a subject of ERA

Broken TODO
```python
def foo():
    # TODO: implement
    pass
```

```shell
ruff "foo.py"  --select TD002,ERA
TD002 Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
```

When I fix this warning
```python
def foo():
    # TODO(CJ): implement
    pass

```

```shell
ruff foo.py --select TD002,ERA
ERA001 [*] Found commented-out code
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

I expect that the TODO comment should not be marked as unused code.  And TD requirements were aligned with other rules.

---

_Label `bug` added by @zanieb on 2023-08-31 18:16_

---

_Comment by @zanieb on 2023-08-31 18:17_

I think it makes sense to add an exception to ERA001 if feasible — the pattern used for matching violations is brittle and has a lot of false positives.

---

_Label `help wanted` added by @zanieb on 2023-08-31 18:17_

---

_Comment by @charliermarsh on 2023-08-31 18:18_

I'm not sure why it triggers in one case but not the other, but yeah I definitely would've expected TODO to be in the allowlist already. Oops!

---

_Referenced in [astral-sh/ruff#7523](../../astral-sh/ruff/pulls/7523.md) on 2023-09-19 15:50_

---

_Closed by @charliermarsh on 2023-09-28 23:13_

---
