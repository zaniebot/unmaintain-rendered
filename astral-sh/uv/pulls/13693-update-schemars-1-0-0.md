```yaml
number: 13693
title: Update schemars 1.0.0
type: pull_request
state: merged
author: ya7010
labels:
  - enhancement
assignees: []
merged: true
base: main
head: update_schemars
created_at: 2025-05-28T05:28:24Z
updated_at: 2025-06-24T19:43:32Z
url: https://github.com/astral-sh/uv/pull/13693
synced_at: 2026-01-10T11:10:42Z
```

# Update schemars 1.0.0

---

_Pull request opened by @ya7010 on 2025-05-28 05:28_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Update [schemars 0.9.0](https://github.com/GREsau/schemars/releases/tag/v0.9.0)

There are differences in the generated JSON Schema and I will [contact the author](https://github.com/GREsau/schemars/issues/407).

## Test Plan


---

_@konstin reviewed on 2025-05-28 08:13_

---

_Review comment by @konstin on `uv.schema.json`:2545 on 2025-05-28 08:13_

This is also a regression in the new version

---

_Comment by @konstin on 2025-05-28 08:42_

Thanks! We can merge the PR when upstream fixed both regressions.

---

_@konstin reviewed on 2025-05-28 09:23_

---

_Review comment by @konstin on `crates/uv-small-str/src/lib.rs`:151 on 2025-05-28 09:23_

These were intentionally `string`, what type we're using for this string internally is an implementation detail and should not be exposed. 

---

_@ya7010 reviewed on 2025-05-28 09:24_

---

_Review comment by @ya7010 on `uv.schema.json`:2545 on 2025-05-28 09:24_

da3b91b1f69108836da0eee9b70f52860825b8e2

In fact, this is the result of auto-generating a class that wraps a string type, but I avoided this by changing the name to a specific name.

---

_@ya7010 reviewed on 2025-05-28 09:30_

---

_Review comment by @ya7010 on `crates/uv-small-str/src/lib.rs`:151 on 2025-05-28 09:30_

Changed to inline.

https://github.com/astral-sh/uv/pull/13693/commits/f2a71fe2d5f084bddab70516c0343d78cb9bf423

---

_Review requested from @konstin by @ya7010 on 2025-05-28 09:51_

---

_Assigned to @zanieb by @zanieb on 2025-05-28 14:26_

---

_Unassigned @zanieb by @zanieb on 2025-05-28 14:26_

---

_Assigned to @konstin by @zanieb on 2025-05-28 14:27_

---

_Comment by @konstin on 2025-05-28 14:36_

Putting this into draft mode until https://github.com/GREsau/schemars/issues/407 is fixed.

---

_Converted to draft by @konstin on 2025-05-28 14:36_

---

_Comment by @ya7010 on 2025-05-29 23:10_

@konstin 

This modification was found to be intentional (https://github.com/GREsau/schemars/issues/120).


---

_Marked ready for review by @ya7010 on 2025-05-29 23:10_

---

_Comment by @konstin on 2025-05-30 07:04_

I've added a specific example of the regressed indents to https://github.com/GREsau/schemars/issues/407#issuecomment-2921417766

---

_Comment by @konstin on 2025-06-18 16:11_

Updated to 1.0.0-rc.1 and forced pushed a rebase onto main, this is now blocked on https://github.com/GREsau/schemars/issues/433

---

_Converted to draft by @konstin on 2025-06-18 16:11_

---

_@konstin requested changes on 2025-06-18 16:19_

Blocked on upstream solving: https://github.com/GREsau/schemars/issues/433

---

_Renamed from "Update schemars 0.9.0" to "Update schemars 1.0.0" by @ya7010 on 2025-06-24 11:29_

---

_Marked ready for review by @ya7010 on 2025-06-24 11:34_

---

_Review requested from @konstin by @ya7010 on 2025-06-24 11:35_

---

_Comment by @ya7010 on 2025-06-24 11:37_

schemars [v1.0.0](https://github.com/GREsau/schemars/releases/tag/v1.0.0) has been released 🎉 

Adjusted so that v1.0.0 can be built.

📝  The handling of line breaks in docstrings has changed, resulting in changes to uv.schema.json.
This requires adjustments to the doc-string of uv source code.

---

_@konstin approved on 2025-06-24 12:47_

Thanks!

---

_Label `enhancement` added by @konstin on 2025-06-24 12:49_

---

_Merged by @konstin on 2025-06-24 19:43_

---

_Closed by @konstin on 2025-06-24 19:43_

---
