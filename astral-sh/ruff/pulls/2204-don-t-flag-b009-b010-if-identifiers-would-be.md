```yaml
number: 2204
title: "Don't flag B009/B010 if identifiers would be mangled"
type: pull_request
state: merged
author: sciyoshi
labels: []
assignees: []
merged: true
base: main
head: no-autofix-mangled-idents
created_at: 2023-01-26T17:24:57Z
updated_at: 2023-01-26T17:57:27Z
url: https://github.com/astral-sh/ruff/pull/2204
synced_at: 2026-01-12T15:55:07Z
```

# Don't flag B009/B010 if identifiers would be mangled

---

_@sciyoshi_

Fixes #2202

---

_Review comment by @charliermarsh on `src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:24 on 2023-01-26 17:29_

Do you mind just moving to `src/python/identifiers.rs`, and adding this docstring (I just write it but hadn't gotten much further :))

```rust
/// Returns `true` if a string is a private identifier, such that, when the
/// identifier is defined in a class definition, it will be mangled prior to
/// code generation.
///
/// See: <https://docs.python.org/3.5/reference/expressions.html?highlight=mangling#index-5>.
```

---

_@charliermarsh reviewed on 2023-01-26 17:29_

---

_Review comment by @charliermarsh on `src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:57 on 2023-01-26 17:30_

I think we should put this just after line 43, and avoid raising this error. What do you think?

(We need to make the same change in `setattr_with_constant.rs` too.)

---

_@charliermarsh reviewed on 2023-01-26 17:30_

---

_@sciyoshi reviewed on 2023-01-26 17:32_

---

_Review comment by @sciyoshi on `src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:57 on 2023-01-26 17:32_

Made the change for setattr, although I think it makes sense to still raise the error - this matches the upstream check's behavior.

---

_@charliermarsh reviewed on 2023-01-26 17:33_

---

_Review comment by @charliermarsh on `src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:57 on 2023-01-26 17:33_

Is it intentional, though, that bugbear raises on these cases?

---

_@sciyoshi reviewed on 2023-01-26 17:40_

---

_Review comment by @sciyoshi on `src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:57 on 2023-01-26 17:40_

I don't see anything in bugbear's issues or code to indicate it's intentional - I can make that change if you'd prefer that.

---

_@charliermarsh reviewed on 2023-01-26 17:43_

---

_Review comment by @charliermarsh on `src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:57 on 2023-01-26 17:43_

I think I'd prefer it, if you don't mind. It just strikes me as very unlikely to be a false negative, and as-is, it'll probably cause confusion.

---

_Renamed from "Don't autofix B009/B010 if identifiers would be mangled" to "Don't flag B009/B010 if identifiers would be mangled" by @sciyoshi on 2023-01-26 17:49_

---

_@sciyoshi reviewed on 2023-01-26 17:50_

---

_Review comment by @sciyoshi on `src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:57 on 2023-01-26 17:50_

Sounds good! Updated with the change.

---

_Merged by @charliermarsh on 2023-01-26 17:56_

---

_Closed by @charliermarsh on 2023-01-26 17:56_

---

_Comment by @charliermarsh on 2023-01-26 17:56_

Awesome, thank you so much!

---

_Branch deleted on 2023-01-26 17:57_

---
