```yaml
number: 6118
title: Improve display of available package ranges
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/availability-range-real
created_at: 2024-08-15T16:03:13Z
updated_at: 2024-08-15T17:28:46Z
url: https://github.com/astral-sh/uv/pull/6118
synced_at: 2026-01-10T13:09:50Z
```

# Improve display of available package ranges

---

_Pull request opened by @zanieb on 2024-08-15 16:03_

Includes the changes from https://github.com/astral-sh/uv/pull/6071 but takes them way further.

When we have the set of available versions for a package, we can do a much better job displaying an error.

For example:

```
❯ uv add 'httpx>999,<9999'
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of httpx are available:
          httpx<=999
          httpx>=9999
      and example==0.1.0 depends on httpx>999,<9999, we can conclude that example==0.1.0 cannot be used.
      And because only example==0.1.0 is available and you require example, we can conclude that the requirements are unsatisfiable.
```

The resolver has demonstrated that the requested range cannot be used because there are only versions in ranges _outside_ the requested range. However, the display of the range of available versions is pretty bad! We say there are versions of httpx available in ranges that definitely have no versions available.

With this pull request, the error becomes:

```
❯ uv add 'httpx>999,<9999'
  × No solution found when resolving dependencies:
  ╰─▶ Because only httpx<=1.0.0b0 is available and example depends on httpx>999,<9999, we can conclude that example's
      requirements are unsatisfiable.
      And because your workspace requires example, we can conclude that your workspace's requirements are unsatisfiable.
```

We achieve this by:

1. Dropping ranges disjoint with the range of available versions, e.g., this removes `httpx>=9999`
2. Replacing ranges that capture the _entire_ range of available versions with the smaller range, e.g., this replaces `httpx<=999` with `<=1.0.0b0`.

~Note that when we perform (2), we may include an additional bound that is not relevant, e.g., we include the lower bound of `>=0.6.7`. This is a bit extraneous, but I don't think it's confusing. We can consider some advanced logic to avoid that later.~ (edit: I did this, it wasn't hard)

We also improve error messages when there is _only_ one version available by showing that version instead of a range.

---

_Label `error messages` added by @zanieb on 2024-08-15 16:03_

---

_@zanieb reviewed on 2024-08-15 16:11_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:164 on 2024-08-15 16:11_

This seems wrong and like a regression?

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:1944 on 2024-08-15 16:13_

Here we know there is exactly one version available, so we show that instead of a range.

---

_@zanieb reviewed on 2024-08-15 16:13_

---

_@zanieb reviewed on 2024-08-15 16:13_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:452 on 2024-08-15 16:13_

This is the idea from https://github.com/astral-sh/uv/pull/6071, there is no `2.0.0` so we shouldn't use `>=` here.

---

_@zanieb reviewed on 2024-08-15 16:15_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:164 on 2024-08-15 16:15_

Oh oops, misread `0.1.0` as `1.0.0` — this is fine.

---

_Marked ready for review by @zanieb on 2024-08-15 16:24_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-15 16:42_

---

_@charliermarsh reviewed on 2024-08-15 16:58_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1094 on 2024-08-15 16:58_

Interesting... This _should_ have holes in it, right? But my guess is it doesn't help / doesn't matter?

---

_@charliermarsh reviewed on 2024-08-15 17:00_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1160 on 2024-08-15 17:00_

I wonder if this is quadratic, and we should instead be collecting all the new ranges and then unioning them all at once at the end? But I don't see a way to do that in the `Range` API.

---

_@charliermarsh approved on 2024-08-15 17:00_

Makes sense.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1117 on 2024-08-15 17:01_

What if the range is a singleton, and it's not in available versions?

---

_@charliermarsh reviewed on 2024-08-15 17:01_

---

_@zanieb reviewed on 2024-08-15 17:03_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1160 on 2024-08-15 17:03_

I wish. I think you can't just push segments yourself because it voids some guarantees.

---

_@zanieb reviewed on 2024-08-15 17:03_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1094 on 2024-08-15 17:03_

It basically doesn't matter in this case, we're just using it to check if the other range is completely out of bounds.

---

_@zanieb reviewed on 2024-08-15 17:04_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1094 on 2024-08-15 17:04_

Maybe I should write a note for that, maybe there are some weird cases where the holes do matter?

---

_@zanieb reviewed on 2024-08-15 17:05_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1094 on 2024-08-15 17:05_

I wrote it this way to avoid doing something like `for version in available_versions { range.contains(version) }` to see if the range contained all or none of the versions.

---

_@charliermarsh reviewed on 2024-08-15 17:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1094 on 2024-08-15 17:07_

Yeah I think this is cool.

---

_@charliermarsh reviewed on 2024-08-15 17:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1160 on 2024-08-15 17:07_

I was hoping you could pass an iterator to `union` or something.

---

_@charliermarsh reviewed on 2024-08-15 17:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1117 on 2024-08-15 17:07_

(Is that impossible?)

---

_@zanieb reviewed on 2024-08-15 17:13_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1117 on 2024-08-15 17:13_

Like this?

```
❯ uv add 'httpx==9999'
  ╰─▶ Because there is no version of httpx==9999 and example depends on httpx==9999, we can conclude that example's requirements
      are unsatisfiable.
      And because your workspace requires example, we can conclude that your workspace's requirements are unsatisfiable.
```

I think that's always handled before we get here. Otherwise, this operation is performed on the complement so I don't think it's possible.

---

_@charliermarsh reviewed on 2024-08-15 17:20_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:1117 on 2024-08-15 17:20_

Yeah like that. Ok I figured it was somehow handled upstream.

---

_@zanieb reviewed on 2024-08-15 17:21_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1094 on 2024-08-15 17:21_

Added a note, we probably wont simplify some things that we could.

---

_@zanieb reviewed on 2024-08-15 17:22_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:1160 on 2024-08-15 17:22_

I added a note. We could follow-up in PubGrub. cc @konstin 

---

_Merged by @zanieb on 2024-08-15 17:28_

---

_Closed by @zanieb on 2024-08-15 17:28_

---

_Branch deleted on 2024-08-15 17:28_

---
