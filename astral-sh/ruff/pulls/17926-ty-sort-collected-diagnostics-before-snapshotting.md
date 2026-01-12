```yaml
number: 17926
title: "[ty] Sort collected diagnostics before snapshotting them in mdtest"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/mdtest-diagnostics-sorted
created_at: 2025-05-07T17:05:37Z
updated_at: 2025-05-07T17:29:55Z
url: https://github.com/astral-sh/ruff/pull/17926
synced_at: 2026-01-12T15:56:08Z
```

# [ty] Sort collected diagnostics before snapshotting them in mdtest

---

_@AlexWaygood_

## Summary

ty's diagnostics are always sorted into a stable order when ty is executed on the command line. However, this sorting is not applied when the diagnostics are collected by mdtests, which can result in them being snapshotted in our tests in a different order than they would appear to users. That's quite confusing.

This PR fixes that by sorting the diagnostics collected by mdtest in the same way that we sort them when `ty` is actually executed on the command line.

## Test Plan

- `cargo test -p ty_python_semantic`
- `cargo test -p ty_project`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-07 17:05_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-07 17:05_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-07 17:05_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-07 17:05_

---

_Label `ty` added by @AlexWaygood on 2025-05-07 17:05_

---

_Comment by @dhruvmanila on 2025-05-07 17:07_

Thank you! I noticed this a couple of times and I was going to open an issue for it.

---

_Review requested from @BurntSushi by @dhruvmanila on 2025-05-07 17:07_

---

_Comment by @github-actions[bot] on 2025-05-07 17:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:246 on 2025-05-07 17:09_

Nit: I sort of like how `TextRange` solved this. It has a `ordering` method. It could help reduce some complexity (no need for an extra struct, less lifetimes)

```rust
pub fn ordering(&self, db: &dyn Db, other: &Self) -> Ordering {
	if let (Some(span1), Some(span2)) = (
      self.primary_span(),
      other.primary_span(),
  ) {
      let order = span1
          .file()
          .path(self.db)
          .as_str()
          .cmp(span2.file().path(self.db).as_str());
      if order.is_ne() {
          return order;
      }

      if let (Some(range1), Some(range2)) = (span1.range(), span2.range()) {
          let order = range1.start().cmp(&range2.start());
          if order.is_ne() {
              return order;
          }
      }
  }
  // Reverse so that, e.g., Fatal sorts before Info.
  let order = self
      .severity()
      .cmp(&other.severity())
      .reverse();
  if order.is_ne() {
      return order;
  }
  self.id().cmp(&other.id())
}
```


---

_@MichaReiser approved on 2025-05-07 17:10_

Nice, thank you

---

_@AlexWaygood reviewed on 2025-05-07 17:23_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/diagnostic/mod.rs`:246 on 2025-05-07 17:23_

I realised I could simplify it to a single lifetime (pushed the change). I weakly prefer encapsulating the sorting logic into a struct that represents the key (the way I have it now), but no strong opinion; happy to revisit this in a followup if others agree!

---

_Merged by @AlexWaygood on 2025-05-07 17:23_

---

_Closed by @AlexWaygood on 2025-05-07 17:23_

---

_Branch deleted on 2025-05-07 17:23_

---

_@BurntSushi reviewed on 2025-05-07 17:29_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:246 on 2025-05-07 17:29_

I don't feel too strongly personally. I'm fine with either approach given that these aren't semver boundaries.

---
