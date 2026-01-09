---
number: 21877
title: noqa for unused suppression reported as unused noqa
type: issue
state: open
author: amyreese
labels:
  - bug
  - suppression
assignees: []
created_at: 2025-12-09T20:29:08Z
updated_at: 2025-12-09T20:30:36Z
url: https://github.com/astral-sh/ruff/issues/21877
synced_at: 2026-01-07T13:12:16-06:00
---

# noqa for unused suppression reported as unused noqa

---

_Issue opened by @amyreese on 2025-12-09 20:29_

Using a `noqa` comment to suppress an unused range suppression will still report the *noqa comment* as unused:

```
$ cat foo.py
# ruff: disable[F401]  # noqa: RUF100
print('hello')
```
```
$ cargo run -p ruff -- check --no-cache --isolated --preview --select F,RUF100 foo.py
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff check --no-cache --isolated --preview --select F,RUF100 foo.py`
RUF100 [*] Unused suppression (unused: `F401`)
 --> foo.py:1:1
  |
1 | # ruff: disable[F401]  # noqa: RUF100
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
2 | print('hello')
  |
help: Remove unused suppression
  - # ruff: disable[F401]  # noqa: RUF100
1 | print('hello')

Found 1 error.
[*] 1 fixable with the `--fix` option.
> [1]
```

---

_Assigned to @amyreese by @amyreese on 2025-12-09 20:29_

---

_Label `rule` added by @amyreese on 2025-12-09 20:29_

---

_Label `bug` added by @amyreese on 2025-12-09 20:30_

---

_Label `suppression` added by @amyreese on 2025-12-09 20:30_

---

_Label `rule` removed by @amyreese on 2025-12-09 20:30_

---
