```yaml
number: 5219
title: "Change to show only the python installed on the system if `--python-preference only-system` is specified"
type: pull_request
state: merged
author: Di-Is
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: fix-python-list-specified-system-only
created_at: 2024-07-19T14:07:13Z
updated_at: 2024-07-20T13:04:20Z
url: https://github.com/astral-sh/uv/pull/5219
synced_at: 2026-01-12T16:06:42Z
```

# Change to show only the python installed on the system if `--python-preference only-system` is specified

---

_@Di-Is_

Fix #5211

## Summary

Change to show only the python installed on the system if `--python-preference only-system` is specified.

Below is an example of running the command before the change, showing Python not installed on the system.

#### Before
```bash
# Check system python
$ uv python --preview list --python-preference only-system
cpython-3.12.4-linux-x86_64-gnu     <download available>
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python3.12
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu     /bin/python3.12
cpython-3.12.3-linux-x86_64-gnu     /bin/python3
cpython-3.11.9-linux-x86_64-gnu     <download available>
cpython-3.10.14-linux-x86_64-gnu    <download available>
cpython-3.9.19-linux-x86_64-gnu     <download available>
cpython-3.8.19-linux-x86_64-gnu     <download available>
cpython-3.7.9-linux-x86_64-gnu      <download available>
```

This PR changes the display to show only Python installed on the system.

#### After

```bash
$ cargo run python --preview list --python-preference only-system
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/uv python list --python-preference only-system --preview`
cpython-3.12.3-linux-x86_64-gnu    /usr/bin/python3.12
cpython-3.12.3-linux-x86_64-gnu    /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu    /bin/python3.12
cpython-3.12.3-linux-x86_64-gnu    /bin/python3
```

## Test Plan
- `cargo run python list --python-preference only-system` in Ubuntu 24.04 to verify the display.
- `cargo run python list` in Ubuntu 24.04 to verify that the results before and after the change were the same.

---

_@zanieb approved on 2024-07-19 14:13_

Thanks!

---

_Label `cli` added by @zanieb on 2024-07-19 14:13_

---

_Label `preview` added by @zanieb on 2024-07-19 14:13_

---

_Merged by @zanieb on 2024-07-19 14:15_

---

_Closed by @zanieb on 2024-07-19 14:15_

---

_Branch deleted on 2024-07-20 13:04_

---
