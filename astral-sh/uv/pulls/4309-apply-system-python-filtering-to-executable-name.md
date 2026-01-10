```yaml
number: 4309
title: Apply system Python filtering to executable name requests
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: release/3
head: zb/executable-name-system
created_at: 2024-06-13T16:57:27Z
updated_at: 2024-08-19T20:51:38Z
url: https://github.com/astral-sh/uv/pull/4309
synced_at: 2026-01-10T13:09:50Z
```

# Apply system Python filtering to executable name requests

---

_Pull request opened by @zanieb on 2024-06-13 16:57_

Executable name requests were being treated as explicit requests to install into system environments, but I don't think it should be as it's implicit what environment you'll end up in. Following #4308, we allow multiple executables to be found so we can filter here.

Concretely, this means `--system` is required to install into a system environment discovered with e.g. `--python=python`. The flag is still not required for cases where we're not mutating environment.

---

_Label `breaking` added by @zanieb on 2024-06-13 16:57_

---

_Renamed from "zb/executable name system" to "Apply system Python filtering to executable name requests" by @zanieb on 2024-06-13 16:57_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:572 on 2024-06-13 17:00_

@BurntSushi Is there a more ergonomic way to spell this? I feel like the only other thing is some helper method that does this on the given `Result` type so I don't need to repeat all this soup?

---

_@zanieb reviewed on 2024-06-13 17:00_

---

_@zanieb reviewed on 2024-06-13 17:07_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:572 on 2024-06-13 17:07_

Moved the refactor into https://github.com/astral-sh/uv/pull/4310 so I can work off that without including the breaking change

---

_Review requested from @charliermarsh by @zanieb on 2024-06-13 17:09_

---

_@BurntSushi reviewed on 2024-06-13 17:10_

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/discovery.rs`:553 on 2024-06-13 17:10_

Hmmm maybe, `result.as_ref().ok().map_or(true, |(source, interpreter)| { ... })`?

I still might slap this into a helper with a nice name though if you need to repeat it a lot.

---

_@BurntSushi approved on 2024-06-13 17:11_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/discovery.rs`:553 on 2024-06-13 17:17_

Thanks!

---

_@zanieb reviewed on 2024-06-13 17:17_

---

_Marked ready for review by @zanieb on 2024-06-13 17:18_

---

_Added to milestone `v0.3.0` by @zanieb on 2024-06-13 17:18_

---

_Comment by @codspeed-hq[bot] on 2024-08-19 20:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/executable-name-system)

### Merging #4309 will **improve performances by 10.39%**

<sub>Comparing <code>zb/executable-name-system</code> (85bf490) with <code>release/3</code> (3eefac4)</sub>



### Summary

`⚡ 1` improvements
`✅ 13` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `release/3` | `zb/executable-name-system` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-long-compatible]` | 2.2 µs | 2 µs | +10.39% |


---

_Merged by @zanieb on 2024-08-19 20:51_

---

_Closed by @zanieb on 2024-08-19 20:51_

---

_Branch deleted on 2024-08-19 20:51_

---
