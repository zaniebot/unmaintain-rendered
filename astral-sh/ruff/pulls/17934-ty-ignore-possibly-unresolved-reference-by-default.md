```yaml
number: 17934
title: "[ty] Ignore `possibly-unresolved-reference` by default"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/disable-possibly-unbound-reference
created_at: 2025-05-08T06:10:30Z
updated_at: 2025-05-08T15:44:58Z
url: https://github.com/astral-sh/ruff/pull/17934
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Ignore `possibly-unresolved-reference` by default

---

_Pull request opened by @MichaReiser on 2025-05-08 06:10_

## Summary

Ignore the `possibly-unbound-reference` rule by default because it is very noisy at the moment. 
We may re-enable it by default in the future.

* mdtests: The rule remains enabled in mdtests and defaults to info severity. The motivation for this is that we want to see how rules interact with each other and that's easiest when all rules are enabled
* mypy-primer: We use a user-level configuration to re-enable the rule in mypy primer. Again, the idea is that we want to see the full impact of every change.

@carljm do you want to disable all possibly- rules or only `possibly-unbound-reference`?

## Test Plan

Updated mdtests


---

_Label `configuration` added by @MichaReiser on 2025-05-08 06:10_

---

_Label `ty` added by @MichaReiser on 2025-05-08 06:10_

---

_Closed by @MichaReiser on 2025-05-08 06:17_

---

_Reopened by @MichaReiser on 2025-05-08 06:17_

---

_Renamed from "[ty] Disable `possibly-unbound-reference` by default" to "[ty] Ignore `possibly-unbound-reference` by default" by @MichaReiser on 2025-05-08 06:20_

---

_Comment by @github-actions[bot] on 2025-05-08 06:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "[ty] Ignore `possibly-unbound-reference` by default" to "[ty] Ignore `possibly-unresolved-reference` by default" by @MichaReiser on 2025-05-08 06:26_

---

_Closed by @MichaReiser on 2025-05-08 06:48_

---

_Reopened by @MichaReiser on 2025-05-08 06:48_

---

_Marked ready for review by @MichaReiser on 2025-05-08 06:58_

---

_Review requested from @carljm by @MichaReiser on 2025-05-08 06:58_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-08 06:58_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-08 06:58_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-08 06:58_

---

_@sharkdp reviewed on 2025-05-08 07:09_

---

_Review comment by @sharkdp on `crates/ty/tests/cli.rs`:376 on 2025-05-08 07:09_

I think this change is wrong/confusing? I would think this should be:
```suggestion
    // Assert that there's a unresolved-reference diagnostic and that
    // division-by-zero has a severity of error by default.
```
The actual test where we change the severities is below.

---

_@sharkdp reviewed on 2025-05-08 07:11_

---

_Review comment by @sharkdp on `crates/ty/tests/cli.rs`:455 on 2025-05-08 07:11_

Similar here, this change looks wrong? It should refer to the first snapshot assertion, not the second-

---

_@sharkdp approved on 2025-05-08 07:12_

Thank you!

---

_Comment by @carljm on 2025-05-08 14:42_

Thank you!

I think we should only disable `possibly-unresolved-reference` for now. The patterns where this causes a false positive are much less common in e.g. a class or module body; all of the possibly-unbound false positives I've seen are `possibly-unresolved-reference`.

---

_Merged by @MichaReiser on 2025-05-08 15:44_

---

_Closed by @MichaReiser on 2025-05-08 15:44_

---

_Branch deleted on 2025-05-08 15:44_

---
