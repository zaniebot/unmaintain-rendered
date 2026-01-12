```yaml
number: 16294
title: bump MSRV to 1.83
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/bump-msrv
created_at: 2025-02-21T02:06:55Z
updated_at: 2025-02-26T14:12:45Z
url: https://github.com/astral-sh/ruff/pull/16294
synced_at: 2026-01-12T15:55:54Z
```

# bump MSRV to 1.83

---

_@carljm_

According to our new MSRV policy (see https://github.com/astral-sh/ruff/issues/16370 ), bump our MSRV to 1.83 (N - 2), and autofix some new clippy lints.

---

_Review requested from @MichaReiser by @carljm on 2025-02-21 02:06_

---

_Review requested from @AlexWaygood by @carljm on 2025-02-21 02:06_

---

_Review requested from @sharkdp by @carljm on 2025-02-21 02:06_

---

_Comment by @github-actions[bot] on 2025-02-21 02:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `breaking` added by @MichaReiser on 2025-02-21 07:05_

---

_Label `breaking` removed by @MichaReiser on 2025-02-21 07:05_

---

_@MichaReiser approved on 2025-02-21 07:07_

This is technically a breaking change for anyone building Ruff from source because they now need a newer Rust version to do so. 

I have a slight preference to keep the current MSRV unless there's a lot of boilerplate involved.

The other thing we have to consider when bumping the MSRV is whether downstream packaging tools have updated to this Rust version (e.g. homebrew). This should be fine, considering that uv already uses 1.83

---

_Comment by @carljm on 2025-02-21 07:32_

Ok I'll try the boilerplate tomorrow. Just annoying writing uglier code that shouldn't need to be that way to accommodate older rust versions :/ doesn't seem like it should be hard for anyone to use 1.83 instead of 1.80, and I'm sure we'll bump it up sometime...

---

_Comment by @MichaReiser on 2025-02-21 08:39_

I'd say it depends on how much more boilerplate it requires. If it's something you can do in 15min, let's keep the MSRV as is. If not, bump it (although a lower MSRV might also be important for other projects using salsa)

---

_Comment by @carljm on 2025-02-22 01:34_

> If it's something you can do in 15min

I wasn't able to find something that compiles within 15min, but maybe you know how to do it.

The problem is at https://github.com/salsa-rs/salsa/blob/1ff42613cbaa699567009895a6d00b87bf7772e6/src/cycle.rs#L97 -- hashset iterators only starting implementing `Default` in Rust 1.83.

In the owning `IntoIterator` impl above I can just replace `.unwrap_or_default()` with `.unwrap_or_else(|| FxHashSet::default().into_iter())`. But in the borrowed version I can't figure out what I can put in the `.unwrap_or_else()` without getting errors about borrowing from a local, or returning the wrong type. Even if I remove the Copied and let it be just an iterator over `&DatabaseKeyIndex`, I still get errors about borrowing from a local if I try to use `FxHashSet::default().iter()`.

---

_Comment by @MichaReiser on 2025-02-22 08:21_

Yeah, I think you'd have to write your own iterator. Which is definitely less nice but it's not a show-stopper:

```rust
impl std::iter::IntoIterator for CycleHeads {
    type Item = DatabaseKeyIndex;
    type IntoIter = std::collections::hash_set::IntoIter<Self::Item>;

    fn into_iter(self) -> Self::IntoIter {
        self.0
            .map(|heads| *heads)
            .unwrap_or_else(|| FxHashSet::default())
            .into_iter()
    }
}

pub struct CycleHeadsIter<'a>(Option<std::collections::hash_set::Iter<'a, DatabaseKeyIndex>>);

impl<'a> Iterator for CycleHeadsIter<'a> {
    type Item = DatabaseKeyIndex;

    fn next(&mut self) -> Option<Self::Item> {
        self.0.as_mut()?.next().copied()
    }

    fn last(self) -> Option<Self::Item> {
        self.0?.last().copied()
    }
}

impl FusedIterator for CycleHeadsIter<'_> {}

impl<'a> std::iter::IntoIterator for &'a CycleHeads {
    type Item = DatabaseKeyIndex;
    type IntoIter = CycleHeadsIter<'a>;

    fn into_iter(self) -> Self::IntoIter {
        CycleHeadsIter(self.0.as_ref().map(|heads| heads.iter()))
    }
}
```

---

_Comment by @carljm on 2025-02-22 18:27_

Thanks! Added this to the Salsa PR to maintain 1.80 compat, will close this for now.

---

_Closed by @carljm on 2025-02-22 18:27_

---

_Reopened by @carljm on 2025-02-25 20:58_

---

_Review requested from @dhruvmanila by @carljm on 2025-02-25 20:59_

---

_@MichaReiser approved on 2025-02-26 07:31_

---

_Label `internal` added by @MichaReiser on 2025-02-26 07:31_

---

_Merged by @carljm on 2025-02-26 14:12_

---

_Closed by @carljm on 2025-02-26 14:12_

---

_Branch deleted on 2025-02-26 14:12_

---
