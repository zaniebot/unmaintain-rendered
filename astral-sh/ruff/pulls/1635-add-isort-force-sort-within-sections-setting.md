```yaml
number: 1635
title: Add isort.force-sort-within-sections setting
type: pull_request
state: merged
author: mattoberle
labels: []
assignees: []
merged: true
base: main
head: isort/force-sort-within-sections
created_at: 2023-01-04T19:54:46Z
updated_at: 2023-01-09T18:24:09Z
url: https://github.com/astral-sh/ruff/pull/1635
synced_at: 2026-01-12T05:36:32Z
```

# Add isort.force-sort-within-sections setting

---

_Pull request opened by @mattoberle on 2023-01-04 19:54_

This commit is a first attempt at addressing issue #1003.

The default `isort` behavior is `force-sort-within-sections = false`,
which places `from X import Y` statements after `import X` statements.

When `force-sort-within-sections = true` all imports are sorted by module name.
When module names are equivalent, the `import` statement comes before the `from` statement.
- https://pycqa.github.io/isort/docs/configuration/options.html#force-sort-within-sections

~Initially I tried out an `enum` + `match` approach but I was struggling to come up with anything that didn't require `Copy` support.~

---

~The approach in this PR _works_ from what I've been able to test...
But it's lacking in clarity.
The `merge_imports` function returns a vector of tuples that represent the final import order:~

```
[(0 or 1, idx)]
```

~Where...~
- ~`0` means use the `import_block.import` vector with index `idx`~
- ~`1` means use the `import_block.import_from` vector with index `idx`~

~Those integers are essentially a stand-in for a type or enum, I'm just not sure what the idiomatic thing to return would be.~

- ~An new enum like `ImportType::ImportFrom / ImportType::Import` defined in `types.rs`?~
- ~Is there a better way to do this lazily (eg. return an iterator) rather than eagerly construct the vector in `merge_imports`?~

**Edit:** Swapped out the previous approach with an iterator and enum match.

---

_@mattoberle reviewed on 2023-01-04 22:31_

---

_Review comment by @mattoberle on `src/isort/sorting.rs`:85 on 2023-01-04 22:31_

Guessing that `.unwrap()` could be unsafe here, although I don't understand the case that results in `module` being `None`.

---

_@andersk reviewed on 2023-01-04 22:34_

---

_Review comment by @andersk on `src/isort/sorting.rs`:85 on 2023-01-04 22:34_

That happens for `from . import foo` (or `..`, `...`, etc.).

---

_@mattoberle reviewed on 2023-01-04 22:56_

---

_Review comment by @mattoberle on `src/isort/sorting.rs`:85 on 2023-01-04 22:56_

Makes sense.
I threw one of those into my test cases but I suppose that since the block in the case is only homogeneous `import from` statements we never hit these middle conditions. 

---

_@mattoberle reviewed on 2023-01-04 23:37_

---

_Review comment by @mattoberle on `src/isort/sorting.rs`:85 on 2023-01-04 23:37_

I dug into this a bit.

Given the current callers, `.unwrap()` will never return `None` because relative imports _always_ use the `from X import Y` syntax. An import like `import .my_module` is invalid.
Relative imports are sorted in their own section and aren't compared to other first-party imports.

That doesn't necessarily guarantee this code path won't be hit with a missing module given a different set of settings down the road.

What's the best way to handle this, something like?
```rs
fn cmp_option_str(a: Option<&String>, b: Option<&String>) -> Ordering {
    match (a, b) {
        (None, None) => Ordering::Equal,
        (None, Some(_)) => Ordering::Less,
        (Some(_), None) => Ordering::Greater,
        (Some(x), Some(y)) => natord::compare_ignore_case(x, y),
    }
}
```

---

_Comment by @charliermarsh on 2023-01-05 19:11_

Will review this today, thank you!

---

_Review requested from @charliermarsh by @charliermarsh on 2023-01-05 19:11_

---

_Review comment by @charliermarsh on `src/isort/mod.rs`:610 on 2023-01-06 02:22_

Nit: can you destructure directly? (Like: `AnyImport::Import((alias, comments))`?)

---

_Review comment by @charliermarsh on `src/isort/mod.rs`:601 on 2023-01-06 02:23_

Maybe instead, `let mut it`, and then call `sorted_by` on it if `force_sort_within_sections`? That way, we avoid the oddity of `else { Ordering::Greater }`.

---

_Review comment by @charliermarsh on `src/isort/sorting.rs`:85 on 2023-01-06 02:23_

Yeah we should do something like what you have there, just to be safe. You can see some examples of that `None` handling in functions like `cmp_level`.

---

_@charliermarsh reviewed on 2023-01-06 02:23_

---

_@mattoberle reviewed on 2023-01-06 17:29_

---

_Review comment by @mattoberle on `src/isort/mod.rs`:601 on 2023-01-06 17:29_

I was sort of able to do this, although I had to `.collect` before (and within) the `if` block to ensure `it ` was the same type.

---

_Comment by @mattoberle on 2023-01-06 19:11_

Okay, I think I have this (nearly) passing (maybe a few tasks to get it up-to-date with upstream).

- Modified some match blocks to deref in the patterns.
- Put the additional sort into an `if` block.
- Replaced the bare `.unwrap` with something safer.

I was struggling to put together a function that would take `Option<&str>` arguments for a comparison.
`alias.name` is always the same type, but `import_from.module` seems to be `Option<String>` or `Option<str>` depending on whether I was running in GHA or locally. (Maybe something to do with the `wasm` target)?

I ultimately just unwrapped to an empty str, which I think results in the same set or sort rules getting applied (`""` is before any non-empty string).

---

In the broader picture: Is it weird to take something called `OrderedImportBlock` and then re-order it?
That was definitely the easy-way, but I started another branch that refactored things a bit...

1. Collect all import data and put them into an unsorted `Vec<EitherImport>` (or unevaluated iterator).
2. Apply sort rules.
3. Iterate over the `EitherImport`s, writing to output.

In that pattern there is no  concept of `OrderedImportBlock` with different `import` and `import_from` vectors.
Sorting the statements is contained within the `cmp_*` function and match blocks can consider the various settings.

---

_Comment by @charliermarsh on 2023-01-06 20:04_

I'd definitely be interested to see your other branch!

---

_Comment by @charliermarsh on 2023-01-06 20:05_

(I can help with the Option<String> conversions, we changed those recently.)

---

_Merged by @charliermarsh on 2023-01-09 06:06_

---

_Closed by @charliermarsh on 2023-01-09 06:06_

---

_Comment by @charliermarsh on 2023-01-09 06:07_

Merging this for now! I see what you're saying with the refactor. I may knock it out, you're also welcome to PR it. Thank you for the contribution :)

---

_@charliermarsh reviewed on 2023-01-09 06:07_

---

_Review comment by @charliermarsh on `src/isort/mod.rs`:601 on 2023-01-09 06:07_

If we use `sort_by` instead of `sorted_by`, we can avoid that allocation.

---

_Comment by @mattoberle on 2023-01-09 18:24_

> Merging this for now! I see what you're saying with the refactor. I may knock it out, you're also welcome to PR it. Thank you for the contribution :)

Thanks for your help with this!
My existing branch on the refactor is incomplete at this point (and out-of-date WRT feedback here).
If I find some time this week I'll work on getting it PR-ready, but if you have a vision don't let that stop you-- I think the exercise will still be worth it on my end.

---
