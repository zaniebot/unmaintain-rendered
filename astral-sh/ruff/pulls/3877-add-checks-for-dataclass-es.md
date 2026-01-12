```yaml
number: 3877
title: "Add checks for `dataclass`es"
type: pull_request
state: merged
author: mosauter
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2023-04-04T16:18:34Z
updated_at: 2023-04-25T19:15:30Z
url: https://github.com/astral-sh/ruff/pull/3877
synced_at: 2026-01-12T15:55:13Z
```

# Add checks for `dataclass`es

---

_@mosauter_

Adds checks specifically for `dataclass`es in combination with mutable defaults.

```python
@dataclass
class A:
    mutable_default: list[int] = [1, 2, 3]

@dataclass
class B:
    even_more_dangerous: A = A()
```

It is quite closely related to the `flake8-bugbear` rules `B006` (`mutable-argument-default`) and `B008` (`function-call-in-default-argument`). As the original bugbear implementation doesn't really catch those errors in dataclasses (esp. the second `B` variant), I created the new errors under `RUF200` and `RUF201`. 

If you have some feedback in that regard it would be highly appreciated.

Also I'm not that experienced with rust in general if you have feedback there, be happy to work that in as well :)

Still working on:
- [x] testing
- [x] documentation

---

_Comment by @github-actions[bot] on 2023-04-04 16:51_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+11, -0, 0 error(s))

<details><summary>airflow (+9, -0)</summary>
<p>

```diff
+ dev/breeze/src/airflow_breeze/params/build_prod_params.py:47:27: RUF009 Do not perform function call `get_airflow_extras` in dataclass defaults
+ dev/breeze/src/airflow_breeze/params/common_build_params.py:42:27: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
+ dev/breeze/src/airflow_breeze/params/common_build_params.py:43:39: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
+ dev/breeze/src/airflow_breeze/params/common_build_params.py:54:27: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
+ dev/breeze/src/airflow_breeze/params/common_build_params.py:56:25: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
+ dev/breeze/src/airflow_breeze/params/shell_params.py:70:27: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
+ dev/breeze/src/airflow_breeze/params/shell_params.py:71:39: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
+ dev/breeze/src/airflow_breeze/params/shell_params.py:87:27: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
+ dev/breeze/src/airflow_breeze/params/shell_params.py:89:25: RUF009 Do not perform function call `os.environ.get` in dataclass defaults
```

</p>
</details>
<details><summary>scikit-build-core (+2, -0)</summary>
<p>

```diff
+ src/scikit_build_core/file_api/model/toolchains.py:41:27: RUF009 Do not perform function call `APIVersion` in dataclass defaults
+ tests/test_settings.py:23:17: RUF009 Do not perform function call `Path` in dataclass defaults
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.04ms     2.7 MB/sec    1.00     15.1±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.3 MB/sec    1.00      3.9±0.03ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    417.3±3.01µs     7.1 MB/sec    1.00    419.1±4.36µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.02ms     3.9 MB/sec    1.00      6.5±0.05ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.02ms     5.2 MB/sec    1.00      7.8±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1755.6±16.27µs     9.5 MB/sec    1.00   1740.5±2.65µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.0±0.53µs    16.2 MB/sec    1.00    182.1±0.55µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.0±0.28ms     2.5 MB/sec    1.00     15.8±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.08ms     4.0 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02   498.5±10.04µs     5.9 MB/sec    1.00    487.2±6.05µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.8±0.08ms     3.8 MB/sec    1.00      6.7±0.05ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.08      8.7±0.53ms     4.7 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1873.0±62.95µs     8.9 MB/sec    1.00  1778.2±38.86µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.9±5.92µs    15.3 MB/sec    1.01    194.2±4.42µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.8±0.10ms     6.7 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @mosauter on 2023-04-06 12:13_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-04-06 17:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:141 on 2023-04-06 22:34_

We can pass in `context: &Context` here as `is_allowed_func(&checker.ctx, ...)`. (We're trying to decouple as much code from `Checker` as we can, so it's preferred to use the underlying members that power semantic queries, like `Context`.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:154 on 2023-04-06 22:36_

Just for convenience, we'll often add a comment like:

```rust
/// RUF201
pub fn function_call_in_dataclass_defaults(checker: &mut Checker, body: &[Stmt]) {
  ...
}
```

Makes it easy to grep for the rule implementation given a code.


---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:185 on 2023-04-06 22:38_

Since there's only one case here, you can write this as an `if let`:

```rust
pub fn mutable_dataclass_default(checker: &mut Checker, body: &[Stmt]) {
    for statement in body {
        if let StmtKind::Assign { value, .. }
        | StmtKind::AnnAssign {
            value: Some(value), ..
        } = &statement.node
        {
            if is_mutable_expr(value) {
                checker
                    .diagnostics
                    .push(Diagnostic::new(MutableDataclassDefault, Range::from(value)));
            }
        }
    }
}
```

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:200 on 2023-04-06 22:39_

Unless I'm misreading, I think you can write this as:

```rust
pub fn is_dataclass(checker: &Checker, decorator_list: &[Expr]) -> bool {
    decorator_list.iter().any(|decorator| {
        checker
            .ctx
            .resolve_call_path(map_callable(decorator))
            .map_or(false, |call_path| {
                call_path.as_slice() == ["dataclasses", "dataclass"]
            })
    })
}
```

Two things: (1) `decorator_list.iter().any` over manual iteration, and (2) we have a `map_callable` utility to handle the `if let ExprKind::Call { ... }` pattern here since it comes up often with decorators.

---

_Review comment by @charliermarsh on `crates/ruff/src/codes.rs`:704 on 2023-04-06 22:40_

I think we should just make these `RUF008` and `RUF009`. The choice of codes within the `RUF` category doesn't have semantic meaning, so I'd rather stick to autoincrement until the point at which we make an explicit decision to do otherwise.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:819 on 2023-04-06 22:41_

Marginal, but can we check if either of the relevant rules are enabled before doing this `is_dataclass` check, since this'll be more expensive (requires resolving all the decorators)? E.g.:

```rust
if self.settings.rules.any_enabled(...) {
  if ruff:rules::is_dataclass(...) {
    ...
  }
}
```

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:143 on 2023-04-06 22:44_

We may want to make this a setting, to allow users to allow certain functions here. But, we can wait on that, see if it's a real need that comes up in Issues.

---

_@charliermarsh reviewed on 2023-04-06 22:45_

This looks great, I really appreciate the thoroughness of the work here. I left some comments but they're mostly minor changes.

---

_Comment by @charliermarsh on 2023-04-06 22:46_

(Also, I'm sorry that I didn't reply when this was originally proposed as a draft.)

---

_Review comment by @mosauter on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:143 on 2023-04-07 08:37_

I actually thought about that (mainly because the bugbear implementation has something similar), but I couldn't come up with something I would really want there. The only thing that is sensible in my mind at least, would be something like "registering" your `NamedTuples` as they are immutable or ignoring the error for one specific function.

So yeah personally I did leave it as "kinda prepared if necessary, but only if needed in the future" :)

---

_Review comment by @mosauter on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:154 on 2023-04-07 08:37_

done that :)

---

_Review comment by @mosauter on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:200 on 2023-04-07 08:37_

I knew something like that existed, thank you! Did replace both.

---

_@mosauter reviewed on 2023-04-07 08:38_

---

_@mosauter reviewed on 2023-04-07 08:39_

---

_Review comment by @mosauter on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:141 on 2023-04-07 08:39_

I see, makes perfect sense. Changed it.

---

_@mosauter reviewed on 2023-04-07 08:39_

---

_Review comment by @mosauter on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:185 on 2023-04-07 08:39_

I actually experimented quite a bit, the syntax is still quite new to me. Somehow I did completely miss doing this, thanks!

---

_Review comment by @mosauter on `crates/ruff/src/codes.rs`:704 on 2023-04-07 08:45_

Makes sense to me. Actually I found it to be quite difficult to choose something right. Might just be because the "new checks" are extremely similar to the bugbear ones and one could certainly make a case for them just being the same.

In the end I thought that matching the bugbear implementation and creating new ones is better. But at some point they probably should be combined (imho). 

---

_@mosauter reviewed on 2023-04-07 08:45_

---

_@mosauter reviewed on 2023-04-07 08:50_

---

_Review comment by @mosauter on `crates/ruff/src/checkers/ast/mod.rs`:819 on 2023-04-07 08:50_

Done

---

_Comment by @mosauter on 2023-04-07 08:53_

And regarding the "late" reply, don't be sorry. I'm following the project for quite a while now and it was (and still is) amazing what you created here. So you absolutely deserve more than ... (looking it up) ... 1 day delay until answering :smile: 

(Also in retrospect, it wasn't ready for review at that point)

---

_@mosauter reviewed on 2023-04-07 09:09_

---

_Review comment by @mosauter on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:143 on 2023-04-07 09:09_

Well, now that I've revisited that thought, it could also match the `IMMUTABLE_FUNCS` from [here (function_call_argument_default.rs)](https://github.com/charliermarsh/ruff/blob/abaf0a198d9d2cebefd5c029bb271be1a3498f20/crates/ruff/src/rules/flake8_bugbear/rules/function_call_argument_default.rs#L33) + the added dataclass-specific `dataclass.field` one.

I'm not sure if you want to do that as it would probably mean exporting one or the other (or both) and having configuration options for two (very far apart) rule checkers in one of them; or duplicating the array on both sides.

---

_@mosauter reviewed on 2023-04-07 09:12_

---

_Review comment by @mosauter on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:143 on 2023-04-07 09:12_

Also this would have implications for "user-added" things. As "sharing" the list would mean the user adds a to-ignore function to the flake8-bugbear-config and it gets ignored in the dataclass as well.

Therefore I'm leaning more to a duplicate of the array at the moment.

---

_@charliermarsh reviewed on 2023-04-09 02:44_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/mutable_defaults_in_dataclass_fields.rs`:143 on 2023-04-09 02:44_

I'm okay to skip for now. Let's see if it comes up. If you _do_ feel like implementing, my gut reaction is just to duplicate for now, but there's not great precedent to follow (it's odd to have two different configuration options for what are really very similar rules, but we also tend to avoid sharing configuration across "sub-linters").

---

_Merged by @charliermarsh on 2023-04-09 02:46_

---

_Closed by @charliermarsh on 2023-04-09 02:46_

---

_Comment by @andersk on 2023-04-20 00:11_

This new rule should respect the same exemptions for mutable defaults with immutable type annotations that B006 does (see [`is_immutable_annotation`](https://github.com/charliermarsh/ruff/blob/v0.0.262/crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs#L169) from #821). For example, this should be allowed:

```python
from dataclasses import dataclass
from typing import Sequence

@dataclass
class A:
    immutable_default: Sequence[int] = [1, 2, 3]
```

---

_Comment by @charliermarsh on 2023-04-20 03:30_

Agreed, created a new issue.

---

_Comment by @henryiii on 2023-04-25 18:42_

This check is likely to have a lot of false positives. If you have an immutable class, like `pathlib.Path`, this is just fine:

```python
@dataclasses.dataclass
class Something:
    dir: Path = Path("build")
```

However, these are now flagged as unsafe. This is also true for frozen dataclasses, named tuples, etc. Maybe some common immutable things could be tracked?

(Both cases in the scikit-build-core ecosystem check above are just fine - one's a Path, and the other is a frozen dataclass).

---

_Comment by @charliermarsh on 2023-04-25 19:10_

Will look into this - I agree those should be excluded, and we do exclude them in the bugbear-equivalent rules IIRC, so it may be a matter of extending that logic to this check.

---

_Comment by @mosauter on 2023-04-25 19:15_

The code is set up similar or rather almost identically like the bugbear equivalent that allows at least for a `Path` and some others. If it's okay just to copy the list from there I could create a small PR for this one.

But as far as I understood up until now Ruff cannot do a deep inspection into "generic" types. So determining that a "call" is a `dataclass` (or a `NamedTuple`) is not possible. (Again to my very limited understanding)

---
