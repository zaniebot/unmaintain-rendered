```yaml
number: 7415
title: Make SIM401 catch ternary operations
type: pull_request
state: merged
author: Flowrey
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: make-SIM401-catch-ternay-operations
created_at: 2023-09-15T18:51:33Z
updated_at: 2023-10-20T21:36:23Z
url: https://github.com/astral-sh/ruff/pull/7415
synced_at: 2026-01-12T15:55:23Z
```

# Make SIM401 catch ternary operations

---

_@Flowrey_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #7288 
Make SIM401 rules to catch ternary operations.

## Test Plan

Tested against SIM401.py fixtures



---

_Marked ready for review by @Flowrey on 2023-09-15 19:39_

---

_Comment by @github-actions[bot] on 2023-09-15 19:50_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -1, 0 error(s))

<details><summary>rotki (+3, -1)</summary>
<p>

<pre>
- [*] 11 fixable with the `--fix` option (223 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 11 fixable with the `--fix` option (225 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ <a href='https://github.com/rotki/rotki/blob/1b0e71f8476f8cc9003891ac2e4c8cce960414c3/rotkehlchen/db/updates.py#L204'>rotkehlchen/db/updates.py:204:27:</a> SIM401 Use `rule_data.get('links', {})` instead of an `if` block
+ <a href='https://github.com/rotki/rotki/blob/1b0e71f8476f8cc9003891ac2e4c8cce960414c3/rotkehlchen/externalapis/cryptocompare.py#L327'>rotkehlchen/externalapis/cryptocompare.py:327:24:</a> SIM401 Use `json_ret.get('Data', json_ret)` instead of an `if` block
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| SIM401 | 2 | 2 | 0 |



---

_Review requested from @charliermarsh by @charliermarsh on 2023-09-16 03:41_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:1051 on 2023-09-18 05:19_

nit: we should probably move this behind the `checker.patch` check as we don't want to perform clone and allocation if the user didn't request a fix for this.

We could actually create a `generate_fix` function which would return a `Fix` to have a linear code flow inside the `checker.patch` block. The function could look something along the lines of:
```rust
fn generate_fix(/* pass in all the required parameters */) -> Fix {
    let node = orelse.clone();
    let node1 = *test_key.clone();
    let node2 = ast::ExprAttribute {
        value: expected_subscript.clone(),
        attr: Identifier::new("get".to_string(), TextRange::default()),
        ctx: ExprContext::Load,
        range: TextRange::default(),
    };
    let node3 = ast::ExprCall {
        func: Box::new(node2.into()),
        arguments: Arguments {
            args: vec![node1, node],
            keywords: vec![],
            range: TextRange::default(),
        },
        range: TextRange::default(),
    };

    let contents = checker.generator().expr(&node3.into());
	Fix::suggested(Edit::range_replacement(
        contents,
        expr.range(),
    ))
}
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:1016 on 2023-09-18 05:21_

What's the motivation behind not adding support for the `not in` comparison?

```python
var = "default" if key not in data else data[key]
# The above code could be converted to `var = data.get(key, "default")`
```
I think it is supported in the expanded version, right? This is what I get:

```diff
-if key not in data:
-    var = "default"
-else:
-    var = data[key]
+var = data.get(key, "default")
```

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/flake8_simplify/SIM401.py`:119 on 2023-09-18 05:26_

Can you please add some more test cases? For example:
```python
var = "default3" if key not in a_dict else a_dict[key]

# using `elif` pattern
var = "..." if foo() else a_dict[key] if key in a_dict else "default"
```

---

_@dhruvmanila reviewed on 2023-09-18 05:27_

---

_@dhruvmanila reviewed on 2023-09-18 05:29_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:1051 on 2023-09-18 05:29_

Oh, never mind, it's being used to display in the violation message. This makes me wonder if it's worth it as we could use a generic "dict.get(key, default)" template.

---

_Label `rule` added by @dhruvmanila on 2023-09-18 06:01_

---

_@konstin reviewed on 2023-09-18 07:58_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:1041 on 2023-09-18 07:58_

nit: could you give them more descriptive names than node 1/2/3?

---

_@Flowrey reviewed on 2023-09-18 16:55_

---

_Review comment by @Flowrey on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:1016 on 2023-09-18 16:55_

I do not supported `not in` comparison because it is not valid python code
```python
var = "default" if key not in data else data[key]
Traceback (most recent call last):
  File "<string>", line 1, in <module>
NameError: name 'key' is not defined
```

---

_@Flowrey reviewed on 2023-09-18 17:36_

---

_Review comment by @Flowrey on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:1041 on 2023-09-18 17:36_

Sure !

---

_Comment by @codspeed-hq[bot] on 2023-09-18 17:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Flowrey:make-SIM401-catch-ternay-operations)

### Merging #7415 will **not alter performance**

<sub>Comparing <code>Flowrey:make-SIM401-catch-ternay-operations</code> (a2ac4b7) with <code>main</code> (7a5f988)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @charliermarsh on 2023-09-21 00:51_

My only hesitation here is whether this should be a separate rule and / or whether it should be behind the preview flag.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-21 00:51_

---

_Comment by @Flowrey on 2023-09-23 15:10_

I've looked at `flake8-simplify` and they dont seem to implement `SIM401` for ternary operators.

---

_@zanieb reviewed on 2023-10-20 19:06_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_simplify/rules/ast_if.rs`:1016 on 2023-10-20 19:06_

That error is because you have not defined `key` not because it's invalid syntax

```python
key = "test"
data = {}

var = "default" if key not in data else data[key]
var = data[key] if key in data else "default"

if key not in data:
    var = "default"
else:
    var = data[key]


if key in data:
    var = data["key"]
else:
    var = "default"

data.get("key", "default")
```

---

_Comment by @charliermarsh on 2023-10-20 19:34_

I'll take this one to completion -- gonna put this behavior under preview mode.

---

_Merged by @charliermarsh on 2023-10-20 20:47_

---

_Closed by @charliermarsh on 2023-10-20 20:47_

---

_Label `preview` added by @zanieb on 2023-10-20 21:36_

---
