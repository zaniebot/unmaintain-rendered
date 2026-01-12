```yaml
number: 10963
title: "[`pylint`] Implement `invalid-length-returned` (`E0303`)"
type: pull_request
state: merged
author: tibor-reiss
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-E0303-invalid-length-returned
created_at: 2024-04-15T20:39:28Z
updated_at: 2024-04-18T02:02:26Z
url: https://github.com/astral-sh/ruff/pull/10963
synced_at: 2026-01-12T15:55:33Z
```

# [`pylint`] Implement `invalid-length-returned` (`E0303`)

---

_@tibor-reiss_

Add pylint rule invalid-length-returned (PLE0303)

See https://github.com/astral-sh/ruff/issues/970 for rules

Test Plan: `cargo test`

TBD: from the description: "Strictly speaking `bool` is a subclass of `int`, thus returning `True`/`False` is valid. To be consistent with other rules (e.g. [PLE0305](https://github.com/astral-sh/ruff/pull/10962) invalid-index-returned), ruff will raise, compared to pylint which will not raise."

---

_Comment by @tibor-reiss on 2024-04-15 20:48_

The tests run locally fine when using commit 670d66f as rebase, but there are failures when using the next commit, c221035. Was there some limit reached (rule is u16 in rule_set.rs / contains()) maybe?

---

_Comment by @charliermarsh on 2024-04-15 20:58_

Try bumping the value in `const RULESET_SIZE: usize = 13;`

---

_Comment by @codspeed-hq[bot] on 2024-04-15 21:41_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tibor-reiss:add-E0303-invalid-length-returned)

### Merging #10963 will **not alter performance**

<sub>Comparing <code>tibor-reiss:add-E0303-invalid-length-returned</code> (b09ac0b) with <code>main</code> (b23414e)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-04-15 22:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/12b9e48324df292379db3eca4e3c54175e5f374e/stdlib/typing.pyi#L330'>stdlib/typing.pyi:330:10:</a> PYI001 Name of private `TypeVar` must start with `_`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/98ef69ce096b72c1deaa3abe217b458ea17139c8/ibis/expr/types/relations.py#L831'>ibis/expr/types/relations.py:831:9:</a> PLE0303 `__len__` does not return a non-negative integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a99fb7d054ee0831f1c00d8bea1b54e383185802/pandas/core/arrays/base.py#L483'>pandas/core/arrays/base.py:483:9:</a> PLE0303 `__len__` does not return a non-negative integer
+ <a href='https://github.com/pandas-dev/pandas/blob/a99fb7d054ee0831f1c00d8bea1b54e383185802/pandas/core/base.py#L349'>pandas/core/base.py:349:9:</a> PLE0303 `__len__` does not return a non-negative integer
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/12b9e48324df292379db3eca4e3c54175e5f374e/stdlib/typing.pyi#L330'>stdlib/typing.pyi:330:10:</a> PYI001 Name of private `TypeVar` must start with `_`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLE0303 | 3 | 3 | 0 | 0 | 0 |
| PYI001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @tibor-reiss on 2024-04-16 05:36_

> Try bumping the value in `const RULESET_SIZE: usize = 13;`

@charliermarsh Is the performance drop of 4% also related to this please? The newly introduced rule is quite lightweight.

Solved, thanks @MichaReiser 

---

_Label `rule` added by @MichaReiser on 2024-04-16 06:30_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:101 on 2024-04-16 06:33_

Nit: You can pass the entire function definition instead of just passing the name and body
```suggestion
                pylint::rules::invalid_length_return(checker, function_def);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/invalid_length_return.rs`:88 on 2024-04-16 06:40_

Nit: 
```suggestion
        matches!(
        value,
        Expr::UnaryOp(ast::ExprUnaryOp {
            op: ast::UnaryOp::USub,
            ..
        })
    )
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/invalid_length_return.rs`:78 on 2024-04-16 06:41_

It could make sense to change the order of the checks because `is_negative_integer` is relatively simple compared to `ResolvedPythonType::from`
```suggestion
            if is_negative_integer(value) ||
             !matches!(
                ResolvedPythonType::from(value),
                ResolvedPythonType::Unknown
                    | ResolvedPythonType::Atom(PythonType::Number(NumberLike::Integer))
            )
            {
```

---

_@MichaReiser approved on 2024-04-16 06:42_

---

_Comment by @MichaReiser on 2024-04-16 12:22_

@tibor-reiss I bumped the index (and fixed the perf regression). You can rebase and we should then see the impact of your rule.

---

_@tibor-reiss reviewed on 2024-04-16 18:14_

---

_Review comment by @tibor-reiss on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:101 on 2024-04-16 18:14_

I took the current bool and str cases as a blueprint, which passed also name and body. What would be the advantage of passing the function_def please? I would still need to reference name and body.

---

_@tibor-reiss reviewed on 2024-04-16 18:15_

---

_Review comment by @tibor-reiss on `crates/ruff_linter/src/rules/pylint/rules/invalid_length_return.rs`:88 on 2024-04-16 18:15_

Thanks, done!

---

_@tibor-reiss reviewed on 2024-04-16 18:16_

---

_Review comment by @tibor-reiss on `crates/ruff_linter/src/rules/pylint/rules/invalid_length_return.rs`:78 on 2024-04-16 18:16_

TLDR: changed.
I was not sure about which should be first because I would expect that there are more cases where the return type is wrong, compared to where the type is ok (int) but the sign not.


---

_Renamed from "[pylint] Implement invalid-length-returned (PLE0303)" to "[`pylint`] Implement `invalid-length-returned` (`E0303`)" by @charliermarsh on 2024-04-18 01:47_

---

_Label `preview` added by @charliermarsh on 2024-04-18 01:47_

---

_Merged by @charliermarsh on 2024-04-18 01:54_

---

_Closed by @charliermarsh on 2024-04-18 01:54_

---
