```yaml
number: 17220
title: "[`pyupgrade`] Preserve parenthesis when fixing native literals containing newlines (`UP018`)"
type: pull_request
state: merged
author: maxmynter
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: native-literals
created_at: 2025-04-05T14:27:39Z
updated_at: 2025-04-24T06:48:02Z
url: https://github.com/astral-sh/ruff/pull/17220
synced_at: 2026-01-12T15:56:00Z
```

# [`pyupgrade`] Preserve parenthesis when fixing native literals containing newlines (`UP018`)

---

_@maxmynter_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Closes #17124 

## Summary
Add a check whether a native literal contains a newline (e.g. `int(+\n1)`) in which case we must preserve the parenthesis to not break syntax.

Note: An alternative could have been stripping the newline, but I think if it's there it's there for a reason; if not other formatting rules can take care of it.


## Test Plan
Extended the existing snapshot test by the affected case.

---

_Comment by @github-actions[bot] on 2025-04-05 14:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @AlexWaygood on 2025-04-06 11:45_

---

_Label `fixes` added by @AlexWaygood on 2025-04-06 11:45_

---

_Comment by @dscorbett on 2025-04-07 12:22_

Does `arg_code.contains('\n')` work when the newline is a carriage return? For example:
```console
$ printf 'int(-\r1)\r' | ruff --isolated check - --select UP018 --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```
If not, see #15230 and #15703 for ways to detect all newlines.

---

_@dhruvmanila reviewed on 2025-04-10 20:05_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/ruff/RUF046.py`:165 on 2025-04-10 20:05_

This does not seem to have a carriage return character as newline. It might have been stripped by your either / git. You'd need to create a separate file for this test case and include it in https://github.com/astral-sh/ruff/blob/main/.gitattributes similar to how other files are included there.

---

_Renamed from "Preserve parenthesis when fixing native literals containing newlines" to "[`pyupgrade`] Preserve parenthesis when fixing native literals containing newlines (`UP018`)" by @dhruvmanila on 2025-04-10 20:06_

---

_@maxmynter reviewed on 2025-04-11 09:16_

---

_Review comment by @maxmynter on `crates/ruff_linter/resources/test/fixtures/ruff/RUF046.py`:165 on 2025-04-11 09:16_

Dang. Mus've been git or pre-commit hooks because i was aware of editor autoformatting and checked it before committing.

---

_Comment by @maxmynter on 2025-04-11 10:23_

I'm unsure how the testfiles will behave when running on different machines and I couldn't test as I have only access to windows machines. So let me know if my approach doesn't work.

For the rules `RUF046` and `UP018` i've added 2 test fixtures each; one with `cr` the other with `lr` newlines. All are persisted in `.gitattributes`. 

Alternatively, we could merge those and use `test eol=keep` but that runs at the risk of devs running into formatting errors when they edit the fixtures. 

---

_Comment by @MichaReiser on 2025-04-11 11:54_

> that runs at the risk of devs running into formatting errors when they edit the fixtures.

That's a problem that exists anyway. Not when commiting, but it means they may accidentally change the line ending locally which might be confusing when debugging a test. You can create an `.editorconfig` file to disable formatting in editors that support and respect `.editorconfig`

https://github.com/astral-sh/ruff/blob/46ab9dec181f7736a0d32be4bad59d00228c1819/crates/ruff_linter/resources/test/fixtures/pycodestyle/.editorconfig#L9-L8

---

_Comment by @maxmynter on 2025-04-12 11:28_

I added the `.editorconfig`s in [67348be](https://github.com/astral-sh/ruff/pull/17220/commits/67348be1ca770d5158181769bf969b1302336f95).

----

I think it makes sense to keep the separate files for the `cr` and `lf` cases. That way we enforce the desired line endings via `.gitattributes`, revert any unwanted formatting changes and the tests fail if functionality breaks.

With `eol=keep` we would keep accidental changes to the file. If these changes slip in, they may easily happen with the snapshot, too. 

I've had some issues getting the linebreak's to render in different editors.

What do you think? 

---

_Review requested from @dhruvmanila by @maxmynter on 2025-04-16 15:40_

---

_@dhruvmanila reviewed on 2025-04-21 19:07_

---

_Review comment by @dhruvmanila on `.gitattributes`:16 on 2025-04-21 19:07_

Do we need to include the snapshot files in here? I don't think so considering that other snapshot files aren't included and there are `.editorconfig` files present in both `ruff` and `pyupgrade` snapshot directories.

---

_@maxmynter reviewed on 2025-04-22 16:12_

---

_Review comment by @maxmynter on `.gitattributes`:16 on 2025-04-22 16:12_

I think we need them. 

I added both mechanisms for specific reasons:
-  `.editorconfig` (added in [67348be](https://github.com/astral-sh/ruff/pull/17220/commits/67348be1ca770d5158181769bf969b1302336f95)): Prevents editors from automatically reformatting the test fixtures when opened
- `.gitattributes`: Ensures Git maintains specific line endings (CR/LF) during checkout/staging irrespective of OS

This way i want to avoid (1) tests from passing incorrectly (if both files get normalized) or (2) breaking unexpectedly (if only one gets normalized).

In [5edfdc8](https://github.com/astral-sh/ruff/pull/17220/commits/5edfdc8f8c6b61d376a687a08e8d9675f3001803) I wanted to add tests for CR and LF lineendings but local normalization made tests passing as both were normalized thus not testing for CRs.

The other test cases don't seem to be sensitive to the specific line endings and should be fine as long as both fixture and snapshot are regularized on checkout by git. But fixing only the fixture, not the snapshot could make tests flaky.

Or am I missing something?

---

_@MichaReiser reviewed on 2025-04-23 06:38_

---

_Review comment by @MichaReiser on `.gitattributes`:16 on 2025-04-23 06:38_

Looking at the crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF046_RUF046_CR.py.snap snapshot, it does seem to render the `\r` differently (`^M`). That's why I think adding the snapshot here isn't needed.

---

_@maxmynter reviewed on 2025-04-23 22:20_

---

_Review comment by @maxmynter on `.gitattributes`:16 on 2025-04-23 22:20_

Allright, addressed in [005a528](https://github.com/astral-sh/ruff/pull/17220/commits/005a5288cf8426765d8249eac0c22f5435eb38f9).

---

_Review requested from @dhruvmanila by @maxmynter on 2025-04-23 22:31_

---

_Merged by @MichaReiser on 2025-04-24 06:48_

---

_Closed by @MichaReiser on 2025-04-24 06:48_

---
