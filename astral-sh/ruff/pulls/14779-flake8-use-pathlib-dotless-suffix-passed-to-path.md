```yaml
number: 14779
title: "[`flake8-use-pathlib`] Dotless suffix passed to `Path.with_suffix()` (`PTH210`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF049
created_at: 2024-12-05T00:16:27Z
updated_at: 2024-12-08T12:05:01Z
url: https://github.com/astral-sh/ruff/pull/14779
synced_at: 2026-01-12T15:55:49Z
```

# [`flake8-use-pathlib`] Dotless suffix passed to `Path.with_suffix()` (`PTH210`)

---

_@InSyncWithFoo_

## Summary

Resolves #14441.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-05 00:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/dotless_with_suffix.rs`:97 on 2024-12-05 16:53_

```suggestion
    if arguments.len() == 1 {
        arguments
            .find_argument("suffix", 0)?
            .as_string_literal_expr()
    } else {
        None
    }
```

You could probably inline this function. It's only used once, and it feels simple enough that it doesn't really need to be extracted into a separate function.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/dotless_with_suffix.rs`:67 on 2024-12-05 16:54_

why are we `return`ing if a fix is unavailable? Surely it's better to mark the diagnostic as only-sometimes fixable rather than just not emitting it if we can't provide an autofix

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/dotless_with_suffix.rs`:60 on 2024-12-05 16:56_

I'd find this easier to understand:

```suggestion
    let string_value = string.value.to_str();
    if string_value.is_empty() || string_value.starts_with('.') {
        return;
    }
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/dotless_with_suffix.rs`:114 on 2024-12-05 17:05_

this suggestion requires an import of `ruff_text_size::Ranged` at the top of the file:

```suggestion
    let first_part = string.value.iter().next()?;
    let after_leading_quote = first_part
        .start()
        .checked_add(first_part.flags.opener_len())?;
    let edit = Edit::insertion(".".to_string(), after_leading_quote);
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/dotless_with_suffix.rs`:116 on 2024-12-05 17:06_

this should definitely be an unsafe fix, because they might be deliberately using a suffix without a leading `.`, in which case the fix would break their code.

---

_@AlexWaygood reviewed on 2024-12-05 17:10_

Thanks. The rule seems reasonable to me. I think we should add it to our `flake8-use-pathlib` category, however. We already have rules such as https://docs.astral.sh/ruff/rules/path-constructor-current-directory/ that tell you how to use pathlib better (rather than just complaining at you if you used `os.path` instead of pathlib).

---

_@InSyncWithFoo reviewed on 2024-12-05 17:20_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/dotless_with_suffix.rs`:67 on 2024-12-05 17:20_

This `return` is expected to be unreachable; if it is, there is a bug somewhere. Should I still implement `AlwaysFixableViolation`, but might not add the fix if it is `None`?

---

_@AlexWaygood reviewed on 2024-12-05 17:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/dotless_with_suffix.rs`:67 on 2024-12-05 17:22_

I see. If the return is expected to be unreachable, you should make that explicit by doing something like

```suggestion
    let diagnostic = Diagnostic::new(DotlessWithSuffix, call.range);
    let Some(fix) = add_leading_dot_fix(string) else {
        unreachable!("Expected always to be able to fix this rule");
    };
    checker.diagnostics.push(diagnostic.with_fix(fix));
```

And then we'll get bug reports telling us if something about our assumptions here is incorrect, which is useful.

---

_Renamed from "[`ruff`] Dotless suffix passed to `Path.with_suffix()` (`RUF049`)" to "[`flake8-use-pathlib`] Dotless suffix passed to `Path.with_suffix()` (`PTH901`)" by @InSyncWithFoo on 2024-12-05 19:50_

---

_Review requested from @AlexWaygood by @AlexWaygood on 2024-12-06 01:17_

---

_Label `rule` added by @MichaReiser on 2024-12-06 09:01_

---

_Label `preview` added by @MichaReiser on 2024-12-06 09:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:912 on 2024-12-06 09:04_

I don't think it's necessary to introduce an entire new prefix. We use `2XX` for rules that have no upstream rule.

I also think we should make the rule name more explicit: `DotlessPathlibWithSuffix` to avoid future conflicts

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_with_suffix.rs`:19 on 2024-12-06 09:06_

We should be more explicit about what the possible errors are here. As a reader, it's not clear to me what that means (what's even type inference)? What are the cases I have to worry about.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_with_suffix.rs`:71 on 2024-12-06 09:10_

We can avoid the `unreachable` by rewiring this a bit
```suggestion
		let [first_part, ..] = string.value.as_slice() else {
        return;
    };

    if first_part.is_empty() || first_part.starts_with('.') {
        return;
    }

    let diagnostic = Diagnostic::new(DotlessWithSuffix, call.range);
    let fix = add_leading_dot_fix(first_part);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_with_suffix.rs`:103 on 2024-12-06 09:10_

Can you explain why the fix is considered unsafe and if it has to be unsafe, can we document the fix safety?

---

_@MichaReiser requested changes on 2024-12-06 09:11_

Thank you. A few smaller coding and documentation changes

---

_@AlexWaygood reviewed on 2024-12-06 09:18_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_with_suffix.rs`:103 on 2024-12-06 09:18_

I asked for @InSyncWithFoo to make the fix unsafe. I think it's important because this fix changes the code from something that always raises an exception at runtime to something that doesn't. By doing that, we're making the assumption that the `with_suffix` call is exactly correct except for not having a leading period. But that assumption might not be accurate, and applying the autofix might conceal other possible bugs, since the runtime will no longer raise an exception on the call

---

_@AlexWaygood reviewed on 2024-12-06 09:19_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_with_suffix.rs`:103 on 2024-12-06 09:19_

Put another way: we know there's a mistake in the code, but are we _sure_ it's just that they forgot a leading period in the string they're passing? Maybe they have the correct string but they're using the wrong method?

---

_@AlexWaygood reviewed on 2024-12-06 09:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_with_suffix.rs`:103 on 2024-12-06 09:20_

I agree that we should add a docs section on fix safety, definitely!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_with_suffix.rs`:71 on 2024-12-06 09:44_

Even with this change, there's still a `checked_add()` call in the `add_leading_dot_fix()` function where a `?` is used. But I think it would be fine to use `.expect()` there to avoid having to return an `Option` from `add_leading_dot_fix()`

---

_@AlexWaygood reviewed on 2024-12-06 09:44_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_with_suffix.rs`:71 on 2024-12-06 11:32_

This change fails the `u'' r'json'` test, as it expects the first part, rather than the entire string, to have at least one character.

---

_@InSyncWithFoo reviewed on 2024-12-06 11:32_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:10 on 2024-12-06 11:36_

```suggestion
/// Checks for `pathlib.Path.with_suffix()` calls where
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:18 on 2024-12-06 11:37_

do you have an example of a situation where this rule might cause a false positive? It seems to me like it's much more likely to cause false negatives than false positives?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:25 on 2024-12-06 11:41_

```suggestion
/// The fix for this rule adds a leading period to the string passed
/// to the `with_suffix()` call. This fix is marked as unsafe, as it
/// changes runtime behaviour: the call would previously always have
/// raised an exception, but no longer will. Moreover, it's impossible
/// to determine if this is the correct fix for a given situation
/// (it's possible that the string was correct but was being passed to
/// the wrong method entirely, for example).
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:27 on 2024-12-06 11:41_

Can you move this section above the `## Known problems` and `## Fix safety` section?

---

_@AlexWaygood reviewed on 2024-12-06 11:44_

---

_@InSyncWithFoo reviewed on 2024-12-06 11:55_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/dotless_pathlib_with_suffix.rs`:18 on 2024-12-06 11:55_

I thought it would be better to mention false positives, even if they are firmly in the minority; no strong opinion, however. Rephrased.

---

_@MichaReiser approved on 2024-12-06 12:08_

Thanks, these documentation improvements are great. I now get a good sense for why the fix is unsafe and what the limitations are. 

---

_Merged by @MichaReiser on 2024-12-06 12:08_

---

_Closed by @MichaReiser on 2024-12-06 12:08_

---

_Branch deleted on 2024-12-06 12:23_

---

_Renamed from "[`flake8-use-pathlib`] Dotless suffix passed to `Path.with_suffix()` (`PTH901`)" to "[`flake8-use-pathlib`] Dotless suffix passed to `Path.with_suffix()` (`PTH210`)" by @AlexWaygood on 2024-12-06 13:24_

---
