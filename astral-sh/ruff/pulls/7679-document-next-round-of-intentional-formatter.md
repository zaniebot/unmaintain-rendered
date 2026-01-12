```yaml
number: 7679
title: Document next round of intentional formatter deviations
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
  - formatter
assignees: []
merged: true
base: main
head: charlie/format
created_at: 2023-09-27T20:18:37Z
updated_at: 2023-09-29T17:28:50Z
url: https://github.com/astral-sh/ruff/pull/7679
synced_at: 2026-01-12T15:55:24Z
```

# Document next round of intentional formatter deviations

---

_@charliermarsh_

## Summary

Based on today's triage with @MichaReiser.

Closes https://github.com/astral-sh/ruff/issues/7652.
Closes https://github.com/astral-sh/ruff/issues/7320.
Closes https://github.com/astral-sh/ruff/issues/7052.
Closes https://github.com/astral-sh/ruff/issues/7314.
Closes https://github.com/astral-sh/ruff/issues/7317.
Closes https://github.com/astral-sh/ruff/issues/7323.
Closes https://github.com/astral-sh/ruff/issues/7320.
Closes https://github.com/astral-sh/ruff/issues/7315.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-27 20:18_

---

_Label `documentation` added by @charliermarsh on 2023-09-27 20:19_

---

_Label `formatter` added by @charliermarsh on 2023-09-27 20:19_

---

_Comment by @codspeed-hq[bot] on 2023-09-27 20:27_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/format)

### Merging #7679 will **improve performances by 4.05%**

<sub>Comparing <code>charlie/format</code> (57068bf) with <code>main</code> (974262a)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/format` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[large/dataset.py]` | 95.7 ms | 92 ms | +4.05% |


---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:329 on 2023-09-28 07:23_

I don't think black is preserving the line breaks. What black does is it tries to fit the entire logical line:

```python
print("aaaaaaa {:.2f}% " "aaaaaaaaaaaaaaaa".format((((df_aaaaaaaaaaa["key"].isna()) | (df_aaaaaaaaaaa["key"] == 0)).sum() / len(df_aaaaaaaaaaa)) * 100))
```

Which is way too long. First, it expands the outermost call

```python
print(
    "aaaaaaa {:.2f}% " "aaaaaaaaaaaaaaaa".format((((df_aaaaaaaaaaa["key"].isna()) | (df_aaaaaaaaaaa["key"] == 0)).sum() / len(df_aaaaaaaaaaa)) * 100)
)
```

But the argument line is still too long. So black searches for the operator with the highest split precedence which in this case is the implicit string concatenation and it splits between the parts

```python
print(
    "aaaaaaa {:.2f}% "
    "aaaaaaaaaaaaaaaa".format((((df_aaaaaaaaaaa["key"].isna()) | (df_aaaaaaaaaaa["key"] == 0)).sum() / len(df_aaaaaaaaaaa)) * 100)
)
```

This makes the first argument-line fit, but not the second. So black continues splitting that one by iteratively searching the highest precedence operator in the line and splitting around all operators with the given priority. 

I think the "easiest" explanation is that Black gives implicit concatenated a high split priority which makes sense in binary like expressions but IMO leads to undesired results in attribute chains.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:357 on 2023-09-28 07:27_

We should try to minimize all the examples to make them easier to understand:
* Remove unnecessary arguments
* Replace complex-expressions with simple expressions 
* Remove code that's unnecessary for the example

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:390 on 2023-09-28 07:36_

I think this applies to all tuples, not just in assignment expressions (or maybe all tuples except in for positions?)

```python
(0.0, 0.0), (0.0, 0.0,)

# Ruffed
(
    (0.0, 0.0),
    (
        0.0,
        0.0,
    ),
)

# Blacked
(0.0, 0.0), (
    0.0,
    0.0,
)
```

There's also another difference where Ruff always parenthesizes single element tuples where Black does not

```python
(0.0, 0.0),

# Ruffed
((0.0, 0.0),)

# Blacked
(0.0, 0.0),
```

This is to avoid accidental tuple expressions (e.g. you delete the second element but forget to remove the `,`)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:490 on 2023-09-28 07:37_

```suggestion
### Type annotations may be parenthesized when expanded ([#7315](https://github.com/astral-sh/ruff/issues/7315))
```

IMO, This applies to all type annotations 

---

_@MichaReiser reviewed on 2023-09-28 07:37_

Thank you!

Please take a look at the performance regression 😂 

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-28 18:30_

---

_Comment by @charliermarsh on 2023-09-28 18:30_

@MichaReiser - Ready for another round!

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/README.md`:329 on 2023-09-28 18:31_

Thanks, this is tricky but I did my best. I appreciate the clarification here.

---

_@charliermarsh reviewed on 2023-09-28 18:31_

---

_@charliermarsh reviewed on 2023-09-28 18:31_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/README.md`:390 on 2023-09-28 18:31_

Thank you. I moved the single-element tuple thing to its own deviation.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:327 on 2023-09-29 09:37_

```suggestion
### Implicit string concatenations attribute access ([#7052](https://github.com/astral-sh/ruff/issues/7052))
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:462 on 2023-09-29 09:39_

Do we want to explain the reasoning (to make it clear that it is a one-element tuple because missing the trailing `,` is easy and you may end up with structs like `assertEquals(a, b), ` which most certainly is unintended (see konsti's PR that removed most these unintended tuple uses from the django project.

---

_@MichaReiser approved on 2023-09-29 09:40_

---

_Merged by @charliermarsh on 2023-09-29 17:27_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---

_Branch deleted on 2023-09-29 17:27_

---
