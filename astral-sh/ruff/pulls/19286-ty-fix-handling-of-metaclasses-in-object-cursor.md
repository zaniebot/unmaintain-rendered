```yaml
number: 19286
title: "[ty] Fix handling of metaclasses in `object.<CURSOR>` completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/object-attr-uses-type-member
created_at: 2025-07-11T14:47:30Z
updated_at: 2025-07-14T12:24:26Z
url: https://github.com/astral-sh/ruff/pull/19286
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Fix handling of metaclasses in `object.<CURSOR>` completions

---

_Pull request opened by @BurntSushi on 2025-07-11 14:47_

Basically, we weren't quite using `Type::member` in every case
correctly. Specifically, this example from @sharkdp:

```
class Meta(type):
    @property
    def meta_attr(self) -> int:
        return 0

class C(metaclass=Meta): ...

C.<CURSOR>
```

While we would return `C.meta_attr` here, we were claiming its type was
`property`. But its type should be `int`.

Ref https://github.com/astral-sh/ruff/pull/19216#discussion_r2197065241


---

_Review requested from @carljm by @BurntSushi on 2025-07-11 14:47_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-07-11 14:47_

---

_Review requested from @sharkdp by @BurntSushi on 2025-07-11 14:47_

---

_Review requested from @dcreager by @BurntSushi on 2025-07-11 14:47_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-07-11 14:47_

---

_Review request for @dcreager removed by @BurntSushi on 2025-07-11 14:47_

---

_Review request for @carljm removed by @BurntSushi on 2025-07-11 14:47_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-07-11 14:47_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-07-11 14:47_

---

_Comment by @github-actions[bot] on 2025-07-11 14:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood reviewed on 2025-07-11 14:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:107 on 2025-07-11 14:54_

not totally sold on the rename for this method 😜

---

_@BurntSushi reviewed on 2025-07-11 14:56_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/ide_support.rs`:107 on 2025-07-11 14:56_

Haha derp. Thanks.

---

_Comment by @BurntSushi on 2025-07-11 15:02_

Interesting that the snapshot in CI is different from what I get locally (the test passes locally): https://github.com/astral-sh/ruff/actions/runs/16223077215/job/45808301696?pr=19286#step:8:4810

---

_Comment by @AlexWaygood on 2025-07-11 15:10_

> Interesting that the snapshot in CI is different from what I get locally (the test passes locally): [astral-sh/ruff/actions/runs/16223077215/job/45808301696?pr=19286#step:8:4810](https://github.com/astral-sh/ruff/actions/runs/16223077215/job/45808301696?pr=19286#step:8:4810)

it looks like it may be a difference between release builds and debug builds? It looks like it's passing the debug-build CI job but not the release-build CI job

---

_Label `ty` added by @dylwil3 on 2025-07-11 15:17_

---

_Label `server` added by @AlexWaygood on 2025-07-11 15:18_

---

_Comment by @BurntSushi on 2025-07-11 15:22_

Nice yeah that was it. I just ended up redacting them here.

---

_@sharkdp approved on 2025-07-14 07:19_

Thank you!

---

_Merged by @BurntSushi on 2025-07-14 12:24_

---

_Closed by @BurntSushi on 2025-07-14 12:24_

---

_Branch deleted on 2025-07-14 12:24_

---
