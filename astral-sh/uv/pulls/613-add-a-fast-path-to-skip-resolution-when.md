```yaml
number: 613
title: Add a fast-path to skip resolution when installation is complete
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/pip-install-noop
created_at: 2023-12-12T04:33:51Z
updated_at: 2023-12-12T17:43:14Z
url: https://github.com/astral-sh/uv/pull/613
synced_at: 2026-01-10T15:44:44Z
```

# Add a fast-path to skip resolution when installation is complete

---

_Pull request opened by @charliermarsh on 2023-12-12 04:33_

For a very large resolution (a few hundred packages), I see 13ms vs. 400ms for a no-op. It's worth optimizing this case, in my opinion.

---

_Label `performance` added by @charliermarsh on 2023-12-12 04:33_

---

_@zanieb reviewed on 2023-12-12 04:48_

---

_Review comment by @zanieb on `crates/puffin-cli/src/commands/pip_install.rs`:71 on 2023-12-12 04:48_

It'd probably be fair to just have this comment without the TODO attached, e.g.
```suggestion
    // Ideally, the resolver would be fast enough to let us remove this check.
```

---

_@zanieb reviewed on 2023-12-12 04:49_

---

_Review comment by @zanieb on `crates/puffin-installer/src/site_packages.rs`:140 on 2023-12-12 04:49_

For my learnings, does pre-specifying the capacity make a big difference?

---

_@charliermarsh reviewed on 2023-12-12 05:43_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/site_packages.rs`:140 on 2023-12-12 05:43_

In general yes -- always good to pre-specify if you can. It means you can allocate the memory upfront, rather than dynamically resizing (which requires further allocations) as you build up the hash map. This also applies to strings, and even to things like (e.g.) reading a file into a buffer: if you know the size in advance, you can allocate the buffer at that size for better efficiency.

---

_@charliermarsh reviewed on 2023-12-12 05:46_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/site_packages.rs`:140 on 2023-12-12 05:46_

I randomly had a benchmark for this in my slides from the Rust Dublin meetup:

<img width="925" alt="Screen Shot 2023-12-12 at 12 46 07 AM" src="https://github.com/astral-sh/puffin/assets/1309177/f0dfe1c1-0180-40ab-af08-7bc4ac116acb">

<img width="1145" alt="Screen Shot 2023-12-12 at 12 46 11 AM" src="https://github.com/astral-sh/puffin/assets/1309177/c440ff93-60e1-4987-a463-cf879468240c">

Just a micro-benchmark though, take it with a grain of salt!


---

_Review comment by @konstin on `crates/distribution-types/src/installed.rs`:140 on 2023-12-12 11:15_

We should attach the filename here

---

_Review comment by @konstin on `crates/puffin-installer/src/site_packages.rs`:140 on 2023-12-12 11:19_

Rust also tries this aggresively internally, e.g. `.collecting()` into a hashmap will allocate based on `iter.size_hint()`

---

_@konstin approved on 2023-12-12 11:20_

---

_Merged by @charliermarsh on 2023-12-12 17:43_

---

_Closed by @charliermarsh on 2023-12-12 17:43_

---

_Branch deleted on 2023-12-12 17:43_

---
