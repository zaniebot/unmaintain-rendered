```yaml
number: 16927
title: Add a check for accidental cache modifications
type: pull_request
state: open
author: konstin
labels:
  - internal
assignees: []
draft: true
base: main
head: konsti/check-cache-changes
created_at: 2025-12-02T11:05:57Z
updated_at: 2025-12-02T14:20:09Z
url: https://github.com/astral-sh/uv/pull/16927
synced_at: 2026-01-12T16:12:31Z
```

# Add a check for accidental cache modifications

---

_@konstin_

Add a script and associated CI check that checks whether a new uv build can use the cache of the previous uv version. This prevents accidental changes to cache keys, e.g. by changing nested data structures.

I've tested the script by using a previous change of https://github.com/astral-sh/uv/pull/16143.

The check can be disabled with a PR label for PRs that change the cache layout. What's missing here is that the base is the last release, meaning that once a PR with that label merges, all following PRs will fail this check, as we currently don't have a good way to ask whether there was a change previously or to download the latest build binary from main as baseline.


---

_Label `internal` added by @konstin on 2025-12-02 11:06_

---

_Comment by @zanieb on 2025-12-02 11:10_

Related https://github.com/astral-sh/uv/issues/1699

cc @BurntSushi 

---

_Comment by @konstin on 2025-12-02 11:14_

I'm taking suggestions if someone has ideas how we can solve the previously-PR-changes the cache problem.

We can check the GitHub API if any PR merged between last release and base had the cache-change label, but that sounds complex, and doesn't capture repeat changes in other PRs. Another option is storing the latest binary builds from main as nightly somewhere.

---

_Comment by @BurntSushi on 2025-12-02 14:18_

> Another option is storing the latest binary builds from main as nightly somewhere.

I kinda like this. Also independently useful.

Another idea: what about cloning current `main` in this CI job and building a `debug` binary of uv on-the-fly to test against?

---

_Comment by @konstin on 2025-12-02 14:20_

CI performance mainly, but you're right that it's a very straightforward solution.

---
