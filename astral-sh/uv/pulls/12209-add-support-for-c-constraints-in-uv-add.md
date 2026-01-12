```yaml
number: 12209
title: "Add support for `-c` constraints in `uv add`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/add-constraints
created_at: 2025-03-17T00:37:51Z
updated_at: 2025-03-17T21:27:34Z
url: https://github.com/astral-sh/uv/pull/12209
synced_at: 2026-01-12T16:10:10Z
```

# Add support for `-c` constraints in `uv add`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/11986.


---

_Review requested from @zanieb by @charliermarsh on 2025-03-17 00:37_

---

_@charliermarsh reviewed on 2025-03-17 00:38_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:249 on 2025-03-17 00:38_

I didn't want to add a `constraints` argument to `do_safe_lock` when only one caller needed it, so I migrated this to a struct with a builder pattern.

---

_Marked ready for review by @charliermarsh on 2025-03-17 00:38_

---

_Label `enhancement` added by @charliermarsh on 2025-03-17 00:38_

---

_Label `cli` added by @charliermarsh on 2025-03-17 00:38_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3293 on 2025-03-17 18:12_

We should probably note these won't be added to the `pyproject.toml` and explain what final effect they have here?

---

_@zanieb reviewed on 2025-03-17 18:12_

---

_@zanieb reviewed on 2025-03-17 18:29_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:724 on 2025-03-17 18:29_

We might need to propagate `--marker` to the constraints, wdyt? I think it's needed for importing a platform-specific lock (e.g., `requirements-win.txt`)

---

_@charliermarsh reviewed on 2025-03-17 18:45_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:724 on 2025-03-17 18:45_

Can you explain the command that would be run and why we need to propagate?

---

_@zanieb reviewed on 2025-03-17 18:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:724 on 2025-03-17 18:53_

`pip-compile` does not add markers to its single platform output. So `pip-compile` on Windows will produce a Windows specific lockfile without markers.

To import this, we need to gate the lock to win32, e.g.:

```
uv add -r requirements-win.in -c requirements-win.txt --marker "sys_platform == 'win32'"
```

I think in reality, this is actually more complicated :/ because you probably have a single `requirements.in` and multiple lockfiles. What you really need is:

```
uv add -r requirements.in \
    -c "requirements-win.txt; sys_platform == 'win32'" \
    -c "requirements-linux.txt; sys_platform == 'linux'"
```

`--marker` doesn't work since it's applied to the input requirements.

For a concrete example, here's what I have in my pip migration draft...

> For example, take a simple dependency:
> 
> ```text title="requirements.in"
> tqdm
> ```
> 
> On Linux, this compiles to:
> 
> ```text title="requirements-linux.txt"
> tqdm==4.67.1
>     # via -r requirements.in
> ```
> 
> While on Windows, this compiles to:
> 
> ```text title="requirements-win.txt"
> colorama==0.4.6
>     # via tqdm
> tqdm==4.67.1
>     # via -r requirements.in
> ```



---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:724 on 2025-03-17 18:54_

I'm not sure if separating input requirements is common. I don't think we should focus on support for that.

---

_@zanieb reviewed on 2025-03-17 18:54_

---

_@charliermarsh reviewed on 2025-03-17 18:55_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:724 on 2025-03-17 18:55_

Hmm ok... We can add the marker... It should be harmless...

---

_@zanieb reviewed on 2025-03-17 19:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:724 on 2025-03-17 19:13_

(Followed-up synchronously because I'm not sure what's best here)

I think not respecting `--marker` to start is fine. I'm not sure what to do for that use-case yet.

---

_@zanieb approved on 2025-03-17 20:52_

---

_Merged by @charliermarsh on 2025-03-17 21:27_

---

_Closed by @charliermarsh on 2025-03-17 21:27_

---

_Branch deleted on 2025-03-17 21:27_

---
