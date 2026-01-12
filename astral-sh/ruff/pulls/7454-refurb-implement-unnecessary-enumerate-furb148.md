```yaml
number: 7454
title: "[`refurb`] Implement `unnecessary-enumerate` (`FURB148`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: unnecessary-enumerate
created_at: 2023-09-17T11:24:42Z
updated_at: 2023-09-25T13:43:58Z
url: https://github.com/astral-sh/ruff/pull/7454
synced_at: 2026-01-12T02:39:10Z
```

# [`refurb`] Implement `unnecessary-enumerate` (`FURB148`)

---

_Pull request opened by @tjkuson on 2023-09-17 11:24_

## Summary

Implement [`no-ignored-enumerate-items`](https://github.com/dosisod/refurb/blob/master/refurb/checks/builtin/no_ignored_enumerate.py) as `unnecessary-enumerate` (`FURB148`). 

The auto-fix considers if a `start` argument is passed to the `enumerate()` function. If only the index is used, then the suggested fix is to pass the `start` value to the `range()` function. So,

```python
for i, _ in enumerate(xs, 1):
    ...
```

becomes

```python
for i in range(1, len(xs)):
    ...
```

If the index is ignored and only the value is ignored, and if a start value greater than zero is passed to `enumerate()`, the rule doesn't produce a suggestion. I couldn't find a unanimously accepted best way to iterate over a collection whilst skipping the first n elements. The rule still triggers, however.

Related to #1348.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-09-17 11:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Marked ready for review by @tjkuson on 2023-09-17 12:31_

---

_Comment by @Skylion007 on 2023-09-17 15:00_

>  I couldn't find a unanimously accepted best way to iterate over a collection whilst skipping the first n elements.

Isn't this explicitly what [itertools.islice](https://docs.python.org/3/library/itertools.html#itertools.islice) is designed for? Specifically, `itertools.islice(start_index, None)`.

---

_Comment by @tjkuson on 2023-09-17 19:57_

> Isn't this explicitly what [itertools.islice](https://docs.python.org/3/library/itertools.html#itertools.islice) is designed for? Specifically, `itertools.islice(start_index, None)`.

Yes, and it's what I would personally do. But, [looking online](https://stackoverflow.com/questions/10079216/skip-first-entry-in-for-loop-in-python), there are alternative methods without a consensus over which is best. My concern is that a non-trivial number of people would consider an `itertools.isclice` auto-fix (or alternative, for that matter) unexpected behaviour. 

Very happy to adapt the merge request to add an `islice` auto-fix if the maintainers support it, however!

EDIT: The `islice` solution has the advantage of working on all iterables, whereas solutions like iterating over a `[n:]` sliced sequence won't work on all iterables. Could be a reason to settle on this fix over others?

---

_Comment by @Skylion007 on 2023-09-18 02:02_

The first upvoted answer wouldn't work on generators. The 2nd upvoted
answer is verbose and just suggests calling next on the iterator, leaving
islice as the most upvoted answer in that StackOverflow post.

On Sun, Sep 17, 2023 at 3:57 PM Tom Kuson ***@***.***> wrote:

> Isn't this explicitly what itertools.islice
> <https://docs.python.org/3/library/itertools.html#itertools.islice> is
> designed for? Specifically, itertools.islice(start_index, None).
>
> Yes, and it's what I would personally do. But, looking online
> <https://stackoverflow.com/questions/10079216/skip-first-entry-in-for-loop-in-python>,
> there are alternative methods without a consensus over which is best. My
> concern is that a non-trivial number of people would consider an
> itertools.isclice auto-fix (or alternative, for that matter) unexpected
> behaviour.
>
> Very happy to adapt the merge request to add an islice auto-fix if the
> maintainers support it, however!
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/pull/7454#issuecomment-1722555214>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAPVMX6MLD56J5SXFBJCRMLX25I35ANCNFSM6AAAAAA43RTTEQ>
> .
> You are receiving this because you commented.Message ID:
> ***@***.***>
>


---

_Review comment by @dhruvmanila on `crates/ruff_python_semantic/src/helpers.rs`:7 on 2023-09-18 05:45_

I wonder if this could be made a method on the `SemanticModel`:
```rust
impl SemanticModel {
	// ...

	pub fn is_expr_unused(&self, expr: &Expr) -> bool {
		// ...
	}	
}

checker.semantic().is_expr_unused(expr)
```

But then it would mean that we would need to visit all relevant expressions in this context which I think is out of scope for this PR.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/refurb/rules/unnecessary_enumerate.rs`:65 on 2023-09-18 05:50_

You could probably just use `map` instead:
```suggestion
        if let Some(suggestion) = iterable_suggestion.map(SourceCodeSnippet::full_display)
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/refurb/rules/unnecessary_enumerate.rs`:80 on 2023-09-18 05:50_

Same suggestion as above.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/refurb/rules/unnecessary_enumerate.rs`:124 on 2023-09-18 05:54_

nit: maybe we could move this to the respective match branch where it's being used.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/refurb/rules/unnecessary_enumerate.rs`:35 on 2023-09-18 05:57_

I believe you meant this:

```suggestion
/// ```python
/// for index, _ in enumerate(sequence):
///     print(index)
///
/// for _, value in enumerate(sequence):
///     print(value)
/// ```
```

---

_@dhruvmanila approved on 2023-09-18 05:58_

This is looking good, thanks for doing this!

I think the implementation is similar to https://docs.astral.sh/ruff/rules/incorrect-dict-iterator/, maybe in the future we could combine and parameterize the implementation.

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-09-18 05:59_

---

_Label `rule` added by @dhruvmanila on 2023-09-18 05:59_

---

_@tjkuson reviewed on 2023-09-18 08:22_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/refurb/rules/unnecessary_enumerate.rs`:35 on 2023-09-18 08:22_

The original intent was to demonstrate that the logic does more than check for underscores, but I think committing your suggestion makes the rule's intent more explicit (more important in documentation).

---

_@tjkuson reviewed on 2023-09-18 08:44_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/refurb/rules/unnecessary_enumerate.rs`:65 on 2023-09-18 08:44_

The suggestion causes a type mismatch, is there a better way?

---

_Comment by @charliermarsh on 2023-09-19 20:07_

Thanks! I made some modifications. I think the `start` argument to enumerate just changes the _value_ of the index, it doesn't skip the first `start` elements. Notice how in [this example](https://docs.python.org/3/library/functions.html#enumerate) from the docs, they iterate over the same arguments, but the index is different. So I changed the fix in "unused index" case to just ignore `start` altogether, and I changed the `unused value` case to avoid fixing if `start` was set to something other than the default. 

---

_Merged by @charliermarsh on 2023-09-19 20:19_

---

_Closed by @charliermarsh on 2023-09-19 20:19_

---

_Comment by @tjkuson on 2023-09-20 08:44_

Oops, that was silly of me. Thanks!

---

_Branch deleted on 2023-09-20 08:44_

---

_Comment by @Skylion007 on 2023-09-25 13:43_

FYI @tjkuson , the fix generates invalid code when the arg is a generator: https://github.com/astral-sh/ruff/issues/7656

---
