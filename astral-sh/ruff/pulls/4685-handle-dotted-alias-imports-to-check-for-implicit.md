```yaml
number: 4685
title: Handle dotted alias imports to check for implicit imports
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/tch002
created_at: 2023-05-27T19:27:45Z
updated_at: 2023-05-28T08:50:46Z
url: https://github.com/astral-sh/ruff/pull/4685
synced_at: 2026-01-12T15:55:16Z
```

# Handle dotted alias imports to check for implicit imports

---

_@dhruvmanila_

## Summary

Dotted alias import such as `import foo.bar as f` are presented as `Importation` instead of `SubmoduleImportation`. Thus, they were compared like `foo == foo.bar` which would conclude that it's not an implicit import (which is incorrect).

The implemented solution differentiates between a dotted alias import and a plain import, takes care of the former by zipping over every module part. The `starts_with` method cannot be used as it could yield false positive (`foo` vs `foobar`, substrings aren't the same).

### Alternative

We could implement a new binding kind for this `SubmoduleAliasImportation`:

```rust
pub struct SubmoduleAliasImportation<'a> {
	pub name: &'a str,
	pub full_name: &'a str,
	pub alias: &'a str,
}
```

Taking the below import as an example, the `name`, `full_name` and `alias` fields would be `foo`, `foo.bar` and `b` respectively.

```python
import foo.bar as b
```

## Analysis

So, the problem here is that a dotted alias import is represented using `Binding::Importation`:

```python
import libcst.matchers as m
#      |-------------|    |
#        full_name       name
```

This means that the comparison is done with `libcst` and `libcst.matchers` which are not the same, thus the former is not considered to be an implicit import of the latter even though it is.

The problem arises with multiple dotted alias import as well:

```python
import libcst.matchers as m  # this
import libcst.matches._visitors as v  # that
```

Here, the same thing happens where `this` is not considered an implicit import of `that`.

Another case:
```python
import libcst
import libcst.matches._visitors as v
```

## Test Plan

Test cases as provided in the Python files.

fixes: #4667


---

_Renamed from "Handle dotted alias to check for implicit imports" to "Handle dotted alias imports to check for implicit imports" by @dhruvmanila on 2023-05-27 19:32_

---

_Comment by @github-actions[bot] on 2023-05-27 19:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.05ms     2.7 MB/sec    1.01     15.2±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.6 MB/sec    1.00      3.7±0.01ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    376.3±1.96µs     7.8 MB/sec    1.01    378.7±0.73µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.1 MB/sec    1.01      6.3±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.4 MB/sec    1.00      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1603.7±2.78µs    10.4 MB/sec    1.00   1604.7±4.38µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.4±2.37µs    16.7 MB/sec    1.01    178.2±1.35µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.01      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.7±0.00ms     7.1 MB/sec    1.00      5.7±0.00ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1125.9±1.57µs    14.8 MB/sec    1.00   1128.5±1.07µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    115.1±1.77µs    25.6 MB/sec    1.00    114.9±0.62µs    25.7 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.4 MB/sec    1.01      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.23ms     2.4 MB/sec    1.01     17.2±0.24ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.05ms     3.9 MB/sec    1.01      4.3±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    507.2±6.84µs     5.8 MB/sec    1.00    503.4±7.97µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.11ms     3.6 MB/sec    1.00      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.09ms     4.9 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1786.5±22.33µs     9.3 MB/sec    1.00  1767.3±24.54µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.7±3.77µs    14.3 MB/sec    1.01    208.7±8.43µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.5±0.05ms     6.2 MB/sec    1.00      6.5±0.08ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01  1240.2±14.13µs    13.4 MB/sec    1.00  1225.3±14.06µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    126.3±2.05µs    23.4 MB/sec    1.00    125.7±1.79µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.1 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:194 on 2023-05-27 19:47_

Could we _just_ use this condition, since it will yield true when `this_name == that_name`?

---

_@charliermarsh reviewed on 2023-05-27 19:47_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:194 on 2023-05-28 02:47_

Not really. `foo` is not the same as `foo.bar` but it is an implicit import. The `zip` will stop when it can't move further making it an ideal choice of comparison.

(Unless you mean something else?)

---

_@dhruvmanila reviewed on 2023-05-28 02:47_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:194 on 2023-05-28 02:57_

Right, but why do we need the `if that_name == that_alias { this_name == that_name }` portion? The tests seem to pass without it.

---

_@charliermarsh reviewed on 2023-05-28 02:57_

---

_@charliermarsh reviewed on 2023-05-28 02:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:194 on 2023-05-28 02:57_

In other words: is this alone not sufficiently general to handle all cases?

```rust
BindingKind::Importation(Importation {
    full_name: that_name,
    ..
}) => {
    // Submodule importation with an alias: `import pkg.A as B`
    this_name
        .split('.')
        .zip(that_name.split('.'))
        .all(|(this_part, that_part)| this_part == that_part)
}
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:194 on 2023-05-28 02:58_

Ah yes. That will work, I just thought to split it out to avoid doing this computation and take the fast path directly.

---

_@dhruvmanila reviewed on 2023-05-28 02:58_

---

_Comment by @dhruvmanila on 2023-05-28 03:14_

Oh, I actually found one more case where this doesn't work (here both are dotted alias imports):

```python
import pkg.bar as B
import pkg.foo as F


def test(value: F.Foo):
    return B.Bar()
```

---

_Comment by @charliermarsh on 2023-05-28 03:16_

Is it sufficient to just compare up to the first dot? Like:

```rust
this_name.split('.').first() == that_name.split('.').first()
```

Or comparable. When would this fail?

---

_Comment by @dhruvmanila on 2023-05-28 03:22_

Yes, I'm thinking of the same thing. I don't see any edge cases as if the root module is same, then it's an implicit import.

(It turns out `Split` doesn't have a `first` method)

---

_Comment by @charliermarsh on 2023-05-28 03:23_

Ok cool -- yeah, I just prefer one check here if we can find a sufficiently general rule :)

---

_Comment by @charliermarsh on 2023-05-28 03:24_

(You can probably do it with `.find('.')` or something.)

---

_Merged by @charliermarsh on 2023-05-28 03:58_

---

_Closed by @charliermarsh on 2023-05-28 03:58_

---

_Comment by @charliermarsh on 2023-05-28 03:58_

Thanks!

---

_Label `bug` added by @charliermarsh on 2023-05-28 03:58_

---

_Branch deleted on 2023-05-28 08:50_

---
