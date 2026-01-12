```yaml
number: 10493
title: Tests hang (waiting for username) in 0.5.17
type: issue
state: closed
author: mgorny
labels:
  - bug
  - testing
assignees: []
created_at: 2025-01-11T05:38:40Z
updated_at: 2025-01-11T14:08:15Z
url: https://github.com/astral-sh/uv/issues/10493
synced_at: 2026-01-12T16:00:15Z
```

# Tests hang (waiting for username) in 0.5.17

---

_@mgorny_

```
     Running `/var/tmp/portage/dev-python/uv-0.5.17/work/uv-0.5.17/target/x86_64-unknown-linux-gnu/debug/deps/uv-4b4c36941a62e59d`

running 1 test
Enter username ('__token__' if using a token): test commands::publish::tests::username_password_sources has been running for over 60 seconds
```

This is on non-interactive terminal. Tried adding `</dev/null`, didn't help.

---

_Comment by @mgorny on 2025-01-11 05:43_

Can also reproduce on a regular terminal, with failure when i type anything:

```
     Running unittests src/lib.rs (target/debug/deps/uv-58c2e9e5d6669689)

running 1 test
Enter username ('__token__' if using a token): 
Enter password: 
test commands::publish::tests::username_password_sources ... FAILED

failures:

---- commands::publish::tests::username_password_sources stdout ----
thread 'commands::publish::tests::username_password_sources' panicked at crates/uv/src/commands/publish.rs:325:9:
assertion `left == right` failed
  left: Some("")
 right: None
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    commands::publish::tests::username_password_sources

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 27.95s

error: test failed, to rerun pass `-p uv --lib`
```

---

_Comment by @charliermarsh on 2025-01-11 13:37_

Thanks, we'll fix this!

---

_Label `bug` added by @charliermarsh on 2025-01-11 13:37_

---

_Label `testing` added by @charliermarsh on 2025-01-11 13:37_

---

_Closed by @charliermarsh on 2025-01-11 14:06_

---

_Closed by @charliermarsh on 2025-01-11 14:06_

---

_Comment by @mgorny on 2025-01-11 14:08_

Thanks a lot!

---
