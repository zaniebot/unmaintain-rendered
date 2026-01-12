```yaml
number: 5884
title: "Introduce `default-source` to slim lockfile"
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: main
head: konsti/default-source
created_at: 2024-08-07T18:18:21Z
updated_at: 2024-08-09T01:40:08Z
url: https://github.com/astral-sh/uv/pull/5884
synced_at: 2026-01-12T16:07:04Z
```

# Introduce `default-source` to slim lockfile

---

_@konstin_

Currently, we're repeating the same `source` line for every package, so the lockfiles have a lot of:

```toml
source = { registry = "https://pypi.org/simple" }
```

This PR introduces a top level `default-source` entry set to the default index URL, if any. When the source matches, we don't repeat the `source` entry. This reduces the number of lines in `uv.lock` noticeably across the board:

* A small data science project: 421 -> 394
* A small bot: 455 -> 426
* Transformers: 5683 -> 5419
* Warehouse: 4632 -> 4306
* Airflow: 2709 -> 2576

Caveat: We don't have good multi-index coverage (#5882).

3/3 for #4893, closes #4893

---

_@charliermarsh reviewed on 2024-08-07 20:39_

What if the default source is never used? Does it still end up in the lockfile?

I wonder if we should only do this when there's exactly one source.


---

_Comment by @zanieb on 2024-08-07 20:49_

I'm not sure those line count changes justify the increased complexity?

---

_Comment by @konstin on 2024-08-07 20:56_

> What if the default source is never used? Does it still end up in the lockfile?

It would, do we have cases where that happens?

> I wonder if we should only do this when there's exactly one source.

Happy to bikeshed the exact semantics, we're not having good coverage of multi-index scenarios right now.

> I'm not sure those line count changes justify the increased complexity?

The complexity didn't significantly increase much imho. The one thing that does get worse is that it's harder to audit which package is from which source.

---

_Comment by @charliermarsh on 2024-08-07 22:27_

I think I'm in favor but I'd probably prefer: we just write it as `source` (not `default-source`), and we only do it if the source is the same for every package.

---

_Comment by @zanieb on 2024-08-07 23:43_

I'm fine with that though it's a big diff once you add a second source — I don't fully understand the motivation for this change.

---

_Comment by @charliermarsh on 2024-08-07 23:44_

It makes the lockfile much more succinct by removing redundant information.

---

_Comment by @zanieb on 2024-08-08 02:22_

Is it much more succinct though? We're talking about 20 lines in a 400 line file.

(I think I see the argument for it being less noisy when there is a single source, but I'm not sure it's worth the added diff when another source is added)

---

_Comment by @charliermarsh on 2024-08-08 02:25_

For the larger lockfiles it's about a 4-7% saving. I dunno, it seems reasonable to me, but I can go either way.

---

_Comment by @charliermarsh on 2024-08-09 01:40_

We decided (in our 1:1) to defer this for now.

---

_Closed by @charliermarsh on 2024-08-09 01:40_

---
