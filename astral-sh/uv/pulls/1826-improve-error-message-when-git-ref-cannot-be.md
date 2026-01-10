```yaml
number: 1826
title: Improve error message when git ref cannot be fetched
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/git-err
created_at: 2024-02-21T19:49:13Z
updated_at: 2024-02-22T01:22:01Z
url: https://github.com/astral-sh/uv/pull/1826
synced_at: 2026-01-10T14:54:43Z
```

# Improve error message when git ref cannot be fetched

---

_Pull request opened by @zanieb on 2024-02-21 19:49_

Follow-up to #1781 improving the error message when a ref cannot be fetched

---

_Label `error messages` added by @zanieb on 2024-02-21 20:08_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-21 20:39_

---

_Review requested from @snowsignal by @zanieb on 2024-02-21 20:39_

---

_Review comment by @snowsignal on `crates/uv/tests/common/mod.rs`:152 on 2024-02-21 22:39_

We could simplify this function to be:
```rust
    pub fn filters(&self) -> Vec<(&str, &str)> {
        // Put test context snapshots before the default filters
        // This ensures we don't replace other patterns inside paths from the test context first
        self.filters
            .iter()
            .chain(INSTA_FILTERS.into_iter())
            .copied()
            .collect()
    }
```
which lets us avoid the repeated `insert` calls.

---

_@snowsignal approved on 2024-02-21 22:50_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:152 on 2024-02-21 22:54_

That fails

```
error[E0271]: type mismatch resolving `<Iter<'_, (&str, &str)> as IntoIterator>::Item == &(String, String)`
   --> crates/uv/tests/common/mod.rs:157:20
    |
157 |             .chain(INSTA_FILTERS.into_iter())
    |              ----- ^^^^^^^^^^^^^^^^^^^^^^^^^ expected `&(String, ...)`, found `&(&str, &str)`
    |              |
    |              required by a bound introduced by this call
    |
    = note: expected reference `&(std::string::String, std::string::String)`
               found reference `&(&str, &str)`
note: the method call chain might not have had the expected associated types
   --> crates/uv/tests/common/mod.rs:157:34
    |
157 |             .chain(INSTA_FILTERS.into_iter())
    |                    ------------- ^^^^^^^^^^^ `IntoIterator::Item` is `&(&str, &str)` here
    |                    |
    |                    this expression has type `&[(&str, &str)]`
note: required by a bound in `std::iter::Iterator::chain`
   --> /Users/mz/.rustup/toolchains/1.76-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/iter/traits/iterator.rs:524:25
    |
521 |     fn chain<U>(self, other: U) -> Chain<Self, U::IntoIter>
    |        ----- required by a bound in this associated function
...
524 |         U: IntoIterator<Item = Self::Item>,
    |                         ^^^^^^^^^^^^^^^^^ required by this bound in `Iterator::chain`

error[E0599]: the method `copied` exists for struct `Chain<Iter<'_, (String, ...)>, ...>`, but its trait bounds were not satisfied
   --> crates/uv/tests/common/mod.rs:158:14
    |
155 | /         self.filters
156 | |             .iter()
157 | |             .chain(INSTA_FILTERS.into_iter())
158 | |             .copied()
    | |             -^^^^^^ method cannot be called on `Chain<Iter<'_, (String, ...)>, ...>` due to unsatisfied trait bounds
    | |_____________|
    | 
 ```

---

_@zanieb reviewed on 2024-02-21 22:54_

---

_@snowsignal reviewed on 2024-02-21 23:39_

---

_Review comment by @snowsignal on `crates/uv/tests/common/mod.rs`:152 on 2024-02-21 23:39_

Okay, what about this?
```rust
    pub fn filters(&self) -> Vec<(&str, &str)> {
        // Put test context snapshots before the default filters
        // This ensures we don't replace other patterns inside paths from the test context first
        self.filters
            .iter()
            .map(|p, r| (p.as_str(), r.as_str()))
            .chain(INSTA_FILTERS.into_iter().copied())
            .collect()
    }
```

---

_@zanieb reviewed on 2024-02-21 23:46_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:152 on 2024-02-21 23:46_

I'm not sure it's any easier to read but it does work!

---

_@snowsignal reviewed on 2024-02-22 00:01_

---

_Review comment by @snowsignal on `crates/uv/tests/common/mod.rs`:152 on 2024-02-22 00:01_

It's easier to read for me, but maybe I'm just accustomed to using iterators for everything 😅 

If you want an approach that avoids iterator chaining, what about this?
```rust
    pub fn filters(&self) -> Vec<(&str, &str)> {
        let mut filters: Vec<(&str, &str)> = vec![];
        // Put test context snapshots before the default filters
        // This ensures we don't replace other patterns inside paths from the test context first
        for (pattern, replacement) in &self.filters {
            filters.push((pattern, replacement));
        }
        filters.extend(INSTA_FILTERS.iter());
        filters
    }
```

---

_Merged by @zanieb on 2024-02-22 01:22_

---

_Closed by @zanieb on 2024-02-22 01:22_

---

_Branch deleted on 2024-02-22 01:22_

---
