```yaml
number: 6061
title: "uv/tests: add an unresolvable test case involving overlapping markers"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/unresolvable-overlapping-markers
created_at: 2024-08-13T16:51:38Z
updated_at: 2024-08-13T17:14:50Z
url: https://github.com/astral-sh/uv/pull/6061
synced_at: 2026-01-12T16:07:11Z
```

# uv/tests: add an unresolvable test case involving overlapping markers

---

_@BurntSushi_

This example came up in discussion and it was initially unclear whether
we should try to support it. Specifically, by automatically assuming
that the `datasets < 2.19` dependency had a marker corresponding to the
negation of the conjunction of the other sibling markers for that same
package. But this was deemed, I think, a little too magical.

This in turn implies that whenever there are sibling dependencies with
overlapping marker expressions, their version constraints also need to
be overlapping. Otherwise, for any marker environment that matches both
marker expressions, it would be impossible to select a single version.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-08-13 16:51_

---

_@ibraheemdev approved on 2024-08-13 16:54_

---

_Merged by @BurntSushi on 2024-08-13 17:14_

---

_Closed by @BurntSushi on 2024-08-13 17:14_

---

_Branch deleted on 2024-08-13 17:14_

---
