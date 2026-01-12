```yaml
number: 5736
title: Respect pre-release preferences from input files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pres
created_at: 2024-08-02T20:35:22Z
updated_at: 2024-08-03T02:01:59Z
url: https://github.com/astral-sh/uv/pull/5736
synced_at: 2026-01-12T16:06:59Z
```

# Respect pre-release preferences from input files

---

_@charliermarsh_

## Summary

Right now, if you have a `requirements.txt` with a pre-release, but the `requirements.in` does not have a pre-release marker for that dependency we drop the pre-release. (In the selector, we end up returning `AllowPrerelease::IfNecessary`, the default.)

I played with a few ways of solving this... The first was to remove that guard altogether. But if we do that, `universal_transitive_disjoint_prerelease_requirement` fails (we use `1.17.0rc1` in both forks, when it should only apply to one of the two).

The second was to do that, but also avoid pushing pre-releases as preferences when we solve a fork. But then `universal_disjoint_prereleases` fails, because we return a different pre-release in each fork.

Finally, I settled on allowing existing pre-releases in forks if they have no markers on them, i.e., they are "global" preferences. I believe this is true IFF the preference came from an existing lockfile.

Closes https://github.com/astral-sh/uv/issues/5729.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-02 20:35_

---

_Label `bug` added by @charliermarsh on 2024-08-02 20:35_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:7644 on 2024-08-02 20:35_

This result is slightly wrong, but... not disastrously so.

---

_@charliermarsh reviewed on 2024-08-02 20:35_

---

_Marked ready for review by @charliermarsh on 2024-08-02 20:35_

---

_@ibraheemdev approved on 2024-08-03 01:17_

This seems like a reasonable approach.

---

_Merged by @charliermarsh on 2024-08-03 02:01_

---

_Closed by @charliermarsh on 2024-08-03 02:01_

---

_Branch deleted on 2024-08-03 02:01_

---
