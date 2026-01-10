```yaml
number: 6160
title: Collapse redundant dependency clauses enumerating available versions
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/collapse-no-versions-redundant
created_at: 2024-08-16T18:11:49Z
updated_at: 2024-08-19T18:02:03Z
url: https://github.com/astral-sh/uv/pull/6160
synced_at: 2026-01-10T13:09:50Z
```

# Collapse redundant dependency clauses enumerating available versions

---

_Pull request opened by @zanieb on 2024-08-16 18:11_

In https://github.com/astral-sh/uv/issues/5046, we show the tautological proof:

```
  ╰─▶ Because colabfold[alphafold]==1.5.5 depends on jax>=0.4.20 and only the following versions of jax are available:
          jax<=0.4.20
          jax==0.4.21
          jax==0.4.22
          jax==0.4.23
          jax==0.4.24
          jax==0.4.25
          jax==0.4.26
          jax==0.4.27
          jax==0.4.28
          jax==0.4.29
          jax==0.4.30
          jax==0.4.31
      we can conclude that colabfold[alphafold]==1.5.5 depends on jax>=0.4.20.
      And because jax>=0.4.20 depends on numpy>=1.26.0, we can conclude that colabfold[alphafold]==1.5.5 depends on numpy>=1.26.0.
      (1)
```

This is a part of the error tree because the statement `colabfold[alphafold]==1.5.5 depends on jax>=0.4.20` is actually a simplification of `colabfold[alphafold]==1.5.5 depends on jax>=0.4.20,<0.5.0` and the no versions clause is a proof of that simplification.

Without simplification, the clause looks like:

```
  ╰─▶ Because colabfold[alphafold]==1.5.5 depends on jax>=0.4.20,<0.5.0 and only the following versions of jax are available:
          jax<=0.4.20
          jax==0.4.21
          jax==0.4.22
          jax==0.4.23
          jax==0.4.24
          jax==0.4.25
          jax==0.4.26
          jax==0.4.27
          jax==0.4.28
          jax==0.4.29
          jax==0.4.30
          jax==0.4.31
      we can conclude that colabfold[alphafold]==1.5.5 depends on one of:
          jax==0.4.20
          jax==0.4.21
          jax==0.4.22
          jax==0.4.23
          jax==0.4.24
          jax==0.4.25
          jax==0.4.26
          jax==0.4.27
          jax==0.4.28
          jax==0.4.29
          jax==0.4.30
          jax==0.4.31
      And because jax>=0.4.20 depends on numpy>=1.26.0, we can conclude that colabfold[alphafold]==1.5.5 depends on numpy>=1.26.0.
```

I don't think we have a great way to avoid performing the simplification of the range conditionally and it makes the error simpler to just jump straight to `colabfold[alphafold]==1.5.5 depends on jax>=0.4.20`. 

The derivation for this clause looks like:

```
          jax==0.4.20 | ==0.4.21 | ==0.4.22 | ==0.4.23 | ==0.4.24 | ==0.4.25 | ==0.4.26 | ==0.4.27 | ==0.4.28 | ==0.4.29 | ==0.4.30 | ==0.4.31 depends on numpy>=1.26.0
            no versions of jax>0.4.20, <0.4.21 | >0.4.21, <0.4.22 | >0.4.22, <0.4.23 | >0.4.23, <0.4.24 | >0.4.24, <0.4.25 | >0.4.25, <0.4.26 | >0.4.26, <0.4.27 | >0.4.27, <0.4.28 | >0.4.28, <0.4.29 | >0.4.29, <0.4.30 | >0.4.30, <0.4.31 | >0.4.31, <0.5.0
            colabfold[alphafold]==1.5.5 depends on jax>=0.4.20, <0.5.0
```

So it looks like we can take trees of this form and drop the "no versions" clause _if_ the ranges are compatible[*]. See [this comment](https://github.com/astral-sh/uv/pull/6160#discussion_r1720280922) for a simpler explanation.

With this pull request, the clause simplifies to

```
╰─▶ Because colabfold[alphafold]==1.5.5 depends on jax>=0.4.20 and jax>=0.4.20 depends on numpy>=1.26.0,
     we can conclude that colabfold[alphafold]==1.5.5 depends on numpy>=1.26.0. (1)
```

Unfortunately, this doesn't change any snapshots in our test suite so I'm uncertain if the strategy generalizes. In some incorrect iterations of this logic, the snapshots did reveal my mistakes.

[*] "if the ranges are compatible" includes a bit of hand-waving. I'm not 100% sure if I've chosen the correct range heuristic here.

---

_Label `error messages` added by @zanieb on 2024-08-16 18:11_

---

_Renamed from "zb/collapse no versions redundant" to "Collapse redundant resolver error clauses enumerating available versions" by @zanieb on 2024-08-16 18:12_

---

_Marked ready for review by @zanieb on 2024-08-16 19:39_

---

_Renamed from "Collapse redundant resolver error clauses enumerating available versions" to "Collapse redundant dependency clauses enumerating available versions" by @zanieb on 2024-08-16 19:40_

---

_Comment by @zanieb on 2024-08-16 19:41_

I think this is quite a bit like PubGrub's `collapse_no_versions` concept but _much_ narrower.

---

_@zanieb reviewed on 2024-08-16 19:42_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:384 on 2024-08-16 19:42_

This explainer might be easier to grok than the real-world example. 

---

_Review requested from @charliermarsh by @zanieb on 2024-08-16 21:05_

---

_Review requested from @konstin by @zanieb on 2024-08-19 14:29_

---

_Review comment by @konstin on `crates/uv-resolver/src/error.rs`:377 on 2024-08-19 16:10_

What are first and second set referring to here?

---

_@konstin approved on 2024-08-19 16:25_

---

_Comment by @konstin on 2024-08-19 16:25_

Can you open an issue in pubgrub to make sure it's tracked upstream too?

---

_Comment by @zanieb on 2024-08-19 17:55_

@konstin I think we need to come up with a test scenario first. I'll open an issue for that.

---

_Merged by @zanieb on 2024-08-19 18:02_

---

_Closed by @zanieb on 2024-08-19 18:02_

---

_Branch deleted on 2024-08-19 18:02_

---
