```yaml
number: 12964
title: "Refactor `ExtraSpecification` to support `default-extras`"
type: pull_request
state: merged
author: blueraft
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-extra-specification
created_at: 2025-04-18T12:37:46Z
updated_at: 2025-04-28T17:30:14Z
url: https://github.com/astral-sh/uv/pull/12964
synced_at: 2026-01-10T11:10:40Z
```

# Refactor `ExtraSpecification` to support `default-extras`

---

_Pull request opened by @blueraft on 2025-04-18 12:37_

## Summary

Part of #8607. This is a pure refactor aimed at paving the way for supporting the `default-extras` configuration in the `pyproject.toml` file.

The `ExtraSpecification` struct has been refactored to align more closely with the [`DependencyGroups`](https://github.com/astral-sh/uv/blob/256b100a9e1980c75f205362c8aac95c036a96ba/crates/uv-configuration/src/dependency_groups.rs#L9) struct.

## Test Plan

Existing tests.


---

_Review requested from @Gankra by @zanieb on 2025-04-18 16:30_

---

_Review comment by @Gankra on `crates/uv-configuration/src/extras.rs`:94 on 2025-04-25 19:19_

note to self: tests got deleted here, were they replaced?

---

_Review comment by @Gankra on `crates/uv/src/commands/project/add.rs`:918 on 2025-04-25 19:23_

hm? what happened here? driveby fix?

---

_Review comment by @Gankra on `crates/uv/src/commands/project/export.rs`:121 on 2025-04-25 19:24_

presuming the followups change this 

---

_Review comment by @Gankra on `crates/uv/src/commands/project/run.rs`:552 on 2025-04-25 19:26_

copy-paste error?

---

_Review comment by @Gankra on `crates/uv-configuration/src/extras.rs`:94 on 2025-04-25 19:29_

...they were not, alas. not necessarily a deal-breaker if the followup adds tests...

---

_@Gankra approved on 2025-04-25 19:29_

This looks great, I left some notes for minor potential errors.

---

_Review comment by @blueraft on `crates/uv/src/commands/project/run.rs`:552 on 2025-04-26 11:29_

oops yes, fixed

---

_Review comment by @blueraft on `crates/uv/src/commands/project/export.rs`:121 on 2025-04-26 11:29_

yep, changed in the other PR

---

_Review comment by @blueraft on `crates/uv/src/commands/project/add.rs`:918 on 2025-04-26 11:30_

drive by fix yes, it now matches what we have in `remove.rs`.

---

_Review comment by @blueraft on `crates/uv-configuration/src/extras.rs`:94 on 2025-04-26 11:50_

uh yeah, added some integration tests in the follow up though

---

_@blueraft reviewed on 2025-04-26 11:51_

---

_@Gankra reviewed on 2025-04-28 14:11_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/add.rs`:918 on 2025-04-28 14:11_

this is my main outstanding concern, rereading to convince myself it's Correct to change this (several places explicitly don't enable defaults, although there's not much intuitive reason to it)

---

_@Gankra reviewed on 2025-04-28 14:45_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/add.rs`:918 on 2025-04-28 14:45_

Ok convinced this is a valid driveby bugfix.

---

_Comment by @Gankra on 2025-04-28 14:45_

@blueraft any outstanding concerns with the PR from you, or ready to land?

---

_Comment by @Gankra on 2025-04-28 14:45_

Alternatively I can just close the PR and we focus on the main one?

---

_Comment by @blueraft on 2025-04-28 14:47_

No concerns on my side! I think we can land this and tweak stuff in the main one

---

_Label `internal` added by @Gankra on 2025-04-28 17:30_

---

_Merged by @Gankra on 2025-04-28 17:30_

---

_Closed by @Gankra on 2025-04-28 17:30_

---
