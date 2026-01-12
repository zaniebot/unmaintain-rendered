```yaml
number: 6512
title: Add an implicit concatenation flag to string and bytes constants
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/implicit-concat
created_at: 2023-08-11T19:24:50Z
updated_at: 2023-08-14T13:55:36Z
url: https://github.com/astral-sh/ruff/pull/6512
synced_at: 2026-01-12T02:52:04Z
```

# Add an implicit concatenation flag to string and bytes constants

---

_Pull request opened by @charliermarsh on 2023-08-11 19:24_

## Summary

Per the discussion in https://github.com/astral-sh/ruff/discussions/6183, this PR adds an `implicit_concatenated` flag to the string and bytes constant variants. It's not actually _used_ anywhere as of this PR, but it is covered by the tests.

Specifically, we now use a struct for the string and bytes cases, along with the `Expr::FString` node. That struct holds the value, plus the flag:

```rust
#[derive(Clone, Debug, PartialEq, is_macro::Is)]
pub enum Constant {
    Str(StringConstant),
    Bytes(BytesConstant),
    ...
}

#[derive(Clone, Debug, PartialEq, Eq)]
pub struct StringConstant {
    /// The string value as resolved by the parser (i.e., without quotes, or escape sequences, or
    /// implicit concatenations).
    pub value: String,
    /// Whether the string contains multiple string tokens that were implicitly concatenated.
    pub implicit_concatenated: bool,
}

impl Deref for StringConstant {
    type Target = str;
    fn deref(&self) -> &Self::Target {
        self.value.as_str()
    }
}

#[derive(Clone, Debug, PartialEq, Eq)]
pub struct BytesConstant {
    /// The bytes value as resolved by the parser (i.e., without quotes, or escape sequences, or
    /// implicit concatenations).
    pub value: Vec<u8>,
    /// Whether the string contains multiple string tokens that were implicitly concatenated.
    pub implicit_concatenated: bool,
}

impl Deref for BytesConstant {
    type Target = [u8];
    fn deref(&self) -> &Self::Target {
        self.value.as_slice()
    }
}
```

## Test Plan

`cargo test`



---

_Comment by @github-actions[bot] on 2023-08-11 19:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-11 20:05_

---

_Label `internal` added by @MichaReiser on 2023-08-14 08:29_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:28 on 2023-08-14 08:30_

Did you refactor this code? Should it be part of this PR?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/comparable.rs`:339 on 2023-08-14 08:32_

Is the idea here that two strings compare equal regardless of whether they are implicitly concatenated? If not, add the `implicit_concatenated` flag to `Str` and `Bytes`.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:515 on 2023-08-14 08:35_

Nit: I find the `collect` + `into` hard to read. It's less expressive as the `Constant::Bytes` before that set focus on that this is a byte string


```suggestion
            value: content.as_bytes().into(),
```


---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:673 on 2023-08-14 08:39_

I believe we need to set the implicit concat flag in this case. But not entirely sure. But you can see how it expands the range of the current element.

---

_@MichaReiser approved on 2023-08-14 08:39_

---

_Review requested from @dhruvmanila by @MichaReiser on 2023-08-14 08:39_

---

_@dhruvmanila reviewed on 2023-08-14 11:23_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:339 on 2023-08-14 11:23_

> Is the idea here that two strings compare equal regardless of whether they are implicitly concatenated?

Yes

```python
>>> "hello" " world" == "hello world"
True
```

---

_@dhruvmanila approved on 2023-08-14 11:45_

Looks good!

(Just a mental node to revisit the implicit string concatenation while working on the parser implementation for f-string part as I'm noticing the complexity involved and the need for `strings.rs`)

---

_@charliermarsh reviewed on 2023-08-14 13:27_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:28 on 2023-08-14 13:27_

Sorry, I think this was accidentally included when I was playing around with removing LibCST. I must've gotten my branches confused.

---

_@charliermarsh reviewed on 2023-08-14 13:27_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/comparable.rs`:339 on 2023-08-14 13:27_

Yeah, that's intentional.

---

_@charliermarsh reviewed on 2023-08-14 13:28_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/comparable.rs`:339 on 2023-08-14 13:28_

(Adding a note.)

---

_@charliermarsh reviewed on 2023-08-14 13:33_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string.rs`:673 on 2023-08-14 13:33_

I think it's fine as-is, because `current` is just `Vec<String>`, and these get converted to `Expr::Constant` when pushed onto `deduped` (which sets the implicit flag).

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string.rs`:515 on 2023-08-14 13:36_

Interestingly, `content.as_bytes().to_vec().into()` actually behaves differently (I'm seeing some lines that aren't too long be marked as such). I just woke up so I'm not sure why. I'm gonna look into it later, it could be a legitimate bug.

```diff
   37    37 │ 10 10 | ) -> None: ...
   38    38 │ 11 11 | def f5(
   39    39 │ 12 12 |     x: bytes = b"50 character byte stringgggggggggggggggggggggggggg",  # OK
   40    40 │
         41 │+PYI053.pyi:18:16: PYI053 [*] String and bytes literals longer than 50 characters are not permitted
         42 │+   |
         43 │+16 | ) -> None: ...
         44 │+17 | def f7(
         45 │+18 |     x: bytes = b"50 character byte stringggggggggggggggggggggggggg\xff",  # OK
         46 │+   |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PYI053
         47 │+19 | ) -> None: ...
         48 │+20 | def f8(
         49 │+   |
         50 │+   = help: Replace with `...`
         51 │+
         52 │+ℹ Suggested fix
         53 │+15 15 |     x: bytes = b"51 character byte stringgggggggggggggggggggggggggg",  # Error: PYI053
         54 │+16 16 | ) -> None: ...
         55 │+17 17 | def f7(
         56 │+18    |-    x: bytes = b"50 character byte stringggggggggggggggggggggggggg\xff",  # OK
         57 │+   18 |+    x: bytes = ...,  # OK
         58 │+19 19 | ) -> None: ...
         59 │+20 20 | def f8(
         60 │+21 21 |     x: bytes = b"51 character byte stringgggggggggggggggggggggggggg\xff",  # Error: PYI053
         61 │+
   41    62 │ PYI053.pyi:21:16: PYI053 [*] String and bytes literals longer than 50 characters are not permitted
   42    63 │    |
   43    64 │ 19 | ) -> None: ...
   44    65 │ 20 | def f8(
```

---

_@charliermarsh reviewed on 2023-08-14 13:36_

---

_Merged by @charliermarsh on 2023-08-14 13:46_

---

_Closed by @charliermarsh on 2023-08-14 13:46_

---

_Branch deleted on 2023-08-14 13:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:515 on 2023-08-14 13:55_

It differs for non ascii characters because `c as u8` truncates the non-ascii part. Whereas `.as_bytes` takes the string as is. 

---

_@MichaReiser reviewed on 2023-08-14 13:55_

---
