```yaml
number: 6425
title: Unexpected line length check behaviour when line has tabs.
type: issue
state: closed
author: Nuno-Mota
labels:
  - bug
assignees: []
created_at: 2023-08-08T12:38:57Z
updated_at: 2023-08-11T02:28:26Z
url: https://github.com/astral-sh/ruff/issues/6425
synced_at: 2026-01-10T11:09:48Z
```

# Unexpected line length check behaviour when line has tabs.

---

_Issue opened by @Nuno-Mota on 2023-08-08 12:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi.

It seems like the counting of line length with tabs has an unexpected behaviour.

From what I can infer, the length of each line is first counted considering that tabs have a size of 1.
If the resulting length is equal to or greater than the `line-length` setting, then the size is recomputed taking into account the `tab-size` setting.
Furthermore, the line needs to have at least 1 space (as discussed in #6424).

Is this the intended behaviour? If so, is there some place that explains the reasoning behind it?

This was tested for `ruff 0.0.282`.

MWE:

pyproject.toml
```toml
[tool.ruff]
line-length = 6
tab-size = 4
show-source = true
```

test file (note that github uses a default tab size of 8)
```python
# aaaa
# aaaaa
    # a
	# a
	# aa
	# aaa
	# aaaa
		# a
		# aa
		# aaa

if True: # noqa: E501
	[12]
	[12 ]
	[1,2]
	[1, 2]
```

ruff's output
```zsh
ruff test3.py 
test3.py:2:7: E501 Line too long (7 > 6 characters)
  |
1 | # aaaa
2 | # aaaaa
  |       ^ E501
3 |     # a
4 |     # a
  |

test3.py:3:7: E501 Line too long (7 > 6 characters)
  |
1 | # aaaa
2 | # aaaaa
3 |     # a
  |       ^ E501
4 |     # a
5 |     # aa
  |

test3.py:6:4: E501 Line too long (9 > 6 characters)
  |
4 |     # a
5 |     # aa
6 |     # aaa
  |       ^^^ E501
7 |     # aaaa
8 |         # a
  |

test3.py:7:4: E501 Line too long (10 > 6 characters)
  |
5 |     # aa
6 |     # aaa
7 |     # aaaa
  |       ^^^^ E501
8 |         # a
9 |         # aa
  |

test3.py:9:3: E501 Line too long (12 > 6 characters)
   |
 7 |     # aaaa
 8 |         # a
 9 |         # aa
   |         ^^^^ E501
10 |         # aaa
   |

test3.py:10:3: E501 Line too long (13 > 6 characters)
   |
 8 |         # a
 9 |         # aa
10 |         # aaa
   |         ^^^^^ E501
11 | 
12 | if True: # noqa: E501
   |

test3.py:14:4: E501 Line too long (9 > 6 characters)
   |
12 | if True: # noqa: E501
13 |     [12]
14 |     [12 ]
   |       ^^^ E501
15 |     [1,2]
16 |     [1, 2]
   |

test3.py:16:4: E501 Line too long (10 > 6 characters)
   |
14 |     [12 ]
15 |     [1,2]
16 |     [1, 2]
   |       ^^^^ E501
   |

Found 8 errors.
✗ - status code 1
```


---

_Comment by @charliermarsh on 2023-08-08 12:55_

Do you mind clarifying what behavior here is unexpected? "...then the size is recomputed taking into account..." sounds like an implementation detail as-described, and not a behavior per se.

---

_Comment by @Nuno-Mota on 2023-08-08 13:41_

> Do you mind clarifying what behavior here is unexpected?

Of course.

The issue is that tab-indented lines that are longer than the `line-length` setting (taking into consideration the `tab-size` setting), might not be flagged for `E501`.

> "...then the size is recomputed taking into account..." sounds like an implementation detail as-described, and not a behavior per se.

That might be. This is simply the behaviour I was able to infer.
In any case it seems to lead to an erroneous line length count in certain cases.

---

_Comment by @charliermarsh on 2023-08-08 18:58_

Ah, I think that's a result of the performance optimization I proposed in https://github.com/astral-sh/ruff/pull/5811/files, which doesn't hold true for tabs (for which a single-byte character can be wider than one byte).

---

_Label `bug` added by @charliermarsh on 2023-08-08 19:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-11 01:43_

---

_Closed by @charliermarsh on 2023-08-11 02:28_

---
