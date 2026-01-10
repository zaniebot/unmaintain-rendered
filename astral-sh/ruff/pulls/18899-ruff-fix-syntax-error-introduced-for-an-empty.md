```yaml
number: 18899
title: "[`ruff`] Fix syntax error introduced for an empty string followed by a u-prefixed string (`UP025`)"
type: pull_request
state: merged
author: lubaskinc0de
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/up025
created_at: 2025-06-23T17:08:34Z
updated_at: 2025-07-01T13:34:08Z
url: https://github.com/astral-sh/ruff/pull/18899
synced_at: 2026-01-10T18:39:09Z
```

# [`ruff`] Fix syntax error introduced for an empty string followed by a u-prefixed string (`UP025`)

---

_Pull request opened by @lubaskinc0de on 2025-06-23 17:08_

## Summary
/closes #18895
## Test Plan


---

_Label `bug` added by @ntBre on 2025-06-23 17:37_

---

_Label `fixes` added by @ntBre on 2025-06-23 17:37_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unicode_kind_prefix.rs`:51 on 2025-06-23 17:54_

I don't think this arithmetic is necessary. The `is_unicode` check above confirms that the first character will be a `u`, so this just boils down to what was inside `Edit::range_deletion` originally.

```suggestion
        let prefix_range = TextRange::at(string.start(), TextSize::new(1));
```

---

_Comment by @github-actions[bot] on 2025-06-23 18:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/unicode_kind_prefix.rs`:51 on 2025-06-23 18:09_

In general, I think something like this might be a simpler version of the current implementation:

```rust
        let prefix_range = TextRange::at(string.start(), TextSize::new(1));

        let locator = checker.locator();
        let content = locator
            .slice(TextRange::new(prefix_range.end(), string.end()))
            .to_owned();
        // If the preceding character is equivalent to the quote character, insert a space to avoid a
        // syntax error. For example, when removing the `u` prefix in `""u""`, rewrite to `"" ""`
        // instead of `""""`.
        // see https://github.com/astral-sh/ruff/issues/18895
        let edit = if locator
            .slice(TextRange::up_to(prefix_range.start()))
            .chars()
            .last()
            .is_some_and(|char| content.starts_with(char))
        {
            Edit::range_replacement(" ".to_string(), prefix_range)
        } else {
            Edit::range_deletion(prefix_range)
        };

        diagnostic.set_fix(Fix::safe_edit(edit));
```

This just gives either a deletion like before or simply replaces the `u` with a space, instead of having to replace the whole string.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP025.py.snap`:284 on 2025-06-23 18:42_

I'm slightly concerned that we're now adding spaces where they aren't totally necessary. The issue indicated a few more constraints on the problem:

> introduce a syntax error when the prefixed string follows a **non-triple-quoted empty** string with the same kind of quotation marks without an intervening space

Cases like this (`"foo""bar"` and even `"foo"""`) are okay, as are empty triple-quoted strings like `""""""""` (8 double quotes):

```shell
$ python -m tokenize <<<'""""""""'
1,0-1,6:            STRING         '""""""'
1,6-1,8:            STRING         '""'
1,8-1,9:            NEWLINE        '\n'
2,0-2,0:            ENDMARKER      ''
```

I think we could check the kind of the preceding token to see if it's a triple-quoted string, but I don't think we can perform an `is_empty` check on the tokens.

@MichaReiser what do you think? Is it okay to add spaces where they aren't strictly necessary, or do you know of a better way to check if the previous string part is non-triple-quoted and empty?

---

_@ntBre reviewed on 2025-06-23 18:42_

Thanks! I had a suggestion for a slightly simpler version of the current implementation and also a question that I'm not sure about either.

---

_@MichaReiser reviewed on 2025-06-24 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP025.py.snap`:284 on 2025-06-24 06:45_

I think the easiest would be to make this a rule that operates on `StringLike` and then iterate over all parts. This also ensures that, when removing multiple parts, that the spacing is only inserted when necessary. 

> I think we could check the kind of the preceding token to see if it's a triple-quoted string, but I don't think we can perform an is_empty check on the tokens.

I think this is correct because of line continuations where subtracting the prefix from the range doesn't yield the correct result:

```py
a= "\
" u"c"
```

> s it okay to add spaces where they aren't strictly necessary, or do you know of a better way to check if the previous string part is non-triple-quoted and empty?

I think it's fine for as long as it only pads at most one space (never results in two consecutive spaces)

---

_@lubaskinc0de reviewed on 2025-06-29 11:52_

---

_Review comment by @lubaskinc0de on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP025.py.snap`:284 on 2025-06-29 11:52_

Going back to this problem, I don't really understand the next steps. Here's the current implementation
```rust
/// UP025
pub(crate) fn unicode_kind_prefix(checker: &Checker, string: &StringLiteral) {
    if string.flags.prefix().is_unicode() {
        let mut diagnostic = checker.report_diagnostic(UnicodeKindPrefix, string.range);

        let prefix_range = TextRange::at(string.start(), TextSize::new(1));
        let locator = checker.locator();
        let content = locator
            .slice(TextRange::new(prefix_range.end(), string.end()))
            .to_owned();

        // If the preceding character is equivalent to the quote character, insert a space to avoid a
        // syntax error. For example, when removing the `u` prefix in `""u""`, rewrite to `"" ""`
        // instead of `""""`.
        // see https://github.com/astral-sh/ruff/issues/18895
        let edit = if locator
            .slice(TextRange::up_to(prefix_range.start()))
            .chars()
            .last()
            .is_some_and(|char| content.starts_with(char))
        {
            Edit::range_replacement(" ".to_string(), prefix_range)
        } else {
            Edit::range_deletion(prefix_range)
        };

        diagnostic.set_fix(Fix::safe_edit(edit));
    }
}
```
As far as I understand, it correctly handles the following cases:
before fix:
```py
a = "\
" u"c"

f"foo"u"bar"

f"foo" u"bar"

""u""
```
after fix:
```py
a = "\
" "c"

f"foo" "bar"

f"foo" "bar"

"" ""
```

In that case, I don't understand the drawbacks of the current simple implementation. Are there any cases that are currently being handled differently than desired?

---

_@ntBre reviewed on 2025-06-30 21:43_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP025.py.snap`:284 on 2025-06-30 21:43_

My argument was that a case like this:

```python
f"foo"u"bar"
```

is _not_ being handled correctly because we're adding a space that isn't necessary. In other words, this fix would be fine:

```python
f"foo""bar"
```

but it's being fixed to

```python
f"foo" "bar"
```

Micha said that could be okay as long as we never add a second space, but I don't entirely follow the line-continuation example. It does seem to be handled properly in the current version, as you said.

As Micha pointed out, another approach would be to iterate over the parts of the string instead of looking at a single part at a time. Then we could check the conditions mentioned in the issue that actually _require_ a space to be inserted: the preceding string part is not triple quoted and not empty.

To be more concrete, we would pass in `value` here (or a transformed version of it to allow passing both f-strings and regular strings) instead of passing a single `string_part` at a time:

https://github.com/astral-sh/ruff/blob/10a14a9fc852afcef30eac5c82ad0671e6ec1b76/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L1575-L1579

With all that being said, maybe it's not worth worrying about and we can just move ahead with this version. I think the space looks better anyway, and I can't come up with a counter-example that adds a second space.

---

_@lubaskinc0de reviewed on 2025-07-01 08:35_

---

_Review comment by @lubaskinc0de on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP025.py.snap`:284 on 2025-07-01 08:35_

I think that the simpler the implementation (and it's quite simple now) the better, and if it works correctly, why not keep it as it is? but it might be worth asking someone else

---

_@lubaskinc0de reviewed on 2025-07-01 08:37_

---

_Review comment by @lubaskinc0de on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP025.py.snap`:284 on 2025-07-01 08:37_

anyway, if you need to do it differently, I'll do it, just tell me

---

_@ntBre reviewed on 2025-07-01 13:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP025.py.snap`:284 on 2025-07-01 13:31_

I think you're right, let's move ahead with the simple fix for now. Thank you!

---

_@ntBre approved on 2025-07-01 13:31_

---

_Renamed from "[``ruff``] Fix introduces a syntax error for an empty string followed by a u-prefixed string (``UP025``)" to "[`ruff`] Fix syntax error introduced for an empty string followed by a u-prefixed string (`UP025`)" by @ntBre on 2025-07-01 13:33_

---

_Merged by @ntBre on 2025-07-01 13:34_

---

_Closed by @ntBre on 2025-07-01 13:34_

---
