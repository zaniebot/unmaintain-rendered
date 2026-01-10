```yaml
number: 11143
title: Filter discovered Python executables by source before querying
type: pull_request
state: merged
author: zanieb
labels:
  - performance
  - error messages
assignees: []
merged: true
base: main
head: zb/filter
created_at: 2025-01-31T19:45:52Z
updated_at: 2025-01-31T21:54:00Z
url: https://github.com/astral-sh/uv/pull/11143
synced_at: 2026-01-10T11:10:34Z
```

# Filter discovered Python executables by source before querying

---

_Pull request opened by @zanieb on 2025-01-31 19:45_

Closes https://github.com/astral-sh/uv/issues/11138

Though I think we could still have a better error message there.

---

_Label `performance` added by @zanieb on 2025-01-31 19:45_

---

_Label `error messages` added by @zanieb on 2025-01-31 19:45_

---

_Review requested from @Gankra by @zanieb on 2025-01-31 19:50_

---

_Review requested from @charliermarsh by @zanieb on 2025-01-31 19:50_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:437 on 2025-01-31 20:44_

This is a `filter_ok` in #11145 

---

_@zanieb reviewed on 2025-01-31 20:44_

---

_@charliermarsh reviewed on 2025-01-31 21:02_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:632 on 2025-01-31 21:02_

"definitively", I think

---

_@charliermarsh reviewed on 2025-01-31 21:03_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:437 on 2025-01-31 21:03_

Is this doing more work than before, since we're returning all the iterators here?

---

_@zanieb reviewed on 2025-01-31 21:07_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:632 on 2025-01-31 21:07_

indeed

---

_@zanieb reviewed on 2025-01-31 21:07_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:632 on 2025-01-31 21:07_

```suggestion
/// preference as a pre-filtering step. We cannot definitively know if a Python interpreter is in
```

---

_@zanieb reviewed on 2025-01-31 21:13_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:437 on 2025-01-31 21:13_

Uhh maybe. I'd have to squint at it a bit.

Generally, it's much less work. This is pre-query (and querying is the slow part) and `EnvironmentPreference::ExplicitSystem` is the common case so more aggressive filtering here should be better.

It seems possible with `EnvironmentPreference::OnlyVirtual` we now traverse the `PATH` directories whereas previously we did not? I'll check on that — because I agree that's not ideal. I'd be sad about doing _both_ this filtering and the previous `match` statement though, they overlap significantly.

---

_@charliermarsh approved on 2025-01-31 21:14_

---

_@zanieb reviewed on 2025-01-31 21:21_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:437 on 2025-01-31 21:21_

Yes you're correct, this now scans more in the `OnlyVirtual` case. However... we only construct that case in `uv-dev` (and tests)

https://github.com/astral-sh/uv/blob/41cd4bee5861cdda4d826c29cd413b97638186cb/crates/uv-dev/src/compile.rs#L26

which was surprising to me. We basically always use this constructor

https://github.com/astral-sh/uv/blob/e26affd27cd76b37f1fa868687a7b324fbf63ba2/crates/uv-python/src/discovery.rs#L1616-L1627

---

_@zanieb reviewed on 2025-01-31 21:23_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:437 on 2025-01-31 21:23_

The other case is that we'll now also perform the scanning for executables from virtual environments if you provide `--system`, though we'll toss those out. Might be worth changing still.

---

_@zanieb reviewed on 2025-01-31 21:25_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:437 on 2025-01-31 21:25_

e.g.

main

```
❯ uv python find -v --system
DEBUG uv 0.5.26+6 (ca5b84027 2025-01-31)
DEBUG Found project root: `/Users/zb/workspace/uv`
DEBUG Project `uv` is marked as unmanaged
DEBUG Reading Python requests from version file at `/Users/zb/workspace/uv/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/Users/zb/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.14.0a4-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.10.16-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.10.5-macos-aarch64-none`
DEBUG Found `cpython-3.12.8-macos-aarch64-none` at `/opt/homebrew/bin/python3.12` (search path)
/opt/homebrew/opt/python@3.12/bin/python3.12
```

vs branch

```
❯ uv python find -v --system
DEBUG uv 0.5.26+7 (9aefd76b4 2025-01-31)
DEBUG Found project root: `/Users/zb/workspace/uv`
DEBUG Project `uv` is marked as unmanaged
DEBUG Reading Python requests from version file at `/Users/zb/workspace/uv/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Ignoring Python interpreter at `/Users/zb/workspace/uv/.venv/bin/python3`: system interpreter required
DEBUG Searching for managed installations at `/Users/zb/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.14.0a4-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.10.16-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.10.5-macos-aarch64-none`
DEBUG Found `cpython-3.12.8-macos-aarch64-none` at `/opt/homebrew/bin/python3.12` (search path)
/opt/homebrew/opt/python@3.12/bin/python3.12
```

---

_@zanieb reviewed on 2025-01-31 21:38_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:437 on 2025-01-31 21:38_

Thanks for flagging https://github.com/astral-sh/uv/pull/11143/commits/02e72be6661b4a0211ad5ec6feb21dc27871de0f

---

_Merged by @zanieb on 2025-01-31 21:53_

---

_Closed by @zanieb on 2025-01-31 21:53_

---

_Branch deleted on 2025-01-31 21:54_

---
