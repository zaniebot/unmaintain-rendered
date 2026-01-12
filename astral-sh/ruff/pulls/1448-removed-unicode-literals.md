```yaml
number: 1448
title: Removed unicode literals
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: unicodeliteral
created_at: 2022-12-29T20:31:39Z
updated_at: 2022-12-30T01:28:22Z
url: https://github.com/astral-sh/ruff/pull/1448
synced_at: 2026-01-12T15:55:06Z
```

# Removed unicode literals

---

_@colin99d_

A part of #827.

---

_@charliermarsh reviewed on 2022-12-29 21:41_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_unicode_literal.rs`:18 on 2022-12-29 21:41_

I left some comments on the version that was closed: https://github.com/charliermarsh/ruff/pull/1447#discussion_r1059136611

---

_@squiddy reviewed on 2022-12-29 21:52_

---

_Review comment by @squiddy on `resources/test/fixtures/pyupgrade/UP025.py`:4 on 2022-12-29 21:52_

What about something like

```python
x = u'Hello "World"'
```

I don't think the current approach would handle that well.

---

_@colin99d reviewed on 2022-12-30 00:19_

---

_Review comment by @colin99d on `resources/test/fixtures/pyupgrade/UP025.py`:4 on 2022-12-30 00:19_

Good catch. I updated how I handled quotations so that this will never happen again

---

_Review comment by @colin99d on `src/pyupgrade/plugins/rewrite_unicode_literal.rs`:18 on 2022-12-30 00:20_

I got those knocked out. Also, about your question on my comments in the tests. They are not necessary, I just think they are helpfull for myself and future contributors, but if you wanted them deleted I can delete them.

---

_@colin99d reviewed on 2022-12-30 00:20_

---

_Merged by @charliermarsh on 2022-12-30 01:11_

---

_Closed by @charliermarsh on 2022-12-30 01:11_

---

_Branch deleted on 2022-12-30 01:28_

---
