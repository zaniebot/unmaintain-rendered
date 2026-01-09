---
number: 10570
title: "Update `PatternMatchMapping` AST node"
type: issue
state: open
author: dhruvmanila
labels:
  - internal
  - parser
  - needs-design
assignees: []
created_at: 2024-03-25T13:11:21Z
updated_at: 2025-04-01T08:52:11Z
url: https://github.com/astral-sh/ruff/issues/10570
synced_at: 2026-01-07T13:12:15-06:00
---

# Update `PatternMatchMapping` AST node

---

_Issue opened by @dhruvmanila on 2024-03-25 13:11_

As discussed in this comment thread: https://github.com/astral-sh/ruff/pull/10477#discussion_r1533482057

Multiple double star pattern isn't allowed in a mapping pattern. The new parser throws an error but drops the earlier node because there's no way to store multiple double star pattern. This requires updating the AST to allow that.

Possible representation (similar to Pyright):
```rs
pub struct PatternMatchMapping {
    pub range: TextRange,
	pub patterns: Vec<MappingPattern>
}

pub enum MappingPattern {
	KeyValue(MappingKeyValuePattern),
	DoubleStar(MappingDoubleStarPattern),
}

pub struct MappingKeyValuePattern {
	pub range: TextRange,
	pub key: Box<Expr>,
	pub pattern: Pattern,
}

pub struct MappingDoubleStarPattern {
	pub range: TextRange,
	pub value: Identifier,
}
```

**This isn't finalized yet.**

---

_Label `internal` added by @dhruvmanila on 2024-03-25 13:11_

---

_Label `parser` added by @dhruvmanila on 2024-03-25 13:11_

---

_Label `needs-design` added by @dhruvmanila on 2024-03-25 13:11_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-03-25 13:11_

---

_Referenced in [astral-sh/ruff#10477](../../astral-sh/ruff/pulls/10477.md) on 2024-03-25 13:11_

---

_Referenced in [astral-sh/ruff#10653](../../astral-sh/ruff/issues/10653.md) on 2024-03-29 07:59_

---

_Referenced in [astral-sh/ruff#11267](../../astral-sh/ruff/pulls/11267.md) on 2024-05-06 16:48_

---

_Unassigned @dhruvmanila by @dhruvmanila on 2024-11-26 08:48_

---

_Comment by @MichaReiser on 2025-04-01 08:52_

Another downside of the current representation is that visiting the node in source order requires comparing text ranges to *guess* when to visit the `rest` pattern

---
