```yaml
number: 17893
title: "[parser] Flag single unparenthesized generator expr with trailing comma in arguments."
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - bug
  - parser
assignees: []
merged: true
base: main
head: syntax-error
created_at: 2025-05-06T17:31:30Z
updated_at: 2025-05-07T18:19:44Z
url: https://github.com/astral-sh/ruff/pull/17893
synced_at: 2026-01-10T18:57:03Z
```

# [parser] Flag single unparenthesized generator expr with trailing comma in arguments.

---

_Pull request opened by @abhijeetbodas2001 on 2025-05-06 17:31_

Fixes #17867

## Summary

The CPython parser does not allow generator expressions which are the sole arguments in an argument list to have a trailing comma.
With this change, we start flagging such instances.

## Test Plan

Added new inline tests.

---

_Review requested from @MichaReiser by @abhijeetbodas2001 on 2025-05-06 17:31_

---

_Review requested from @dhruvmanila by @abhijeetbodas2001 on 2025-05-06 17:31_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/src/parser/mod.rs`:665 on 2025-05-06 17:33_

Instead of special-casing inside `parse_comma_separated_list` itself, I felt returning the `trailing_comma_range` to be simpler. But please let me know if this feels hacky.

---

_@abhijeetbodas2001 reviewed on 2025-05-06 17:33_

---

_Comment by @abhijeetbodas2001 on 2025-05-06 17:33_

@ntBre could you please review?

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/inline/ok/args_unparenthesized_generator.py`:2 on 2025-05-06 18:18_

I think this is the cause of the CI failure:
```suggestion
sum((x for x in range(10)),)
```

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:664 on 2025-05-06 18:25_

This is just a note to other potential reviewers, but I highly recommend hiding whitespace from the diff here. The only change in this function is capturing the value `trailing_comma_range`, which is now returned by `parse_comma_separated_list`.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/mod.rs`:665 on 2025-05-06 18:33_

This looks reasonable to me (and the change is quite small overall once I hid the whitespace diff), but I have two ideas here:
- Can we just return a `bool` instead of an `Option<TextRange>`? It looks like we only ever use the `Option` to call `is_some`, so we could just call `is_some` in the return here.
- Could we update the `recovery_context_kind` we pass in here so that we reuse the existing `OtherError` on line 657?

The second of these would prevent the need for the first and seems like a more robust approach, but I'm not really familiar with the available `RecoveryContextKind`s. If we can't get that to work, I think the current approach (with the `bool` modification) would be fine. @dhruvmanila would know better than me though!

---

_@ntBre reviewed on 2025-05-06 18:34_

---

_Comment by @github-actions[bot] on 2025-05-06 18:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_@abhijeetbodas2001 reviewed on 2025-05-06 18:57_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/src/parser/mod.rs`:665 on 2025-05-06 18:57_

The context is `RecoveryContextKind::Arguments`, which alows trailing commas.

By updating `recovery_context_kind`, do you mean making it so that `OtherError` is raised even for `RecoveryContextKind::Arguments`? And later on replace that `OtherError` with the more specialized `UnparenthesizedGeneratorExpression` (or remove it entirely, if generators are not involved)?

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/src/parser/expression.rs`:664 on 2025-05-06 18:57_

(I also did not know about this setting. Thank you!)

---

_@abhijeetbodas2001 reviewed on 2025-05-06 18:57_

---

_@abhijeetbodas2001 reviewed on 2025-05-06 18:58_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/resources/inline/ok/args_unparenthesized_generator.py`:2 on 2025-05-06 18:58_

Oops, looks like a `git add` miss on my end. Fixed.

---

_@abhijeetbodas2001 reviewed on 2025-05-06 19:01_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/src/parser/mod.rs`:665 on 2025-05-06 19:01_

I addressed the first point and pushed here.
Happy to revert/change if we decide on the other way.

---

_Comment by @abhijeetbodas2001 on 2025-05-06 19:04_

Thanks for reviewing, Brent. I addressed all but one comment.
Please see the question at https://github.com/astral-sh/ruff/pull/17893#discussion_r2076077042

---

_@ntBre reviewed on 2025-05-06 19:55_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/mod.rs`:665 on 2025-05-06 19:55_

Sorry for the confusion, I pulled down the PR locally, and I think I understand what's going on better now. My suggestion was basically a worse wording of your idea to add special-casing inside of `parse_comma_separated_list`, but I see now that the proper error (`UnparenthesizedGeneratorExpression`) is emitted from `validate_arguments`, so I think your implementation totally makes sense.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:2529 on 2025-05-06 19:58_

Minor docs suggestion, something like:

> 2. If there is more than one argument (positional or keyword), **or a single argument with a trailing comma,** ...

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/parser/expression.rs`:2547 on 2025-05-06 20:00_

I believe we can simplify this to
```suggestion
        if has_trailing_comma || arguments.len() > 1 {
```

since we'll either have 0 arguments, in which case the loop is never entered and the conditional doesn't matter; 1 argument in which case only `has_trailing_comma` matters; or many arguments, in which case `arguments.len() > 1` applies.

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/inline/ok/args_unparenthesized_generator.py`:3 on 2025-05-06 20:02_

I guess one more nit while I'm here, this is slightly confusing with the name of the test (`unparenthesized_generator`), but it's also annoying to have to rename snapshots and such, so I'm okay with leaving it as is.

---

_@ntBre approved on 2025-05-06 20:02_

Thanks, this looks good to me! And thanks for clearing up my confusion from earlier. I found a couple of minor nits, but otherwise I just want to give Dhruv a chance to look at this if he wants to.

---

_Label `bug` added by @ntBre on 2025-05-06 20:05_

---

_Label `parser` added by @ntBre on 2025-05-06 20:05_

---

_@abhijeetbodas2001 reviewed on 2025-05-07 06:02_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/resources/inline/ok/args_unparenthesized_generator.py`:3 on 2025-05-07 06:02_

Hmm, if the premise is that un-parenthesized expressions are bad, then I think it would be natural for the "ok" parts of the test to be good (ie, have parens)?

---

_@abhijeetbodas2001 reviewed on 2025-05-07 06:04_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_parser/resources/inline/ok/args_unparenthesized_generator.py`:3 on 2025-05-07 06:04_

I actually added one more inline test here. Not related to the changes in this PR specifically, but I felt the case of multiple arguments was missing from the "ok" section.

---

_Comment by @abhijeetbodas2001 on 2025-05-07 06:09_

Thanks Brent. I've addressed the review comments. For the test name -- IMO the name isn't exactly incorrect, so I'd prefer to keep it the same way.
(Although I did add a new inline test in the "ok" section which you might want to review.)

---

_@MichaReiser approved on 2025-05-07 07:50_

---

_@dhruvmanila approved on 2025-05-07 12:22_

Thank you!

---

_@ntBre approved on 2025-05-07 18:11_

Looks great, thank you! I think it's fine to leave the test name as is too.

---

_Merged by @ntBre on 2025-05-07 18:11_

---

_Closed by @ntBre on 2025-05-07 18:11_

---

_Branch deleted on 2025-05-07 18:19_

---
