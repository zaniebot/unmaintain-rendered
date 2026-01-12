```yaml
number: 8858
title: "docs: document how to mimic --verbose with environment variable RUST_LOG"
type: pull_request
state: merged
author: Phlogistique
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-11-06T09:23:11Z
updated_at: 2024-11-07T23:34:11Z
url: https://github.com/astral-sh/uv/pull/8858
synced_at: 2026-01-12T16:08:32Z
```

# docs: document how to mimic --verbose with environment variable RUST_LOG

---

_@Phlogistique_

The doc was unclear to me and I had to dig in the code to understand that RUST_LOG could do the same as adding `--verbose`

---

_@j178 reviewed on 2024-11-06 09:57_

---

_Review comment by @j178 on `docs/configuration/environment.md`:174 on 2024-11-06 09:57_

This file is generated from `crates/uv-static/env_vars.rs`, you should change that file instead.

---

_@Phlogistique reviewed on 2024-11-06 16:04_

---

_Review comment by @Phlogistique on `docs/configuration/environment.md`:174 on 2024-11-06 16:04_

oops! thanks!

---

_@Phlogistique reviewed on 2024-11-07 08:39_

---

_Review comment by @Phlogistique on `docs/configuration/environment.md`:174 on 2024-11-07 08:39_

Done, sadly the doc generator removes the empty line after the bullet points, which will break the formatting :|

---

_Comment by @Phlogistique on 2024-11-07 08:39_

@NameNoQuality is that the wrong link?

---

_@konstin approved on 2024-11-07 08:56_

---

_@konstin approved on 2024-11-07 08:58_

---

_Merged by @konstin on 2024-11-07 09:24_

---

_Closed by @konstin on 2024-11-07 09:24_

---
