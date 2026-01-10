```yaml
number: 19063
title: "[`flake8-bugbear`] Support non-context-manager calls in `B017`"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-19050
created_at: 2025-07-01T02:25:44Z
updated_at: 2025-07-08T19:10:00Z
url: https://github.com/astral-sh/ruff/pull/19063
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-bugbear`] Support non-context-manager calls in `B017`

---

_Pull request opened by @danparizher on 2025-07-01 02:25_

## Summary

Fixes #19050

---

_Comment by @github-actions[bot] on 2025-07-01 02:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/c5a75f2498c86850c4ce13bcf10d56efc92394a4/testing/code/test_code.py#L91'>testing/code/test_code.py:91:15:</a> B017 Do not assert blind exception: `Exception`
+ <a href='https://github.com/pytest-dev/pytest/blob/c5a75f2498c86850c4ce13bcf10d56efc92394a4/testing/example_scripts/fixtures/fill_fixtures/test_conftest_funcargs_only_available_in_subdir/sub2/conftest.py#L9'>testing/example_scripts/fixtures/fill_fixtures/test_conftest_funcargs_only_available_in_subdir/sub2/conftest.py:9:5:</a> B017 Do not assert blind exception: `Exception`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B017 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:111 on 2025-07-01 14:48_

Would it help to destructure the whole `ExprCall` here? I think we could use `func`, `arguments`, and even `range` down below.

```suggestion
    let ast::ExprCall { func, arguments, range, .. } = call;
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:144 on 2025-07-01 14:52_

Do you think we could reuse the code from `assert_raises_exception` for this? This looks correct and pretty similar, but I think we want to keep the two in sync. I could be missing a bigger difference, but intuitively it seems like once we've destructured the `Call` in `assert_raises_exception`, the logic should be the same.

---

_@ntBre reviewed on 2025-07-01 14:56_

Thanks! This looks good overall, just a couple of minor suggestions. Can you also gate this behind preview since it's an expansion of a stable rule? Here's an example:

https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/preview.rs#L44

---

_Label `rule` added by @ntBre on 2025-07-01 14:56_

---

_Label `preview` added by @ntBre on 2025-07-01 14:56_

---

_@danparizher reviewed on 2025-07-01 15:44_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:111 on 2025-07-01 15:44_

Good idea — done. `assert_raises_exception_call` now takes it so we can use it directly.

---

_@danparizher reviewed on 2025-07-01 15:44_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:144 on 2025-07-01 15:44_

Absolutely. I pulled the shared checks into a helper, `detect_blind_exception`, which both the context-manager path and the new call-form path now invoke. This keeps the two in sync.

---

_Comment by @danparizher on 2025-07-01 15:46_

Preview flag added. The call-form check only runs when `is_assert_raises_exception_call_enabled(settings)` is `true`.

---

_Comment by @ntBre on 2025-07-01 16:01_

Great, thanks! Could you also add a preview test? You can just make a copy of the `rules` function and the `B017` `test_case` and set `LinterSettings::preview` to `PreviewMode::Enabled`.

https://github.com/astral-sh/ruff/blob/48366a7bbb5e555b288b4a00e5bc862670e8dbc1/crates/ruff_linter/src/rules/flake8_bugbear/mod.rs#L73-L81

---

_Comment by @danparizher on 2025-07-01 16:34_

Added!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:77 on 2025-07-01 21:12_

I think this is supposed to be `is_none` like before right?

```suggestion
    if is_pytest_raises && arguments.find_keyword("match").is_none() {
```

It's a little hard to tell because the diff shifted around, but I'm pretty sure one of the snapshots is showing a different result.

I think this `is_some` is actually correct and the existing `is_none` is a bug unless I'm misunderstanding something, but I'd rather handle that separately, I guess.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B017.py`:46 on 2025-07-01 21:15_

I know it would mess up the nice sections here, but it would be easier for reviewing the test diff if these were at the bottom with the other new tests. If you put enough blank lines, the non-preview snapshot should be unchanged, and only the new preview snapshot will be different. Alternatively, you could rename this file to B017_0.py and add B017_1.py with the new tests.

I was fine with this layout before, but I'd like to be sure we're not changing any logic with the `is_none` vs `is_some` thing, and this will make that easier.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:140 on 2025-07-01 21:18_

I don't think this `find_argument` call will ever succeed. If we have fewer than 2 arguments, there won't be an argument in position 1, only a maximum of 1 argument in position 0. Should this be an `||`? If so, we might need a test exercising this. 

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:134 on 2025-07-01 21:20_

I think this is supposed to be the `call` range to match the `with` version.

```suggestion
        checker.report_diagnostic(AssertRaisesException { exception }, range);
```

with `range` from the `ExprCall` above.

---

_@ntBre reviewed on 2025-07-01 21:21_

Thanks!

I noticed a few more things here, but it's looking good.

---

_@danparizher reviewed on 2025-07-01 22:07_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:140 on 2025-07-01 22:07_

My understanding is that `Arguments::find_argument` doesn’t just look at positional arguments – it first looks for a keyword with the given name and only falls back to the positional slot if that keyword isn’t present:

```rust
pub fn find_argument(&self, name: &str, position: usize) -> Option<ArgOrKeyword> {
    self.find_keyword(name)              // ← check `func=…`
        .map(ArgOrKeyword::from)
        .or_else(|| self.find_positional(position).map(ArgOrKeyword::from))
}
```
So in a call such as
```python
pytest.raises(Exception, func=my_func)
```
- `arguments.args.len()` is `1` (the `Exception`), so `len() < 2` is `true`.
- `find_argument("func", 1)` does return `Some(...)` because it finds the keyword `func=…`.

If we changed the guard to `||`, we would exit early whenever either condition is true, and we’d wrongly skip the check in the keyword-argument case.

---

_@danparizher reviewed on 2025-07-01 22:12_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:77 on 2025-07-01 22:12_

Unless I'm missing something, I believe `is_some()` is the right test here. `pytest.raises(..., match="pattern")` is a form that already narrows the check to a specific exception message (equivalent to the `assertRaisesRegex` case for `unittest`), so when that `match=` keyword is present we want to *skip* B017, because it’s no longer a “blind” exception assertion.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:140 on 2025-07-01 22:13_

Ahh, I think you're right. `arguments.args.len()` is only the positional arguments. I was thinking of `arguments.len()`, which includes both. 

---

_@ntBre reviewed on 2025-07-01 22:13_

---

_@ntBre reviewed on 2025-07-01 22:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/assert_raises_exception.rs`:77 on 2025-07-01 22:31_

Hmm, maybe I got confused by the logic here. The snapshot on `main` looks correct. Let's regroup the test cases and make sure none of the stable results change, but I think you may be right.

`is_some` definitely makes sense intuitively, I guess the condition was negated in the original code.

---

_@danparizher reviewed on 2025-07-01 23:33_

---

_Review comment by @danparizher on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B017.py`:46 on 2025-07-01 23:33_

I split up the test cases, let me know if that's what you were looking for. Feel free to commit directly if there is another format that makes more sense 🙂

---

_@ntBre reviewed on 2025-07-02 13:10_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/snapshots/ruff_linter__rules__flake8_bugbear__tests__B017_B017_0.py.snap`:47 on 2025-07-02 13:10_

I pushed a couple of commits to minimize the diff a bit more. I think something _is_ slightly off with the new logic because it looks like this is getting flagged now when it should be allowed, but I haven't pinned down where it is.

---

_@danparizher reviewed on 2025-07-02 16:23_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_bugbear/snapshots/ruff_linter__rules__flake8_bugbear__tests__B017_B017_0.py.snap`:47 on 2025-07-02 16:23_

I found the issue—we weren't considering the second positional string/bytes literal argument. I added in that additional check so now it's not registering as a false positive anymore.

---

_@ntBre approved on 2025-07-08 19:02_

This looks good, thank you!

Unrelated to your changes, from looking at the code while reviewing the PR, I've noticed some other issues with the rule. For example, the rule has false negatives when the exceptions are provided as keyword arguments:

```python
import pytest
import unittest

with pytest.raises(Exception):
    pass

with pytest.raises(expected_exception=Exception):  # false negative
    pass

class TestClass(unittest.TestCase):
    def test_okay(self):
        with self.assertRaises(Exception):
            pass

    def test_false_negative(self):
        with self.assertRaises(exception=Exception):  # false negative
            pass
```

We can go ahead and land this, though, and I'll open a follow-up issue for these.

[playground](https://play.ruff.rs/e1ad5948-95f6-43bd-b687-b3dd95e6429f)

---

_Renamed from "[`flake8-bugbear`] support non-`with` calls in `B017`" to "[`flake8-bugbear`] Support non-context-manager calls in `B017`" by @ntBre on 2025-07-08 19:04_

---

_Merged by @ntBre on 2025-07-08 19:04_

---

_Closed by @ntBre on 2025-07-08 19:04_

---

_Branch deleted on 2025-07-08 19:10_

---
