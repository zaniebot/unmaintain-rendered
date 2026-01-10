```yaml
number: 709
title: "Refactor `DistFinder` to allow handling errors"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/dist-finder-error
created_at: 2023-12-19T20:32:41Z
updated_at: 2023-12-20T04:07:56Z
url: https://github.com/astral-sh/uv/pull/709
synced_at: 2026-01-10T15:44:44Z
```

# Refactor `DistFinder` to allow handling errors

---

_Pull request opened by @konstin on 2023-12-19 20:32_

For the install tests, i need the ability to ignore failures in the `DistFinder`. To avoid just copy&pasting a version that collects errors separately, i followed https://gendignoux.com/blog/2021/04/01/rust-async-streams-futures-part1.html and switched the custom channel over to an async stream yielding `Result` items.

I like the async streams mirror the normal iterator api.

---

_Review requested from @charliermarsh by @konstin on 2023-12-19 20:32_

---

_@konstin reviewed on 2023-12-19 20:33_

---

_Review comment by @konstin on `crates/puffin-resolver/src/finder.rs`:97 on 2023-12-19 20:33_

I've removed the pre-allocation, the stream size hint should be sufficient.

---

_@konstin reviewed on 2023-12-19 20:34_

---

_Review comment by @konstin on `crates/puffin-resolver/src/finder.rs`:94 on 2023-12-19 20:34_

Do we still need this shortcut after the refactoring?

---

_Review comment by @konstin on `crates/puffin-resolver/src/finder.rs`:87 on 2023-12-19 20:35_

I've removed the chunks here, do we need them specifically when the stream is already parallel?

---

_@konstin reviewed on 2023-12-19 20:35_

---

_Comment by @charliermarsh on 2023-12-19 20:44_

I'm trying to see the bigger picture -- how does this enable ignoring failures? And what are those failures that need to be ignored?

---

_Comment by @konstin on 2023-12-19 20:55_

It is similar to the current resolve-many test, that only logs and counts failures in resolutions, but doesn't terminate on the first resolution failure like pip-compile does.

For the install-many test, let's say i have 10k packages i want to install, but 1k of those don't have an installation candidate in my venv for whatever reason - Many reasons are fine, e.g. windows only packages or those that don't support the active python version. I want to query the 10k requirements in parallel and have an adapter on the stream that collects the 9k that works and logs that 1k didn't work. This is different from the main pip-sync mode, where we want to fail and report once we see the first failure (fail fast). The new `Fn(&[Requirement]) -> impl Stream<Item = Result<(PackageName, Dist), ResolveError>>` allows both with minimal overhead.

It's also a +42 −85 with (imho) readable benefits so i'm happy about changing this by itself.

---

_Comment by @charliermarsh on 2023-12-19 21:06_

👍 Sounds good, I will review this later tonight and merge as appropriate!

---

_@charliermarsh approved on 2023-12-20 04:02_

---

_Merged by @charliermarsh on 2023-12-20 04:07_

---

_Closed by @charliermarsh on 2023-12-20 04:07_

---

_Branch deleted on 2023-12-20 04:07_

---
