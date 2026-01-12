```yaml
number: 3090
title: "Add Type List Support for ColdFusion/CFML & BoxLang"
type: pull_request
state: closed
author: JamoCA
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2025-07-05T00:39:49Z
updated_at: 2025-09-20T01:08:42Z
url: https://github.com/BurntSushi/ripgrep/pull/3090
synced_at: 2026-01-12T18:23:15Z
```

# Add Type List Support for ColdFusion/CFML & BoxLang

---

_@JamoCA_

Requested here and now submitting a PR.
https://github.com/BurntSushi/ripgrep/issues/3089#issuecomment-3037430356

---

_@BurntSushi reviewed on 2025-08-19 20:38_

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:43 on 2025-08-19 20:38_

I've changed this to `*.cfc` and `*.cfm`. It doesn't make sense for everyone using this file type to pay for case insensitive matching all of the time. For users where this matters, they can use the `--glob-case-insensitive` flag.

---

_Label `rollup` added by @BurntSushi on 2025-08-19 20:38_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
