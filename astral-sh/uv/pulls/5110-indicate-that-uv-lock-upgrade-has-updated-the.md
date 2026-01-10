```yaml
number: 5110
title: "Indicate that `uv lock --upgrade` has updated the lock file"
type: pull_request
state: merged
author: blueraft
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: lock-update-log
created_at: 2024-07-16T15:15:08Z
updated_at: 2024-07-17T09:18:03Z
url: https://github.com/astral-sh/uv/pull/5110
synced_at: 2026-01-10T13:42:52Z
```

# Indicate that `uv lock --upgrade` has updated the lock file

---

_Pull request opened by @blueraft on 2024-07-16 15:15_

## Summary

Resolves #4346, I've gone with the suggested `cargo` approach here.

## Test Plan

`cargo test`

```console
❯ ../target/debug/uv lock --upgrade
warning: `uv lock` is experimental and may change without warning.
Resolved 11 packages in 41ms
Updating flask v2.3.3 -> v3.0.3
note: pass `--verbose` to see 9 unchanged dependencies
❯ ../target/debug/uv lock --upgrade -vv
    0.002478s DEBUG uv uv 0.2.24
warning: `uv lock` is experimental and may change without warning.
....
Resolved 11 packages in 50ms
    0.103703s DEBUG uv::commands::project::lock Unchanged blinker v1.8.2
    0.103719s DEBUG uv::commands::project::lock Unchanged click v8.1.7
    0.103731s DEBUG uv::commands::project::lock Unchanged colorama v0.4.6
    0.103742s DEBUG uv::commands::project::lock Unchanged flask v3.0.3
    0.103754s DEBUG uv::commands::project::lock Unchanged importlib-metadata v8.0.0
    0.103767s DEBUG uv::commands::project::lock Unchanged itsdangerous v2.2.0
    0.103778s DEBUG uv::commands::project::lock Unchanged jinja2 v3.1.4
    0.103788s DEBUG uv::commands::project::lock Unchanged markupsafe v2.1.5
    0.103798s DEBUG uv::commands::project::lock Unchanged werkzeug v3.0.3
    0.103809s DEBUG uv::commands::project::lock Unchanged zipp v3.19.2
```


---

_Review requested from @BurntSushi by @zanieb on 2024-07-16 16:46_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-16 16:46_

---

_Comment by @charliermarsh on 2024-07-16 20:33_

I got this one.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-16 20:33_

---

_Label `enhancement` added by @charliermarsh on 2024-07-16 20:33_

---

_Label `preview` added by @charliermarsh on 2024-07-16 20:33_

---

_Comment by @charliermarsh on 2024-07-17 00:51_

There's one problem here which is that we can have multiple versions in the new and existing lockfile. I'm not sure what the best logic is here...

---

_Comment by @charliermarsh on 2024-07-17 00:51_

I'll try something a little different.

---

_Merged by @charliermarsh on 2024-07-17 01:14_

---

_Closed by @charliermarsh on 2024-07-17 01:14_

---

_Branch deleted on 2024-07-17 09:18_

---
