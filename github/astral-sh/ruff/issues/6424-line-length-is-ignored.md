---
number: 6424
title: Line length is ignored.
type: issue
state: closed
author: Nuno-Mota
labels:
  - documentation
assignees: []
created_at: 2023-08-08T10:50:46Z
updated_at: 2024-04-26T15:38:27Z
url: https://github.com/astral-sh/ruff/issues/6424
synced_at: 2026-01-07T13:12:15-06:00
---

# Line length is ignored.

---

_Issue opened by @Nuno-Mota on 2023-08-08 10:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hi.

I'm not sure if this is intended behaviour, but it seems that the line length of in-line comments that do not have a space after `#` is ignored.
This was tested for `ruff 0.0.282`.

For example:

pyproject.toml
```toml
[tool.ruff]
line-length = 4
show-source = true
```

Test file
```python
#aaa
#aaaa
# aa
# aaa
```

ruff's output
```zsh
ruff check test.py 
test.py:4:5: E501 Line too long (5 > 4 characters)
  |
2 | #aaaa
3 | # aa
4 | # aaa
  |     ^ E501
  |

Found 1 error.
✗ - status code 1
```

---

_Comment by @Nuno-Mota on 2023-08-08 12:31_

While looking into another issue, I realised that the issue might actually simply be related to the lack of a space in the line.
For example, modifying the above test file to this one:

```python
#aaa
#aaaa
# aa
# aaa
print('no-spaces')
```

Gives the same output as before (barring the new line):

ruff's output
```zsh
ruff check test.py 
test.py:4:5: E501 Line too long (5 > 4 characters)
  |
2 | #aaaa
3 | # aa
4 | # aaa
  |     ^ E501
5 | print('no-spaces')
  |

Found 1 error.
✗ - status code 1
```

---

_Comment by @charliermarsh on 2023-08-08 12:34_

Yeah that’s intentional behavior - we don’t flag overlong lines that consist of a single word with no breakable whitespace. This is generally good since it avoids false positives for URLs and other words that can’t be split across multiple lines without hurting readability.

---

_Referenced in [astral-sh/ruff#6425](../../astral-sh/ruff/issues/6425.md) on 2023-08-08 12:38_

---

_Comment by @Nuno-Mota on 2023-08-08 13:50_

Ok, that's good to know.
Can this be added to the documentation somewhere? Potentially in the `line-length` setting?

In any case, I think the rule makes sense most of the times.
My only concern is potentially for tab-indented lines that are long, but breakable without hurting readability.
Such cases would fail silently, while the error raised for the cases you mentioned can be suppressed.

However, realistically speaking, I believe such tab-indented cases would be rare (I can't think of any atm), so it might be more beneficial maintaining the current behaviour (and document it somewhere, imo).

---

_Label `documentation` added by @charliermarsh on 2023-08-08 18:45_

---

_Comment by @charliermarsh on 2023-08-08 18:46_

I just checked, it looks like we _do_ mention this in the documentation for the rule: https://beta.ruff.rs/docs/rules/line-too-long/. Does that look sufficient?

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-08 18:46_

---

_Comment by @Nuno-Mota on 2023-08-08 21:18_

Ah, it looks like I completely missed it. My apologies.
Maybe it's worth a mention of exceptions (with a reference to that explanation) [here](https://beta.ruff.rs/docs/settings/#line-length)? Just a suggestion, though.

In any case, thank you for looking into it!

---

_Comment by @charliermarsh on 2023-08-08 21:20_

No worries, thanks for reporting these!

---

_Closed by @charliermarsh on 2023-08-08 21:20_

---

_Comment by @asmaier on 2024-04-26 15:32_

I believe you should mention in the documentation at https://docs.astral.sh/ruff/settings/#line-length that the rule E501 is disabled by default. That means to enforce a `line-length` one has to configure in `pyproject.toml`:

```
[tool.ruff]
line-length=120
[tool.ruff.lint]
extend-select = ["E501"]
```

Without the last line `ruff check` will not find any line-length violations. 

---

_Referenced in [maxscheurer/responsefun#10](../../maxscheurer/responsefun/pulls/10.md) on 2025-03-12 14:49_

---
