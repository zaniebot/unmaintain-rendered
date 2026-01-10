```yaml
number: 11486
title: "[flake8-pyi] Implement PYI057"
type: pull_request
state: merged
author: tomasr8
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pyi057
created_at: 2024-05-21T21:56:00Z
updated_at: 2024-05-29T10:14:21Z
url: https://github.com/astral-sh/ruff/pull/11486
synced_at: 2026-01-10T21:55:59Z
```

# [flake8-pyi] Implement PYI057

---

_Pull request opened by @tomasr8 on 2024-05-21 21:56_

## Summary

Adds [Y057](https://github.com/PyCQA/flake8-pyi/blob/main/ERRORCODES.md) from flake8-pyi.

I don't think we can apply any autofix here?

I'm new to rust so apologies in advance if I've made some rookie mistakes :sweat_smile: 

## Test Plan

`cargo test` + `cargo insta review`


---

_Review requested from @AlexWaygood by @tomasr8 on 2024-05-21 21:56_

---

_Comment by @github-actions[bot] on 2024-05-21 22:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/b2f6e5221b180d42a3770543a72036baa52b3907/stdlib/_collections_abc.pyi#L10'>stdlib/_collections_abc.pyi:10:5:</a> PYI057 Do not use `typing.ByteString`, which has unclear semantics and is deprecated
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI057 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @zanieb on 2024-05-21 22:17_

Hi! The primary reviewer for this is out right now so it might be a bit slower than usual to get reviewed.

Could you change the rule to be in `Preview` instead of the `Stable` group?

Additionally, I think use of `resolve_qualified_name` might be helpful in your implementation:

https://github.com/astral-sh/ruff/blob/f79c980e17bf1ef6064a589dfc5e6b1c288e860f/crates/ruff_python_semantic/src/model.rs#L726-L741

---

_Comment by @zanieb on 2024-05-21 22:17_

Welcome to the project :)

---

_Comment by @tomasr8 on 2024-05-21 22:29_

> Additionally, I think use of resolve_qualified_name might be helpful in your implementation:

I thought there might be something like this already, but couldn't find it. Thanks!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bytestring_usage.rs`:9 on 2024-05-27 10:34_

```suggestion
/// Checks for uses of `typing.ByteString` or `collections.abc.ByteString`.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bytestring_usage.rs`:15 on 2024-05-27 10:37_

It's true that the Python docs don't actually mention this, but it might be worth mentioning here that `typing_extensions` backports this for older Python versions:

```suggestion
/// `collections.abc.Buffer` (or the `typing_extensions` backport
/// on Python <3.12) or a union like `bytes | bytearray | memoryview` instead.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bytestring_usage.rs`:56 on 2024-05-27 11:00_

This message should be a succinct summary of what the user should do to fix the violation (if an autofix is available, and you're using e.g. VSCode, double-clicking on this message when you hover over the violation in the editor will apply the autofix).

In this case I think I'd probably not implement this method at all! Since we're not offering an autofix, it's not necessary. And it's hard for us to tell exactly what the best fix is in any one case -- in some cases it might be to use `collections.abc.Buffer`, in some cases it might be to use `typing_extensions.Buffer`, and in some cases it might be to use a union. (The ambiguity is precisely why we've deprecated `ByteString`!)

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bytestring_usage.rs`:32 on 2024-05-27 11:18_

Rather than storing the full name as a string on the violation struct, I'd be tempted to do something like this, so that we only record the module where `ByteString` came from:

```rs
#[violation]
pub struct ByteStringUsage {
    origin: ByteStringOrigin,
}

impl Violation for ByteStringUsage {
    const FIX_AVAILABILITY: FixAvailability = FixAvailability::None;

    #[derive_message_formats]
    fn message(&self) -> String {
        let ByteStringUsage { origin } = self;
        format!("Do not use `{origin}.ByteString`, which has unclear semantics and is deprecated")
    }
}

#[derive(Debug, Clone, Copy, Eq, PartialEq)]
enum ByteStringOrigin {
    Typing,
    CollectionsAbc,
}

impl std::fmt::Display for ByteStringOrigin {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        f.write_str(match self {
            Self::Typing => "typing",
            Self::CollectionsAbc => "collections.abc",
        })
    }
}
```

The diagnostic has the invariant that the error message will always be the same, except for the detail of where `ByteString` is coming from; by using an enum here, we make that clearer for readers of the code by encoding the invariant in the type. Using an enum also means that the diagnostic struct will take up less space in memory -- that's not that important here, as the diagnostic structs aren't performance-critical, but it's always nice to use less memory where possible :-)

---

_@AlexWaygood reviewed on 2024-05-27 11:19_

Great to see you around here @tomasr8! :-D Overall this looks great.

---

_Comment by @tomasr8 on 2024-05-29 09:49_

oops didn't notice you already fixed the conflict :sweat_smile: 

---

_Comment by @AlexWaygood on 2024-05-29 09:49_

hehe, no worries

---

_Comment by @tomasr8 on 2024-05-29 09:53_

Thanks for the review! (as always :D)

I removed the `fix_title` implementation and used an enum instead of the `full_name` string as you suggested.

The `bytestring_import` function should also be a bit more efficient now since I check for the module id before entering the loop and return early if it doesn't match.


---

_@AlexWaygood approved on 2024-05-29 10:01_

Thanks very much! Looks great now. I just pushed a couple of nitpick-fixes to your branch :-)

---

_Label `rule` added by @AlexWaygood on 2024-05-29 10:01_

---

_Label `preview` added by @AlexWaygood on 2024-05-29 10:01_

---

_@AlexWaygood reviewed on 2024-05-29 10:03_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bytestring_usage.rs`:68 on 2024-05-29 10:03_

@tomasr8: I added this in my latest push. It's an optimisation we do in a lot of rules. It's a very cheap check to see whether any of the relevant modules we care about have been imported at all. If not, we can just short-circuit the rest of the rule :-)

---

_Merged by @AlexWaygood on 2024-05-29 10:04_

---

_Closed by @AlexWaygood on 2024-05-29 10:04_

---

_@tomasr8 reviewed on 2024-05-29 10:07_

---

_Review comment by @tomasr8 on `crates/ruff_linter/src/rules/flake8_pyi/rules/bytestring_usage.rs`:68 on 2024-05-29 10:07_

Ah cool! I'll keep that in mind for future PRs :)

---

_Branch deleted on 2024-05-29 10:07_

---
