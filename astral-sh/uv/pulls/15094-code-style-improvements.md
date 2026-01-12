```yaml
number: 15094
title: Code style improvements
type: pull_request
state: open
author: adamnemecek
labels:
  - internal
assignees: []
base: main
head: uv4
created_at: 2025-08-05T21:05:29Z
updated_at: 2025-08-12T17:29:43Z
url: https://github.com/astral-sh/uv/pull/15094
synced_at: 2026-01-12T16:11:34Z
```

# Code style improvements

---

_@adamnemecek_

Continuation of #15074.

---

_@zanieb reviewed on 2025-08-05 21:34_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_cli_reference.rs`:135 on 2025-08-05 21:34_

I actually don't think this reads clearer?

---

_@zanieb reviewed on 2025-08-05 21:36_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index_url.rs`:85 on 2025-08-05 21:36_

I think just `inner` would be fine here too.

---

_@adamnemecek reviewed on 2025-08-05 21:39_

---

_Review comment by @adamnemecek on `crates/uv-distribution-types/src/index_url.rs`:85 on 2025-08-05 21:39_

Fixed.

---

_@adamnemecek reviewed on 2025-08-05 21:39_

---

_Review comment by @adamnemecek on `crates/uv-dev/src/generate_cli_reference.rs`:135 on 2025-08-05 21:39_

Fixed.

---

_Renamed from "additional refactoring" to "Code style improvements" by @konstin on 2025-08-06 11:21_

---

_Label `internal` added by @konstin on 2025-08-06 11:21_

---

_Comment by @adamnemecek on 2025-08-12 16:37_

@charliermarsh @zanieb 
Can you please merge this before it gets stale?

Thanks

---

_Comment by @zanieb on 2025-08-12 17:19_

I'm more on the fence about these changes than the other ones you put up. Like, I feel like the changes to `crates/uv-resolver/src/lock/mod.rs` and `crates/uv-distribution-types/src/index_url.rs` make a lot of sense but I'm less sure about all the `max` changes and `vec![]` over `Vec::default` etc. Bundling all these changes together makes it harder to discuss, and the lack of any commentary about the motivation for the changes isn't helping. I think if you want to make a stylistic change, e.g., `vec![]` vs `Vec::default`, each style difference should be a single pull request so we can discuss whether we want to make that change.

---

_Comment by @adamnemecek on 2025-08-12 17:22_

I can never the vec stuff. I just have not seen it much,  `Vec::new()` and `vec![]` seem to be the norm. Also `vec![]` is already used all over the place.

---

_Comment by @zanieb on 2025-08-12 17:29_

The vec stuff is just an example. There are like 4+ categories of stylistic changes here. If we're mixing multiple styles and want to consolidate to one, we should have a way to enforce the style.

---
