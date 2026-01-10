```yaml
number: 7920
title: ":wrench: add `--install-dir` arg to `uv python` commands"
type: pull_request
state: merged
author: danielgafni
labels:
  - cli
assignees: []
merged: true
base: main
head: python-install-dir-cli-arg
created_at: 2024-10-04T10:52:50Z
updated_at: 2024-12-10T17:04:32Z
url: https://github.com/astral-sh/uv/pull/7920
synced_at: 2026-01-10T11:59:59Z
```

# :wrench: add `--install-dir` arg to `uv python` commands

---

_Pull request opened by @danielgafni on 2024-10-04 10:52_

## Summary

This PR adds `--install-dir` argument for the following commands:
- `uv python install`
- `uv python uninstall`

The `UV_PYTHON_INSTALL_DIR` env variable can be used to set it (previously it was also used internally). 

Any more commands we would want to add this to? 

## Test Plan

For now just manual test (works on my machine hehe)

```
❯ ./target/debug/uv python install --install-dir /tmp/pythons 3.8.12
Searching for Python versions matching: Python 3.8.12
Installed Python 3.8.12 in 4.31s
 + cpython-3.8.12-linux-x86_64-gnu
❯ /tmp/pythons/cpython-3.8.12-linux-x86_64-gnu/bin/python --help
usage: /tmp/pythons/cpython-3.8.12-linux-x86_64-gnu/bin/python [option] ... [-c cmd | -m mod | file | -] [arg] ...
```

Open to add some tests after the initial feedback. 

---

_Comment by @danielgafni on 2024-10-04 10:56_

Warning: my Rust is very very weak so be prepared to encounter something which is not ideal :) 

---

_Comment by @danielgafni on 2024-10-04 21:07_

@zanieb pinging you since 

> I'd also be down to accept a target directory in the CLI.

---

_Assigned to @zanieb by @zanieb on 2024-10-04 21:17_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:68 on 2024-10-14 20:26_

Instead, I'd update `from_settings` to take an `Option<PathBuf>` like we do for the `Cache` struct

https://github.com/astral-sh/uv/blob/d0dda3798d2514350ab29e4dc3870cd56479a713/crates/uv-cache/src/cli.rs#L42

---

_@zanieb reviewed on 2024-10-14 20:26_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4241 on 2024-10-14 20:27_

Can you drop this trailing whitespace? 

---

_@zanieb reviewed on 2024-10-14 20:27_

---

_@zanieb reviewed on 2024-10-14 20:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3562 on 2024-10-14 20:28_

In the existing documentation we say

> the directory where uv will store managed Python installations.

Should we be changing it here? Should we change this to say something similar like

> The directory to store the Python installation in.

---

_Comment by @zanieb on 2024-10-14 20:29_

Thank you! A couple minor comments. We can't quite add tests for this yet because we currently avoid downloading managed Python installations in the test suite.

---

_Label `cli` added by @zanieb on 2024-10-14 20:29_

---

_@danielgafni reviewed on 2024-10-16 22:40_

---

_Review comment by @danielgafni on `crates/uv/tests/it/pip_sync.rs`:66 on 2024-10-16 22:40_

is this expected? it has my absolute path here...

---

_Comment by @danielgafni on 2024-10-17 11:36_

@zanieb could you take a look again please? 

---

_Review requested from @zanieb by @danielgafni on 2024-10-17 11:36_

---

_Comment by @zanieb on 2024-12-10 15:14_

@danielgafni

My sincere apologies for the delay, I lost track of this one. I was actually confused yesterday because I thought this had been merged already but couldn't find the feature.

---

_@zanieb approved on 2024-12-10 15:23_

---

_Merged by @zanieb on 2024-12-10 17:04_

---

_Closed by @zanieb on 2024-12-10 17:04_

---
