```yaml
number: 3157
title: "cli: document that `-c/--count` can be inconsistent with `-l/--files-with-matches`"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/document-count-inconsistency
created_at: 2025-09-22T21:05:54Z
updated_at: 2025-09-23T00:24:54Z
url: https://github.com/BurntSushi/ripgrep/pull/3157
synced_at: 2026-01-12T18:23:15Z
```

# cli: document that `-c/--count` can be inconsistent with `-l/--files-with-matches`

---

_@BurntSushi_

This is unfortunate, but is a known bug that I don't think can be fixed
without either making `-l/--files-with-matches` much slower or changing
what "binary filtering" means by default.

In this PR, we document this inconsistency since users may find it quite
surprising. The actual work-around is to disable binary filtering with
the `--binary` flag.

We add a test confirming this behavior.

Closes #3131


---

_Merged by @BurntSushi on 2025-09-23 00:24_

---

_Closed by @BurntSushi on 2025-09-23 00:24_

---

_Branch deleted on 2025-09-23 00:24_

---
