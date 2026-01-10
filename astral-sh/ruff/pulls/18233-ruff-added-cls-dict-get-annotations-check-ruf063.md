```yaml
number: 18233
title: "[`ruff`] Added `cls.__dict__.get('__annotations__')` check (`RUF063`)"
type: pull_request
state: merged
author: dericcrago
labels:
  - rule
  - python314
assignees: []
merged: true
base: main
head: 17853-cls-dict-annotations
created_at: 2025-05-20T21:25:21Z
updated_at: 2025-06-20T13:39:25Z
url: https://github.com/astral-sh/ruff/pull/18233
synced_at: 2026-01-10T18:39:08Z
```

# [`ruff`] Added `cls.__dict__.get('__annotations__')` check (`RUF063`)

---

_Pull request opened by @dericcrago on 2025-05-20 21:25_

Added `cls.__dict__.get('__annotations__')` check for Python 3.10+ and Python < 3.10 with `typing-extensions` enabled.

Closes #17853 

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Added `cls.__dict__.get('__annotations__')` check for Python 3.10+ and Python < 3.10 with `typing-extensions` enabled.

## Test Plan

`cargo test`


---

_Label `rule` added by @AlexWaygood on 2025-05-20 21:27_

---

_Comment by @AlexWaygood on 2025-05-20 21:32_

looks like you need to regenerate the JSON schema -- running `cargo dev generate-all` and committing the changes should fix it!

---

_Comment by @github-actions[bot] on 2025-05-20 21:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:103 on 2025-05-21 02:00_

I don't think you need this check. There's no particular reason the class would be stored directly in a variable and not e.g. in a list.

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:12 on 2025-05-21 02:02_

This is incorrect, the `__dict__.get` approach is reliable on all Python versions up to 3.14. (On 3.14 it will only return any annotations if the class is defined under `from __future__ import annotations`.) However, it's better to use `inspect.get_annotations` or one the other functions, because (a) it avoids accessing undocumented internals and (b) it's forward-compatible with Python 3.14.

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:24 on 2025-05-21 02:04_

This section is relevant to accessing `cls.__annotations__`, not `cls.__dict__.get("__annotations__")`. The former is unsafe in the presence of metaclasses (on 3.14+ only if `from __future__ import annotations` is used; on earlier versions always).

I think Ruff should have a separate rule against accessing `cls.__annotations__` on a class object, but that's not what this rule is about.

---

_@JelleZijlstra reviewed on 2025-05-21 02:04_

---

_@JelleZijlstra reviewed on 2025-05-21 02:20_

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:24 on 2025-05-21 02:20_

I opened #18236 about this.

---

_Comment by @dericcrago on 2025-05-21 14:11_

Thank you for the review @JelleZijlstra! I will work on these changes.

---

_@dericcrago reviewed on 2025-05-22 16:58_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:103 on 2025-05-22 16:58_

This check has been removed.

---

_@dericcrago reviewed on 2025-05-22 16:59_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:24 on 2025-05-22 16:59_

This has been removed.

---

_@dericcrago reviewed on 2025-05-22 16:59_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:12 on 2025-05-22 16:59_

This has been updated.

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:79 on 2025-05-23 02:47_

"enabled" is an odd word choice, is that used elsewhere in Ruff? I'd use "installed" (this also occurs a few other times in the file).

---

_@JelleZijlstra reviewed on 2025-05-23 02:49_

Looks good, thanks!

The error message might be a bit long with mentioning all the different `get_annotations()` functions.

---

_@dericcrago reviewed on 2025-05-23 04:50_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:79 on 2025-05-23 04:50_

Thanks for taking another look @JelleZijlstra!

You're right, the original error message was a bit lengthy. I've trimmed about 25 characters off, but I'm happy to iterate on this further if you have other suggestions for conciseness.

Regarding "enabled" vs. "installed"; thanks for pointing that out. I think some distinction and mixed use might be necessary. For example, I think that:

* the module [typing-extensions](https://pypi.org/project/typing-extensions/) should be **installed**
* the ruff setting [typing-extensions](https://docs.astral.sh/ruff/settings/#lint_typing-extensions) should be **enabled**

Does this distinction for using 'enabled' in the context of the ruff setting seem reasonable to you? I'm open to suggestions if you still feel like "installed" makes more sense or if a different term would be clearer here.

I've pushed the above changes for review.

---

_@JelleZijlstra reviewed on 2025-05-23 05:10_

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:79 on 2025-05-23 05:10_

Oh I see, didn't realize that was a reference to a Ruff setting.

---

_Review requested from @dylwil3 by @MichaReiser on 2025-05-23 14:22_

---

_Comment by @MichaReiser on 2025-05-23 14:23_

@dylwil3 could you review this PR (as the author of the issue and it being Python 3.14 related?)

---

_Label `python314` added by @MichaReiser on 2025-05-23 14:23_

---

_@dericcrago reviewed on 2025-05-23 15:37_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:79 on 2025-05-23 15:37_

That's understandable, the code was a bit ambiguous regarding the `typing-extensions` module vs setting. I'm hopeful that the changes in the last commit make that distinction clearer. Please let me know if it's still not quite hitting the mark! Thanks for sticking with me @JelleZijlstra! 

---

_Review requested from @ntBre by @ntBre on 2025-05-30 16:09_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF061.py`:16 on 2025-06-04 18:31_

nit: I don't think the trailing `# RUF061` comments are helpful here. To me that suggests that these *should* trigger a warning.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:8 on 2025-06-04 18:35_

I would personally probably lean toward using a generic variable name like `x` or `foo` instead of `<identifier>` throughout the docs. I'd also omit the `[, <default>]` here. I think a reader would have to be pretty pedantic to assume that the rule wouldn't work if they included a default :laughing: 

```suggestion
/// Checks for uses of `foo.__dict__.get("__annotations__")`
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:132 on 2025-06-04 18:40_

You might need to rebase on/merge `main`. I've made some recent changes here that will probably cause CI failures when you next push a commit. I think this will end up looking like:

```rust
checker.report_diagnostic(ClassDictAnnotations, call.range());
```

So not too bad, just something to be prepared for.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:92 on 2025-06-04 18:41_

What do you think about storing the `PythonVersion` and `typing_extensions` bool on the `ClassDictAnnotations`? Then we could offer a precise message here based on the user's actual settings.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:70 on 2025-06-04 18:41_

I would typically omit these rather than say explicitly that they aren't provided, but I'm okay with leaving them, especially if you want to add a fix in a follow-up PR.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:79 on 2025-06-04 18:42_

This is also the default, so you could omit this line if you wanted.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:74 on 2025-06-04 18:49_

Our [naming convention](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#rule-naming-convention) is to make the rule read well as "allow ${rule}" which I think this violates slightly. To me, "allow class dict annotations" reads like we're allowing you to annotate a dict like

```python
class C[T]:
    dict: T
```

or something. I guess I want to put `Get` (or maybe `Access`?) in here somewhere, but I'm not coming up with any great ideas. What do you think?

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:1027 on 2025-06-04 18:56_

I hate to say this because I know it's painful to renumber rules, but https://github.com/astral-sh/ruff/pull/17368 is also using RUF061, and https://github.com/astral-sh/ruff/pull/17281 is using RUF062. Would you mind renumbering to RUF063?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:102 on 2025-06-04 19:03_

nit: I think these trailing comments are a bit redundant with the `1. ...`, `2. ...` style comments above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:119 on 2025-06-04 19:05_

nit: I'd probably convert this part into an early return: 
```suggestion
    if !call.arguments.keywords.is_empty() || call.arguments.len() > 2 {
        return;
    }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:132 on 2025-06-04 19:10_

Then you can also do something like this:

```suggestion
    let Some(first_arg) = &call.arguments.find_positional(0) else {
        return;
    };

    let is_first_arg_correct = first_arg
        .as_string_literal_expr()
        .is_some_and(|s| s.value.to_str() == "__annotations__");
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:128 on 2025-06-04 19:11_

```suggestion
```

---

_@ntBre reviewed on 2025-06-04 19:15_

Thanks, this is looking great overall! I just have a few stylistic nits/suggestions.

---

_@dericcrago reviewed on 2025-06-08 19:31_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:102 on 2025-06-08 19:31_

removed trailing comments

---

_Review comment by @dericcrago on `crates/ruff_linter/src/codes.rs`:1027 on 2025-06-08 19:31_

renumbered to RUF063

---

_@dericcrago reviewed on 2025-06-08 19:31_

---

_@dericcrago reviewed on 2025-06-08 19:33_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:132 on 2025-06-08 19:33_

thanks for this

---

_@dericcrago reviewed on 2025-06-08 19:34_

---

_Review comment by @dericcrago on `crates/ruff_linter/resources/test/fixtures/ruff/RUF061.py`:16 on 2025-06-08 19:34_

removed trailing comments on ones that should not trigger

---

_@dericcrago reviewed on 2025-06-08 20:21_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:92 on 2025-06-08 20:21_

that's a great idea! I've implemented it in b5a45a8

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:74 on 2025-06-08 20:40_

Thanks, that's really helpful. How about using `GetAnnotationsFromClassDict` for the name?

Thinking more about your suggestion to use `Access` also made me wonder about a related case: should this rule also consider `foo.__dict__["__annotations__"]`?

Curious what you think about both of those questions. Thanks!

---

_@dericcrago reviewed on 2025-06-08 20:40_

---

_@dericcrago reviewed on 2025-06-08 20:48_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:70 on 2025-06-08 20:48_

Thanks, that makes sense. I had started working on fixes in this PR originally but pulled them out to keep this PR focused.

My plan is to create a ticket for it and handle it in an immediate follow-up PR. Does that sound good to you?

---

_@dericcrago reviewed on 2025-06-08 20:49_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:79 on 2025-06-08 20:49_

see [comment](https://github.com/astral-sh/ruff/pull/18233#discussion_r2134823374) above

---

_@ntBre reviewed on 2025-06-18 16:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:70 on 2025-06-18 16:50_

Yep, sounds good!

---

_@ntBre reviewed on 2025-06-18 16:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:74 on 2025-06-18 16:57_

I think `GetAnnotationsFromClassDict` sounds pretty good. I do like the simplicity of the current name, but a longer one is probably more precise and helpful.

And I think you're right about `foo.__dict__["__annotations__"]`. I tried both out in a Python 3.14 REPL, and they both seem to fail:

```pycon
>>> class C:
...     x: int
...
>>> C.__dict__.get("__annotations__")
>>> C.__annotations__
{'x': <class 'int'>}
>>> C.__dict__.get("__annotations__")
>>> C.__dict__["__annotations__"]
Traceback (most recent call last):
  File "<python-input-8>", line 1, in <module>
    C.__dict__["__annotations__"]
    ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^
KeyError: '__annotations__'
```

whereas they both work on 3.13:

```pycon
>>> class C:
...     x: int
...
>>> C.__dict__["__annotations__"]
{'x': <class 'int'>}
>>> C.__dict__["__annotations__"]
{'x': <class 'int'>}
>>> C.__dict__.get("__annotations__")
{'x': <class 'int'>}
```

Hopefully it's not too much work to check that too!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:88 on 2025-06-18 17:02_

Tiny nit: you could move `Use ` to the `format!` call since it's shared across these branches.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:141 on 2025-06-18 17:04_

I think you can unwrap this block, the braces might be left over from an earlier version.

---

_@ntBre approved on 2025-06-18 17:06_

Thanks, this looks great! Just a couple of small nits, the merge conflicts, and possibly extending the rule to `__dict__["__annotations__"]` as you said (and renaming). This is otherwise good to go!

---

_@dericcrago reviewed on 2025-06-19 06:43_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:141 on 2025-06-19 06:43_

this should be resolved with the latest commits

---

_@dericcrago reviewed on 2025-06-19 06:43_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:88 on 2025-06-19 06:43_

this should be resolved with the latest commits

---

_@dericcrago reviewed on 2025-06-19 06:57_

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:74 on 2025-06-19 06:57_

Thanks for confirming the behavior on different Python versions!

When I went to add the check for `foo.__dict__["__annotations__"]`, I ended up splitting the logic into `access_annotations_from_class_dict_with_get` and `access_annotations_from_class_dict_by_key` to handle both access methods. At that point, `AccessAnnotationsFromClassDict` felt like a more fitting name, so I went with that.

Let me know what you think of this approach. I wasn't aware of an existing pattern for handling two different scenarios, so I'm happy to refactor if you have a better idea!

---

_Review comment by @dericcrago on `crates/ruff_linter/src/rules/ruff/rules/class_dict_annotations.rs`:70 on 2025-06-19 07:10_

Created #18785 

---

_@dericcrago reviewed on 2025-06-19 07:10_

---

_Review requested from @ntBre by @MichaReiser on 2025-06-19 14:30_

---

_@ntBre approved on 2025-06-20 13:27_

This is great, thank you!

---

_Renamed from "Added `cls.__dict__.get('__annotations__')` check" to "[`ruff`] Added `cls.__dict__.get('__annotations__')` check (`RUF063`)" by @ntBre on 2025-06-20 13:32_

---

_Merged by @ntBre on 2025-06-20 13:32_

---

_Closed by @ntBre on 2025-06-20 13:32_

---

_Branch deleted on 2025-06-20 13:39_

---
