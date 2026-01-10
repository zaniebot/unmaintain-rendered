```yaml
number: 17835
title: "[ty] Detect overloads decorated with `@dataclass_transform`"
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - ty
assignees: []
merged: true
base: main
head: dataclass
created_at: 2025-05-04T19:31:22Z
updated_at: 2025-05-07T16:27:20Z
url: https://github.com/astral-sh/ruff/pull/17835
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Detect overloads decorated with `@dataclass_transform`

---

_Pull request opened by @abhijeetbodas2001 on 2025-05-04 19:31_

## Summary

Fixes #17541

Before this change, in the case of overloaded functions, `@dataclass_transform` was detected only when applied to the implementation, not the overloads.
However, the spec also allows this decorator to be applied to any of the overloads as well.
With this PR, we start handling `@dataclass_transform`s applied to overloads.

## Test Plan

Fixed existing TODOs in the test suite.


---

_Comment by @github-actions[bot] on 2025-05-04 19:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
strawberry (https://github.com/strawberry-graphql/strawberry)
- error[lint:unknown-argument] strawberry/relay/types.py:878:25: Argument `start_cursor` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:879:25: Argument `end_cursor` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:880:25: Argument `has_previous_page` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:881:25: Argument `has_next_page` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:906:21: Argument `start_cursor` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:907:21: Argument `end_cursor` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:908:21: Argument `has_previous_page` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:909:21: Argument `has_next_page` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:942:17: Argument `start_cursor` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:943:17: Argument `end_cursor` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:944:17: Argument `has_previous_page` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] strawberry/relay/types.py:945:17: Argument `has_next_page` does not match any known parameter of bound method `__init__`
+ error[lint:invalid-return-type] strawberry/tools/create_type.py:67:12: Return type does not match returned value: Expected `type`, found `<decorator produced by dataclass-like function>`
+ error[lint:invalid-return-type] strawberry/tools/merge_types.py:35:12: Return type does not match returned value: Expected `type`, found `<decorator produced by dataclass-like function>`
- Found 462 diagnostics
+ Found 452 diagnostics

```
</details>


---

_Comment by @abhijeetbodas2001 on 2025-05-04 19:45_

This is as of now incomplete, in that it only handles the cases where the function which is supposed to act like a dataclass decorator is being used in a "call" context, like so:

```py
@versioned_class(version=2)
class D2:
    x: str
```

This PR does not yet fix cases where `versioned_class` is used in a "name load" context:

```py
@versioned_class
class D1:
    x: str
```

This is because, in the later case, logic to determine if the `FunctionLiteral` acts like a dataclass decorator is here:

https://github.com/astral-sh/ruff/blob/e95130ad8062f64a11247d51a72f62bb735ddd19/crates/ty_python_semantic/src/types/infer.rs#L2101-L2106

For the `FunctionLiteral` case of `versioned_class`, my understanding is that the only live binding of `versioned_class` at the use is the implementation. If the implementation does not have `dataclass_transform` applied to it (and instead a prior overload has it), we will not find any `dataclass_params` in the above code, and will not deem `versioned_class` to be dataclass-like.

@dhruvmanila I would appreciate any thoughts on how to handle this case. We will need to probably add in some custom behavior to "carry over" the `@dataclass_transform` decorator between bindings. I am not sure how to do this. I am also not sure whether this has to be done more generally for decorators, instead of just for `dataclass_transform`.

---

_Label `ty` added by @AlexWaygood on 2025-05-04 23:37_

---

_Comment by @abhijeetbodas2001 on 2025-05-05 18:14_

I (ab)used `to_overloaded` to fix the above issue also.
Marking this as not-draft to get some reviews.

---

_Marked ready for review by @abhijeetbodas2001 on 2025-05-05 18:14_

---

_Review requested from @carljm by @abhijeetbodas2001 on 2025-05-05 18:14_

---

_Review requested from @AlexWaygood by @abhijeetbodas2001 on 2025-05-05 18:14_

---

_Review requested from @sharkdp by @abhijeetbodas2001 on 2025-05-05 18:14_

---

_Review requested from @dcreager by @abhijeetbodas2001 on 2025-05-05 18:14_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-05 18:21_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2105 on 2025-05-06 00:32_

```suggestion
                // We do not yet detect or flag `@dataclass_transform` applied to more than one
                // overload, or an overload and the implementation both. Nevertheless, this is not
                // allowed. We do not try to treat the offenders intelligently -- just use the
                // params of the last seen usage of `@dataclass_transform`
```

---

_Review requested from @dhruvmanila by @dhruvmanila on 2025-05-06 05:20_

---

_@carljm reviewed on 2025-05-06 14:50_

---

_@abhijeetbodas2001 reviewed on 2025-05-06 15:34_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/infer.rs`:2105 on 2025-05-06 15:34_

fixed, thanks!

---

_Comment by @sharkdp on 2025-05-06 19:01_

Thank you very much for working on this! I am planning to do a full review soon. One thing that I hoped would be happening with this PR is that we understand [`strawberry.type`](https://github.com/strawberry-graphql/strawberry/blob/ca61f9fa9b3e7f08cee71b1b7f5e49deb390eb27/strawberry/federation/object_type.py#L100-L173). I would imagine that this would cause some ecosystem changes once we do.

But it does not seem to work yet, on your branch:

`strawberry_test.py:`
```py
import strawberry

@strawberry.type
class User:
    name: str
    age: int

User(name="Patrick", age=100)
```

```
▶ uv run --no-project --with strawberry-graphql cargo run --bin ty -- check strawberry_test.py
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `/home/shark/.cargo-target/debug/ty check strawberry_test.py`
error: lint:unknown-argument: Argument `name` does not match any known parameter of bound method `__init__`
  --> strawberry_test.py:10:6
   |
10 | User(name="foo", age=8)
   |      ^^^^^^^^^^
   |

error: lint:unknown-argument: Argument `age` does not match any known parameter of bound method `__init__`
  --> strawberry_test.py:10:18
   |
10 | User(name="foo", age=8)
   |                  ^^^^^
   |

Found 2 diagnostics
```

It might be interesting to understand why this does not work yet (not implying that you need to do this, I can also look into it when I get around to it).

---

_Comment by @abhijeetbodas2001 on 2025-05-06 19:20_

Interesting. I would certainly be interested in investigating this, but it does not give any errors for me though:

```shell
[~/code/ruff] $ git reset --hard origin/dataclass
HEAD is now at 518ef57c2 fix typo in comment
[~/code/ruff] $ git lol -3
518ef57c2 (HEAD -> dataclass, origin/dataclass) fix typo in comment
837ad29b4 fix name load usage also
828248142 [ty] Detect overloads decorated with `@dataclass_transform`
[~/code/ruff] $ uv run --no-project --with strawberry-graphql cargo run --bin ty -- check /tmp/strawberry_test.py
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.51s
     Running `target/debug/ty check /tmp/strawberry_test.py`
All checks passed!
```

Just to confirm, you're on the latest version of this branch right? I had force pushed a couple of times here.

---

_Comment by @sharkdp on 2025-05-06 19:29_

> Just to confirm, you're on the latest version of this branch right? I had force pushed a couple of times here.

Oh, indeed. I must have done something wrong when switching branches. It works as expected — nice!

I expected more ecosystem changes in strawberry, but it looks like in mypy_primer, we're only checking the "strawberry" subfolder, and not strawberry's tests, which would contain many examples. The occurrences of `@strawberry.type` inside the "strawberry" source folder are mostly in doc comments.

Sorry for the distraction.

---

_Comment by @abhijeetbodas2001 on 2025-05-07 06:16_

Hmmph, I don't understand how `mypy_primer` works (or even what it does, actually!).
Let me see if I can figure that out and include their tests in the analysis.

---

_Comment by @sharkdp on 2025-05-07 07:28_

> Hmmph, I don't understand how `mypy_primer` works (or even what it does, actually!).
> Let me see if I can figure that out and include their tests in the analysis.

Sorry, I didn't mean to imply that we should change that. I think it's fine as is. It is configured here: https://github.com/hauntsaninja/mypy_primer/blob/a4e8faf6832df707a36a4225f88ec4dc04733e96/mypy_primer/projects.py#L1517

We use mypy_primer to run ty across a set of open source projects (the ones you see in that linked file). Once for the version on `main`, and once for the feature branch. It then computes a diff of ty's diagnostics output and [posts it as a PR comment](https://github.com/astral-sh/ruff/pull/17835#issuecomment-2849381547)

---

_@sharkdp approved on 2025-05-07 13:50_

This looks great — thank you very much!

---

_Merged by @sharkdp on 2025-05-07 13:51_

---

_Closed by @sharkdp on 2025-05-07 13:51_

---

_Branch deleted on 2025-05-07 16:27_

---
