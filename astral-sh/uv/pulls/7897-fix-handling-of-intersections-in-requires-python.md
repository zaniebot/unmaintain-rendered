```yaml
number: 7897
title: "Fix handling of != intersections in `requires-python`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/req
created_at: 2024-10-03T14:01:56Z
updated_at: 2024-10-09T22:42:48Z
url: https://github.com/astral-sh/uv/pull/7897
synced_at: 2026-01-10T12:53:58Z
```

# Fix handling of != intersections in `requires-python`

---

_Pull request opened by @charliermarsh on 2024-10-03 14:01_

## Summary

The issue here is that, if you user has a `requires-python` like `>= 3.7, != 3.8.5`, this gets expanded to the following bounds:

- `[3.7, 3.8.5)`
- `(3.8.5, ...`

We then convert this to the specific `>= 3.7, < 3.8.5, > 3.8.5`. But the commas in that expression are conjunctions... So it's impossible to satisfy? No version is both `< 3.8.5` and `> 3.8.5`.

Instead, we now preserve the input `requires-python` and just concatenate the terms, only using PubGrub to compute the _bounds_.

Closes https://github.com/astral-sh/uv/issues/7862.


---

_Label `bug` added by @charliermarsh on 2024-10-03 14:01_

---

_Converted to draft by @charliermarsh on 2024-10-03 14:09_

---

_@charliermarsh reviewed on 2024-10-03 14:24_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:3939 on 2024-10-03 14:24_

We now retain this more-or-less verbatim. I think this is ok since we'll ignore these anywhere that it matters when we actually _use_ it.

---

_Review requested from @konstin by @charliermarsh on 2024-10-03 14:24_

---

_Marked ready for review by @charliermarsh on 2024-10-03 14:24_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/requires_python.rs`:99 on 2024-10-03 14:24_

This isn't quite correct -- we can't just AND these, since a `!=` specifier gets converted to `<` and `>` the value. So instead we retain the actual specifiers now.

---

_@charliermarsh reviewed on 2024-10-03 14:24_

---

_Review requested from @zanieb by @zanieb on 2024-10-03 14:29_

---

_@charliermarsh reviewed on 2024-10-03 17:59_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:381 on 2024-10-03 17:59_

This is a pain. Maybe we do want to apply _some_ simplifications here...? Like ignoring redundant clauses?

---

_Comment by @charliermarsh on 2024-10-03 18:00_

@ibraheemdev -- You may be interested in this too.

---

_@charliermarsh reviewed on 2024-10-03 18:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:381 on 2024-10-03 18:03_

I guess we could do something like: only add the specifier if it changes the computed bounds? That could work.

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:381 on 2024-10-03 18:45_

Can you use `Range::subset_of`?

---

_@zanieb reviewed on 2024-10-03 18:45_

---

_Comment by @charliermarsh on 2024-10-08 22:09_

Can I get review on this one?

---

_@zanieb reviewed on 2024-10-08 22:38_

---

_Review comment by @zanieb on `crates/uv-resolver/src/requires_python.rs`:103 on 2024-10-08 22:38_

Does the doc comment for this function (`intersection`) need to be updated?

---

_@zanieb approved on 2024-10-08 23:14_

---

_@zanieb reviewed on 2024-10-08 23:16_

---

_Review comment by @zanieb on `crates/uv/tests/sync.rs`:381 on 2024-10-08 23:16_

Here's a POC of using `subset_of`. I'm guessing this can be done in a simpler way?

https://github.com/astral-sh/uv/pull/8026

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:381 on 2024-10-09 12:39_

Good idea. I will do something like that to at least avoid these common cases.

---

_@charliermarsh reviewed on 2024-10-09 12:39_

---

_@konstin reviewed on 2024-10-09 12:51_

---

_Review comment by @konstin on `crates/uv/tests/sync.rs`:381 on 2024-10-09 12:51_

My expectation is that we should be able to represent `RequiresPython` as a single pubgrub range, extracting the range's lower or upper bound as needed; My quick attempt at that failed though because there seem other constraints too.

---

_Merged by @charliermarsh on 2024-10-09 22:24_

---

_Closed by @charliermarsh on 2024-10-09 22:24_

---

_Branch deleted on 2024-10-09 22:24_

---

_@charliermarsh reviewed on 2024-10-09 22:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/sync.rs`:381 on 2024-10-09 22:27_

I ended up writing a routine to convert to an "optimal" PEP 440 specifier using similar logic to the existing method, but for multiple bounds. It's kind of tedious but does work as expected.

---

_Comment by @charliermarsh on 2024-10-09 22:42_

I didn't mean to merge this as-is; I thought I had pushed https://github.com/astral-sh/uv/pull/8060 but apparently I'm tired.

---
