```yaml
number: 7248
title: Implement bump mechanism (mostly ported from rye)
type: pull_request
state: closed
author: nrbnlulu
labels:
  - enhancement
assignees: []
base: main
head: nir/bump-version
created_at: 2024-09-10T09:15:11Z
updated_at: 2025-03-20T21:55:13Z
url: https://github.com/astral-sh/uv/pull/7248
synced_at: 2026-01-10T11:10:34Z
```

# Implement bump mechanism (mostly ported from rye)

---

_Pull request opened by @nrbnlulu on 2024-09-10 09:15_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Implement bump mechanism following `major.minor.patch` format.
fix #6298

**I chose the bump name since version is already used though I would consider removing the current `version` handler for this**
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Simple idiomatic tests modifying the test pyprojec and checking the output statically

<!-- How was it tested? -->


---

_Marked ready for review by @nrbnlulu on 2024-09-11 10:52_

---

_Comment by @nrbnlulu on 2024-09-11 14:58_

windows test is failing though it seems more like a flaky test rather than something related to my changes... 

---

_Review requested from @zanieb by @charliermarsh on 2024-09-11 20:38_

---

_Assigned to @zanieb by @charliermarsh on 2024-09-11 20:38_

---

_Label `enhancement` added by @charliermarsh on 2024-09-11 20:38_

---

_Review comment by @nrbnlulu on `crates/uv/src/commands/project/bump.rs`:16 on 2024-09-12 09:12_

```suggestion
/// Bump set or view project version
```

---

_@nrbnlulu reviewed on 2024-09-12 09:12_

---

_Comment by @zanieb on 2024-09-15 15:14_

I'll review this. We'll need to consider what version management interface we want in the long run.

---

_Comment by @notypecheck on 2024-09-19 07:49_

PEP440 https://peps.python.org/pep-0440/
and commitizen config https://commitizen-tools.github.io/commitizen/commands/bump/#version_scheme
could be good reference points

---

_Comment by @zanieb on 2024-10-06 14:43_

I still haven't had time to decide if we want to replace `uv version` for this.

---

_Comment by @nrbnlulu on 2024-10-13 12:33_

Is this PR wanted? should I resolve conflicts?

---

_Comment by @n4z4m3 on 2024-11-11 01:54_

> Is this PR wanted? should I resolve conflicts?

I would find this useful, but I see there has been no action on this for a while. Currently I use bump-my-version to do this, but it would be great to have it built into uv.


---

_Comment by @nrbnlulu on 2024-11-11 09:57_

yeah. the maintainers said they want to inspect other project \ alternative interfaces to have better context on this feature.
if someone is interested doing the research it would be cool. 

---

_Comment by @mthipparthi on 2024-11-27 23:42_

This is great feature and removes the dependency on bump version. This one along with change-log generation makes this tool complete.

---

_Comment by @zanieb on 2024-11-27 23:51_

I think about this pretty often, but I'm still not quite sure what to do, e.g., `cargo version` is not used for this purpose. 

---

_Comment by @Ephil012 on 2025-01-12 23:26_

Out of curiosity, is there any timeline for this potentially getting merged? This is one of the main features I am missing from poetry

---

_Comment by @Gankra on 2025-03-20 21:55_

I've absorbed this work into #12349, thanks for all the work on this!

---

_Closed by @Gankra on 2025-03-20 21:55_

---
