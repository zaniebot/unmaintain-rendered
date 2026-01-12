```yaml
number: 18645
title: "[`refurb`] Make the fix for `FURB163` unsafe for `log2`, `log10`, `*args`, and deleted comments"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix/furb163
created_at: 2025-06-12T11:51:42Z
updated_at: 2025-06-16T18:17:50Z
url: https://github.com/astral-sh/ruff/pull/18645
synced_at: 2026-01-12T15:56:23Z
```

# [`refurb`] Make the fix for `FURB163` unsafe for `log2`, `log10`, `*args`, and deleted comments

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
/closes #18639
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
update snapshots
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-12 11:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @chirizxc on 2025-06-12 12:12_

do we have to update doc as well? because examples from the [ussie](https://github.com/astral-sh/ruff/issues/18639#issue-3138599071) and the doc are different

i mean 
![изображение](https://github.com/user-attachments/assets/3846f580-d8d0-4553-8c8e-d569b5d5387e)
![изображение](https://github.com/user-attachments/assets/7afe0034-0f7f-4aa7-9431-273b071f135c)


---

_Comment by @chirizxc on 2025-06-12 12:27_

although it should only work for float

---

_@chirizxc reviewed on 2025-06-12 12:46_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:139 on 2025-06-12 12:46_

i'm not sure, but maybe such a `is_float` already exists somewhere in the helper or something like that

---

_Comment by @chirizxc on 2025-06-12 13:19_

there's also a question about [ruff_python_ast/src/generated.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/src/generated.rs) file
![изображение](https://github.com/user-attachments/assets/a3ba28ec-3ae2-4698-9037-dd322b33e162)

rustrover writes that `crate::` is not needed in 834 places, should the `crates/ruff_python_ast/generate.py` itself be corrected?


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:139 on 2025-06-12 17:24_

There's this:

https://github.com/astral-sh/ruff/blob/4cfaa39412ab33825c834f7da4bca3f96e550ba1/crates/ruff_python_semantic/src/analyze/typing.rs#L981-L984

which would work on a `Binding` if you wanted to resolve variables too, but do we need to check if the argument is a float? You can see the same floating point behavior with less exotic arguments:

```pycon
>>> math.log2(10)
3.321928094887362
>>> math.log(10, 2)
3.3219280948873626
```

and I think we're already checking for number literals.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:156 on 2025-06-12 17:25_

nit: I think you can use `arg.is_starred_expr` here.

---

_@ntBre reviewed on 2025-06-12 17:32_

Thanks! I just had a couple of suggestions.

And yes, we need to update the documentation with a `## Fix safety` section because the fix is now unsafe in many cases.

---

_Comment by @ntBre on 2025-06-12 17:33_

> there's also a question about [ruff_python_ast/src/generated.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/src/generated.rs) file ![изображение](https://private-user-images.githubusercontent.com/109767616/454415857-a3ba28ec-3ae2-4698-9037-dd322b33e162.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDk3NDk4NDgsIm5iZiI6MTc0OTc0OTU0OCwicGF0aCI6Ii8xMDk3Njc2MTYvNDU0NDE1ODU3LWEzYmEyOGVjLTNhZTItNDY5OC05MDM3LWRkMzIyYjMzZTE2Mi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNjEyJTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDYxMlQxNzMyMjhaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0zZjBjNzgyZjIyYzhlMjkwMTQxYmI5MzU2OTYwZjZjYzg3MDI0MDgwZDI1NjUxMzJiZjllMzgwNDYwM2FhNjYzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.I_VCJxgt8BZUJ1xaFWKyVcgtMCxA-WCvp0LDwG6Kf1M)
> 
> rustrover writes that `crate::` is not needed in 834 places, should the `crates/ruff_python_ast/generate.py` itself be corrected?

This is interesting, I guess clippy doesn't warn about this. We could probably open a separate issue or PR for this, if you wanted!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:43 on 2025-06-12 19:42_

I think I would treat these two cases separately. My understanding is that the fix is unsafe when comments are in the replacement range because they will be deleted, which seems a bit separate from either semantics or readability.

And I don't think the `*args` case has any effect on readability, it "just" changes the semantics of the code by raising an error when a different number of arguments are passed.

---

_@ntBre approved on 2025-06-12 19:51_

Nice! One more docs nit, but this is otherwise good to go.

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:43 on 2025-06-12 20:05_

if write it like this, i think it would make more sense:

This fix is marked unsafe when the argument is a starred expression, as this changes the call semantics and may raise runtime errors. It is also unsafe if comments are present within the call, as they will be removed. Additionally, `math.log(x, base)` and `math.log2(x)` / `math.log10(x)` may differ due to floating-point rounding.

---

_@chirizxc reviewed on 2025-06-12 20:05_

---

_Label `fixes` added by @MichaReiser on 2025-06-13 06:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/redundant_log_base.rs`:44 on 2025-06-16 18:07_

```suggestion
/// and `math.log2(x)` / `math.log10(x)` may differ due to floating-point rounding, so 
/// the fix is also unsafe when making this transformation.
```

---

_@ntBre approved on 2025-06-16 18:07_

Thanks!

---

_Renamed from "[`refurb`] make fix for FURB163 (`redundant-log-base`) unsafe if log2/log10, starred arg, or comments" to "[`refurb`] Make the fix for `FURB163` unsafe for `log2`, `log10`, `*args`, and deleted comments" by @ntBre on 2025-06-16 18:09_

---

_Merged by @ntBre on 2025-06-16 18:13_

---

_Closed by @ntBre on 2025-06-16 18:13_

---

_Branch deleted on 2025-06-16 18:17_

---
