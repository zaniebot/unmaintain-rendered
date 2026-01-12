```yaml
number: 1444
title: Fix the current GitHub Actions workflow
type: pull_request
state: closed
author: nickelc
labels: []
assignees: []
base: master
head: gh-actions
created_at: 2019-12-10T13:55:55Z
updated_at: 2020-01-11T17:56:53Z
url: https://github.com/BurntSushi/ripgrep/pull/1444
synced_at: 2026-01-12T18:23:13Z
```

# Fix the current GitHub Actions workflow

---

_@nickelc_

zsh auto completions test was missing the `TARGET` env var.
`cross` is required for the musl build

see: https://github.com/nickelc/ripgrep/runs/341954437

---

_Comment by @BurntSushi on 2020-01-11 17:36_

Thanks for attempting this. GA was in an intermediate state that I never quite finished. I switched back to Travis/AppVeyor temporarily, but I'm hoping to take another crack at this soon.

---

_Closed by @BurntSushi on 2020-01-11 17:36_

---

_Branch deleted on 2020-01-11 17:56_

---
