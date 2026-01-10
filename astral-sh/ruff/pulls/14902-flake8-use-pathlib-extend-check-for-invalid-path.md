```yaml
number: 14902
title: "[`flake8-use-pathlib`] Extend check for invalid path suffix to include the case `\".\"` (`PTH210`)"
type: pull_request
state: merged
author: TheBits
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: main
created_at: 2024-12-10T23:33:32Z
updated_at: 2024-12-12T19:30:18Z
url: https://github.com/astral-sh/ruff/pull/14902
synced_at: 2026-01-10T20:42:27Z
```

# [`flake8-use-pathlib`] Extend check for invalid path suffix to include the case `"."` (`PTH210`)

---

_Pull request opened by @TheBits on 2024-12-10 23:33_

## Summary

Hi!

Thank you @InSyncWithFoo for the your contribution #14779!

I suggest adding a test that makes use of the `suffix='.'`. It must fail. Right now, ruff doesn't show any errors in this test.

## Test Plan

Tests fails


---

_Label `bug` added by @dylwil3 on 2024-12-10 23:41_

---

_Label `rule` added by @dylwil3 on 2024-12-10 23:41_

---

_Label `preview` added by @dylwil3 on 2024-12-10 23:41_

---

_Comment by @dylwil3 on 2024-12-10 23:43_

Thanks for suggesting this test and pointing out the bug! Of course, since it fails we can't merge it in without also having the fix 😄 Would you be interested in taking a look and fleshing out your PR? I believe this early return needs to be modified:

https://github.com/astral-sh/ruff/blob/d4126f6049721d3c78591b6a1c1a78f4b4081329/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs#L78-L80

Edit: Actually the change will be a _little_ more involved because I don't think it makes sense to offer a fix in this case, so you'll have to change the rule to be only _sometimes_ fixable. Let me know if you're interested and/or need any pointers navigating that change.

---

_Comment by @InSyncWithFoo on 2024-12-11 01:59_

The rule, which is currently called <code><em>dotless</em>-pathlib-with-suffix</code>, might also need to be renamed. Its documentation requires an update as well.

---

_Review requested from @MichaReiser by @TheBits on 2024-12-11 02:09_

---

_Review requested from @dhruvmanila by @TheBits on 2024-12-11 02:09_

---

_Comment by @github-actions[bot] on 2024-12-11 02:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @TheBits on 2024-12-11 02:20_

@dylwil3 Yes, I need help. I tried to figure it out and fix the `dotless_pathlib_with_suffix`. Have I made the appropriate corrections?

---

_Comment by @MichaReiser on 2024-12-11 07:41_

> The rule, which is currently called `_dotless_-pathlib-with-suffix`, might also need to be renamed. Its documentation requires an update as well.

Agree, we should probably rename it to `invalid-pathlib-with-suffix` or similar.

---

_@MichaReiser reviewed on 2024-12-11 07:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:59 on 2024-12-11 07:42_

We need to update the fix title for the `.` case because the fix is always shown as help text and *add a leading dot* is misleading for `.`

---

_@MichaReiser reviewed on 2024-12-11 07:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:102 on 2024-12-11 07:44_

I'd prefer avoiding `try_set_fix` when the check when to set a diagnostic is fairly trivial:

```suggestion
		if string_value != "." {
      let after_leading_quote = string.start() + first_part.flags.opener_len();
	    diagnostic.set_fix(Fix::unsafe_edit(Edit::insertion(".".to_string(), after_leading_quote)));
    }
```

---

_@dylwil3 requested changes on 2024-12-11 12:38_

Looking good! In addition to Micha's suggestions, could you update the documentation comment?

---

_Comment by @dylwil3 on 2024-12-11 12:47_

We may want to add a TODO comment for when Python 3.14 is released/supported by Ruff. The rule should revert to the original behavior in that case since [`"."` is a valid suffix in 3.14](https://docs.python.org/3.14/library/pathlib.html#pathlib.PurePath.suffix).

---

_Comment by @TheBits on 2024-12-11 13:31_

> Looking good! In addition to Micha's suggestions, could you update the documentation comment?

Thanks! I will try to update documentation comment.

---

_@TheBits reviewed on 2024-12-11 13:50_

---

_Review comment by @TheBits on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:102 on 2024-12-11 13:50_

Thank you. This is helpful.

Yesterday I spent time to write this part of the function in rust. I had not understood the semantic of `try_to_fix`. I thought it was necessary to specify an `Err` if there is no fix.

---

_Review requested from @carljm by @TheBits on 2024-12-11 15:33_

---

_Review requested from @AlexWaygood by @TheBits on 2024-12-11 15:33_

---

_Review requested from @sharkdp by @TheBits on 2024-12-11 15:33_

---

_Review request for @carljm removed by @AlexWaygood on 2024-12-11 15:37_

---

_Review request for @sharkdp removed by @AlexWaygood on 2024-12-11 15:37_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-12-11 15:37_

---

_Review request for @dhruvmanila removed by @AlexWaygood on 2024-12-11 15:37_

---

_Review requested from @dylwil3 by @TheBits on 2024-12-11 15:45_

---

_Review requested from @MichaReiser by @TheBits on 2024-12-11 15:45_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/invalid_pathlib_with_suffix.rs`:54 on 2024-12-11 19:24_

```suggestion
    // TODO: Since "." is a correct suffix in Python 3.14,
    // we will need to update this rule and documentation
    // once Ruff supports Python 3.14.
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/invalid_pathlib_with_suffix.rs`:68 on 2024-12-11 19:27_

```suggestion
            "Remove \".\" or extend to valid suffix"
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/invalid_pathlib_with_suffix.rs`:63 on 2024-12-11 19:31_

The original message is more helpful in the majority of cases.

```suggestion
        if !self.single_dot {"Dotless suffix passed to `.with_suffix()`".to_string()} else {
            "Invalid suffix passed to `.with_suffix()`".to_string()
         }
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/invalid_pathlib_with_suffix.rs`:41 on 2024-12-11 19:34_

I think you can just move this inside the explanation of fix safety (maybe at the end), and say

```rust
/// No fix is offered if the suffix `"."` is given, since the intent is unclear.
```

---

_@dylwil3 requested changes on 2024-12-11 19:37_

Few more nits - almost there!

---

_@TheBits reviewed on 2024-12-12 12:01_

---

_Review comment by @TheBits on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/invalid_pathlib_with_suffix.rs`:54 on 2024-12-12 12:01_

Is there a way to get the target-version settings inside a rule handler?

---

_@TheBits reviewed on 2024-12-12 12:07_

---

_Review comment by @TheBits on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/invalid_pathlib_with_suffix.rs`:68 on 2024-12-12 12:07_

Thanks!

---

_Review requested from @dylwil3 by @MichaReiser on 2024-12-12 13:35_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/invalid_pathlib_with_suffix.rs`:54 on 2024-12-12 19:15_

Yep, you'd use `checker.settings.target_version`, which spits out a `PythonVersion` enum member. But that enum doesn't yet contain `Py314`.

If you wanted you could use the condition `if checker.settings.target_version > PythonVersion::Py313` and write the code now, but I'd rather leave this as a TODO since who knows what other changes might be made (or reverted) by the time 3.14 is released.

---

_@dylwil3 reviewed on 2024-12-12 19:15_

---

_@dylwil3 approved on 2024-12-12 19:21_

LGTM! Thanks so much for your patience, and for the great contribution!

---

_Renamed from "[`flake8-use-pathlib`] The Path.with_suffix('.') test should fail (`PTH210`)" to "[`flake8-use-pathlib`] Extend check for invalid path suffix to include the case `"."` (`PTH210`)" by @dylwil3 on 2024-12-12 19:22_

---

_Merged by @dylwil3 on 2024-12-12 19:30_

---

_Closed by @dylwil3 on 2024-12-12 19:30_

---
