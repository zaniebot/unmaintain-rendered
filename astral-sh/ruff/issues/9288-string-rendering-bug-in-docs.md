```yaml
number: 9288
title: String-rendering bug in docs
type: issue
state: closed
author: diceroll123
labels:
  - bug
  - documentation
assignees: []
created_at: 2023-12-26T21:16:14Z
updated_at: 2023-12-28T14:44:52Z
url: https://github.com/astral-sh/ruff/issues/9288
synced_at: 2026-01-12T15:54:49Z
```

# String-rendering bug in docs

---

_@diceroll123_

It seems that the docs message for `FURB118` is cut off.

<img width="1646" alt="image" src="https://github.com/astral-sh/ruff/assets/1243957/88d8b970-839f-4da5-b82b-793676075f44">

But the format string in the code is 
```rust
format!("Use `operator.{operator}` instead of defining a {target}")
```

---

_Label `documentation` added by @charliermarsh on 2023-12-26 21:56_

---

_Label `bug` added by @charliermarsh on 2023-12-28 13:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-28 13:14_

---

_Comment by @charliermarsh on 2023-12-28 13:17_

Something happening in the Markdown-to-HTML conversion, maybe some lack of escaping?

---

_Comment by @charliermarsh on 2023-12-28 13:17_

The Markdown looks right, but un-backticked format placeholders are missing upon conversion to HTML.

---

_Comment by @charliermarsh on 2023-12-28 14:14_

It's probably the `attr_list` extension, the last `{}` placeholder is being treated as an HTML attribute.

---

_Closed by @charliermarsh on 2023-12-28 14:44_

---
