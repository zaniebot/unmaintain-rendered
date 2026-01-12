```yaml
number: 4605
title: Retry on spurious failures when caching built wheels
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/rename-retry-wheel
created_at: 2024-06-27T23:02:27Z
updated_at: 2024-06-28T14:23:11Z
url: https://github.com/astral-sh/uv/pull/4605
synced_at: 2026-01-12T16:06:20Z
```

# Retry on spurious failures when caching built wheels

---

_@zanieb_

https://github.com/astral-sh/uv/pull/2419 appears to have only applied this retry to wheels that were already downloaded (though I would have to look more carefully to be certain). In https://github.com/astral-sh/uv/issues/1491, we've gotten continued reports of spurious failures on Windows and tracing reveals that we are not applying our retry logic during the rename. I believe we're in this code path — switching to our backoff retry should resolve the failures.

---

_Label `bug` added by @zanieb on 2024-06-27 23:02_

---

_Comment by @zanieb on 2024-06-27 23:13_

There are various other `rename` calls that we may want to audit as well. I've changed the remaining async ones in #4606, I don't see reason to change the synchronous ones unless we see problems?

---

_Comment by @zanieb on 2024-06-27 23:19_

I believe this is only called at https://github.com/astral-sh/uv/blob/f01aebe6dd2b284f40fe96bb72f35cd019d2975f/crates/uv-distribution/src/source/mod.rs#L1430 when performing `build_distribution` i.e. placing a built wheel in the cache

The other pull request includes `persist_archive` which is called at https://github.com/astral-sh/uv/blob/f01aebe6dd2b284f40fe96bb72f35cd019d2975f/crates/uv-distribution/src/source/mod.rs#L863
i.e. extracting a source distribution to the cache 

---

_Comment by @zanieb on 2024-06-27 23:21_

Here's the [error](https://github.com/astral-sh/uv/issues/1491#issuecomment-2195778958) we're attempting to address

> Caused by: Failed to write to the distribution cache
>  Caused by: failed to rename file from \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\.tmpc3E2Z7\pywinusb-0.4.2 to \\?\C:\Users\gitlabwinrunner\AppData\Local\Temp\.tmpanLS7H\built-wheels-v3\index\70e6dbc41e17fe7c\pywinusb\0.4.2\or1SrhePMDCdKnzdyzMki\pywinusb-0.4.2.zip

---

_Marked ready for review by @zanieb on 2024-06-27 23:22_

---

_Comment by @axel-kah on 2024-06-27 23:23_

I took a guess and pulled the binary from this PRs build binary | windows job:
https://github.com/astral-sh/uv/actions/runs/9704735622/job/26785531647?pr=4605
https://github.com/astral-sh/uv/actions/runs/9704735622/artifacts/1646714270

When I use that in conjunction with hatch, then all I get is this 😬 
```
$ hatch version
Setting up build environment for missing dependencies
Installing Python distribution: 3.11
thread 'main' has overflowed its stack
```

---

_Comment by @zanieb on 2024-06-27 23:27_

@axel-kah The debug builds have a weirdly small stack size on Windows — not a problem in releases but we have to work around it during testing.

```powershell
$Env:UV_STACK_SIZE = '2000000'
```

More details in our [contributing guide](https://github.com/astral-sh/uv/blob/631994c485560fa950bc576bc15c99a761c6b932/CONTRIBUTING.md#testing-on-windows)

---

_Comment by @axel-kah on 2024-06-27 23:47_

After setting the stack size the debug build works - but run's into the same error. I'll give #4606 a shot as well.

EDIT: and no instance of retrying in the verbose log.

---

_Comment by @axel-kah on 2024-06-28 00:09_

Hm, neither this nor #4606 seem to eliminate this problem so far.

---

_@konstin approved on 2024-06-28 11:52_

---

_Comment by @zanieb on 2024-06-28 14:23_

That's surprising :(

---

_Merged by @zanieb on 2024-06-28 14:23_

---

_Closed by @zanieb on 2024-06-28 14:23_

---

_Branch deleted on 2024-06-28 14:23_

---
