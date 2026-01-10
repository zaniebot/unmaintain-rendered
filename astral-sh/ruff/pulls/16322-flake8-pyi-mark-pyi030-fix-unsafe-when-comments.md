```yaml
number: 16322
title: "[`flake8-pyi`] Mark `PYI030` fix unsafe when comments are deleted"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
  - fixes
  - preview
assignees: []
merged: true
base: main
head: fix-PYI030
created_at: 2025-02-22T22:54:51Z
updated_at: 2025-02-23T21:26:28Z
url: https://github.com/astral-sh/ruff/pull/16322
synced_at: 2026-01-10T19:49:01Z
```

# [`flake8-pyi`] Mark `PYI030` fix unsafe when comments are deleted

---

_Pull request opened by @VascoSch92 on 2025-02-22 22:54_

The PR addresses issue #16317 

---

_Review requested from @AlexWaygood by @VascoSch92 on 2025-02-22 22:54_

---

_Comment by @github-actions[bot] on 2025-02-22 23:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:145 on 2025-02-23 01:48_

Could we just return the `Edit` from both branches of the `if other_exprs.is_empty()`? Then we could avoid duplicating the comment check. I'm picturing something like this truncated example:

```rust
let edit = if other_exprs.is_empty() {
    Edit::range_replacement(checker.generator().expr(&literal), expr.range())
} else {
    // ...
    Edit::range_replacement(content, expr.range())
};

if checker.comment_ranges().intersects(expr.range()) {
    Fix::unsafe_edit(edit)
 } else {
     Fix::safe_edit(edit)
}
```

The code was already structured like this before your changes, but I also find it a bit weird to have this huge block inside of the `diagnostic.set_fix` call. That makes sense when using `Diagnostic::try_set_fix` with a closure but not so much here. For the sake of an easy-to-review diff, we might want to leave that alone for now, though.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:33 on 2025-02-23 01:54_

I know I linked to TC006, which has this kind of phrasing, but I would probably phrase this more like "This fix is marked unsafe if it would delete any comments within the replacement range."

And if you want to go above and beyond you can include an example like [SIM905](https://docs.astral.sh/ruff/rules/split-static-string/#fix-safety). Maybe something like

```python
field26: (
    # preserved comment
        Literal["a", "b"]
        # deleted comment
        | Literal["c", "d"]  # preserved again
)
```

(assuming I've identified where comments are preserved and deleted correctly)

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI030.py`:101 on 2025-02-23 02:02_

It feels wrong for me to complain about having too many tests, but are any of these cases addressing particular subtleties here? They all look roughly the same to me, and I think they are all unsafe fixes? Maybe it would be more helpful if each comment were something like "ok" or "preserved" for any comments we preserve and "deleted" or "not ok" for deleted comments.

Either that or trimming down the sheer number of cases would make this a little easier to review, at least for me.

---

_@ntBre reviewed on 2025-02-23 02:07_

Thanks for taking care of this so quickly! I have minor nits about the code and docs and one larger question about the number of tests, which I have not reviewed fully yet.

---

_Label `fixes` added by @ntBre on 2025-02-23 02:08_

---

_Label `preview` added by @ntBre on 2025-02-23 02:08_

---

_@AlexWaygood reviewed on 2025-02-23 14:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI030.py`:101 on 2025-02-23 14:31_

I agree with Brent -- I think just this first snippet would probably be sufficient to test this PR :-)

This kind of thing is something we do in lots of rules, so I think we can be pretty confident that this patch works without testing every single edge case in this rule's fixtures. And having too many tests can be confusing for future readers of the fixture -- the reader could be left thinking that the rule is more complex than they thought it was, or they could be left wondering why there are so many snippets when they all seem to be testing the same thing.

---

_@VascoSch92 reviewed on 2025-02-23 19:31_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:145 on 2025-02-23 19:31_

It makes sense. I changed ;-) 

---

_@VascoSch92 reviewed on 2025-02-23 19:32_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI030.py`:101 on 2025-02-23 19:32_

Ok, no problem. I just left the first two test-cases. I hope it is enough. Let me know

---

_Review requested from @ntBre by @VascoSch92 on 2025-02-23 19:33_

---

_Comment by @VascoSch92 on 2025-02-23 20:37_

About the `CI/mkdocs` failing: should I had a `KNOWN_FORMATTING_VIOLATIONS` ? 

---

_@AlexWaygood approved on 2025-02-23 21:19_

Thank you! I fixed the docs formatting issue :-) It was complaining that you didn't have enough spaces before an inline comment

---

_Renamed from "[PYI030] Mark fix unsafe when comments are deleted" to "[`flake8-pyi`] Mark `PYI030` fix unsafe when comments are deleted" by @AlexWaygood on 2025-02-23 21:19_

---

_Label `bug` added by @AlexWaygood on 2025-02-23 21:19_

---

_Merged by @AlexWaygood on 2025-02-23 21:22_

---

_Closed by @AlexWaygood on 2025-02-23 21:22_

---
