```yaml
number: 13873
title: "Support python@latest for newest, stable, discoverable Python"
type: pull_request
state: open
author: maxmynter
labels: []
assignees: []
base: main
head: latest-python
created_at: 2025-06-05T20:13:09Z
updated_at: 2025-06-13T11:43:01Z
url: https://github.com/astral-sh/uv/pull/13873
synced_at: 2026-01-10T11:10:42Z
```

# Support python@latest for newest, stable, discoverable Python

---

_Pull request opened by @maxmynter on 2025-06-05 20:13_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes #13535

## Summary

Support `python@latest` to find and use the latest stable Python version available on the system.
It finds the highest discoverable, stable Python version excluding prerelease
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Tested locally with `target/debug/uv tool run python@latest --version`.
Added test cases in `crates/uv-python/src/lib.rs`

<!-- How was it tested? -->


---

_Assigned to @zanieb by @zanieb on 2025-06-06 03:42_

---

_Comment by @maxmynter on 2025-06-06 12:47_

When (attempting) merge conflict resolution I seem to have regressed some of the snapshots. I will take some time later today, to look into it. 

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:1062 on 2025-06-06 19:36_

What's the difference between this `Latest` behavior and the existing sort that `python_interpreters` is already returning, which I believe is determined over here: https://github.com/astral-sh/uv/blob/5b0133c0ecedf71a23e5d9bf6fbefc91008e5101/crates/uv-python/src/installation.rs#L480-L492

Via here: https://github.com/astral-sh/uv/blob/5b0133c0ecedf71a23e5d9bf6fbefc91008e5101/crates/uv-python/src/managed.rs#L240

---

_@oconnor663 reviewed on 2025-06-06 19:36_

---

_Comment by @oconnor663 on 2025-06-06 19:38_

We might want to update this comment to refer to a different word that "sounds like a version specifier but actually isn't and also ends with `t`". Maybe..."oldest"? 😄 

https://github.com/astral-sh/uv/blob/5b0133c0ecedf71a23e5d9bf6fbefc91008e5101/crates/uv-python/src/discovery.rs#L2468-L2469

---

_@maxmynter reviewed on 2025-06-06 23:12_

---

_Review comment by @maxmynter on `crates/uv-python/src/discovery.rs`:1062 on 2025-06-06 23:12_

Refactored to use the implemented `Ord` in [5a45d4e](https://github.com/astral-sh/uv/pull/13873/commits/5a45d4ee85611c8975938cceba3ebfa0571a79c7)

---

_Review requested from @oconnor663 by @maxmynter on 2025-06-06 23:41_

---

_@oconnor663 reviewed on 2025-06-07 00:55_

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:1062 on 2025-06-07 00:55_

What I'm thinking is more like, what's the behavior difference between `@latest` and `@any`? (I'm sure there's something I just haven't figured out what.)

---

_@zanieb reviewed on 2025-06-07 05:11_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1062 on 2025-06-07 05:11_

I would expect `@latest` to scan _all_ Python interpreters and select the latest version, whereas `@any` would stop on the first interpreter.


---

_Comment by @zanieb on 2025-06-07 05:12_

Thanks for the pull request! I'll review it next week.

I wonder if we want to do this. Should `@latest` just be an alias for 3.13? I think if I had automatic Python downloads turned on I'd expect the latest version of 3.13 to be installed? I also worry scanning all the interpreters on the system will be slow? I think we need to consider the user experience a bit here.

---

_@oconnor663 reviewed on 2025-06-09 16:52_

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:1062 on 2025-06-09 16:52_

@zanieb how do you think about that in combination with the fact that the list that `@any` looks at is already sorted by version? (IIUC it's actually sorted by implementation [e.g. `cpython`] first and then version.) So picking the latest and picking the first are approximately the same operation?

---

_@zanieb reviewed on 2025-06-09 19:58_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1062 on 2025-06-09 19:58_

`@any` isn't sorted by version during discovery though, because we short-circuit when we find an interpreter that meets the request. Sorting only makes a difference when selecting a Python download or iterating over managed Python installations.

---

_Comment by @maxmynter on 2025-06-12 15:54_

Yeah, we could hardcode a specific stable python version and then bump if whenever a new stable one is released.
I think if I do `@latest` that may also just be what i expect -- take and / or download the newest stable one.

If I wanted the newest on my system not overall, I propably have a reason for this specific version and know which one it is. At that point it's probably better to be explicit and use `@x.yz` instead of `@latest` anyway.

---

_Comment by @zanieb on 2025-06-13 11:43_

I think with downloads enabled, I'd expect it to take the latest available stable download. Otherwise, I'd expect it to take the latest on my machine.

---
