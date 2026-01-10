```yaml
number: 18541
title: Add lint rule for calling chmod with non-octal integers
type: pull_request
state: merged
author: RazerM
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/chmod-non-octal
created_at: 2025-06-07T22:26:51Z
updated_at: 2025-06-19T09:31:46Z
url: https://github.com/astral-sh/ruff/pull/18541
synced_at: 2026-01-10T18:39:08Z
```

# Add lint rule for calling chmod with non-octal integers

---

_Pull request opened by @RazerM on 2025-06-07 22:26_

Resolves #18464

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds a new lint rule to find cases where chmod was called with a decimal integer, e.g. `os.chmod(400)` looks like it would set `u=r,go=` permissions but actually sets `u=rw,g=w,o=`. The intent was probably `0o400`.

## Test Plan

cargo test


---

_Review requested from @ntBre by @ntBre on 2025-06-08 03:35_

---

_Label `rule` added by @ntBre on 2025-06-08 03:35_

---

_Comment by @github-actions[bot] on 2025-06-08 03:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e614be661b70c7102495f39aa44d9e5f3a0f7fcb/devel-common/src/tests_common/test_utils/gcp_system_helpers.py#L175'>devel-common/src/tests_common/test_utils/gcp_system_helpers.py:175:32:</a> RUF064 Non-octal mode
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF064 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-06-09 17:26_

Related clippy rule https://rust-lang.github.io/rust-clippy/master/index.html#non_octal_unix_permissions

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1030 on 2025-06-09 17:28_

I do like clippy's naming because it leaves the door open to lint for other cases than just `chmod` (if there are any)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/chmod_non_octal.rs`:107 on 2025-06-09 17:30_

I don't think we should offer a fix here. The main promise of the rule is that the code is probably incorrect and just rewriting the code from decimal to octal doesn't fix the root problem -> That the code is probably just incorrect.


I'd be okay if we allowlisted some common chmods and offered rewriting them but I don't think we should offer rewriting in general.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/chmod_non_octal.rs`:55 on 2025-06-09 17:31_

See my comment below. I don't think we should offer a suggestion here because the promise of the rule is that we believe that the used decimal permissions are actually wrong and simply rewriting to octal doesn't fix that.

---

_@MichaReiser reviewed on 2025-06-09 17:32_

Thank you. 

This mostly looks good. My main feedback is that I don't think we should offer a fix here.

---

_Comment by @MichaReiser on 2025-06-09 18:23_

@ntBre reached out to me on Discord and I think I could be persuaded that we should offer a fix (but I would remove the suggestion from the lint message, given that the fix will show the mode).

I mainly focused on existing (working) code. The fix would be incorrect in those cases and is probably going to break the user code. This seems fine, given that the fix is marked as unsafe. 

What I think is the main promise of the rule is that it catches an incorrect permission *when* you're writing new code. Providing a fix seems very useful in those cases. And I think that should work well because we do show unsafe fixes in the IDE as code actions that users can apply manually. 


---

_@RazerM reviewed on 2025-06-09 19:21_

---

_Review comment by @RazerM on `crates/ruff_linter/src/codes.rs`:1030 on 2025-06-09 19:21_

I'm definitely open to naming suggestions. My only gripe with the phrase "unix permissions" is that this API is available on Windows in Python (albeit only the `0o400` and `0o200` flags do something)

---

_Comment by @RazerM on 2025-06-09 19:34_

I also hemmed and hawed about whether to include a fix, and whether it should be limited to certain common cases (e.g. `644` -> `0o644`).

A [public code search](https://sourcegraph.com/search?q=context:global+%5C.chmod%5C%28%5B1-9%5D%5B0-9%5D%2B%5C%29+lang:Python+&patternType=regexp&sm=0) is interesting.

There definitely are cases where a *correct* decimal integer is used, and often but not always there is a comment of the equivalent octal. This may stem back to code bases that supported Python 2.5 and Python 3 (octal syntax `0o` came in 2.6).
  
E.g. the pytest test suite contains [`chmod(256)`](https://github.com/pytest-dev/pytest/blob/9e9633de9da7a9fab03b4bba3a326bf85b412050/testing/test_assertrewrite.py#L1029) which is equivalent to `0o400`. Our suggested fix would be wrong.

---

_@ntBre reviewed on 2025-06-09 19:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:1030 on 2025-06-09 19:39_

I think `NonOctalPermissions` would be fine (just dropping the unix part from clippy's name). As Micha said, it looks like many of the [`os`](https://docs.python.org/3/library/os.html) methods take a `mode` argument, so we could reuse a more generally-named rule for those too.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_octal_permissions.rs`:34 on 2025-06-10 10:56_

It would be great if we could be more concrete here. I think the airflow example that you mentioned would be a great example

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_octal_permissions.rs`:108 on 2025-06-10 10:57_

```suggestion
        diagnostic.set_fix(Fix::unsafe_edit(edit));
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_octal_permissions.rs`:55 on 2025-06-10 10:58_

I'd remove the `suggested` as it is also visible in the fix

---

_@MichaReiser reviewed on 2025-06-10 10:58_

Okay, I think we can go with a fix and see whether users find it useful. 

I've a few smaller remarks.

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/ruff/rules/non_octal_permissions.rs`:108 on 2025-06-10 22:05_

done

---

_@RazerM reviewed on 2025-06-10 22:05_

---

_@RazerM reviewed on 2025-06-10 22:05_

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/ruff/rules/non_octal_permissions.rs`:55 on 2025-06-10 22:05_

done

---

_@RazerM reviewed on 2025-06-10 22:14_

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/ruff/rules/non_octal_permissions.rs`:71 on 2025-06-10 22:14_

How would you feel about showing a representation of the mode as written e.g.

```
t.py:3:17: RUF064 Non-octal mode (u=rw,g=w,o=)
  |
1 | import os
2 |
3 | os.chmod("foo", 400)
  |                 ^^^ RUF064
4 | os.chmod("foo", 256)
  |
  = help: Replace with octal literal

t.py:4:17: RUF064 Non-octal mode (u=r,go=)
  |
3 | os.chmod("foo", 400)
4 | os.chmod("foo", 256)
  |                 ^^^ RUF064
  |
  = help: Replace with octal literal
```

---

_Comment by @RazerM on 2025-06-10 22:57_

I'm tempted to make the fix logic this:

```rust
fn suggest_fix(mode: u16) -> Option<u16> {
    // These suggestions are in the form of
    // <missing `0o` prefix> | <mode as decimal> => <octal>
    // If <as decimal> could theoretically be a valid octal literal, the
    // comment explains why it's deemed unlikely to be intentional.
    match mode {
        400 | 256 => Some(0o400), // -w-r-xrw-, group/other > user unlikely
        440 | 288 => Some(0o440),
        444 | 292 => Some(0o444),
        600 | 384 => Some(0o600),
        640 | 416 => Some(0o640), // r----xrw-, other > user unlikely
        644 | 420 => Some(0o644), // r---w----, group write but not read unlikely
        660 | 432 => Some(0o660), // r---wx-w-, write but not read unlikely
        664 | 436 => Some(0o664), // r---wxrw-, other > user unlikely
        666 | 438 => Some(0o666),
        700 | 448 => Some(0o700),
        744 | 484 => Some(0o744),
        750 | 488 => Some(0o750),
        755 | 493 => Some(0o755),
        770 | 504 => Some(0o770), // r-x---r--, other > group unlikely
        775 | 509 => Some(0o775),
        776 | 510 => Some(0o776), // r-x--x--x, seems unlikely
        777 | 511 => Some(0o777), // r-x--x--x, seems unlikely
        _ => None
    }
}
```


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/non_octal_permissions.rs`:71 on 2025-06-11 05:34_

I like the idea but I think it's confusing in the fix message because it gives the impression that `u=rw, ...` was the non octal mode, which it wasn't. 

I think this is a case where sub diagnostics (@ntBre is working on this) would be very useful. You could then add two hints (needs better wording but roughly):

```
info: 0oxxx (256) is u=rw, g=w, o=
info: 0o256 is u=....
```

But our diagnostics don't support this today :(

---

_@MichaReiser reviewed on 2025-06-11 05:34_

---

_Comment by @MichaReiser on 2025-06-11 06:42_

> I'm tempted to make the fix logic this:

I like it!

---

_@ntBre reviewed on 2025-06-11 10:18_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/non_octal_permissions.rs`:71 on 2025-06-11 10:18_

Added to #17203!

---

_@MichaReiser approved on 2025-06-19 09:29_

Awesome, thank you

---

_Merged by @MichaReiser on 2025-06-19 09:30_

---

_Closed by @MichaReiser on 2025-06-19 09:30_

---
