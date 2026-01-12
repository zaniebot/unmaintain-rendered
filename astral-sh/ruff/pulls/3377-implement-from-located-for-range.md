```yaml
number: 3377
title: "Implement `From<Located>` for `Range`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/into
created_at: 2023-03-07T00:46:33Z
updated_at: 2023-03-08T18:50:22Z
url: https://github.com/astral-sh/ruff/pull/3377
synced_at: 2026-01-12T04:39:44Z
```

# Implement `From<Located>` for `Range`

---

_Pull request opened by @charliermarsh on 2023-03-07 00:46_

## Summary

Rather than using a custom `Range::from_located`, we can just implement `From` and use a more idiomatic conversion.

No functional changes, purely a refactor.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-07 00:46_

---

_Review requested from @konstin by @charliermarsh on 2023-03-07 00:46_

---

_Review comment by @MichaReiser on `crates/ruff/src/ast/helpers.rs`:943 on 2023-03-07 09:30_

Nit: Writing `into` everywhere is a bit cumbersome. You can improve the ergonomics by changing `locator.slice` to 

```rust
    pub fn slice<R: Into<Range>>(&self, range: R) -> &'a str {
        let index = self.get_or_init_index();
		let range = range.into();
        let start = truncate(range.location, index, self.contents);
        let end = truncate(range.end_location, index, self.contents);
        &self.contents[start..end]
    }
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:227 on 2023-03-07 09:33_

Nit: I prefer to use `Range::from(stmt)` over `stmt.into()` because the former is explicit about the conversation (I need to use IDE features to know what conversation `stmt.into()` performs).

---

_@MichaReiser approved on 2023-03-07 09:33_

---

_@konstin approved on 2023-03-07 10:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/ast/helpers.rs`:943 on 2023-03-07 15:20_

Oh that's nice!

---

_@charliermarsh reviewed on 2023-03-07 15:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/ast/helpers.rs`:943 on 2023-03-08 00:33_

What's your opinion on this vs. requiring callers to call `Range::from`, as below?

---

_@charliermarsh reviewed on 2023-03-08 00:33_

---

_@MichaReiser reviewed on 2023-03-08 08:32_

---

_Review comment by @MichaReiser on `crates/ruff/src/ast/helpers.rs`:943 on 2023-03-08 08:32_

Ha! You caught me.

I don't have a very consistent explanation for it. My thinking is that using the `Into<Range>` "overload" of `slice` pushes the logic and complexity into the `slice` function. The reader of the caller doesn't need to be concerned with the conversation happening. The API of `slice` guarantees that the conversation is OK for all types implementing `Into<Range>`. For as far as readers are concerned, `locator.slice(stmt)` passes a range to `slice`. 

---

_Merged by @charliermarsh on 2023-03-08 18:50_

---

_Closed by @charliermarsh on 2023-03-08 18:50_

---

_Branch deleted on 2023-03-08 18:50_

---
