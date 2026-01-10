```yaml
number: 17340
title: "Add `Checker::import_from_typing`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/refactor-typing-imports
created_at: 2025-04-10T18:18:56Z
updated_at: 2025-04-14T15:52:59Z
url: https://github.com/astral-sh/ruff/pull/17340
synced_at: 2026-01-10T19:40:37Z
```

# Add `Checker::import_from_typing`

---

_Pull request opened by @ntBre on 2025-04-10 18:18_

Summary
--

This PR replaces uses of version-dependent imports from `typing` or `typing_extensions` with a centralized `Checker::import_from_typing` method.

The idea here is to make the fix for #9761 (whatever it ends up being) applicable to all of the rules performing similar checks.

I found these with a pretty naive grep for `"typing_extensions"` in the `ruff_linter/src/rules` directory, so if there are any rules I missed, please let me know. @AlexWaygood mentioned "lots of rules" [here](https://github.com/astral-sh/ruff/issues/9761#issuecomment-2790492853), so I think this is likely.

Test Plan
--

Existing tests for FAST002, PYI019, and PYI034.


---

_Label `internal` added by @ntBre on 2025-04-10 18:18_

---

_Comment by @github-actions[bot] on 2025-04-10 18:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ntBre on 2025-04-10 18:25_

---

_Review requested from @AlexWaygood by @ntBre on 2025-04-10 18:25_

---

_Comment by @dhruvmanila on 2025-04-10 19:00_

> This PR replaces uses of version-dependent imports from `typing` or `typing_extensions` with a centralized `Checker::import_from_typing` method.

Is the goal to replace all edit generation from `typing` module to be using this method? I think that makes sense as it would then be easier to find all usages of it via this method.

> I found these with a pretty naive grep for `"typing_extensions"` in the `ruff_linter/src/rules` directory, so if there are any rules I missed, please let me know.

You can go through the references of `get_or_import_symbol` which should have some of the symbols that are being imported from `typing`.

For example,

https://github.com/astral-sh/ruff/blob/410aa4b8fdce3ad0e8c32c8801a4bbe7df0d632f/crates/ruff_linter/src/rules/flake8_annotations/helpers.rs#L129-L142

---

_Comment by @ntBre on 2025-04-10 19:10_

> Is the goal to replace all edit generation from `typing` module to be using this method? I think that makes sense as it would then be easier to find all usages of it via this method.

My goal was just to replace imports that might come from `typing` or `typing_extensions`, but that's a good idea.
This could catch uses that should have considered checking the version and using `typing_extensions` but didn't.
 
> You can go through the references of `get_or_import_symbol` which should have some of the symbols that are being imported from `typing`.
> 
> For example,
> 
> https://github.com/astral-sh/ruff/blob/410aa4b8fdce3ad0e8c32c8801a4bbe7df0d632f/crates/ruff_linter/src/rules/flake8_annotations/helpers.rs#L129-L142

Yeah this is a good example, it looks like these are both in `typing_extensions`, so this could check both `Never` vs `NoReturn` and also the module.

The one downside of replacing all `typing` imports is that it will make the false positive issue from #9761 worse, i.e. affecting more rules, but only briefly, I plan to work on that after this.


---

_Comment by @ntBre on 2025-04-10 20:02_

I went through all of the references for `get_or_import_symbol` and think I've handled all of the `typing` imports. 

`PYI058` looks a lot like it needs the new method, but it does its own bookkeeping on whether `typing`, `typing_extensions`, or `collections.abc` is already imported and reuses whichever one is already present, so I think it's okay to leave alone.

https://github.com/astral-sh/ruff/blob/8b2727cf67ef4ed43723ff5c11ea936838e25c26/crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs#L128-L142

I believe the same is true for `UP006`, which ends up in this function:

https://github.com/astral-sh/ruff/blob/8b2727cf67ef4ed43723ff5c11ea936838e25c26/crates/ruff_python_stdlib/src/typing.rs#L340-L355

I actually accepted new snapshot results for `PYI026`, which was previously using `ImportRequest::import` instead of `ImportRequest::import_from`. It seems to be the only one doing that, and the example in the [docs](https://docs.astral.sh/ruff/rules/type-alias-without-annotation/) also uses `ImportFrom`, so hopefully this doesn't need to be preview-gated.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:134 on 2025-04-10 20:24_

Is this the lowest Python version? The `NoReturn` seems to have been introduced in 3.6 (https://docs.python.org/3/library/typing.html#typing.NoReturn). If so, we could introduce a `PythonVersion::lowest` method. What do you think?

(Same comment for other places using this pattern.)

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:540 on 2025-04-10 20:28_

Should this be a method on the `Importer` instead? This is, I think, the main interface to generate import edits:

https://github.com/astral-sh/ruff/blob/172af7b4b0b2864681d607cd1ad36f9d3ec256b5/crates/ruff_linter/src/checkers/ast/mod.rs#L434-L437

---

_@dhruvmanila approved on 2025-04-10 20:28_

---

_@ntBre reviewed on 2025-04-10 20:58_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:540 on 2025-04-10 20:58_

It could be, but we get the Python version from `Checker` and also the `SemanticModel` for `Importer::get_or_import_symbol`, so we'd have to pass a couple more arguments if it were an `Importer` method.

I also think the `Checker` will be more likely to contain any additional information we need to fix #9761, such as the `LinterSettings`, if we add a configuration option.

---

_@ntBre reviewed on 2025-04-10 21:01_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:134 on 2025-04-10 21:01_

Yeah I wondered about that. I think 3.8 is our lowest supported version? That was the motivation for picking it. I like the idea of a `lowest` method to make that more explicit.

With this `PythonVersion` representation we could define `lowest` as something exotic like 0.0, but I'll go with 3.8 in the off chance it appears in an error or log message.

---

_@dhruvmanila reviewed on 2025-04-10 21:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/ast/mod.rs`:540 on 2025-04-10 21:13_

The `Checker` creates the `Importer` so we can just update the `Importer` to also contain a reference to the `SemanticModel` but I trust your judgement about the issue that needs to be fixed so we can leave it as is for now and do a follow-up change if required.

---

_@dhruvmanila reviewed on 2025-04-10 21:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:134 on 2025-04-10 21:13_

I think the lowest supported version is 3.7 ?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:134 on 2025-04-10 21:14_

Oh you're right, that explains why there's a `PY37` constant. I'll go with that.

---

_@ntBre reviewed on 2025-04-10 21:14_

---

_@ntBre reviewed on 2025-04-10 21:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:540 on 2025-04-10 21:39_

Oh interesting, I see. I think I'll lean toward leaving it like this for now, but I'll keep this in mind if we don't end up using any more code from the `Checker`. It would logically make more sense on the `Importer`, as you said.

---

_@MichaReiser approved on 2025-04-11 06:41_

We can more freely change behavior of fixes because they aren't observed as "breaking" for users unlike rules that can trigger new lint violations. The main reason for gating fixes behind preview is to get more feedback on the fixe's correctness.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:545 on 2025-04-11 12:34_

not really something to be addressed in this PR, but FWIW I don't _love_ the return type of `get_or_import_symbol`. It seems too easy to misuse it by throwing away the second element of the tuple when you get an `Ok(Edit, String)` returned. E.g. https://github.com/astral-sh/ruff/pull/15853

It also seems like it's too easy to call `.ok()` on the `Result` returned, when what you're _meant_ to do is to use it in combination with `try_set_fix()` so that the information from the error is logged to the terminal when `--verbose` is specified on the CLI

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:542 on 2025-04-11 12:39_

nit: maybe this parameter name would be slightly clearer about what the parameter indicates?

```suggestion
        version_added_to_typing: PythonVersion,
```

---

_@AlexWaygood approved on 2025-04-11 12:44_

This is great, thank you!

---

_@ntBre reviewed on 2025-04-11 13:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:545 on 2025-04-11 13:31_

Agreed, I thought that return type looked a bit suspicious (especially when I considered adding a third element to the tuple :laughing:)

I'll keep an eye on this and see if I can come up with anything.

---

_@ntBre reviewed on 2025-04-11 13:32_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:542 on 2025-04-11 13:32_

I like that, thanks!

---

_Merged by @ntBre on 2025-04-11 13:37_

---

_Closed by @ntBre on 2025-04-11 13:37_

---

_Branch deleted on 2025-04-11 13:37_

---

_Comment by @Skylion007 on 2025-04-13 15:35_

Can this be merged with the logic for the import upgrading rules in PYUP as that should have version info for all these import calls.

---

_Comment by @ntBre on 2025-04-14 13:21_

> Can this be merged with the logic for the import upgrading rules in PYUP as that should have version info for all these import calls.

Do you have any specific pyupgrade rules in mind? I looked through them briefly and didn't see any obvious ones. I thought most of these rules only run at all when the target Python version is high enough, but it's certainly plausible that this logic could be generalized and used elsewhere. I guess the idea here was that `typing` was a common-enough specific case that it made sense to have a special method for it.

---

_Comment by @Skylion007 on 2025-04-14 15:26_

https://github.com/astral-sh/ruff/blob/701aecb2a63b72fa7cc36d5db77a3c14d0ac226a/crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs#L204

---

_Comment by @Skylion007 on 2025-04-14 15:26_

> > Can this be merged with the logic for the import upgrading rules in PYUP as that should have version info for all these import calls.
> 
> Do you have any specific pyupgrade rules in mind? I looked through them briefly and didn't see any obvious ones. I thought most of these rules only run at all when the target Python version is high enough, but it's certainly plausible that this logic could be generalized and used elsewhere. I guess the idea here was that `typing` was a common-enough specific case that it made sense to have a special method for it.

https://github.com/astral-sh/ruff/blob/701aecb2a63b72fa7cc36d5db77a3c14d0ac226a/crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_import.rs#L204

---

_Comment by @ntBre on 2025-04-14 15:52_

Ah yes, thank you! Those did not come up in my searches but look very relevant.

---
