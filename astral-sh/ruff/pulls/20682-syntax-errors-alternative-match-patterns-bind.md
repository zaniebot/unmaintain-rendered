```yaml
number: 20682
title: "[syntax-errors] Alternative `match` patterns bind different names"
type: pull_request
state: merged
author: 11happy
labels: []
assignees: []
merged: true
base: main
head: alternate_pattern_binding
created_at: 2025-10-02T16:06:11Z
updated_at: 2025-10-27T19:31:33Z
url: https://github.com/astral-sh/ruff/pull/20682
synced_at: 2026-01-12T15:57:07Z
```

# [syntax-errors] Alternative `match` patterns bind different names

---

_@11happy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR implements semantic syntax error where alternative patterns bind different names

## Test Plan

<!-- How was it tested? -->
I have written inline tests as directed in #17412 


---

_Review requested from @MichaReiser by @11happy on 2025-10-02 16:06_

---

_Review requested from @dhruvmanila by @11happy on 2025-10-02 16:06_

---

_Comment by @11happy on 2025-10-05 06:19_

test `/ruff/crates/ty_python_semantic/resources/mdtest/import/star.md` is failing as now two syntax errors are emitted at same line line 295 & 366, Can we add multiple syntax errors on the same line ?

Thank you

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@irrefutable_case_pattern.py.snap`:436 on 2025-10-06 20:37_

It looks like something is going wrong with the error message here (```expected ``, ...```).

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__star_pattern_usage.py.snap`:624 on 2025-10-06 20:38_

These also show the empty placeholders in the error message.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1789 on 2025-10-06 20:41_

I think some other interesting cases involve more nested structure. That's the reason we have to do this check in a visitor:

```py
match x:
    case [a] | [C(x)]: ...
```

We could also add some `test_ok` cases, like `case [a] | [C(a)]: ...` which should work. You might be able to adapt some of the cases above from `duplicate_match_class_attr` if you want even deeper nesting.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1766 on 2025-10-06 21:43_

What do you think about something more like this?

```rust
                let mut previous_names: Option<FxHashSet<&ast::name::Name>> = None;
                for pattern in patterns {
                    let mut visitor = Self {
                        names: FxHashSet::default(),
                        ctx: self.ctx,
                    };
                    visitor.visit_pattern(pattern);
                    match previous_names {
                        Some(ref previous_names) => {
                            for missing_name in previous_names.symmetric_difference(&visitor.names)
                            {
                                SemanticSyntaxChecker::add_error(
                                    self.ctx,
                                    SemanticSyntaxErrorKind::AlternateBindedPattern(
                                        missing_name.to_string(),
                                        missing_name.to_string(),
                                    ),
                                    pattern.range(),
                                );
                            }
                        }
                        None => previous_names = Some(visitor.names),
                    }
                }
```

The nice thing about this is that we can avoid allocating additional `Vec`s, but the error message might be a bit worse. I'm just passing the same `missing_name` twice here, obviously we'd have to do something different. But I think an error message either like Python itself:

```pycon
>>> match 42:
...     case [x] | [C(x), C(y)]: ...
...
  File "<python-input-12>", line 2
    case [x] | [C(x), C(y)]: ...
         ^^^^^^^^^^^^^^^^^^
SyntaxError: alternative patterns bind different names
```

or like Rust:

```
error[E0408]: variable `y` is not bound in all patterns
 --> try.rs:4:9
  |
4 |         x | y => todo!(),
  |         ^   - variable not in all patterns
  |         |
  |         pattern doesn't bind `y`

error[E0408]: variable `x` is not bound in all patterns
 --> try.rs:4:13
  |
4 |         x | y => todo!(),
  |         -   ^ pattern doesn't bind `x`
  |         |
  |         variable not in all patterns
```

where we either mention zero or one variable name instead of both sets would be fine.

---

_@ntBre reviewed on 2025-10-06 21:44_

Thank you! This looks good overall. I just had a couple of suggestions about test cases and a bit of a simplification/possible performance improvement.

---

_Comment by @ntBre on 2025-10-06 21:46_

> test `/ruff/crates/ty_python_semantic/resources/mdtest/import/star.md` is failing as now two syntax errors are emitted at same line line 295 & 366, Can we add multiple syntax errors on the same line ?

Yes, I think you can stack the comments, although you may need to move the existing one to the previous line like this:

```
    # error: [invalid-syntax] "name capture `E` makes remaining patterns unreachable"
    # error: [invalid-syntax] "your error here"
    case E | F:  
        ...
```

---

_@11happy reviewed on 2025-10-08 16:55_

---

_Review comment by @11happy on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__star_pattern_usage.py.snap`:624 on 2025-10-08 16:55_

done , now I am mentioning only a single variable as rust does.

---

_@11happy reviewed on 2025-10-08 16:56_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1789 on 2025-10-08 16:56_

yes I added some more deeper nested cases & test_ok as well.

---

_@11happy reviewed on 2025-10-08 16:57_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1766 on 2025-10-08 16:57_

yeah , I agree this approach is much better & have updated accordingly & changed the diagnostic accordingly following similar to rust.

---

_Review requested from @carljm by @11happy on 2025-10-08 17:05_

---

_Review requested from @AlexWaygood by @11happy on 2025-10-08 17:05_

---

_Review requested from @sharkdp by @11happy on 2025-10-08 17:05_

---

_Review requested from @dcreager by @11happy on 2025-10-08 17:05_

---

_Comment by @11happy on 2025-10-08 17:11_

> > test `/ruff/crates/ty_python_semantic/resources/mdtest/import/star.md` is failing as now two syntax errors are emitted at same line line 295 & 366, Can we add multiple syntax errors on the same line ?
> 
> Yes, I think you can stack the comments, although you may need to move the existing one to the previous line like this:
> 
> ```
>     # error: [invalid-syntax] "name capture `E` makes remaining patterns unreachable"
>     # error: [invalid-syntax] "your error here"
>     case E | F:  
>         ...
> ```

Sure, have added accordingly.
Thank you : )

---

_Comment by @github-actions[bot] on 2025-10-08 17:23_

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

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1511 on 2025-10-15 17:06_

nit: Could we add some docs here? Just for more context, we don't currently show any of these docs anywhere, but we want to keep  them up to date in case we display them to users like rule docs in the future. That's also why most of the early ones are more elaborate with examples  and such.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1766 on 2025-10-15 17:37_

After looking at this again, I think I'd actually prefer the simpler error that Python emits for now. Using `symmetric_difference` like I suggested doesn't result in a very nice error message in some cases.

I think ideally we would emit something very similar to the rustc diagnostic, but we don't have access to an actual `Diagnostic` type in the `SemanticSyntaxChecker` at the moment. I might open a separate issue to follow up on that because it seems useful more generally.

For reference, I took a look at how Rust emits this [diagnostic](https://github.com/rust-lang/rust/blob/28d0a4a205f9e511ad2f51ee79a4aa19a704a455/compiler/rustc_resolve/src/late.rs#L3714-L3723) today. Something like you had initially actually looks closer, but again, we can't really make use of the additional information to render a nice diagnostic currently.

---

_@ntBre reviewed on 2025-10-15 17:37_

---

_@11happy reviewed on 2025-10-16 17:09_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1766 on 2025-10-16 17:09_

sure, 

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:1511 on 2025-10-16 17:09_

done

---

_@11happy reviewed on 2025-10-16 17:09_

---

_Comment by @11happy on 2025-10-16 17:15_

rebased : )

---

_Review request for @dcreager removed by @AlexWaygood on 2025-10-16 17:27_

---

_Review request for @carljm removed by @AlexWaygood on 2025-10-16 17:27_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-10-16 17:27_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-10-16 17:27_

---

_@ntBre reviewed on 2025-10-17 21:07_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:1511 on 2025-10-17 21:07_

That looks great, thank you!

---

_@ntBre approved on 2025-10-17 21:32_

Thank you!

I pushed two small commits, one using the full match pattern's range like CPython does. I think the narrower range could be helpful in the future, but the whole thing seems okay for now. And one renaming the error to `DifferentMatchPatternBindings`. I still don't totally love the name, but it's not user-facing, so we can always iterate on it if needed.

---

_Renamed from "[syntax-errors]:  implement semantic syntax error alternate_binded_pattern" to "[syntax-errors] Alternative `match` patterns bind different names" by @ntBre on 2025-10-17 21:35_

---

_Merged by @ntBre on 2025-10-17 21:35_

---

_Closed by @ntBre on 2025-10-17 21:35_

---

_Comment by @henryiii on 2025-10-27 19:31_

This change seems to now cause a SyntaxError on the following valid code that doesn't use different names:

```
invalid-syntax: alternative patterns bind different names
   --> src/sp_repo_review/checks/ruff.py:127:17
    |
125 |           match ruff:
126 |               case (
127 | /                 {"lint": {"select": x} | {"extend-select": x}}
128 | |                 | {"select": x}
129 | |                 | {"extend-select": x}
    | |______________________________________^
130 |               ):
131 |                   return cls.code in x or "ALL" in x
    |
```

See https://github.com/scientific-python/cookie/pull/663

---
