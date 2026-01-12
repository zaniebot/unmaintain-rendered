```yaml
number: 3122
title: "Update docs to clarify that `.git` is covered by `--hidden` and not `--ignore-vcs`."
type: pull_request
state: closed
author: lgarron
labels:
  - rollup
assignees: []
base: master
head: dot-git-is-hidden-not-vcs
created_at: 2025-08-08T01:43:57Z
updated_at: 2025-09-20T01:08:44Z
url: https://github.com/BurntSushi/ripgrep/pull/3122
synced_at: 2026-01-12T18:23:15Z
```

# Update docs to clarify that `.git` is covered by `--hidden` and not `--ignore-vcs`.

---

_@lgarron_

Addresses: https://github.com/BurntSushi/ripgrep/issues/3121

---

_@ltrzesniewski reviewed on 2025-08-08 09:05_

---

_Review comment by @ltrzesniewski on `crates/core/flags/defs.rs`:2758 on 2025-08-08 09:05_

```suggestion
\flag{hidden}, you must explicitly ignore them using another flag or ignore file.
```

---

_Review comment by @lgarron on `crates/core/flags/defs.rs`:2758 on 2025-08-08 11:44_

@ltrzesniewski Thanks! 😅

That would take it over 80 lines (which most of the file avoids), so I'll apply a fix with wrapping.

---

_@lgarron reviewed on 2025-08-08 11:44_

---

_Label `rollup` added by @BurntSushi on 2025-08-19 23:53_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
