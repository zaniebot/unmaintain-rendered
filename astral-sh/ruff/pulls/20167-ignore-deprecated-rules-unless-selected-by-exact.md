```yaml
number: 20167
title: Ignore deprecated rules unless selected by exact code
type: pull_request
state: merged
author: LoicRiegel
labels:
  - breaking
  - cli
assignees: []
merged: true
base: brent/0.13.0
head: feat/ignore-deprecated-rules-by-unless-selected-by-exact-code
created_at: 2025-08-30T14:45:35Z
updated_at: 2025-09-08T16:10:11Z
url: https://github.com/astral-sh/ruff/pull/20167
synced_at: 2026-01-10T17:46:21Z
```

# Ignore deprecated rules unless selected by exact code

---

_Pull request opened by @LoicRiegel on 2025-08-30 14:45_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #18349

After this change:
- All deprecated rules are deselected by default
- They are only selected if the user specifically selects them by code, e.g. `--select UP038`
- Thus, `--select ALL --select UP --select UP0` won't select the deprecated rule UP038
- Documented the change in version policy. From now on, deprecating a rule should increase the minor version

## Test Plan

Integration tests in "integration_tests.rs"

Also tested with a temporary test package:
```
~> ../../ruff/target/debug/ruff.exe check --select UP038
warning: Rule `UP038` is deprecated and will be removed in a future release.
warning: Detected debug build without --no-cache.
UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
 --> main.py:2:11
  |
1 | def main():
2 |     print(isinstance(25, (str, int)))
  |           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
help: Convert to `X | Y`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).

~> ../../ruff/target/debug/ruff.exe check --select UP03
warning: Detected debug build without --no-cache.
All checks passed!

~> ../../ruff/target/debug/ruff.exe check --select UP0
warning: Detected debug build without --no-cache.
All checks passed!

~> ../../ruff/target/debug/ruff.exe check --select UP
warning: Detected debug build without --no-cache.
All checks passed!

~> ../../ruff/target/debug/ruff.exe check --select ALL
# warnings and errors, but because of other errors, UP038 was deselected
```

---

_Added to milestone `v0.13` by @ntBre on 2025-09-03 14:53_

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:1473 on 2025-09-03 14:56_

This is a pretty small nit, but I'd lean toward not changing any names or comments unless they're really outdated. I think this case, and `deprecated_multiple_direct` at least were okay with the old names.

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:1515 on 2025-09-03 14:58_

Similar to the above, I think the name here was okay. But the new comment makes sense to me given the changed test result.

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:1545 on 2025-09-03 15:00_

I'd kind of prefer to preserve the existing tests and add new ones rather than modify these, unless they're now redundant with other tests. Is that the case?

---

_@ntBre reviewed on 2025-09-03 15:04_

Thank you!  The code and docs changes look good to me, I just had a few nits about the tests.

---

_Label `breaking` added by @ntBre on 2025-09-03 15:05_

---

_Label `cli` added by @ntBre on 2025-09-03 15:05_

---

_Comment by @ntBre on 2025-09-03 15:07_

I also labeled this breaking, as Micha mentioned [here](https://github.com/astral-sh/ruff/issues/18349#issuecomment-2929741873). But we're preparing for a minor release next week, so we can merge this into the [release branch](https://github.com/astral-sh/ruff/pull/20194) instead of `main`. GitHub warned me that changing the base branch might remove review comments, though, so I held off for now.

---

_Comment by @github-actions[bot] on 2025-09-03 15:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/59fc8d51b8805fd9b427b67f0f1282c6a2e12ee2/rotkehlchen/externalapis/blockscout.py#L205'>rotkehlchen/externalapis/blockscout.py:205:40:</a> UP038 Use `X | Y` in `isinstance` call instead of `(X, Y)`
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




---

_Comment by @LoicRiegel on 2025-09-03 15:23_

Thank you for the review, I will make the changes now.
Do you want me to keep the commit history clean, or will you squash everything anyway?

---

_Comment by @ntBre on 2025-09-03 15:25_

Thank you! We'll squash everything when merging, so it's actually a bit easier to review if you push additional commits, in my opinion

---

_Comment by @LoicRiegel on 2025-09-03 21:41_

I just pushed three commits where
- I reverted the tests to the way they were
- Wrote comments for all tests concerned by this change so they are more precise and all consistent
- I swapped two tests to make the order consistent, I hope that's okay

Tell me if everything's alright now :)

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:1473 on 2025-09-05 14:15_

I think the `without preview` part is still accurate right? I do slightly prefer your wording otherwise, but I'd probably still leave this comment alone just to minimize the diff.

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:1532 on 2025-09-05 14:16_

Yeah looks like the preview behavior is still preserved, which I think is desired. I'd say just to revert this comment change.

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:1548 on 2025-09-05 14:19_

The old test still seems relevant to me. It shows that indirect selection in preview does _not_ produce the failure that a direct selection in preview does. I think we can revert the changes to this test.

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:1567 on 2025-09-05 14:20_

Ohh, I see. It looks like you swapped the order of these two tests. I think the existing tests were okay, we can probably just revert this too.

---

_@ntBre reviewed on 2025-09-05 14:22_

Thanks! Just a few more test suggestions to reduce the diff a bit more. I have a better understanding of the tests now and think they mostly still look relevant in their original form.

---

_@LoicRiegel reviewed on 2025-09-05 14:53_

---

_Review comment by @LoicRiegel on `crates/ruff/tests/integration_test.rs`:1473 on 2025-09-05 14:53_

Is minimizing the diff such a big goal?
In my experience, it's better to allow (even encourage) small improvements like this. This helps keep the code clean and up-to-date.
If the change is too big (like renaming a variable which is used a lot across the project), I'd want it to be a proper PR/commit but in my case, the diff is small.

That said, I understand it's important in this project, so I'll comply, no problem:)

---

_Review comment by @ntBre on `crates/ruff/tests/integration_test.rs`:1473 on 2025-09-05 15:29_

It's not such a big deal, I just lean toward preserving the original code/comments if it's not a really clear improvement. This is just a slight phrasing improvement, so I don't think it's worth adding another step to checking the git-blame in the future, for example. In other cases I'd probably agree with you :)

---

_@ntBre reviewed on 2025-09-05 15:29_

---

_@LoicRiegel reviewed on 2025-09-07 11:20_

---

_Review comment by @LoicRiegel on `crates/ruff/tests/integration_test.rs`:1567 on 2025-09-07 11:20_

reverted

---

_@LoicRiegel reviewed on 2025-09-07 11:21_

---

_Review comment by @LoicRiegel on `crates/ruff/tests/integration_test.rs`:1548 on 2025-09-07 11:21_

reverted

---

_@LoicRiegel reviewed on 2025-09-07 11:21_

---

_Review comment by @LoicRiegel on `crates/ruff/tests/integration_test.rs`:1532 on 2025-09-07 11:21_

reverted

---

_@LoicRiegel reviewed on 2025-09-07 11:21_

---

_Review comment by @LoicRiegel on `crates/ruff/tests/integration_test.rs`:1473 on 2025-09-07 11:21_

reverted

---

_Comment by @LoicRiegel on 2025-09-07 11:21_

> Thanks! Just a few more test suggestions to reduce the diff a bit more. I have a better understanding of the tests now and think they mostly still look relevant in their original form.

Done :)
Now the diff is as small as it can be :)

---

_@ntBre approved on 2025-09-08 15:58_

Thank you!

---

_Renamed from "Feat/ignore deprecated rules by unless selected by exact code" to "Ignore deprecated rules unless selected by exact code" by @ntBre on 2025-09-08 15:59_

---

_Merged by @ntBre on 2025-09-08 16:10_

---

_Closed by @ntBre on 2025-09-08 16:10_

---
