```yaml
number: 5148
title: "Search for all `python3.x` in PATH"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/search-for-any-minor
created_at: 2024-07-17T15:51:15Z
updated_at: 2024-07-23T14:06:30Z
url: https://github.com/astral-sh/uv/pull/5148
synced_at: 2026-01-10T13:37:23Z
```

# Search for all `python3.x` in PATH

---

_Pull request opened by @konstin on 2024-07-17 15:51_

Search for all `python3.x` minor versions in PATH, skipping those we already know we can use.

For example, let's say `python` and `python3` are Python 3.10. When a user requests `>= 3.11`, we still need to find a `python3.12` in PATH. We do so with a regex matcher.

Fixes #4709


---

_Label `enhancement` added by @konstin on 2024-07-17 15:51_

---

_Review requested from @zanieb by @konstin on 2024-07-17 15:51_

---

_@konstin reviewed on 2024-07-17 15:57_

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:444 on 2024-07-17 15:57_

It's silly we can't use capture groups for this but the `which` api is too locked down for that

---

_@zanieb reviewed on 2024-07-17 17:20_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:444 on 2024-07-17 17:20_

I feel like we should just not use `which` and we should iterate over the executables in the directory manually. A regex feels heavier than it needs to be. Wdyt?

---

_Comment by @zanieb on 2024-07-17 17:22_

Thanks for tackling this one!

---

_@konstin reviewed on 2024-07-17 19:45_

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:444 on 2024-07-17 19:45_

That's an implementation i used to have (without `which` it's much better than the current one), but it failed because `which` doesn't expose `Checker` so we'd have to vendor that code from them to check whether we can use a specific file.

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:444 on 2024-07-17 22:59_

I'm okay with vendoring that code if you are.

---

_@zanieb reviewed on 2024-07-17 22:59_

---

_@konstin reviewed on 2024-07-18 10:45_

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:444 on 2024-07-18 10:45_

Added `is_executable`

---

_Comment by @zanieb on 2024-07-18 14:49_

Should we be using a regex at all now? Or can we just use some simple string parsing?

---

_Comment by @konstin on 2024-07-18 14:50_

I find the regex easier to follow along

---

_Comment by @zanieb on 2024-07-18 14:53_

I worry a bit about performance since this isn't included in any benchmarks, but trust your intuition on it.

---

_Comment by @konstin on 2024-07-18 14:56_

I'm confident that the regex is fast. Andrew has done great performance work there :)

---

_Comment by @zanieb on 2024-07-18 14:58_

@BurntSushi notorious for writing slow code hehe

---

_@zanieb approved on 2024-07-18 14:58_

---

_Merged by @konstin on 2024-07-18 15:00_

---

_Closed by @konstin on 2024-07-18 15:00_

---

_Branch deleted on 2024-07-18 15:00_

---

_Comment by @BurntSushi on 2024-07-23 14:06_

In terms of perf, this is also including regex compilation, which is definitely _not_ known for being fast. This might actually be a case where `regex-lite` could be faster overall. Another potential problem is if the function being added here is called more than once for a particular set of inputs. Then you're rebuilding the regex each time.

I don't mean to suggest anything should change though. I wouldn't be surprised if the perf difference was marginal.

---
