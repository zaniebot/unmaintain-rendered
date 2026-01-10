```yaml
number: 5562
title: "audit universal resolver's robustness with respect to false positives/negatives with routines like \"are these two marker expressions disjoint\""
type: issue
state: closed
author: BurntSushi
labels:
  - internal
assignees: []
created_at: 2024-07-29T14:49:22Z
updated_at: 2024-08-16T17:29:05Z
url: https://github.com/astral-sh/uv/issues/5562
synced_at: 2026-01-10T04:53:49Z
```

# audit universal resolver's robustness with respect to false positives/negatives with routines like "are these two marker expressions disjoint"

---

_Issue opened by @BurntSushi on 2024-07-29 14:49_

Right now, the universal resolver relies on an internal routine, `marker::is_disjoint`, to report whether two marker expressions could _ever_ evaluate to `true` for the same marker environment. If they can't, then they are considered disjoint.

At present, we use this to determine whether to create forks between conflicting dependency specifications. For example:

```
a==1.0.0 ; sys_platform == 'linux'
a==2.0.0 ; sys_platform == 'windows'
```

It is indeed the case that `sys_platform == 'linux'` and `sys_platform == 'windows'` can literally never be active for the same marker environment, and thus they are considered disjoint. This in turn "allows" the universal resolver to fork. (Although we are very likely going to need to allow forks even in cases of non-disjointness, as explored in #4732.)

But, we do use disjointness checking elsewhere. For example, when creating a fork inside the resolver, we remove any dependencies whose marker expressions are disjoint with that fork's marker expression. This is because those dependencies can never appear in environments that use that fork. But if disjointness checking is wrong, then this could lead to incorrect results.

What is known is that our disjointness checking _is_ wrong today. Specifically, I believe perfect disjointness checking is NP-complete. So right now, we only use heuristics. What is not quite known is whether its "wrongness" is limited to only false positives, limited to only false negatives, or is both. Moreover, I do not believe the callers of disjointness checking have been audited from what they are robust to. Sometimes false negatives are okay in the sense that they might just lead to not being able to produce a resolution, for example.

This is also somewhat shifting as mentioned via #4732, and also the work that @ibraheemdev is doing based on marker normalization.

---

_Label `internal` added by @BurntSushi on 2024-07-29 14:49_

---

_Comment by @ibraheemdev on 2024-07-29 20:43_

I believe we are going to end up in a place where marker satisfiability can have false positives but never false negatives, i.e. we might think a marker tree is satisfiable when it is actually not, but we never claim a marker tree is not satisfiable when it actually is. This is mostly due to the `in` operator on strings which can have interactions that are difficult to model (for example, `key > 'z' and key in 'linux'` is not satisfiable, but that is not easy to see), but there may be other places this is possible that we have not accounted for yet so it probably a good idea to remain pessimistic.

Similarly, the disjointness of two markers is equivalent to asking whether their conjunction is not satisfiable, so we should account for false negatives but not false positives. i.e. it is possible that we think two markers are not disjoint when they actually are (such as `key > 'z'` and `key in 'linux'`), but we should never claim that two markers are disjoint when there is a potential intersection.

---

_Comment by @BurntSushi on 2024-08-16 17:29_

I think we're all set here with @ibraheemdev's work in #5898.

---

_Closed by @BurntSushi on 2024-08-16 17:29_

---
