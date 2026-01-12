```yaml
number: 8706
title: "Add Implementation for Pylint E1132: Repeated Keyword"
type: pull_request
state: merged
author: AlexBieg
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: Pylint-E1132
created_at: 2023-11-15T23:01:27Z
updated_at: 2023-11-19T00:33:11Z
url: https://github.com/astral-sh/ruff/pull/8706
synced_at: 2026-01-12T15:55:26Z
```

# Add Implementation for Pylint E1132: Repeated Keyword

---

_@AlexBieg_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds the Pylint rule E1132 to check for repeated keyword arguments in a function call.

## Test Plan

Tested via the included unit tests and manual spot checking.


---

_Comment by @github-actions[bot] on 2023-11-15 23:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2023-11-16 01:03_

Awesome, thanks for this PR and for getting involved!

There's _some_ overlap here with https://github.com/astral-sh/ruff/pull/8450, I'm almost wondering if they should one rule since they both only deal with static dictionary spreads, but I think I need to take a closer look at the code before I have a clear answer to that.

---

_Label `rule` added by @charliermarsh on 2023-11-16 01:03_

---

_Comment by @AlexBieg on 2023-11-16 17:23_

Oh yeah! I totally did not see that other PR. I'm happy to help merge stuff if we go down that route. 

I also should have mentioned this in the summary, but this is my first meaningful Rust code besides random side projects, so feel free to be extra nit-picky with your comments 😄 

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/repeated_keywords.rs`:67 on 2023-11-18 01:22_

Since you asked for nitpicks: it'd be more common to express this as `keywords: &[Keyword]` -- a slice, rather than a vector. You wouldn't need to change the call-site at all, but a slice is basically a more "permissive" type: you can always make a slice from a vector, but you can't go the other way around (if you have a slice like `&[Keyword]`, you can't pass that to a function that takes `&Vec<Keyword>`).

---

_@charliermarsh reviewed on 2023-11-18 01:22_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/repeated_keywords.rs`:37 on 2023-11-18 01:23_

I think more common would be to destructure here, like:

```rust
let Self { duplicate_keyword} = self;
format!("Repeated keyword argument: `{duplicate_keyword}`")
```

---

_@charliermarsh reviewed on 2023-11-18 01:23_

---

_@charliermarsh reviewed on 2023-11-18 01:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`:780 on 2023-11-18 01:25_

Most rules don't yet follow this pattern, but we tend to prefer passing `call` here in lieu of `keywords`, and then accessing `call.keywords` as-needed within `repeated_keywords`. The benefit is that we ensure callers pass an AST node of the right type, rather than passing some arbitrary vector of keywords. That benefit isn't so clear in _this_ case, but consider the call to `nested_min_max` right above, where the third argument is just an arbitrary `&Expr`. A caller could pass _any_ expression, but in truth, we'd prefer that call to take an `ExprCall`.

---

_@charliermarsh reviewed on 2023-11-18 01:33_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/repeated_keywords.rs`:93 on 2023-11-18 01:33_

My advice would be to drop `generate_record_func`, and rewrite this as:

```rust
pub(crate) fn repeated_keywords(checker: &mut Checker, keywords: &Vec<Keyword>) {
    let mut seen =
        FxHashSet::with_capacity_and_hasher(keywords.len(), BuildHasherDefault::default());

    for keyword in keywords {
        if let Some(id) = &keyword.arg {
            if !seen.insert(id.as_str()) {
                checker.diagnostics.push(Diagnostic::new(
                    RepeatedKeywords {
                        duplicate_keyword: id.to_string(),
                    },
                    keyword.range(),
                ));
            }
        } else if let Expr::Dict(ExprDict {
            // We only want to check dict keys if there is NO arg associated with them
            keys,
            range: _,
            values: _,
        }) = &keyword.value
        {
            for key in keys.iter().flatten() {
                if let Expr::StringLiteral(ExprStringLiteral {
                    value,
                    range: _,
                    unicode: _,
                    implicit_concatenated: _,
                }) = key
                {
                    if !seen.insert(value.as_str()) {
                        checker.diagnostics.push(Diagnostic::new(
                            RepeatedKeywords {
                                duplicate_keyword: value.to_string(),
                            },
                            key.range(),
                        ));
                    }
                }
            }
        }
    }
}
```

There are a couple distinct changes here, so I'll walk through them one by one:

1. We tend to use `FxHashSet` over the standard library's `HashSet`. It tends to perform much better for smaller inputs.
2. You can initialize the hash set with a known capacity, which avoids resizing it unnecessarily for cases in which we know its approximate size.
3. In Rust, `seen.insert` returns a boolean (`false` if the element was already in the set). So we can both insert the element and test if it was already present in a single operation. The behavior here _is_ different than in your version, since we'll now flag the same argument multiple times if it's repeated more than twice, but honestly I think that's fine.
4. We can use a `FxHashSet<&str>` rather than an `FxHashSet<String>`, since we know the data will live longer than the hash set -- so there's no need to allocate a new String and have the hash set own the data. It can just use references.
5. I think the use of `Box`, `dyn`, etc. is not totally necessary since we're only calling this function twice, and after the refactor its been reduced to just a couple lines.

Happy to answer any questions!

---

_Comment by @charliermarsh on 2023-11-18 01:33_

Okay, it's probably fine to just proceed with this rule on its own. Left a few comments!

---

_@AlexBieg reviewed on 2023-11-18 20:33_

---

_Review comment by @AlexBieg on `crates/ruff_linter/src/rules/pylint/rules/repeated_keywords.rs`:93 on 2023-11-18 20:33_

That makes a ton of sense. Thanks for such a detailed review! Needing to use the `Box` and `dyn` in what was logically a pretty simple function felt wrong, so I'm glad there was a more straightforward way of doing things

---

_Comment by @AlexBieg on 2023-11-18 20:39_

@charliermarsh Okay this is ready for another pass. All of your comments should be addressed. I also updated some of my destructuring statements to use `..` so that unused values weren't just destructured into `_` variables. I think it made it easier to read, but happy to change it back if it's not the preferred style. 

---

_@charliermarsh approved on 2023-11-19 00:21_

Thanks!

---

_Label `preview` added by @charliermarsh on 2023-11-19 00:22_

---

_Merged by @charliermarsh on 2023-11-19 00:26_

---

_Closed by @charliermarsh on 2023-11-19 00:26_

---
