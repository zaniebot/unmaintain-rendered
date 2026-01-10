```yaml
number: 4835
title: Print Installation Path if not in working directory
type: pull_request
state: closed
author: danielenricocahall
labels: []
assignees: []
base: main
head: issue-2155-print-install-path
created_at: 2024-07-05T20:16:01Z
updated_at: 2024-10-01T16:51:28Z
url: https://github.com/astral-sh/uv/pull/4835
synced_at: 2026-01-10T12:53:31Z
```

# Print Installation Path if not in working directory

---

_Pull request opened by @danielenricocahall on 2024-07-05 20:16_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Addressing the issue detailed [here](https://github.com/astral-sh/uv/issues/2155) regarding the visibility of the Python path when doing `uv install`.

## Test Plan

TBD - can't quite reproduce the errors I'm seeing in CI locally.


---

_Comment by @zanieb on 2024-07-06 03:18_

Don't spend too much time on the snapshot filtering, I can futz with that part.

---

_Comment by @danielenricocahall on 2024-07-06 04:06_

> Don't spend too much time on the snapshot filtering, I can futz with that part.

Thank you! My hero - I’ve been wrestling with that and can’t quite reproduce this last failure. 

---

_Comment by @zanieb on 2024-07-10 01:08_

Sorry lost track of this one! I'll give this a look tomorrow.

---

_Assigned to @zanieb by @zanieb on 2024-07-10 01:08_

---

_Comment by @danielenricocahall on 2024-07-10 01:36_

> Sorry lost track of this one! I'll give this a look tomorrow.

No worries and thank you!!

---

_@zanieb reviewed on 2024-07-10 14:31_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:236 on 2024-07-10 14:31_

I'm hesitant to display these as user-facing messages for these internal environments — I wonder if there's a good way to avoid that?

---

_Comment by @zanieb on 2024-07-10 14:57_

I think the additional filters in https://github.com/astral-sh/uv/pull/4787 will be helpful here.

---

_@zanieb reviewed on 2024-07-10 14:58_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1092 on 2024-07-10 14:58_

Shouldn't we be showing `[VENV]` not `[VENV]/bin/python3`? It seems a bit weird to show the path to the interpreter executable.

---

_@danielenricocahall reviewed on 2024-07-13 00:03_

---

_Review comment by @danielenricocahall on `crates/uv/tests/pip_install.rs`:1092 on 2024-07-13 00:03_

Ah yeah, we could do that - something like:

```rust
        if is_outside_working_directory {
            if let Some(parent_dir) = python_executable.parent().and_then(|p| p.parent()) {
                writeln!(
                    printer.stderr(),
                    "{}",
                    format!(
                        "Installing to environment at {}",
                        parent_dir.user_display()
                    )
                        .dimmed()
                )?;
            }
        }
```
? This yields `[VENV]/` instead of `[VENV]/bin/python`

---

_Comment by @danielenricocahall on 2024-07-13 00:07_

> I think the additional filters in #4787 will be helpful here.

Hm are you thinking we use them before the parent dir evaluation step?

---

_@danielenricocahall reviewed on 2024-07-13 00:11_

---

_Review comment by @danielenricocahall on `crates/uv/tests/run.rs`:236 on 2024-07-13 00:11_

Ah so revert the `[RANDOM]` filter and have an additional conditional to ensure we don't print in the event it's an internal environment e.g; `[CACHE]/environments-v1...`?

---

_Comment by @danielenricocahall on 2024-07-19 01:08_

Hi @zanieb ! Just wanted to follow-up here if/when you have a moment 😄 

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:236 on 2024-07-19 17:04_

Yeah I think so. I'm curious for @charliermarsh's opinion.

---

_@zanieb reviewed on 2024-07-19 17:04_

---

_@zanieb reviewed on 2024-07-19 17:05_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:1092 on 2024-07-19 17:05_

I think you just want `PythonEnvironment::root()`, i.e., `venv.root()`?

---

_Comment by @zanieb on 2024-07-19 17:06_

Thanks for the nudge! Sorry I'm stretched a little thin on contributions :) Let me know if you have more questions!

---

_Comment by @zanieb on 2024-07-19 17:07_

I don't actually think you'll need the new test filters if you're not displaying the Python executable anymore.

---

_Comment by @zanieb on 2024-10-01 16:51_

Continuing this in https://github.com/astral-sh/uv/pull/7850

---

_Closed by @zanieb on 2024-10-01 16:51_

---
