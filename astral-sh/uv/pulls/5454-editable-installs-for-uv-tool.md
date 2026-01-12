```yaml
number: 5454
title: "Editable installs for `uv tool`"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: editable-uv-tool
created_at: 2024-07-25T17:49:24Z
updated_at: 2024-07-26T20:30:15Z
url: https://github.com/astral-sh/uv/pull/5454
synced_at: 2026-01-12T16:06:49Z
```

# Editable installs for `uv tool`

---

_@blueraft_

## Summary

Resolves #5436. 

## Test Plan

`cargo test` 

```console
❯ ./target/debug/uv tool install -e ~/black
warning: `uv tool install` is experimental and may change without warning
Resolved 6 packages in 894ms
   Built black @ file:///Users/ahmedilyas/black
Prepared 1 package in 468ms
Installed 6 packages in 6ms
 + black==24.4.3.dev23+g7e2afc9 (from file:///Users/ahmedilyas/black)
 + click==8.1.7
 + mypy-extensions==1.0.0
 + packaging==24.1
 + pathspec==0.12.1
 + platformdirs==4.2.2
Installed 2 executables: black, blackd
```

venv has the `.pth` files.
```console
❯ eza /Users/ahmedilyas/Library/Application\ Support/uv/tools/black/lib/python3.12/site-packages/
_black.pth       _virtualenv.py                         click                  mypy_extensions-1.0.0.dist-info  packaging                 pathspec                   platformdirs
_virtualenv.pth  black-24.4.3.dev23+g7e2afc9.dist-info  click-8.1.7.dist-info  mypy_extensions.py               packaging-24.1.dist-info  pathspec-0.12.1.dist-info  platformdirs-4.2.2.dist-info
```


---

_@charliermarsh reviewed on 2024-07-25 18:11_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:549 on 2024-07-25 18:11_

Can we show the receipt here?

---

_@charliermarsh reviewed on 2024-07-25 18:12_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:549 on 2024-07-25 18:12_

Can we also add a test for `uv tool install back` after `uv tool install -e ./scripts/packages/black_editable`?

---

_@blueraft reviewed on 2024-07-25 18:28_

---

_Review comment by @blueraft on `crates/uv/tests/tool_install.rs`:549 on 2024-07-25 18:28_

> Can we show the receipt here?

`uv tool install` exits with a failure for the `black_editable` package since it has no executables. So no receipt gets created in this scenario.

---

_@charliermarsh reviewed on 2024-07-25 20:41_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:549 on 2024-07-25 20:41_

Can we add an executable to it?

---

_@blueraft reviewed on 2024-07-25 20:58_

---

_Review comment by @blueraft on `crates/uv/tests/tool_install.rs`:549 on 2024-07-25 20:58_

I've added a simple executable and added additional snapshot tests.

> Can we also add a test for uv tool install back after uv tool install -e ./scripts/packages/black_editable?

I added this too, but it just installs `black` version from the PyPi server in this case, do you think that's the right behaviour here? 



---

_Comment by @charliermarsh on 2024-07-26 19:49_

I'd like to merge https://github.com/astral-sh/uv/pull/5489 before this.

---

_@charliermarsh reviewed on 2024-07-26 19:49_

---

_Review comment by @charliermarsh on `crates/uv/tests/tool_install.rs`:549 on 2024-07-26 19:49_

I think that's because you're creating a new context here. If you remove that, it retains the install (but the behavior is kind of confusing -- https://github.com/astral-sh/uv/pull/5489 helps).


---

_Comment by @charliermarsh on 2024-07-26 19:56_

Actually, it's ok to merge as-is. There are some confusing behaviors, but they also exist on `main`.

---

_Label `enhancement` added by @charliermarsh on 2024-07-26 19:56_

---

_Label `preview` added by @charliermarsh on 2024-07-26 19:56_

---

_Merged by @charliermarsh on 2024-07-26 20:30_

---

_Closed by @charliermarsh on 2024-07-26 20:30_

---
