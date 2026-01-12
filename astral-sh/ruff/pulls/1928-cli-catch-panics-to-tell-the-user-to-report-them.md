```yaml
number: 1928
title: "cli: Catch panics to tell the user to report them"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: please-report-panics
created_at: 2023-01-17T03:41:15Z
updated_at: 2023-01-17T22:17:10Z
url: https://github.com/astral-sh/ruff/pull/1928
synced_at: 2026-01-12T05:25:13Z
```

# cli: Catch panics to tell the user to report them

---

_Pull request opened by @not-my-profile on 2023-01-17 03:41_

_No description provided._

---

_Comment by @charliermarsh on 2023-01-17 12:34_

Were you running into a panic somewhere specific?

---

_Comment by @not-my-profile on 2023-01-17 12:44_

No this is just a precaution seeing issues like #1845.

Users might not know what to do when they ruff panics, so I think it makes sense to ask them to report panics to us when they occur. Since I think we should treat any panic that occurs during normal operation as a bug.

---

_Review comment by @messense on `ruff_cli/src/main.rs`:95 on 2023-01-17 13:45_

I think you meant to use [`std::panic::set_hook`](https://doc.rust-lang.org/std/panic/fn.set_hook.html)? Maybe also tell user to report the backtrace by re-run with `RUST_BACKTRACE=1`.

---

_@messense reviewed on 2023-01-17 13:45_

---

_@not-my-profile reviewed on 2023-01-17 13:56_

---

_Review comment by @not-my-profile on `ruff_cli/src/main.rs`:95 on 2023-01-17 13:56_

I meant to use `catch_unwind` because it does not replace the default panic output it just gets invoked before it so the regular output telling the user to re-run with RUST_BACKTRACE=1 is still printed.

---

_Review comment by @messense on `ruff_cli/src/main.rs`:95 on 2023-01-17 14:00_

But this currently always print the message?

```
cargo run -- --no-cache scripts/add_plugin.py
    Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff --no-cache scripts/add_plugin.py`
ruff crashed ... this should not be happening :/
Please report it on GitHub: https://github.com/charliermarsh/ruff/issues/new?title=Panic+when+%2E%2E%2E
scripts/add_plugin.py:105:89: E501 Line too long (111 > 88 characters)
Found 1 error(s).
```

---

_@messense reviewed on 2023-01-17 14:00_

---

_Review comment by @not-my-profile on `ruff_cli/src/main.rs`:95 on 2023-01-17 17:21_

Ah yes, thanks! Good catch :) I have updated it to use `set_hook` as you suggested and now it only prints when it actually panics. I am still invoking the default hook for the RUST_BACKTRACE=1 message.

---

_@not-my-profile reviewed on 2023-01-17 17:21_

---

_Merged by @charliermarsh on 2023-01-17 22:17_

---

_Closed by @charliermarsh on 2023-01-17 22:17_

---
