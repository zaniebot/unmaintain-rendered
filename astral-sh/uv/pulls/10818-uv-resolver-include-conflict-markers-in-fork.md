```yaml
number: 10818
title: "uv-resolver: include conflict markers in fork markers"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/overlapping-marker
created_at: 2025-01-21T16:50:49Z
updated_at: 2025-01-21T19:04:36Z
url: https://github.com/astral-sh/uv/pull/10818
synced_at: 2026-01-10T11:45:12Z
```

# uv-resolver: include conflict markers in fork markers

---

_Pull request opened by @BurntSushi on 2025-01-21 16:50_

When support for conflicting extras/groups was initially added, I
stopped short of including the conflict markers in uv's "fork markers"
in the lock file. That is, the fork markers are markers that indicate
the different splits uv took during resolution, which we record, I
believe, to avoid spurious updates to the lock file as a result of
using them as preferences.

One interesting result of omitting the conflict markers from the fork
markers is that sometimes this would result in duplicate markers. In
response, I wrote a function that stripped off the conflict markers and
deduplicated the remainder. My thinking at the time was that it wasn't
clear whether we needed to keep conflict markers around.

It looks like #10783 demonstrates a case where we do, seemingly, need
them. Namely, it's a case where after stripping conflict markers, you
don't end up with duplicate markers, but you do end up with overlapping
markers. Overlapping fork markers are bad juju for the same reason that
overlapping resolver forks are bad juju: you can end up with multiple
versions of the same package in the same environment.

I don't know how to fix overlapping markers without just including the
conflict markers. So that's what this PR does. Because of this, there
will be some churn in lock files, but this only applies to projects that
define conflicting extras.

This PR includes a regression test from #10783. I also manually tried
the original reproduction in #10772 (where adding `numpy<2` caused `uv
sync` to fail), and things worked.

Fixes #10772, Fixes #10783


---

_Comment by @charliermarsh on 2025-01-21 17:28_

Could we possibly avoid this in some cases, even when conflicting extras are defined? Like, what if the markers aren't overlapping?

---

_Comment by @BurntSushi on 2025-01-21 17:33_

Yeah I maybe I could modify this to collect only the PEP 508 markers and then only include the conflict portions if _any_ of the PEP 508 markers are not disjoint with one another.

There's definitely another aspect of this which is whether the conflict markers are necessary to avoid the problems that `resolution-markers` were created to mitigate in the first place. But I'm pretty unsure about that.

---

_Comment by @charliermarsh on 2025-01-21 17:45_

I would be curious if that change reduces the churn at all!

---

_Comment by @BurntSushi on 2025-01-21 18:46_

Yeah it definitely reduces churn! It looks like we did have one existing test case that had overlapping markers (but was very easy to miss) that is also fixed too.

---

_Review requested from @charliermarsh by @BurntSushi on 2025-01-21 19:01_

---

_@charliermarsh approved on 2025-01-21 19:01_

---

_Merged by @BurntSushi on 2025-01-21 19:04_

---

_Closed by @BurntSushi on 2025-01-21 19:04_

---

_Branch deleted on 2025-01-21 19:04_

---

_Label `bug` added by @BurntSushi on 2025-01-21 19:04_

---
