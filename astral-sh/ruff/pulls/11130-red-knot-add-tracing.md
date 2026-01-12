```yaml
number: 11130
title: "red-knot: Add tracing"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: red-knot
head: red-knot-tracing
created_at: 2024-04-24T14:09:19Z
updated_at: 2024-04-24T15:44:05Z
url: https://github.com/astral-sh/ruff/pull/11130
synced_at: 2026-01-12T15:55:34Z
```

# red-knot: Add tracing

---

_@MichaReiser_

## Summary

Setup a tracing subscriber for red-knot. I think that will become handy when debugging queries:

## Test Plan

Example output

```
┐red_knot::files::intern{self={}}
┘
   0.000170s DEBUG red_knot start analysis for workspace
┐red_knot::parse::parse{file_id=FileId(0)}
├─┐red_knot::source::source_text{file_id=FileId(0)}
├─┘
┘
┐red_knot::parse::parse{file_id=FileId(0)}
┘
┐red_knot::parse::parse{file_id=FileId(0)}
┘
```


---

_Review requested from @carljm by @MichaReiser on 2024-04-24 14:09_

---

_Comment by @carljm on 2024-04-24 14:41_

Awesome!

---

_@carljm approved on 2024-04-24 14:41_

---

_Merged by @MichaReiser on 2024-04-24 15:44_

---

_Closed by @MichaReiser on 2024-04-24 15:44_

---

_Branch deleted on 2024-04-24 15:44_

---
