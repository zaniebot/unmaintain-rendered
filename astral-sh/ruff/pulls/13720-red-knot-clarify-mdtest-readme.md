```yaml
number: 13720
title: "[red-knot] clarify mdtest README"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/mdtest-readme
created_at: 2024-10-11T18:53:29Z
updated_at: 2024-10-11T19:36:50Z
url: https://github.com/astral-sh/ruff/pull/13720
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] clarify mdtest README

---

_@carljm_

Address a potential point of confusion that bit a contributor in https://github.com/astral-sh/ruff/pull/13719

Also remove a no-longer-accurate line about bare `error: ` assertions (which are no longer allowed) and clarify another point about which kinds of error assertions to use.


---

_Label `red-knot` added by @carljm on 2024-10-11 18:53_

---

_Review requested from @MichaReiser by @carljm on 2024-10-11 18:53_

---

_Review requested from @AlexWaygood by @carljm on 2024-10-11 18:53_

---

_@AlexWaygood reviewed on 2024-10-11 19:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:31 on 2024-10-11 19:02_

We could also link directly to https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/numbers.md, as an actual example of what the syntax should look like?

---

_@AlexWaygood reviewed on 2024-10-11 19:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:31 on 2024-10-11 19:03_

ooh... We could also put this paragraph inside `<!---` and `--->`, so that you _only_ see the paragraph if you're reading the raw source, and the paragraph is hidden if you're reading a rendered version!

---

_@AlexWaygood approved on 2024-10-11 19:07_

LGTM. https://github.com/astral-sh/ruff/pull/13720#discussion_r1797329789 would be nice but isn't crucial!

---

_@carljm reviewed on 2024-10-11 19:22_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:31 on 2024-10-11 19:22_

> ooh... We could also put this paragraph inside `<!---` and `--->`, so that you _only_ see the paragraph if you're reading the raw source, and the paragraph is hidden if you're reading a rendered version!

Heh, I thought about this, but then thought it's perhaps too clever and just confusing. But maybe it's worth doing...

---

_@carljm reviewed on 2024-10-11 19:23_

---

_Review comment by @carljm on `crates/red_knot_test/README.md`:31 on 2024-10-11 19:23_

Seems to work fine, let's do it :)

---

_@AlexWaygood reviewed on 2024-10-11 19:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/README.md`:31 on 2024-10-11 19:25_

Nice!! :D

---

_Merged by @carljm on 2024-10-11 19:36_

---

_Closed by @carljm on 2024-10-11 19:36_

---

_Branch deleted on 2024-10-11 19:36_

---
