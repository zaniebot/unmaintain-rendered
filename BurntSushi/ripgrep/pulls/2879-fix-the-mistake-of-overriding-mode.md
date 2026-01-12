```yaml
number: 2879
title: fix the mistake of overriding mode
type: pull_request
state: closed
author: QWZeng
labels: []
assignees: []
base: master
head: qingwen/fix
created_at: 2024-09-03T10:44:39Z
updated_at: 2024-09-03T10:48:13Z
url: https://github.com/BurntSushi/ripgrep/pull/2879
synced_at: 2026-01-12T18:23:14Z
```

# fix the mistake of overriding mode

---

_@QWZeng_

a non-searching mode can override non-searching mode. It’s likely just a mistake or typo :
if !matches!(*self, Mode::Search(_));
it should be：
if !matches!(new, Mode::Search(_))

---

_Closed by @QWZeng on 2024-09-03 10:48_

---
