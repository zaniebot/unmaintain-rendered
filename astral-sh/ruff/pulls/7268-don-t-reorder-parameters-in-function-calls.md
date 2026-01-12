```yaml
number: 7268
title: "Don't reorder parameters in function calls"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: dont-reorder-parameters-in-function-calls
created_at: 2023-09-11T12:05:57Z
updated_at: 2023-09-13T09:08:10Z
url: https://github.com/astral-sh/ruff/pull/7268
synced_at: 2026-01-12T02:45:39Z
```

# Don't reorder parameters in function calls

---

_Pull request opened by @konstin on 2023-09-11 12:05_

## Summary

In `f(*args, a=b, *args2, **kwargs)` the args (`*args`, `*args2`) and keywords (`a=b`, `**kwargs`) are interleaved, which we previously didn't handle.

Fixes #6498

**main**

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| **django**       |           0.99966 |              2760 |                58 |
| transformers |           0.99930 |              2587 |               447 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99825 |               648 |                22 |
| zulip        |           0.99950 |              1437 |                27 |

**PR**

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| **django**       |           0.99967 |              2760 |                53 |
| transformers |           0.99930 |              2587 |               447 |
| twine        |           1.00000 |                33 |                 0 |
| typeshed     |           0.99983 |              3496 |                18 |
| warehouse    |           0.99825 |               648 |                22 |
| zulip        |           0.99950 |              1437 |                27 |


## Test Plan

New fixtures


---

_@konstin reviewed on 2023-09-11 12:07_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:45 on 2023-09-11 12:07_

This isn't really part of the PR but i got confused so i split this into it's own function (no functional changes). It's a separate commit already i can easily split the branch if you want

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/other/arguments.rs`:66 on 2023-09-11 12:08_

i unfortunately didn't find any pretty way to write this. itertool's `merge_by` also doesn't work due to different type

---

_@konstin reviewed on 2023-09-11 12:08_

---

_Label `formatter` added by @konstin on 2023-09-11 12:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:45 on 2023-09-11 13:45_

I think I would prefer if you write it as (I'm somewhat certain that this should work):

```rust
fn format_function_header(item: &StmtFunctionDef) -> impl Format<PyFormatContext<'_>> {
	format_with(move |f| {
		... 
	})
}
```

It avoids the nested function call. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:69 on 2023-09-11 13:47_

Isn't it guaranteed that `args.next()` must always return a value here?


```suggestion
                            if let Some(arg) = args.next().unwrap() {
```



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@expression__call.py.snap`:259 on 2023-09-11 13:49_

Can we add some tests with comments too?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:66 on 2023-09-11 13:54_

Considerably more code but avoids `peekable`. Not sure if it easier to understand due to avoiding `map`

```rust
let mut args = args.iter();
let mut keywords = keywords.iter();

let mut current_arg = args.next();
let mut current_keyword = keywords.next();

while let (Some(arg), Some(keyword)) = (current_arg, current_keyword) {
    if arg.start() < keyword.start() {
        joiner.entry(arg, &arg.format());
        current_arg = args.next();
    } else {
        joiner.entry(keyword, &keyword.format());
        current_keyword = keywords.next();
    }
}

for arg in current_arg.into_iter().chain(args) {
    joiner.entry(arg, &arg.format());
}

for keyword in current_keyword.into_iter().chain(keywords) {
    joiner.entry(keyword, &keyword.format());
}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:71 on 2023-09-11 13:55_

Nit: Preserve is the default

```suggestion
                                    .entry(arg, &arg.format();
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:61 on 2023-09-11 13:55_

Nit: It may be worth explaining that the args and kwargs are formatted in their appearance in source and that the code here is a merge sort.

---

_@MichaReiser approved on 2023-09-11 13:56_

Can you add the similarity index comparison between the baseline and this PR?

---

_Comment by @konstin on 2023-09-12 09:48_

Current dependencies on/for this PR:
* main
  * **PR #7302** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7302" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #7268** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/7268" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/7268?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-09-12 10:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@MichaReiser reviewed on 2023-09-12 11:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/arguments.rs`:66 on 2023-09-12 11:14_

This will be nice and clean with your `ArgOrKeyword` abstraction :)

---

_Renamed from "Dont reorder parameters in function calls" to "Don't reorder parameters in function calls" by @charliermarsh on 2023-09-12 12:56_

---

_Converted to draft by @konstin on 2023-09-12 13:44_

---

_@konstin reviewed on 2023-09-12 15:19_

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/format@expression__call.py.snap`:531 on 2023-09-12 15:19_

these get switched but that seems fine to me

---

_Marked ready for review by @konstin on 2023-09-12 15:19_

---

_@konstin reviewed on 2023-09-13 08:36_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_function_def.rs`:45 on 2023-09-13 08:36_

i prefer the flatter function body

---

_Merged by @konstin on 2023-09-13 09:01_

---

_Closed by @konstin on 2023-09-13 09:01_

---

_Branch deleted on 2023-09-13 09:01_

---
