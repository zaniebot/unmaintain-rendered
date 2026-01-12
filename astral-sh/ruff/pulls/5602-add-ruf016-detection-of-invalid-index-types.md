```yaml
number: 5602
title: "Add RUF016: Detection of invalid index types"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: rule/ruf015
created_at: 2023-07-07T22:21:30Z
updated_at: 2023-07-12T05:23:07Z
url: https://github.com/astral-sh/ruff/pull/5602
synced_at: 2026-01-12T03:36:55Z
```

# Add RUF016: Detection of invalid index types

---

_Pull request opened by @zanieb on 2023-07-07 22:21_

Detects invalid types for tuple, list, bytes, string indices.

For example, the following will raise a `TypeError` at runtime and when imported Python will display a `SyntaxWarning`

```python
var = [1, 2, 3]["x"]
```

```
example.py:1: SyntaxWarning: list indices must be integers or slices, not str; perhaps you missed a comma?
  var = [1, 2, 3]["x"]
Traceback (most recent call last):
  File "example.py", line 1, in <module>
    var = [1, 2, 3]["x"]
          ~~~~~~~~~^^^^^
TypeError: list indices must be integers or slices, not str
```

Previously, Ruff would not report the invalid syntax but now a violation will be reported. This does not apply to cases where a variable, call, or complex expression is used in the index — detection is roughly limited to static definitions, which matches Python's warnings.

```
❯ ./target/debug/ruff example.py --select RUF015 --show-source --no-cache
example.py:1:17: RUF015 Indexed access to type `list` uses type `str` instead of an integer or slice.
  |
1 | var = [1, 2, 3]["x"]
  |                 ^^^ RUF015
  |
```

Closes https://github.com/astral-sh/ruff/issues/5082
xref https://github.com/python/cpython/commit/ffff1440d118cae189a6c2baf595dda52cdc7c3a


---

_Comment by @github-actions[bot] on 2023-07-07 22:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.01      8.6±0.46ms     4.8 MB/sec     1.00      8.4±0.37ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1981.2±115.85µs     8.4 MB/sec    1.01  1997.7±147.35µs     8.3 MB/sec
formatter/numpy/globals.py                 1.01   227.4±14.01µs    13.0 MB/sec     1.00   225.5±13.82µs    13.1 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.19ms     6.1 MB/sec     1.02      4.3±0.21ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.62ms     2.7 MB/sec     1.01     15.0±0.53ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.7±0.15ms     4.5 MB/sec     1.00      3.6±0.15ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   471.5±37.62µs     6.3 MB/sec     1.02   478.9±25.68µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.37ms     3.8 MB/sec     1.00      6.6±0.29ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.29ms     5.6 MB/sec     1.00      7.2±0.24ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1583.3±67.33µs    10.5 MB/sec     1.02  1608.8±84.20µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.04   191.8±16.00µs    15.4 MB/sec     1.00    184.2±9.93µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.14ms     7.7 MB/sec     1.00      3.3±0.19ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.06ms     4.3 MB/sec    1.00      9.5±0.05ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.01ms     7.9 MB/sec    1.02      2.2±0.09ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    229.7±5.18µs    12.8 MB/sec    1.01    232.4±7.65µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.06ms     5.4 MB/sec    1.00      4.7±0.05ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     15.9±0.15ms     2.6 MB/sec    1.00     15.6±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.04ms     4.0 MB/sec    1.00      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   427.6±11.45µs     6.9 MB/sec    1.00    426.3±4.69µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.05ms     3.6 MB/sec    1.00      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.05ms     4.9 MB/sec    1.00      8.1±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1672.7±12.11µs    10.0 MB/sec    1.00  1668.8±18.43µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.7±1.73µs    16.3 MB/sec    1.00    180.5±4.26µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.02ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-10 14:47_

In a comparison with Python's syntax warnings, we use a different message format and perform additional validation of the types used _in_ slices.

```
❯ python crates/ruff/resources/test/fixtures/ruff/RUF015.py     
RUF015.py:20: SyntaxWarning: str indices must be integers or slices, not str; perhaps you missed a comma?
RUF015.py:21: SyntaxWarning: str indices must be integers or slices, not str; perhaps you missed a comma?
RUF015.py:24: SyntaxWarning: bytes indices must be integers or slices, not str; perhaps you missed a comma?
RUF015.py:27: SyntaxWarning: list indices must be integers or slices, not str; perhaps you missed a comma?
RUF015.py:28: SyntaxWarning: tuple indices must be integers or slices, not str; perhaps you missed a comma?
RUF015.py:31: SyntaxWarning: list indices must be integers or slices, not str; perhaps you missed a comma?
RUF015.py:34: SyntaxWarning: str indices must be integers or slices, not tuple; perhaps you missed a comma?
RUF015.py:37: SyntaxWarning: list indices must be integers or slices, not str; perhaps you missed a comma?
RUF015.py:40: SyntaxWarning: list indices must be integers or slices, not float; perhaps you missed a comma?
RUF015.py:43: SyntaxWarning: list indices must be integers or slices, not dict; perhaps you missed a comma?
RUF015.py:46: SyntaxWarning: list indices must be integers or slices, not dict; perhaps you missed a comma?
RUF015.py:49: SyntaxWarning: list indices must be integers or slices, not tuple; perhaps you missed a comma?
RUF015.py:52: SyntaxWarning: list indices must be integers or slices, not list; perhaps you missed a comma?
RUF015.py:55: SyntaxWarning: list indices must be integers or slices, not set; perhaps you missed a comma?
RUF015.py:58: SyntaxWarning: list indices must be integers or slices, not bytes; perhaps you missed a comma?
RUF015.py:76: SyntaxWarning: list indices must be integers or slices, not str; perhaps you missed a comma?
```

```
❯ ./target/debug/ruff check --select RUF015 crates/ruff/resources/test/fixtures/ruff/RUF015.py
RUF015.py:20:13: RUF015 Indexed access to type `str` uses type `str` instead of an integer or slice.
RUF015.py:21:14: RUF015 Indexed access to type `str` uses type `str` instead of an integer or slice.
RUF015.py:24:14: RUF015 Indexed access to type `bytes` uses type `str` instead of an integer or slice.
RUF015.py:27:17: RUF015 Indexed access to type `list` uses type `str` instead of an integer or slice.
RUF015.py:28:17: RUF015 Indexed access to type `tuple` uses type `str` instead of an integer or slice.
RUF015.py:31:30: RUF015 Indexed access to type `list comprehension` uses type `str` instead of an integer or slice.
RUF015.py:34:13: RUF015 Indexed access to type `str` uses type `tuple` instead of an integer or slice.
RUF015.py:37:14: RUF015 Indexed access to type `list` uses type `str` instead of an integer or slice.
RUF015.py:40:14: RUF015 Indexed access to type `list` uses type `float` instead of an integer or slice.
RUF015.py:43:14: RUF015 Indexed access to type `list` uses type `dict` instead of an integer or slice.
RUF015.py:46:14: RUF015 Indexed access to type `list` uses type `dict comprehension` instead of an integer or slice.
RUF015.py:49:14: RUF015 Indexed access to type `list` uses type `tuple` instead of an integer or slice.
RUF015.py:52:14: RUF015 Indexed access to type `list` uses type `list comprehension` instead of an integer or slice.
RUF015.py:55:14: RUF015 Indexed access to type `list` uses type `set` instead of an integer or slice.
RUF015.py:58:14: RUF015 Indexed access to type `list` uses type `bytes` instead of an integer or slice.
RUF015.py:61:17: RUF015 Slice in indexed access to type `list` uses type `str` as bound instead of an integer.
RUF015.py:62:17: RUF015 Slice in indexed access to type `list` uses type `str` as bound instead of an integer.
RUF015.py:63:17: RUF015 Slice in indexed access to type `list` uses type `float` as bound instead of an integer.
RUF015.py:64:17: RUF015 Slice in indexed access to type `list` uses type `set` as bound instead of an integer.
RUF015.py:67:19: RUF015 Slice in indexed access to type `list` uses type `str` as bound instead of an integer.
RUF015.py:68:19: RUF015 Slice in indexed access to type `list` uses type `str` as bound instead of an integer.
RUF015.py:69:19: RUF015 Slice in indexed access to type `list` uses type `float` as bound instead of an integer.
RUF015.py:70:19: RUF015 Slice in indexed access to type `list` uses type `set` as bound instead of an integer.
RUF015.py:73:17: RUF015 Slice in indexed access to type `list` uses type `str` as bound instead of an integer.
RUF015.py:73:21: RUF015 Slice in indexed access to type `list` uses type `str` as bound instead of an integer.
RUF015.py:76:17: RUF015 Indexed access to type `list` uses type `str` instead of an integer or slice.
```

I've ensured that each of the slice cases raises an error at runtime

```python


Traceback (most recent call last):
  File "/Users/mz/eng/src/charliermarsh/ruff/example.py", line 3, in <module>
    var = [1, 2, 3]["x":2]
          ~~~~~~~~~^^^^^^^
TypeError: slice indices must be integers or None or have an __index__ method

Traceback (most recent call last):
  File "/Users/mz/eng/src/charliermarsh/ruff/example.py", line 4, in <module>
    var = [1, 2, 3][f"x":2]
          ~~~~~~~~~^^^^^^^^
TypeError: slice indices must be integers or None or have an __index__ method

Traceback (most recent call last):
  File "/Users/mz/eng/src/charliermarsh/ruff/example.py", line 5, in <module>
    var = [1, 2, 3][1.2:2]
          ~~~~~~~~~^^^^^^^
TypeError: slice indices must be integers or None or have an __index__ method

Traceback (most recent call last):
  File "/Users/mz/eng/src/charliermarsh/ruff/example.py", line 6, in <module>
    var = [1, 2, 3][{"x"}:2]
          ~~~~~~~~~^^^^^^^^^
TypeError: slice indices must be integers or None or have an __index__ method

Traceback (most recent call last):
  File "/Users/mz/eng/src/charliermarsh/ruff/example.py", line 9, in <module>
    var = [1, 2, 3][0:"x"]
          ~~~~~~~~~^^^^^^^
TypeError: slice indices must be integers or None or have an __index__ method

Traceback (most recent call last):
  File "/Users/mz/eng/src/charliermarsh/ruff/example.py", line 10, in <module>
    var = [1, 2, 3][0:f"x"]
          ~~~~~~~~~^^^^^^^^
TypeError: slice indices must be integers or None or have an __index__ method

Traceback (most recent call last):
  File "/Users/mz/eng/src/charliermarsh/ruff/example.py", line 11, in <module>
    var = [1, 2, 3][0:1.2]
          ~~~~~~~~~^^^^^^^
TypeError: slice indices must be integers or None or have an __index__ method
```

---

_Marked ready for review by @zanieb on 2023-07-10 16:17_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-10 16:38_

I'm not enthused about this pattern but it also doesn't really make sense to implement names for expressions that are not supported by this rule

---

_@zanieb reviewed on 2023-07-10 16:38_

---

_Review requested from @MichaReiser by @zanieb on 2023-07-10 16:44_

---

_Review requested from @charliermarsh by @zanieb on 2023-07-10 16:44_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:147 on 2023-07-10 17:02_

Can this return `&'static str`? Same with `expression_type_name`. The allocation (`.to_string()`) shouldn't be necessary since it's just returning a constant value.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:14 on 2023-07-10 17:03_

We could also mention that this will yield a `SyntaxWarning` at parse-time.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:49 on 2023-07-10 17:05_

One thing you can do here (which is newer so not all rules are written this way):

```rust
pub(crate) fn invalid_index_type<'a>(checker: &mut Checker, expr: &'a ast::ExprSubscript) { ... }
```

That ensures that we only pass in an expression of the correct type, as opposed to passing in two expressions that are _assumed_ to be fields of some specific expression type.

You might have to change `ast/mod.rs` to use:

```rust
Expr::Subscript(subscript @ ast::ExprSubscript {
    value,
    slice,
    ctx,
    range: _,
}) => { ... }
```

---

_@charliermarsh reviewed on 2023-07-10 17:08_

---

_@zanieb reviewed on 2023-07-10 17:10_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:49 on 2023-07-10 17:10_

I like that, also that `var @ match` syntax is useful I think I can use that elsewhere too

---

_@charliermarsh reviewed on 2023-07-10 17:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-10 17:11_

The way I'd think about this is to make this condition unrepresentable by coupling the detection conditions above to the "ability to generate a name for the expression". Will come up with an example, one sec...


---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:147 on 2023-07-10 17:12_

Definitely, but won't I need to allocate `to_string()` when I place it in the `InvalidIndexType` struct?

---

_@zanieb reviewed on 2023-07-10 17:12_

---

_@charliermarsh reviewed on 2023-07-10 17:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-10 17:16_

For example, you could introduce a new type like this:

```rust
#[derive(Debug)]
enum IndexType<'a> {
    Constant(&'a Constant),
    JoinedStr,
    List,
    ListComp,
    DictComp,
    Set,
    Dict,
    Tuple,
}

impl fmt::Display for IndexType {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match self {
            Self::Constant(constant) => f.write_str(&constant_type_name(constant)),
            Self::JoinedStr => f.write_str("str"),
            Self::List => f.write_str("list"),
            Self::ListComp => f.write_str("list comprehension"),
            Self::DictComp => f.write_str("dict comprehension"),
            Self::Set => f.write_str("set"),
            Self::Dict => f.write_str("dict"),
            Self::Tuple => f.write_str("tuple"),
        }
    }
}
```

(Or replace the constant variant with `ConstantStr` and `ConstantBytes`.)

Then, with a method like:

```rust
impl<'a> IndexType {
    fn try_from(expr: &'a Expr) -> Option<Self<'a>> {
        match expr {
            Expr::Constant(ExprConstant { value, .. }) => Some(Self::Constant(value)),
            Expr::JoinedStr(_) => Some(Self::JoinedStr),
            Expr::List(_) => Some(Self::List),
            Expr::ListComp(_) => Some(Self::ListComp),
            Expr::DictComp(_) => Some(Self::DictComp),
            Expr::Set(_) => Some(Self::Set),
            Expr::Dict(_) => Some(Self::Dict),
            Expr::Tuple(_) => Some(Self::Tuple),
            _ => None,
        }
    }
}
```

If you do this, then the first line of this method could be like:

```rust
let Some(value) = IndexType::try_from(value) else {
    return None;
};

// Maybe some other conditions depending on where you are in the code?
if !matches!(value, IndexType::List | IndexType::Dict | ...) {
   return None;
}
```


---

_@charliermarsh reviewed on 2023-07-10 17:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:125 on 2023-07-10 17:16_

Does this intentionally omit `SetComp`?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:147 on 2023-07-10 17:18_

Yes! But it's best to defer that allocation until it's necessary (both in practice and just as a matter of API design), both because it lets you avoid allocations in some cases (maybe the caller doesn't need a `String`), and because you can always convert a `&str` to `String` but not other way around (so returning `&str` is more flexible). In this case, you'd call `.to_string()` on the returned value of this method.

---

_@charliermarsh reviewed on 2023-07-10 17:18_

---

_@charliermarsh reviewed on 2023-07-10 17:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-10 17:19_

(There might be some intricacies in this rule that might this pattern not as trivial as I've painted it above, but I'm more trying to demonstrate how I'd think about the logic and responsibilities. How can we use types to make `.expect` and `.unwrap` calls unnecessary or even impossible?)

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:147 on 2023-07-10 17:21_

👍 thanks!

---

_@zanieb reviewed on 2023-07-10 17:21_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:125 on 2023-07-10 17:25_

Nope! Adding test coverage...

---

_@zanieb reviewed on 2023-07-10 17:25_

---

_@zanieb reviewed on 2023-07-10 17:27_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-10 17:27_

Makes sense, I can definitely do this if it feels worth the time.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-10 17:31_

I think it's worth a try. @MichaReiser may have a different opinion if you want him to weigh in too :)

---

_@charliermarsh reviewed on 2023-07-10 17:31_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:29 on 2023-07-11 13:03_

Nit: I prefer to avoid abbreviations when naming variables. 
```suggestion
    variable_type: String,
    index_type: String,
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:152 on 2023-07-11 13:04_

Should we list the expressions here to force a compile error when a new expression was added.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-11 13:06_

The `IndexType` may also be something that's valuable for other rules and, thus, would make sense to have in our ast crate. But it would then probably make sense that each variant stores the corresponding expression node (similar to `AnyFunctionDefinition`), implements `Ranged`.

---

_@MichaReiser approved on 2023-07-11 13:06_

---

_@zanieb reviewed on 2023-07-11 13:38_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-11 13:38_

@MichaReiser that's not an enumeration of valid index types. That's just an enumeration of types that we can reasonably detect as _invalid_ index types. It's more of a `CheckableLiteralType` which could also be useful elsewhere? 

---

_@zanieb reviewed on 2023-07-11 13:46_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:152 on 2023-07-11 13:46_

I don't think so. Unless new literal types are introduced, a new expression is unlikely to be detectable by this rule as a violation.

---

_@MichaReiser reviewed on 2023-07-11 14:44_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:92 on 2023-07-11 14:44_

Oh whoops. I like charlie's proposal because it's explicit about which types can exist (I don't have to infer it from looking at other functions) but your solution is fine too.

---

_@zanieb reviewed on 2023-07-11 15:55_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:77 on 2023-07-11 15:55_

@charliermarsh I retained this expect because I'd rather not silently return in this case. A developer has broken this rule if the type is not checkable.

---

_Renamed from "Add RUF015: Detection of invalid index types" to "Add RUF016: Detection of invalid index types" by @zanieb on 2023-07-11 17:05_

---

_Comment by @zanieb on 2023-07-11 17:06_

Note I had to rebase this due to conflict with #5549 which took the RUF015 code — some of the commits will not pass tests anymore.

---

_@zanieb reviewed on 2023-07-11 17:07_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs`:81 on 2023-07-11 17:07_

I narrowed this to match the pattern used in `InvalidIndexType`

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:42 on 2023-07-12 03:51_

Nit: destructure `is_slice` above? Since you're already destructuring `value_type` and `index_type`. You may then have to do `if *is_slice`.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:77 on 2023-07-12 03:54_

To me it would be a bit more natural to do this before the `if !matches` (like `let Some(value_type) = ... else { return; };`), then have an `if !matches(value_type, CheckableExprType::List | ...)` to enforce the further narrowing. That way the `expect` is unnecessary as the condition is impossible. But it's not blocking.

---

_@charliermarsh approved on 2023-07-12 03:54_

---

_@charliermarsh approved on 2023-07-12 03:55_

Looks great :)

---

_@zanieb reviewed on 2023-07-12 04:03_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:42 on 2023-07-12 04:03_

Honestly I don't understand what difference the destructing makes at all, why do we do it here?

---

_@charliermarsh reviewed on 2023-07-12 04:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:42 on 2023-07-12 04:05_

The nice thing about the destructuring is that we can inline the variables directly in the `format!` call. This is valid:

```rust
let MultiValueRepeatedKeyLiteral { name } = self;
format!("Dictionary key literal `{name}` repeated")
```

However, this is not:

```rust
format!("Dictionary key literal `{self.name}` repeated")
```

Instead, you'd have to do:

```rust
format!("Dictionary key literal `{}` repeated", self.name)
```

That's the main benefit of destructuring in these violation implementations -- that we can inline values in the `format!` calls.

And then, if we're going to destructure _some_ members, IMO it's preferable to be consistent and destructure everything rather than mixing and matching.

---

_@zanieb reviewed on 2023-07-12 04:07_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:77 on 2023-07-12 04:07_

I did this before then moved it back to an `expect`, I feel like it confuses the logic of the rule. Like...

If the value is a list, byte, string, or tuple then we want to enforce this rule.

To display a violation, we happen to use `CheckableExprType` to generate a type name for the value. However, this is just for display purposes not a limitation of the rule itself. 

If someone were to extend the rule to apply to a new type, e.g. byte arrays, I would not expect this function to exit early because there is not an implementation for displaying the type name. In this case, the `expect` would point a developer to the next step in their implementation.

🤷‍♀️ it does feel like a subtle stylistic choice in the end — maybe you shouldn't have sent me that BurntSushi article 😝

I'm kind of curious to see what lessons I learn from making my own stylistic choices but I'm happy to follow precedent for the project too!

---

_@charliermarsh reviewed on 2023-07-12 04:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:77 on 2023-07-12 04:14_

This makes sense. I'd say run with what you have here! I tend to be pretty defensive around `expect` and `unwrap` (which are ~equivalent) because they're not recoverable in any way, so e.g. if we shipped a release that had an oversight here, Ruff would bail entirely on files that hit this codepath.

The "right" answer here may be to extend `CheckableExprType` to instead implement all expression types (so it doesn't return `Option`), or perhaps to use a debug assertion so that we see this failure in debug builds but fail silently in releases. But it's also ok for now, we know that it's a superset of expressions on the `match` anyway. (And we're obviously overdoing it on this one point just for educational / discussion purposes, it's not a sticking point in practice :))

---

_@zanieb reviewed on 2023-07-12 04:18_

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:77 on 2023-07-12 04:18_

> or perhaps to use a debug assertion so that we see this failure in debug builds but fail silently in releases.

I'm pretty happy with that as an approach.

Although... I'd also _want_ a user to report a failure here because it's a bug in Ruff. I guess it's a matter of if we want to panic and make the rule entirely unusable for their code or fail silently and _hope_ they notice that the rule isn't raising a violation. I can understand preferring the second as a safer user experience :)

---

_Review comment by @zanieb on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:77 on 2023-07-12 04:25_

I wanted `debug_panic!(...)` but I guess `debug_assert!(false, ...)` will do https://github.com/astral-sh/ruff/pull/5602/commits/e058e240574053c900129bcfae2c18016ab61563

---

_@zanieb reviewed on 2023-07-12 04:25_

---

_@charliermarsh reviewed on 2023-07-12 04:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/invalid_index_type.rs`:77 on 2023-07-12 04:25_

I'm happy with whatever! If it were me on a desert island, I _guess_ I'd write something like `let Some(value_type) = CheckableExprType::try_from(value) else { return; };` but I'm not convinced that's the best option. This looks good to me.

(Again just to complete the picture, I _think_ if we hit a panic here, the behavior the user would see is that we'd throw up a message asking them to file an issue and wouldn't print any violations for the failing file, but would print out violations for all other files as usual.)

---

_Merged by @zanieb on 2023-07-12 05:23_

---

_Closed by @zanieb on 2023-07-12 05:23_

---
