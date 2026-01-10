```yaml
number: 19156
title: "[`pyupgrade`] Remove `non-pep604-isinstance` (`UP038`)"
type: pull_request
state: merged
author: CodeMan62
labels:
  - rule
  - breaking
assignees: []
merged: true
base: brent/0.13.0
head: remove-up038
created_at: 2025-07-05T17:45:25Z
updated_at: 2025-09-08T17:34:40Z
url: https://github.com/astral-sh/ruff/pull/19156
synced_at: 2026-01-10T17:46:21Z
```

# [`pyupgrade`] Remove `non-pep604-isinstance` (`UP038`)

---

_Pull request opened by @CodeMan62 on 2025-07-05 17:45_

## Summary
This PR Removes deprecated UP038 as per instructed in #18727 
closes #18727 
## Test Plan
I have run tests non of them failing 

One Question i have is do we have to document that UP038 is removed?  


---

_Review requested from @AlexWaygood by @CodeMan62 on 2025-07-05 17:45_

---

_Label `breaking` added by @AlexWaygood on 2025-07-05 17:46_

---

_Label `do-not-merge` added by @AlexWaygood on 2025-07-05 17:46_

---

_Comment by @AlexWaygood on 2025-07-05 17:47_

Thanks. Unfortunately we won't be able to merge this until the next minor release (v0.13), as it's a breaking change.

---

_Added to milestone `v0.13` by @AlexWaygood on 2025-07-05 17:47_

---

_Review comment by @AlexWaygood on `changelogs/0.1.x.md`:93 on 2025-07-05 17:49_

we shouldn't be removing mentions of the rule from old changelogs. That's a useful record of the changes that were made in previous versions; just because the rule is being removed doesn't mean we should try to erase history :-)

---

_@AlexWaygood reviewed on 2025-07-05 17:49_

---

_Comment by @CodeMan62 on 2025-07-05 17:52_

no worries Thanks for letting me know I will fix these failing CI 

---

_@CodeMan62 reviewed on 2025-07-05 17:53_

---

_Review comment by @CodeMan62 on `changelogs/0.1.x.md`:93 on 2025-07-05 17:53_

Ok I will revert this. thanks 

---

_Comment by @github-actions[bot] on 2025-07-05 18:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/2fdda4d3a7507b13f4c6d994d789a2da173349cd/rotkehlchen/externalapis/blockscout.py#L205'>rotkehlchen/externalapis/blockscout.py:205:40:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP038 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood requested changes on 2025-07-07 11:56_

Rather than removing the rule implementation at all, I think we just want to change the category of the rule to `Removed`, similar to the changes we made to `S320` in https://github.com/astral-sh/ruff/pull/18617. That way it doesn't break the configuration setup of lots of projects. (We can see the effect it currently has from the ecosystem report)

---

_Comment by @CodeMan62 on 2025-07-07 15:56_

sounds good me :+1:  doing it 
EDIT:- DONE

---

_Review comment by @AlexWaygood on `changelogs/0.1.x.md`:1 on 2025-07-07 16:45_

please revert all changes to this file

---

_Review comment by @AlexWaygood on `changelogs/0.10.x.md`:1 on 2025-07-07 16:45_

please revert all changes to this file

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP038.py`:1 on 2025-07-07 16:46_

please revert all changes to this file

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP038.py.snap`:1 on 2025-07-07 16:46_

please revert all changes to this file

---

_@AlexWaygood had review dismissed on 2025-07-07 16:46_

---

_Comment by @AlexWaygood on 2025-07-07 16:52_

Wait, the changes you previously had applied to `python/py-fuzzer/pyproject.toml` looked correct to me 😆 I don't think https://github.com/astral-sh/ruff/pull/19156/commits/2daaf98678463df141a115d577f98364b8e7bcbb was something you needed to do

---

_Comment by @CodeMan62 on 2025-07-07 16:56_

give me 2 mins i am reverting snapshots too i need to do that locally 

---

_@AlexWaygood reviewed on 2025-07-07 17:16_

---

_Review comment by @AlexWaygood on `python/py-fuzzer/pyproject.toml`:66 on 2025-07-07 17:16_

```suggestion
```

---

_@CodeMan62 reviewed on 2025-07-07 17:18_

---

_Review comment by @CodeMan62 on `python/py-fuzzer/pyproject.toml`:66 on 2025-07-07 17:18_

thanks

---

_Comment by @CodeMan62 on 2025-07-07 17:20_

Thanks! I made alot of mistakes here but i am unable to discard snapshot changes 

---

_Comment by @AlexWaygood on 2025-07-08 18:33_

This looks good to me now, but we'll still need to wait for the v0.13 release before merging it :-)

---

_Converted to draft by @dhruvmanila on 2025-07-10 06:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_isinstance.rs`:37 on 2025-09-05 13:46_

```suggestion
/// This rule was removed as using [PEP 604] syntax in `isinstance` and `issubclass` calls
```

---

_@ntBre approved on 2025-09-05 13:47_

---

_Marked ready for review by @ntBre on 2025-09-05 13:48_

---

_Renamed from "Remove deprecated UP038" to "[`pyupgrade`] Remove `non-pep604-isinstance` (`UP038`)" by @ntBre on 2025-09-05 13:58_

---

_Merged by @ntBre on 2025-09-05 14:08_

---

_Closed by @ntBre on 2025-09-05 14:08_

---

_Label `do-not-merge` removed by @ntBre on 2025-09-08 17:34_

---

_Label `rule` added by @ntBre on 2025-09-08 17:34_

---
