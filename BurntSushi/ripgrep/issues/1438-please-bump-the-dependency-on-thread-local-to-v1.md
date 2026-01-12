```yaml
number: 1438
title: Please bump the dependency on thread_local to v1.0.0
type: issue
state: closed
author: paride
labels:
  - enhancement
assignees: []
created_at: 2019-12-04T10:50:43Z
updated_at: 2020-03-15T13:45:16Z
url: https://github.com/BurntSushi/ripgrep/issues/1438
synced_at: 2026-01-12T16:13:23Z
```

# Please bump the dependency on thread_local to v1.0.0

---

_@paride_

Please bump the dependency on thread_local to version 1.0.0. The dependency is pulled in at least in grep_regex, ignore, pcre2, regex, but a ripgrep run on the project directory will help to find all its occurrences :)

This will help the packaging in Debian/Ubuntu and other distros.

Thanks!

---

_Comment by @BurntSushi on 2020-03-15 13:45_

This appears to have been done in cb2f6ddc61b79b7acf59bb00a6be9f1740aa55b8.

---

_Closed by @BurntSushi on 2020-03-15 13:45_

---

_Label `enhancement` added by @BurntSushi on 2020-03-15 13:45_

---
