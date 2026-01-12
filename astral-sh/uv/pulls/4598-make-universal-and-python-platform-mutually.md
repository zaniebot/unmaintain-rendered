```yaml
number: 4598
title: "Make `--universal` and `--python-platform` mutually exclusive"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/plat
created_at: 2024-06-27T18:31:47Z
updated_at: 2024-06-27T18:51:45Z
url: https://github.com/astral-sh/uv/pull/4598
synced_at: 2026-01-12T16:06:20Z
```

# Make `--universal` and `--python-platform` mutually exclusive

---

_@charliermarsh_

## Summary

Open to just making this a warning but no strong opinion.

Closes https://github.com/astral-sh/uv/issues/4593.

## Test Plan

Failure:

```
❯ echo "pandas==2.2.2" | cargo run pip compile --universal -p 3.11 --no-header - --python-platform linux
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/uv pip compile --universal -p 3.11 --no-header - --python-platform linux`
error: the argument '--universal' cannot be used with '--python-platform <PYTHON_PLATFORM>'

Usage: uv pip compile --universal --python-version <PYTHON_VERSION> --no-header <SRC_FILE>...

For more information, try '--help'.
```


---

_Label `cli` added by @charliermarsh on 2024-06-27 18:31_

---

_Renamed from "Make --universal and --python-platform mutually exclusive" to "Make `--universal` and `--python-platform` mutually exclusive" by @charliermarsh on 2024-06-27 18:32_

---

_Comment by @charliermarsh on 2024-06-27 18:37_

I think I'll just make it a warning, shrug.

---

_@zanieb approved on 2024-06-27 18:42_

If a warning, we ignore the `python-platform` entirely?

---

_Comment by @charliermarsh on 2024-06-27 18:42_

Yeah we ignore it either way.

---

_Comment by @zanieb on 2024-06-27 18:43_

Let's error then.

---

_Comment by @charliermarsh on 2024-06-27 18:43_

Ok, fine with me.

---

_@konstin approved on 2024-06-27 18:45_

---

_Merged by @charliermarsh on 2024-06-27 18:51_

---

_Closed by @charliermarsh on 2024-06-27 18:51_

---

_Branch deleted on 2024-06-27 18:51_

---
