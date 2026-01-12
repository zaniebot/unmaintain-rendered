```yaml
number: 7130
title: "pep508: add graph debug representation for `MarkerTree`"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/marker-tree-debug-graph
created_at: 2024-09-06T17:07:51Z
updated_at: 2024-09-09T20:12:12Z
url: https://github.com/astral-sh/uv/pull/7130
synced_at: 2026-01-12T16:07:42Z
```

# pep508: add graph debug representation for `MarkerTree`

---

_@BurntSushi_

This PR revives #6129, but is less bold:

* It doesn't rename anything. (I think the rename is probably right
  though.)
* It doesn't change the _default_ `Debug` impl. Instead, it offers this
  as a new `MarkerTree::debug_graph` method.

I found this pretty useful for debugging since it gives a display format
that is more faithful to the internal representation of a `MarkerTree`.
So I think it's worth having around. But making it available in `Debug`
is perhaps a bridge too far since it isn't as familiar as the typical
PEP 508 representation and isn't as succinct.

I did consider printing this when using `{:#?}` (i.e., the "alternate"
debug representation), but too many things use that (like `insta` I
think) to make it practical.

Closes #6129


---

_Review requested from @ibraheemdev by @BurntSushi on 2024-09-06 17:08_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-09-06 17:08_

---

_Review requested from @konstin by @BurntSushi on 2024-09-06 17:08_

---

_@ibraheemdev approved on 2024-09-06 20:39_

---

_Merged by @BurntSushi on 2024-09-06 20:47_

---

_Closed by @BurntSushi on 2024-09-06 20:47_

---

_Branch deleted on 2024-09-06 20:47_

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/tree.rs`:1275 on 2024-09-09 14:26_

Should this be `not in`?

---

_Review comment by @konstin on `crates/pep508-rs/src/marker/tree.rs`:1290 on 2024-09-09 14:27_

Should this be `extra != {}`?

---

_@konstin reviewed on 2024-09-09 14:28_

I love this!

---

_Comment by @BurntSushi on 2024-09-09 20:12_

Yeah I think both of your comments are right. I opened a follow-up here at #7230.

---
