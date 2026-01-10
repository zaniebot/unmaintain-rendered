```yaml
number: 4996
title: "pep508: add MarkerTree::negate"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/marker-negation
created_at: 2024-07-11T18:07:51Z
updated_at: 2024-07-12T11:37:38Z
url: https://github.com/astral-sh/uv/pull/4996
synced_at: 2026-01-10T13:42:52Z
```

# pep508: add MarkerTree::negate

---

_Pull request opened by @BurntSushi on 2024-07-11 18:07_

This does pretty much what you might expect: it takes an existing
`MarkerTree` and returns a new one that evaluates to the opposite of the
marker given for all possible marker environments. For the most part.

There are two main hitches here.

The first is that some `MarkerTree` values are nonsense. For example,
`"foo" == "bar"` and `sys_platform ~= "linux"`. Both such expressions
always return false today. Technically, a logical negation would imply
that they would always return true in the result. But this would imply
that negation turns nonsense into sense. We shouldn't do that. So the
negation of nonsense remains nonsense.

The second is that negating compatible version constraints like
`python_version ~= "3.6"` cannot be done in a single marker expression.
Instead, the negation is itself a disjunction of negations. In this
case, `python_version < 3.6 or python_version != 3.6.*`.

I've added some basic tests to cover this new operation.

The main motivation for adding a negation operation is in service of
fixing bugs #4732, #4687, #4640 and #4992.


---

_Review requested from @konstin by @BurntSushi on 2024-07-11 18:08_

---

_Review requested from @ibraheemdev by @BurntSushi on 2024-07-11 18:08_

---

_@zanieb approved on 2024-07-11 18:25_

Nice job :)

---

_@ibraheemdev approved on 2024-07-11 18:38_

Nice!

---

_@konstin approved on 2024-07-12 07:14_

Nice work, that's a very elegant solution!

---

_Merged by @BurntSushi on 2024-07-12 11:37_

---

_Closed by @BurntSushi on 2024-07-12 11:37_

---

_Branch deleted on 2024-07-12 11:37_

---
