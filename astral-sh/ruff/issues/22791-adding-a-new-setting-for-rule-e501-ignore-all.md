```yaml
number: 22791
title: "Adding a new setting for rule E501 (`ignore-all-comments`)"
type: issue
state: open
author: chirizxc
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2026-01-21T19:28:40Z
updated_at: 2026-01-21T21:56:09Z
url: https://github.com/astral-sh/ruff/issues/22791
synced_at: 2026-01-21T22:07:06Z
```

# Adding a new setting for rule E501 (`ignore-all-comments`)

---

_@chirizxc_

### Summary

Example:

`main.py`:
```py
x = 1  # type: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
y = 2  # text text text text text
k = 3  # noqa
```
`ruff.toml`:
```toml
line-length = 5

[lint]
select = ["E501"]
task-tags = [
    "type",
]

[lint.pycodestyle]
ignore-overlong-task-comments = false
```
```bash
❯ ruff check main.py                                                                                                                           
E501 Line too long (33 > 5)                                                                                                                    
 --> main.py:2:6
  |
1 | x = 1  # type: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
2 | y = 2  # text text text text text
  |      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
3 | k = 3  # noqa
  |

Found 1 error.
```
I think it would be reasonable to make a non-default setting that can be enabled via
```toml
[lint.pycodestyle]
ignore-all-comments = true
```
That is, in this case, all of these cases would be reported, as in the original flake8.

I mean that with the settings we could completely disable https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_trivia/src/pragmas.rs

This question arose during a comparison of flake8 and ruff in a conversation about dishka on Telegram, screenshot example from @Tishka17: 

<img width="1096" height="705" alt="Image" src="https://github.com/user-attachments/assets/ddc19008-c330-4bd6-b10e-25823dcdd363" />

As you can see in the photo, the comments are not legible, i.e., they are simply not visible.

---

_Renamed from "Adding a new setting for rule E501" to "Adding a new setting for rule E501 (`ignore-all-comments`)" by @chirizxc on 2026-01-21 19:29_

---

_Comment by @amyreese on 2026-01-21 19:54_

why not just disable E501 and rely on a formatter to enforce line length?

---

_Comment by @chirizxc on 2026-01-21 19:56_

> why not just disable E501 and rely on a formatter to enforce line length?

The formatter is not entirely suitable, at least in that it violates PEP8.

See: https://github.com/astral-sh/ruff/issues/20454#issuecomment-3304157224

---

_Comment by @chirizxc on 2026-01-21 20:01_

For me, `black` (and its implementation within `ruff`) is a very strange formatter, at least the style that was used before literally bloated the code and made it less readable (in my opinion).

I can give you an example of the [old format](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-multiline-dictionary-and-list-indentation-for-sole-function-parameter) that black used to have:

<img width="954" height="768" alt="Image" src="https://github.com/user-attachments/assets/01f5c743-043c-496f-bb3d-7ebe162005c5" />

It is unclear why this was not done earlier, but remained unchanged for many years.

---

_Comment by @ntBre on 2026-01-21 21:48_

I guess I can see why you would want a mechanism for finding these lines, but there's often not much you can do to fix it, which I think is why these are skipped in the current implementation (https://github.com/astral-sh/ruff/pull/7692). For example, our `noqa` comments (and ty's) need to be at the end of the line and can't really be moved, unless you somehow restructure the code itself.

---

_Label `configuration` added by @ntBre on 2026-01-21 21:48_

---

_Label `needs-decision` added by @ntBre on 2026-01-21 21:48_

---

_Comment by @chirizxc on 2026-01-21 21:51_

> I guess I can see why you would want a mechanism for finding these lines, but there's often not much you can do to fix it, which I think is why these are skipped in the current implementation ([#7692](https://github.com/astral-sh/ruff/pull/7692)). For example, our `noqa` comments (and ty's) need to be at the end of the line and can't really be moved, unless you somehow restructure the code itself.

This setting will be optional, and most likely only those who need it will use it. This setting will not conflict with others:

<img width="1387" height="936" alt="Image" src="https://github.com/user-attachments/assets/8fde5cd5-01d2-436b-ab5d-befb5b08f1e4" />

---

_Comment by @chirizxc on 2026-01-21 21:54_

The value `line-length = 5` is just an example. In real projects, such as the screenshot above from dishka, you can shift the code slightly so that it fits on the screen and you don't have to scroll

<img width="1670" height="782" alt="Image" src="https://github.com/user-attachments/assets/1d20e585-4083-4fbc-99e7-0f6176495a33" />

vvv

<img width="961" height="457" alt="Image" src="https://github.com/user-attachments/assets/ffd67001-317e-4dec-b973-6f7070e004dd" />

---

_Comment by @chirizxc on 2026-01-21 21:56_

The same flake8 complains about this, we must take this setting into account for commands that have switched to ruff:

<img width="1202" height="233" alt="Image" src="https://github.com/user-attachments/assets/30d25538-ccf5-4576-9edd-7e304e78da93" />

---
