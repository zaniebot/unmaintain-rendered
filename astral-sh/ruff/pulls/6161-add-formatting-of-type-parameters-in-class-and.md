```yaml
number: 6161
title: Add formatting of type parameters in class and function definitions
type: pull_request
state: merged
author: zanieb
labels:
  - formatter
assignees: []
merged: true
base: main
head: zanie/695-format
created_at: 2023-07-28T19:23:48Z
updated_at: 2023-08-02T20:45:13Z
url: https://github.com/astral-sh/ruff/pull/6161
synced_at: 2026-01-12T15:55:20Z
```

# Add formatting of type parameters in class and function definitions

---

_@zanieb_

Part of #5062 
Closes https://github.com/astral-sh/ruff/issues/5931

Implements formatting of a sequence of type parameters in a dedicated struct for reuse by classes, functions, and type aliases (preparing for #5929). Adds formatting of type parameters in class and function definitions — previously, they were just elided. 

---

_Comment by @github-actions[bot] on 2023-07-28 19:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.13ms     4.9 MB/sec    1.00      8.3±0.15ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.02  1675.5±59.89µs     9.9 MB/sec    1.00  1648.6±31.97µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    186.7±6.27µs    15.8 MB/sec    1.01    188.1±6.58µs    15.7 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.14ms     7.1 MB/sec    1.00      3.5±0.10ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.8±0.05ms     3.8 MB/sec    1.00     10.8±0.04ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.03ms     6.0 MB/sec    1.00      2.8±0.02ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    381.1±2.22µs     7.7 MB/sec    1.00    380.1±2.35µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.03ms     5.2 MB/sec    1.03      5.0±0.13ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.00      5.7±0.04ms     7.1 MB/sec    1.01      5.8±0.05ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1196.6±4.60µs    13.9 MB/sec    1.00   1194.1±7.16µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    132.6±1.71µs    22.3 MB/sec    1.00    132.4±1.29µs    22.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.03ms    10.2 MB/sec    1.01      2.5±0.04ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01     12.6±0.77ms     3.2 MB/sec     1.00     12.5±0.76ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.04      2.5±0.19ms     6.6 MB/sec     1.00      2.4±0.19ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   261.8±20.98µs    11.3 MB/sec     1.01   265.4±25.19µs    11.1 MB/sec
formatter/pydantic/types.py                1.04      5.5±0.41ms     4.7 MB/sec     1.00      5.3±0.35ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     17.2±0.85ms     2.4 MB/sec     1.01     17.5±1.13ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.6±0.34ms     3.6 MB/sec     1.00      4.5±0.38ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   556.2±56.95µs     5.3 MB/sec     1.01   560.0±50.88µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.45ms     3.3 MB/sec     1.02      7.9±0.47ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.5±0.51ms     4.3 MB/sec     1.00      9.5±0.64ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1926.2±129.22µs     8.6 MB/sec    1.00  1921.8±117.94µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   216.3±23.63µs    13.6 MB/sec     1.05   226.6±27.81µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.31ms     6.0 MB/sec     1.00      4.2±0.35ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-28 19:48_

Ooooh yes

---

_Marked ready for review by @zanieb on 2023-07-28 20:12_

---

_Review requested from @MichaReiser by @zanieb on 2023-07-28 22:25_

---

_Review requested from @konstin by @zanieb on 2023-07-28 22:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:280 on 2023-07-31 08:17_

Would you mind adding an example with a trailing `[` and leading `]` comment


```suggestion
def type_param_comments[ # trailing bracket comment
    # leading comment
    A,

    # in between comment

    B, 
    # another leading comment
    C,
    D, # trailing comment
    # trailing end of line comment
]():
    pass
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:265 on 2023-07-31 08:18_

Would you mind adding an example for an empty type param that contains comments

```suggestion
def type_param_empty_comments[ # trailing
	# own line 
]: pass
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:38 on 2023-07-31 08:20_

Nit: Use `first()` to avoid `unwrap()` and `if` can be used in expression positions.


```suggestion
            let sequence_end = if let Some(first) = bases.first() {
				first.start()
			} else if Some(keyword) = keywords.first() {
				keyword.start()
			} else {
				body.first().unwrap().start()
			};
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:54 on 2023-07-31 08:22_

Nit: I would move the `parenthesized` call into `FormatTypeParamsClause` because it seems to be necessary on every call site. 

We could even go one step further and also move the `is_empty` check into `FormatTypeParamsClause`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/type_param/type_param_param_spec.rs`:14 on 2023-07-31 08:35_

Nit
```suggestion
        write!(f, [text("**"), name.format()])
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/type_param/type_param_type_var_tuple.rs`:14 on 2023-07-31 08:35_

Nit:

```suggestion
        let TypeParamTypeVarTuple { range: _, name } = item;
        write!(f, [text("*"), name.format()])
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/type_param/type_param_clause.rs`:5 on 2023-07-31 08:36_

Nit: `crate::prelude::*` should give you most relevant imports

```suggestion
use crate::prelude::*;
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/type_param/type_param_clause.rs`:11 on 2023-07-31 08:43_

It's generally preferred to pass slices rather than references to vectors. 

Rust passes a pointer to the vector's address if you pass a vector by reference. That result is that accessing the first element requires two dereferences:

1. Derefence the `&Vec` to get `Vec``
2. Dereference `start` pointer to get the first element


Rust avoids the first dereference if you pass the vec as a slice because it directly passes the start and end positions (omitting the Vec's capacity). This removes the need for the first dereference.

```suggestion
    pub(crate) type_params: &'a [TypeParam],
```

---

_@MichaReiser approved on 2023-07-31 08:43_

Niiiice! This is looking good. I have two test cases that I recommend you adding and I think we can move some more logic into the clause formatting helper. 

---

_Label `formatter` added by @konstin on 2023-07-31 12:42_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/class_definition.py`:195 on 2023-07-31 13:16_

Could you add an empty parentheses test case such as
```python
class TestEmptTypeParams[  # end-of-line comment
    # own line comment
]:
    pass

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:37 on 2023-07-31 13:18_

Does this mean we remove empty brackets (`class A[]: pass` -> `class A: pass`), and if so, is that intended?

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/type_param/type_param_clause.rs`:11 on 2023-07-31 13:28_

Another reason is that `&[T]` is in a way a superclass to `&Vec<T>` (`Vec<T>` derefs to `[T]`) and allows passing e.g. slices of a vec too.

---

_@konstin approved on 2023-07-31 13:29_

---

_@zanieb reviewed on 2023-07-31 13:49_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:37 on 2023-07-31 13:49_

That is not valid syntax

```python
❯ python3.12 -c "class Foo[]: ..."
  File "<string>", line 1
    class Foo[]: ...
              ^
SyntaxError: invalid syntax
```

---

_@zanieb reviewed on 2023-07-31 13:49_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:265 on 2023-07-31 13:49_

Should we handle this? It's not valid syntax

```
❯ python3.12 -c "class Foo[]: ..."
  File "<string>", line 1
    class Foo[]: ...
              ^
SyntaxError: invalid syntax
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:37 on 2023-07-31 13:49_

That's good :)

---

_@MichaReiser reviewed on 2023-07-31 13:49_

---

_@zanieb reviewed on 2023-07-31 13:50_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:265 on 2023-07-31 13:50_

```
❯ python3.12 -c "def foo[](): ..."
  File "<string>", line 1
    def foo[](): ...
           ^
SyntaxError: expected '('
```

---

_@konstin reviewed on 2023-07-31 13:53_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:37 on 2023-07-31 13:53_

yep that definitely makes things easier, could you add a comment about that given that cases where empty optional parentheses aren't allowed are otherwise rare?

---

_@zanieb reviewed on 2023-07-31 14:50_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/statement/stmt_class_def.rs`:37 on 2023-07-31 14:50_

👍 

---

_@zanieb reviewed on 2023-07-31 17:50_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:278 on 2023-07-31 17:50_

eek, formats to invalid syntax 
```python
# Type parameter comments

def type_param_comments[
    # trailing bracket comment
    # leading comment
    A,
    # in between comment
    B,
    # another leading comment
    C,
    D,  # trailing comment
]# trailing bracket comment
():
```

---

_@MichaReiser reviewed on 2023-07-31 21:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:278 on 2023-07-31 21:00_

This is weird....

Okay, so the issue is that `TypeParams` isn't a node. This means the formatter only sees the type params, but the ranges of the type params all end short before the trailing comment. [Playground](https://play.ruff.rs/?secondary=AST#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksuflJmPicWqpZQpWi1YWEBPWNFbF5CSiKuHA9uanpmSOisPwoGJS4GjwAFroAHKu9PORFnLZNYAAsvcWl5UJsogCiMRdQAGKQbC1QW5ooPPIDxJx4upAADtIAMwARhcN1E+Fg0AA1ihVhhoLh0ls+HhBGBQBBRDJARh+JwPpkeIRBrp8HBirFRMRCP9-pliMQMOIpLIlBoMRTYFTbpBafTGcz0rhZsTSbh+BpyZSUNSoHBEEgiq9EZpEbhpDKeXK+WMMrMiPBZB0eNJ-uhtcU2ABfXpQ2HwjBkJFoQm6LHYyBLJRoHBodAYHgSf4sziZMoZLXnbFegD0wf+ENjUDjCkUCZDydjkDjAbkceISwkj1jz1EWCWKCwMKD5sD8SwKH+aNwVpQtvt0LhCI4GjIKCUHr5bRSUkIWwosywioxTE7uQdPedXF4-AxntEHG4fAE+LSBumxDODEYC8h3adOBDmSrAh+AmHOcVyDDZWws4wSEJSwwcOkSAZJgHKSEkPBcrK55QEuV4IICygaEsJ6blAKKcE26jaH8BghJwYQAMJwdICFIQAOqRxAmAY5EGPh5FhORlFhCE5FoMABw2gYGCsexNphN42ZQBI-AYHUajfHsuguFBkAwQiAyKBIxDSnQKGQBI0BaCqygLHMSSLCsdDSRAdqLpeCIAI6EAgiQbnquDTKKGBWTZVB-GgCCEFOgnqYQa6ObMLm2e5nnefKYgIFgx46RoznWcFdARV5lA+dAcgIJwmBUDOFq6DwiiEB2Jldo6CInPgT5evqmQYEMEhuTGKaQEYGlwoJohGNAxBumU7VQEYmT-NCTZ9c1dQlGg1Dyh142wGgRgBhSfngaNRh1SgRirQosCFZttxnsVZmlUGmWyJwIYZCtql8i6uDqCU0B8HIsznf8l0YgCShgVNN1IqKwH-JwHo2jJcl1ha2BVjWUqVTS+XoSe3K8jm8QoCGHwSAg4iUHOtyiGagMLKW2LllABW4Hw9UYCg22EI9gYusU2DQkyDWnuF5OU7MNNwHTiSYAGOCKI9GRzgdYCmRex0aO8iQJLD0EdGUD61dA9W4ym-WjTLPBy+U02oTrevExApN9AUtRK62qvqxUoPmfiIbTFghLadgeAzshfIvsqEjLdwMx5QVRUSyVy6vZd7u4M9FMPnZz7TF1bMoaI-yul1fz-Ggo2AkoijIJn0A54gPCwNIAB08QMlQH3-DoBv8oOZAZG2iXEI+DcFMQGRQgXiU8BVDe4JIgJ-LgSYNxpPBDTZ0xkH8TujVPM+l5wZDl4CK+Z7wo08DC-CJIofx7znCDQootewDnSIlJnbYN3AWzhn8j+jUsZ8IHInAoEgH1LHIJsJb22OoCWyHxjzSBStdHMdQEiklmGnTIFMqzFAxPlQq4VEFq3eMoPYNsqDg32JAEkQ0siYKUNg+GeDaYEIJkQo8+s+RYPqlQ2YNDmT52VHQ4+dIUoc2gJwVB90rLhlmFPSsokMjZDxlAAAQknS4WgmwtgfKNRRyjWyjQAGq8xQJcRQ+cj4NwAPIAGV9GGNGgASWMRYqRDdLjR3DHgeqFM7FGJkfySKcIeCVwMVI-a-DBEENHMIrgNVxG-nwFI5o4UNKKFrIgsCKC2boJDpLaCDsOgkEDCBVxV1MR6gPDVdMKger4IgjqGSgjLoK1khkDCSBhahjgCoVB7ZwrRMUBhb4Cw1ABQ6XyXpGh+kzGpkona3x0STC9NeMgYyuqOzeooApSNdQ5mIENV2eAgzCwDrpa8Gkg4YL5EBAYzpZDcLoGkzpDTZjdxWV+H8IlijK2mXQNZ4UZxM2KPeJ6+xPl8i6RhHgCAgxwRmaIGE6QkB3TqBfD4iCzSQqgNC5Ad1ljhkwEi6M7M+RothXMSKcBJFzQGCi82wsVTp0UJgOewtFC4pyDmTI0J-lLPehgM5R9EpEBWSgj4oLmYIFQYwllKARGZDHMsgpzLZks3abEvkOABBbApmLcKpTGgar5OkOYMxmQDi6fsOVUwDWInwIfDlKyMQYBBOFAKhr3ioCSIQjELhbndMDMULBiQKWvOtty6RmtZITngaNY8NK6VrwZVqTuSwsUYBxaNeFx4k1KGRQ3RAM5YCkoDEfQJQzqytg3CDQ6OIsAzgHHUjSWlrykK0ISXFYIZIWn+AiOqMNoFVWKbMdaGsmrFB4AAVQnp4xISgAAi6KI3vFHfhBVu9ByKGnbChdXUfohqHaOgAsljPyZDx3LtXbgPd2ND0hq6tIXAWBTFzrHZe4g16sAABVj0zobtu-4b7jyTsekXBuFJuDwPUc2TRWa8AaB3TXaAMsl6aUnZwfAFVC05iwAqlh78BbVgyCLC+-rUToUw1jFkOHhagvw80Vt0h-RUDNFAwpOZqqzE-gMRAuktjEFrIc767ZqMeSivR-YalmOkaFnhnVOYGRwQGGaMTuGKNi2ozBeOXpQky1FMoKKdty1QEBI5L2CclQYA0hodC8hdFuuDYO-Ko0yDSGCqhr0tbnTCxvSg3QIIABMcTNL3RJIMDEAA2XzWkOQYgAKyhepYkfJkXjKh1yICOkGhhYBjqXCZs6hTTnVmATLtYA1m2hADaSIZXFpgDoUYZhRgePqvoLcSd8oADElW9mOQ0GAMgwsazvDAHVngIBGAGDCHQNOTIgA)

The easiest from a formatter perspective would be to introduce a new `TypeParams` node with a range from the opening `[` to the closing `]`. It doesn't seem entirely unreasonable to me, considering that other nodes with parentheses like `Arguments`, `ExprList` etc, are also nodes. However, this would require changes to `Preorder` visitor. 

The alternative is to implement your custom logic in `placement.rs`. 

---

_@zanieb reviewed on 2023-08-02 15:56_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/type_param/type_param_clause.rs`:11 on 2023-08-02 15:56_

Thanks for the additional knowledge!

---

_@zanieb reviewed on 2023-08-02 15:56_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/function.py`:278 on 2023-08-02 15:56_

Resolved by #6261 

---

_Review comment by @zanieb on `crates/ruff_python_formatter/src/comments/placement.rs`:567 on 2023-08-02 16:17_

As implemented in #6274 

---

_@zanieb reviewed on 2023-08-02 16:17_

---

_@charliermarsh approved on 2023-08-02 17:28_

Looks great.

---

_Merged by @zanieb on 2023-08-02 20:29_

---

_Closed by @zanieb on 2023-08-02 20:29_

---

_Branch deleted on 2023-08-02 20:29_

---
