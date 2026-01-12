```yaml
number: 5004
title: f-string position is miscomputed in multiple cases
type: issue
state: closed
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-10T12:28:26Z
updated_at: 2024-02-27T17:39:05Z
url: https://github.com/astral-sh/ruff/issues/5004
synced_at: 2026-01-12T15:54:45Z
```

# f-string position is miscomputed in multiple cases

---

_@addisoncrump_

The position of errors is miscomputed in f-strings:

```
$ ruff --no-cache test2.py --show-source
error: Failed to parse test2.py:1:5: f-string: single '}' is not allowed
test2.py:1:5: E999 SyntaxError: f-string: single '}' is not allowed
  |
1 | f' }'
  |     ^ E999
  |

Found 1 error.
```

Moreover, it is not sound in the presence of carriage return + newline:

```
$ $ cat -e test3.py 
f'''^M$
^M$
^M$
^M$
^M$
 }'''^M$
$ ruff test3.py --fix --show-source --no-cache
error: Failed to parse test3.py:4:2: f-string: single '}' is not allowed
test3.py:4:2: E999 SyntaxError: f-string: single '}' is not allowed
  |
4 |   
5 | | 
  | |_^ E999
6 |    }'''
  |

Found 1 error.
```

In rare cases, this may lead to a panic if multi-byte UTF-8 characters are present (carriage return + newline followed by UTF-8 char before mismatched `}`):

```
$ ruff --no-cache minimized-from-8e22bc8c0f8f652f62a9a858e232b0bbf18e702b
warning: Linting panicked minimized-from-8e22bc8c0f8f652f62a9a858e232b0bbf18e702b: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'byte index 10 is not a char boundary; it is inside '〈' (bytes 8..11) of `f'''

〈}'''
`'
```

Locator::after slices a source code snippet at the byte level. This may lead to an invalid UTF-8 slice if the index is in the middle of a code point, which occurs during f-string computation. This occurs in multiple f-string error types.

I am still searching for a root cause.

---

_Renamed from "f-string single right brace position is miscomputed" to "f-string position is miscomputed" by @addisoncrump on 2023-06-10 12:42_

---

_Renamed from "f-string position is miscomputed" to "f-string position is miscomputed in multiple cases" by @addisoncrump on 2023-06-10 12:49_

---

_Label `bug` added by @dhruvmanila on 2023-06-10 14:40_

---

_Comment by @charliermarsh on 2024-02-25 22:09_

Any idea if this is still true?

---

_Comment by @MichaReiser on 2024-02-26 07:22_

I believe this is fixed. At least the offsets in the playground look correct https://play.ruff.rs/957af831-fd05-416e-8cff-e23cab5cca2a

---

_Comment by @addisoncrump on 2024-02-27 17:39_

I don't have an example testcase presently, but if I come across one I'll reopen this.

---

_Closed by @addisoncrump on 2024-02-27 17:39_

---
