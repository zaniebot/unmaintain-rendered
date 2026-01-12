```yaml
number: 17632
title: "ruff_db: add tests for annotations with no ranges"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/fix-only-file-path-rendering
created_at: 2025-04-25T17:07:50Z
updated_at: 2025-04-25T17:25:22Z
url: https://github.com/astral-sh/ruff/pull/17632
synced_at: 2026-01-12T15:56:02Z
```

# ruff_db: add tests for annotations with no ranges

---

_@BurntSushi_

... and fix the case where an annotation with a `Span` but no
`TextRange` or message gets completely dropped.

---

_Review requested from @carljm by @BurntSushi on 2025-04-25 17:07_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-25 17:07_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-25 17:07_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-25 17:07_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-25 17:07_

---

_Review request for @dcreager removed by @BurntSushi on 2025-04-25 17:08_

---

_Review request for @carljm removed by @BurntSushi on 2025-04-25 17:08_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-04-25 17:08_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-04-25 17:08_

---

_Label `internal` added by @BurntSushi on 2025-04-25 17:08_

---

_Label `red-knot` added by @BurntSushi on 2025-04-25 17:08_

---

_Label `diagnostics` added by @BurntSushi on 2025-04-25 17:08_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:839 on 2025-04-25 17:19_

I'm torn on this. Mainly because it will break https://github.com/astral-sh/ruff/pull/17631 I know, I'm selfish  😅

The reason I want to include a `File` in that message is so that the diagnostic sorting keeps working. But I don't think I want a code frame. 

I think I'd be fine if we don't render primary annotations without a span or message. But not sure. I'm also okay removing the `file` in my other PR.

---

_@MichaReiser approved on 2025-04-25 17:19_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:839 on 2025-04-25 17:23_

You know what. Let me change my PR. The sorting doesn't matter in that case because there won't be any other diagnostics for the same file. 

---

_@MichaReiser reviewed on 2025-04-25 17:23_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:839 on 2025-04-25 17:24_

I lean towards going with this PR and removing the `file` in your other PR. I realize the sorting may not be ideal, but I think it's worse to silently drop some part of the diagnostic from the rendered output.

An alternative for your conundrum is to augment the diagnostic model to support attaching files (or even `Span`s) that aren't annotations/code-frames. But I haven't fully thought that through.

---

_@BurntSushi reviewed on 2025-04-25 17:24_

---

_Merged by @BurntSushi on 2025-04-25 17:25_

---

_Closed by @BurntSushi on 2025-04-25 17:25_

---

_Branch deleted on 2025-04-25 17:25_

---
