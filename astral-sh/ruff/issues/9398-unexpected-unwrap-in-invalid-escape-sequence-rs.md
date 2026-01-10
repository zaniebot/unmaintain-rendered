```yaml
number: 9398
title: "Unexpected unwrap in `invalid_escape_sequence.rs`"
type: issue
state: closed
author: th3w1zard1
labels:
  - bug
assignees: []
created_at: 2024-01-05T07:17:36Z
updated_at: 2024-01-27T16:54:06Z
url: https://github.com/astral-sh/ruff/issues/9398
synced_at: 2026-01-10T11:09:51Z
```

# Unexpected unwrap in `invalid_escape_sequence.rs`

---

_Issue opened by @th3w1zard1 on 2024-01-05 07:17_

Been getting these errors in vs code as of late:
```
thread 'main' panicked at crates\ruff_linter\src\rules\pycodestyle\rules\invalid_escape_sequence.rs:215:18: called `Option::unwrap()` on a `None` value note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace )
```
This error message asked me to post here.

Would like to point out this usually happens while I'm typing code, so obviously there are going to be syntax errors but I'm not sure why Ruff is trying to parse them. If I need to post this on a different repo please let me know.


---

_Comment by @charliermarsh on 2024-01-05 12:24_

Thank you. Do you happen to know what version of Ruff you're using? Or of the VS Code extension?

---

_Label `bug` added by @charliermarsh on 2024-01-05 12:24_

---

_Renamed from "[Panic]" to "Unexpected unwrap in `invalid_escape_sequence.rs`" by @charliermarsh on 2024-01-05 12:28_

---

_Comment by @th3w1zard1 on 2024-01-08 06:20_

I update all my extensions and be code daily but I can check this next Im at my pc. Thanks for the quick response. Am running windows 10

---

_Comment by @th3w1zard1 on 2024-01-08 06:21_

Usually happens when I'm actively typing f strings, or when I'm backspacing some of statements

---

_Comment by @charliermarsh on 2024-01-16 03:42_

It sounds like this was resolved in newer versions of Ruff, based on this similar issue: https://github.com/astral-sh/ruff/issues/9542.

---

_Closed by @charliermarsh on 2024-01-16 03:42_

---

_Comment by @th3w1zard1 on 2024-01-27 16:53_

Can confirm I haven't had it happen in two weeks. Thanks again!

---
