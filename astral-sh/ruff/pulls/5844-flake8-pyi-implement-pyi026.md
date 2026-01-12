```yaml
number: 5844
title: "[`flake8-pyi`] Implement PYI026"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI026
created_at: 2023-07-17T22:52:00Z
updated_at: 2023-07-27T21:03:45Z
url: https://github.com/astral-sh/ruff/pull/5844
synced_at: 2026-01-12T03:30:21Z
```

# [`flake8-pyi`] Implement PYI026

---

_Pull request opened by @LaBatata101 on 2023-07-17 22:52_

## Summary
Checks for `typehint.TypeAlias` annotation in type aliases. See [original source](https://github.com/PyCQA/flake8-pyi/blob/main/pyi.py#L1085).
```
$ flake8 --select Y026 crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:4:1: Y026 Use typing_extensions.TypeAlias for type aliases, e.g. "NewAny: TypeAlias = Any"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:5:1: Y026 Use typing_extensions.TypeAlias for type aliases, e.g. "OptinalStr: TypeAlias = typing.Optional[str]"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:6:1: Y026 Use typing_extensions.TypeAlias for type aliases, e.g. "Foo: TypeAlias = Literal['foo']"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:7:1: Y026 Use typing_extensions.TypeAlias for type aliases, e.g. "IntOrStr: TypeAlias = int | str"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:8:1: Y026 Use typing_extensions.TypeAlias for type aliases, e.g. "AliasNone: TypeAlias = None"
```

```
$ ./target/debug/ruff --select PYI026 crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi --no-cache
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:4:1: PYI026 Use `typing.TypeAlias` for type aliases in `NewAny`, e.g. "NewAny: typing.TypeAlias = Any"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:5:1: PYI026 Use `typing.TypeAlias` for type aliases in `OptinalStr`, e.g. "OptinalStr: typing.TypeAlias = typing.Optional[str]"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:6:1: PYI026 Use `typing.TypeAlias` for type aliases in `Foo`, e.g. "Foo: typing.TypeAlias = Literal["foo"]"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:7:1: PYI026 Use `typing.TypeAlias` for type aliases in `IntOrStr`, e.g. "IntOrStr: typing.TypeAlias = int | str"
crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi:8:1: PYI026 Use `typing.TypeAlias` for type aliases in `AliasNone`, e.g. "AliasNone: typing.TypeAlias = None"
Found 5 errors.
```

ref: #848 

## Test Plan

Snapshots, manual runs of flake8.


---

_Comment by @github-actions[bot] on 2023-07-17 23:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>typeshed (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/032f9195f9e3788a6e5c7ecb086f173a3ac92a95/stdlib/builtins.pyi#L1664'>stdlib/builtins.pyi:1664:1:</a> PYI026 Use `typing.TypeAlias` for type alias, e.g., `_SupportsSomeKindOfPow: typing.TypeAlias = _SupportsPow2[Any, Any] | _SupportsPow3NoneOnly[Any, Any] | _SupportsPow3[Any, Any, Any]`
+ <a href='https://github.com/python/typeshed/blob/032f9195f9e3788a6e5c7ecb086f173a3ac92a95/stdlib/builtins.pyi#L220'>stdlib/builtins.pyi:220:1:</a> PYI026 Use `typing.TypeAlias` for type alias, e.g., `_LiteralInteger: typing.TypeAlias = _PositiveInteger | _NegativeInteger | Literal[0]`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI026 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.03ms     3.7 MB/sec    1.00     10.9±0.03ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.00ms     7.5 MB/sec    1.01      2.2±0.06ms     7.5 MB/sec
formatter/numpy/globals.py                 1.04   256.3±11.23µs    11.5 MB/sec    1.00    246.9±0.78µs    12.0 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.01ms     5.4 MB/sec    1.01      4.8±0.01ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.04ms     2.6 MB/sec    1.00     15.3±0.03ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.9±0.00ms     4.2 MB/sec    1.00      3.9±0.00ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    518.2±0.82µs     5.7 MB/sec    1.00    515.2±1.17µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.01ms     3.6 MB/sec    1.00      7.0±0.01ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.02ms     5.1 MB/sec    1.00      7.9±0.02ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1687.6±7.22µs     9.9 MB/sec    1.00   1676.1±4.23µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.1±0.35µs    15.9 MB/sec    1.00    184.5±0.32µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.02ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.4±0.33ms     3.6 MB/sec    1.08     12.4±0.27ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.05ms     7.4 MB/sec    1.07      2.4±0.04ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00    251.0±7.05µs    11.8 MB/sec    1.04    261.4±7.59µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.08ms     5.3 MB/sec    1.11      5.4±0.08ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.2±0.33ms     2.5 MB/sec    1.00     16.2±0.36ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.09ms     3.8 MB/sec    1.00      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   505.4±10.55µs     5.8 MB/sec    1.00    499.1±9.31µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.5±0.21ms     3.4 MB/sec    1.00      7.4±0.17ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.16ms     4.9 MB/sec    1.01      8.4±0.17ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1714.4±21.75µs     9.7 MB/sec    1.02  1741.5±29.81µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.4±3.44µs    15.4 MB/sec    1.02    194.9±5.02µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.06ms     6.9 MB/sec    1.01      3.7±0.09ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb reviewed on 2023-07-17 23:56_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.py`:5 on 2023-07-17 23:56_

```suggestion
OptionalStr = typing.Optional[str]
```

---

_@zanieb reviewed on 2023-07-17 23:56_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.py`:11 on 2023-07-17 23:56_

```suggestion
OptionalStr: TypeAlias = typing.Optional[str]
```

---

_@zanieb reviewed on 2023-07-17 23:57_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi`:5 on 2023-07-17 23:57_

```suggestion
OptionalStr = typing.Optional[str]
```

---

_@zanieb reviewed on 2023-07-17 23:57_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi`:11 on 2023-07-17 23:57_

```suggestion
OptionalStr: TypeAlias = typing.Optional[str]
```

---

_@zanieb reviewed on 2023-07-17 23:59_

Welcome and thanks for contributing!

This seems like a good candidate for a suggested or automatic fix if your up for it :)

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:126 on 2023-07-18 00:01_

I'd either exclude the name so this reads: 

> Use `typing.TypeAlias` for type aliases. e.g. "AliasNone: typing.TypeAlias = None"`


```suggestion
        format!("Use `typing.TypeAlias` for type aliases, e.g. \"{name}: typing.TypeAlias = {value}\"")
```

or make it singular

> Use `typing.TypeAlias` for type alias `AliasNone`, e.g. "AliasNone: typing.TypeAlias = None"

---

_@zanieb reviewed on 2023-07-18 00:01_

---

_Comment by @LaBatata101 on 2023-07-18 00:07_

> Welcome and thanks for contributing!
> 
> This seems like a good candidate for a suggested or automatic fix if your up for it :)

I'll try to add the automatic fix. Do you have any examples for this?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:435 on 2023-07-18 00:07_

What's the motivation for this change? I believe it's semantically equivalent, but the former enforces the condition with a single match.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:507 on 2023-07-18 00:08_

Ditto here.

---

_Comment by @zanieb on 2023-07-18 00:09_

@LaBatata101 here's an example that changes one type annotation to another https://github.com/astral-sh/ruff/pull/4695/files#diff-f72967b46be9c2c0fed6dc2bf98259327c03cc36dd28839e72cc106a60f552dbR83-R91

---

_@zanieb reviewed on 2023-07-18 00:10_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI026.pyi`:15 on 2023-07-18 00:10_

Perhaps a test case with another existing annotation would be good e.g. `IntOrStr: Foo = int | str`

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:574 on 2023-07-18 00:11_

I think more idiomatic in our codebase would be to remove `is_any`, and do:

```rust
if let Expr::Name(ast::ExprName { id, .. }) = target {
    if id.as_str() == "Any" {
        return;
    }
};
```

That way, IMO, the condition reads more linearly: "if the `value` is a `Name`, and that `Name` is "any", then return".

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:574 on 2023-07-18 00:11_

However, we have a special utility for this, which will ensure that the `Any` here actually refers to `typing.Any`, and that we match things like `x: typing.Any` instead of just `x: Any`:

```rust
if checker.semantic().match_typing_expr(value, "Any") {
    return;
}
```

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:568 on 2023-07-18 00:12_

Should this rule run with multiple targets? (E.g., `x = y = 1`.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:638 on 2023-07-18 00:15_

It looks like the original implementation in flake8-pyi has slightly different criteria: https://github.com/PyCQA/flake8-pyi/blob/2a86db8271dc500247a8a69419536240334731cf/pyi.py#L1085.

It looks like they trigger if:

- The value is a subscript.
- The value is a PEP 604-style union.
- The value is `Any`.
- The value is `None`.

It looks like the current implementation here is more permissive. I think we need some tweaks to be more conservative, to avoid false positives. E.g., we should omit _any_ expression that isn't a subscript (like, we should omit all call expressions), but we should allow the constant `None`.


---

_@charliermarsh reviewed on 2023-07-18 00:17_

---

_@AlexWaygood reviewed on 2023-07-18 01:47_

---

_Review comment by @AlexWaygood on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:638 on 2023-07-18 01:47_

The length of the docstring on that flake8-pyi method there is a testament to the number of times we had to revise that check at flake8-pyi before we got to a place where the balance between false negatives and false positives was where we wanted it to be... There are _still_ a few false positives that flake8-pyi's Y026 produces, but overall I'm pretty happy with it now

---

_@LaBatata101 reviewed on 2023-07-18 19:47_

---

_Review comment by @LaBatata101 on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:435 on 2023-07-18 19:47_

Whaaat, I don't remember changing that part of the code. I'll revert the changes.



---

_Review requested from @zanieb by @LaBatata101 on 2023-07-19 22:51_

---

_Review requested from @charliermarsh by @LaBatata101 on 2023-07-19 22:51_

---

_Label `rule` added by @charliermarsh on 2023-07-20 01:28_

---

_@charliermarsh reviewed on 2023-07-20 01:34_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:615 on 2023-07-20 01:34_

I reordered these operations from least- to most-expensive.

---

_@charliermarsh reviewed on 2023-07-20 01:34_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:604 on 2023-07-20 01:34_

I renamed the method and rule to be a little more in-line with our conventions ("describe the bad case").

---

_@charliermarsh approved on 2023-07-20 01:35_

Looks great -- nice work, thank you!

---

_Merged by @charliermarsh on 2023-07-20 01:39_

---

_Closed by @charliermarsh on 2023-07-20 01:39_

---

_Branch deleted on 2023-07-27 21:03_

---
