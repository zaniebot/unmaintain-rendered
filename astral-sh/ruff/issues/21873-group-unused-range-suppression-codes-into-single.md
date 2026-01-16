```yaml
number: 21873
title: Group unused range suppression codes into single diagnostic
type: issue
state: closed
author: amyreese
labels:
  - bug
  - rule
  - suppression
assignees: []
created_at: 2025-12-09T20:22:03Z
updated_at: 2026-01-16T19:08:50Z
url: https://github.com/astral-sh/ruff/issues/21873
synced_at: 2026-01-16T20:03:31Z
```

# Group unused range suppression codes into single diagnostic

---

_@amyreese_

### Summary

If a range suppression has more than one unused code, ruff will produce a separate diagnostic for each unused code, rather than grouping them into a single diagnostic like will happen for unused noqa:

```
$ cat ~/scratch/ruff/foo.py
# ruff: disable[F401, F501]
print("hello")  # noqa: F401 F501
```

```
$ cargo run -p ruff -- check --no-cache --isolated --preview --select F,RUF100 ~/scratch/ruff/foo.py
RUF100 [*] Unused suppression (unused: `F401`)
 --> /Users/amethyst/scratch/ruff/foo.py:1:17
  |
1 | # ruff: disable[F401, F501]
  |                 ^^^^
2 | print("hello")  # noqa: F401 F501
  |
help: Remove unused suppression
  - # ruff: disable[F401, F501]
1 + # ruff: disable[F501]
2 | print("hello")  # noqa: F401 F501
3 |

RUF100 [*] Unused suppression (unused: `F501`)
 --> /Users/amethyst/scratch/ruff/foo.py:1:23
  |
1 | # ruff: disable[F401, F501]
  |                       ^^^^
2 | print("hello")  # noqa: F401 F501
  |
help: Remove unused suppression
  - # ruff: disable[F401, F501]
1 + # ruff: disable[F401]
2 | print("hello")  # noqa: F401 F501
3 |

RUF100 [*] Unused `noqa` directive (unused: `F401`, `F501`)
 --> /Users/amethyst/scratch/ruff/foo.py:2:17
  |
1 | # ruff: disable[F401, F501]
2 | print("hello")  # noqa: F401 F501
  |                 ^^^^^^^^^^^^^^^^^
  |
help: Remove unused `noqa` directive
1 | # ruff: disable[F401, F501]
  - print("hello")  # noqa: F401 F501
2 + print("hello")
3 |

Found 3 errors.
[*] 3 fixable with the `--fix` option.
> [1]
```

### Version

_No response_

---

_Assigned to @amyreese by @amyreese on 2025-12-09 20:22_

---

_Label `bug` added by @amyreese on 2025-12-09 20:22_

---

_Label `rule` added by @amyreese on 2025-12-09 20:22_

---

_Label `suppression` added by @amyreese on 2025-12-09 20:29_

---

_Comment by @amyreese on 2025-12-09 20:34_

Consider how ty does this: https://github.com/astral-sh/ruff/blob/adf468beb53bfc0a06698d4e4f265a02545f37a1/crates/ty_python_semantic/src/suppression.rs#L260-L289

---

_Comment by @MichaReiser on 2026-01-10 10:03_

@amyreese are you planning to come back to this?

---

_Closed by @amyreese on 2026-01-16 19:08_

---
