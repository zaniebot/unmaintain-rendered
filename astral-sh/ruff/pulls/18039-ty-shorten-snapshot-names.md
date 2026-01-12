```yaml
number: 18039
title: "[ty] Shorten snapshot names"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: ty-snapshot-filenames
created_at: 2025-05-12T11:34:32Z
updated_at: 2025-05-13T17:02:34Z
url: https://github.com/astral-sh/ruff/pull/18039
synced_at: 2026-01-12T15:56:10Z
```

# [ty] Shorten snapshot names

---

_@InSyncWithFoo_

## Summary

The multiple segments of a snapshot filename are now contracted if they are longer than a predefined limit (currently 20). To disambiguate siblings with a long common prefix, the hash of the full name is appended to the contracted name. The hasher is [`rustc_stable_hash`'s `StableSipHasher128`](https://docs.rs/rustc-stable-hash/latest/rustc_stable_hash/hashers/type.StableSipHasher128.html).

Snapshot names as they are recorded in the snapshots themselves remain unchanged. The environment variable `MDTEST_TEST_FILTER` also continues to work as expected (it is checked against the full name).

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_@MichaReiser reviewed on 2025-05-12 11:36_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/1._attribute_assignment…_-_2._Attribute_assignment…_-_11._Setting_attributes_o….snap`:6 on 2025-05-12 11:36_

It would be great if we can avoid truncating the test names here, considering that no path limit applies here.

---

_@MichaReiser reviewed on 2025-05-12 11:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/1._overloads.md_-_2._Overloads_-_14._Invalid_-_21._Inconsistent_decorat…_-_24._`@final`.snap`:6 on 2025-05-12 11:45_

I find the numbers confusing and would remove them

* Overloads: is the top level title, why is it 2nd?
* Invalid is the 7th or 8th level-2 heading. It's possible that it's the 14th test but that's not how other editors (e.g. word) number headings.

Given that the headings aren't numbered in the markdown, I recommend removing the numbers again.

---

_Label `internal` added by @AlexWaygood on 2025-05-12 14:11_

---

_Label `testing` added by @AlexWaygood on 2025-05-12 14:11_

---

_Label `ty` added by @AlexWaygood on 2025-05-12 14:11_

---

_@InSyncWithFoo reviewed on 2025-05-12 15:26_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/snapshots/1._overloads.md_-_2._Overloads_-_14._Invalid_-_21._Inconsistent_decorat…_-_24._`@final`.snap`:6 on 2025-05-12 15:26_

These numbers are the section IDs. I find them confusing too. They are, however, necessary to differentiate between sibling sections whose headers share a common prefix longer than the limit allows. Would it be better to use hashes instead?

---

_@MichaReiser reviewed on 2025-05-12 16:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/snapshots/1._overloads.md_-_2._Overloads_-_14._Invalid_-_21._Inconsistent_decorat…_-_24._`@final`.snap`:6 on 2025-05-12 16:23_

Hashes would be very unreadable. I think we could do:

* Use the index of the last section (24) as suffix? Meaning, only number the test sections but not every segment
* Truncate by using the first 20 and last 8 characters (assuming that the suffix is more likely to be different between tests)

---

_Comment by @github-actions[bot] on 2025-05-13 11:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:956:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
+ error[type-assertion-failure] tests/annotations/declarations.py:961:5: Argument does not have asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 647 diagnostics
+ Found 650 diagnostics

```
</details>


---

_Comment by @InSyncWithFoo on 2025-05-13 11:38_

@MichaReiser Now snapshots only have their names changed, and new filenames have an extra ID at the very end. What do you think?

---

_Comment by @MichaReiser on 2025-05-13 11:41_

I realised yesterday night that including the index in the snapshot change is problematic because it means that many snapshots will change when a new test gets added (because all their names change and insta won't recognize that these are still the same tests). I'm sorry I didn't realize this sooner. I'd expect that git should generally be able to recognize that the file was renamed. 

However, this makes me think if we could use a more stable id. E.g using the subheading index instead of the absolute index:

```md
# Main

## Sub (1)

### SubSub (1)

### SubSub (2)

## Sub (2)

### SubSub (1)
```

This would limit the "blast" radius significantly. 



---

_Comment by @InSyncWithFoo on 2025-05-13 11:46_

...or we can use hexadecimal hashes of the uncontracted names, which doesn't require each test knowing about its index at all?

---

_Comment by @MichaReiser on 2025-05-13 11:49_

We could do that too. It feels a bit more magic. 

As an alternative. Does the testing framework know about all tests upfront? If so, could we use a hashmap from `truncated` name to index and only append a disambiguator (the index) if the truncated name of two tests using snapshots collide?

---

_Comment by @InSyncWithFoo on 2025-05-13 11:59_

> Does the testing framework know about all tests upfront?

Yes, but I think a conditional disambiguator would complicate matters further. As you have pointed out, using an index-based disambiguator means new tests might involuntarily break old ones, whereas with hashes the only thing to worry about would be to pick a stable algorithm.

---

_Comment by @MichaReiser on 2025-05-13 12:07_

> Yes, but I think a conditional disambiguator would complicate matters further. As you have pointed out, using an index-based disambiguator means new tests might involuntarily break old ones, whereas with hashes the only thing to worry about would be to pick a stable algorithm.

I think that should be fine in that case because I assume requiring a disambiguator should be rare (and we should only do it for the first conflicting test). The nice thing about this is that there's the option to change the test title. 

---

_Comment by @InSyncWithFoo on 2025-05-13 15:10_

I ended up using hashes because that seems to be the easier solution.

---

_Marked ready for review by @InSyncWithFoo on 2025-05-13 15:22_

---

_Review requested from @carljm by @InSyncWithFoo on 2025-05-13 15:22_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-05-13 15:22_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-05-13 15:22_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-05-13 15:22_

---

_Comment by @github-actions[bot] on 2025-05-13 15:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/gpt4-1_prompting_guide.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 580 column 29
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/gpt4-1_prompting_guide.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 580 column 29
```

</p>
</details>




---

_Label `internal` removed by @MichaReiser on 2025-05-13 16:11_

---

_Review comment by @MichaReiser on `crates/ty_test/src/parser.rs`:83 on 2025-05-13 16:12_

```suggestion
```

This code isn't performance sensitive and the compiler should know what's best

---

_Review comment by @MichaReiser on `crates/ty_test/src/parser.rs`:99 on 2025-05-13 16:12_

```suggestion
```

---

_@MichaReiser approved on 2025-05-13 16:13_

Thanks

---

_Merged by @MichaReiser on 2025-05-13 16:43_

---

_Closed by @MichaReiser on 2025-05-13 16:43_

---

_Branch deleted on 2025-05-13 17:02_

---
