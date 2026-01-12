```yaml
number: 3525
title: Apply advisory locks when building source distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/build-lock
created_at: 2024-05-11T17:18:13Z
updated_at: 2024-05-13T14:42:21Z
url: https://github.com/astral-sh/uv/pull/3525
synced_at: 2026-01-12T16:05:41Z
```

# Apply advisory locks when building source distributions

---

_@charliermarsh_

## Summary

I don't love this, but it turns out that setuptools is not robust to parallel builds: https://github.com/pypa/setuptools/issues/3119. As a result, if you run uv from multiple processes, and they each attempt to build the same source distribution, you can hit failures.

This PR applies an advisory lock to the source distribution directory. We apply it unconditionally, even if we ultimately find something in the cache and _don't_ do a build, which helps ensure that we only build the distribution once (and wait for that build to complete) rather than kicking off builds from each thread.

Closes https://github.com/astral-sh/uv/issues/3512.

## Test Plan

Ran:

```sh
#!/bin/bash
make_venv(){
    target/debug/uv venv $1
    source $1/bin/activate
    target/debug/uv pip install opentracing --no-deps --verbose
}

for i in {1..8}
do
   make_venv ./$1/$i &
done
```

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-11 17:18_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-11 17:18_

---

_Review requested from @konstin by @charliermarsh on 2024-05-11 17:18_

---

_Label `bug` added by @charliermarsh on 2024-05-11 17:18_

---

_@charliermarsh reviewed on 2024-05-11 17:18_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:399 on 2024-05-11 17:18_

Realizing... I think I need this to be async? Is that right @ibraheemdev?

---

_@zanieb reviewed on 2024-05-11 18:09_

---

_Review comment by @zanieb on `crates/uv-distribution/src/source/mod.rs`:399 on 2024-05-11 18:09_

Looks like you hold it across `await` points, so yeah.

---

_@charliermarsh reviewed on 2024-05-11 18:36_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:399 on 2024-05-11 18:36_

It’s not a mutex though, it’s a flock. I’d need to do some research.

---

_@zanieb reviewed on 2024-05-11 19:15_

---

_Review comment by @zanieb on `crates/uv-distribution/src/source/mod.rs`:399 on 2024-05-11 19:15_

Hm.. aren't you still yielding control? So some other task could hit this lock and block without awaiting and returning control to the event loop and consequently to this task? e.g.

- Task A takes lock
- Task A yields to event loop
- Task B attempts to take lock
- Lock is not available so Task B is blocked but has not yielded to the event loop
- Task A is never returned to
- Deadlock

---

_@charliermarsh reviewed on 2024-05-11 21:23_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:399 on 2024-05-11 21:23_

Wrapped it in a tokio task.

---

_Review comment by @konstin on `crates/uv-distribution/src/source/mod.rs`:400 on 2024-05-13 08:35_

Can we put this into a function?

---

_@konstin approved on 2024-05-13 08:39_

---

_@BurntSushi approved on 2024-05-13 14:28_

LGTM.

---

_@BurntSushi reviewed on 2024-05-13 14:28_

---

_Review comment by @BurntSushi on `crates/uv-distribution/src/source/mod.rs`:399 on 2024-05-13 14:28_

Yeah I agree with this.

---

_Merged by @charliermarsh on 2024-05-13 14:42_

---

_Closed by @charliermarsh on 2024-05-13 14:42_

---

_Branch deleted on 2024-05-13 14:42_

---
