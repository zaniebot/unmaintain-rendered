```yaml
number: 14986
title: "[red-knot] More useful property-tests based on trivia of set theory"
type: issue
state: closed
author: cake-monotone
labels:
  - testing
  - ty
assignees: []
created_at: 2024-12-15T16:11:50Z
updated_at: 2025-01-12T13:21:31Z
url: https://github.com/astral-sh/ruff/issues/14986
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] More useful property-tests based on trivia of set theory

---

_Issue opened by @cake-monotone on 2024-12-15 16:11_

Maybe we can enhance safety by adding tests inspired by fundamental set theory principles.

### Tests Based on Trivia of Set Theory

1. **Subset of Universal Set**  
   For all sets $A$, $A \subseteq U$, where $U$ is the universal set.

```rust
t.is_fully_static() => t.is_subtype_of(KnownClass::Object.to_instance())
```

2. **Empty Set as a Subset**  
   The empty set $\emptyset$ is a subset of every set:  
   $\emptyset \subseteq A$ for all sets $A$.

```rust
t.is_fully_static() => Type::Never.is_subtype_of(t)
```

3. **Negation should be disjoint from itself**  
   For any set $A$, the intersection of a set with its complement is the empty set:
  $A \cap A^c = \emptyset$.

```rust
t.is_fully_static() => t.is_disjoint_from(t.negate())
```

4. **Intersection as a Subset**
   For any sets $A$, $B$, the intersection $A \cap B$ is a subset of both $A$ and $B$.

```rust
s.is_fully_static() && t.is_fully_static() => s.intersect(t).is_subtype_of(s) && s.intersect(t).is_subtype_of(t)
```

5. **Subset of Union**
  Any set is a subset of the union of itself with another set:
  $A \subseteq A \cup B$ and $B \subseteq A \cup B$.

```rust
s.is_fully_static() && t.is_fully_static() => s.is_subtype_of(s.union(t)) && t.is_subtype_of(s.union(t))
```

I think these tests contribute more significantly to `IntersectionBuilder` and `UnionBuilder` rather than directly to `Type::is_subtype_of` and `Type::is_disjoint_from`.

---

_Label `red-knot` added by @MichaReiser on 2024-12-15 16:18_

---

_Label `testing` added by @AlexWaygood on 2024-12-15 16:49_

---

_Closed by @AlexWaygood on 2025-01-12 13:21_

---
