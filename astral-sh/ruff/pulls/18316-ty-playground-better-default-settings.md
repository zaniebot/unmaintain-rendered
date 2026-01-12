```yaml
number: 18316
title: "[ty] Playground: Better default settings"
type: pull_request
state: merged
author: sharkdp
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: david/playground-default-settings
created_at: 2025-05-26T11:56:13Z
updated_at: 2025-05-26T12:14:25Z
url: https://github.com/astral-sh/ruff/pull/18316
synced_at: 2026-01-12T15:56:16Z
```

# [ty] Playground: Better default settings

---

_@sharkdp_

## Summary

The playground default settings set the `division-by-zero` rule severity to `error`. This slightly confusing because `division-by-zero` is now disabled by default. I am assuming that we have a `rules` section in there to make it easier for users to customize those settings (in addition to what the JSON schema gives us).

Here, I'm proposing a different default rule-set (`"undefined-reveal": "ignore"`) that I would personally find more helpful for the playground, since we're using it so frequently for MREs that often involve some `reveal_type` calls.



---

_Label `playground` added by @sharkdp on 2025-05-26 11:56_

---

_Label `ty` added by @sharkdp on 2025-05-26 11:56_

---

_Review requested from @MichaReiser by @sharkdp on 2025-05-26 12:11_

---

_@MichaReiser approved on 2025-05-26 12:13_

Thanks. That's definetely the better default and a good rule to demonstrate how the settings work

---

_Merged by @sharkdp on 2025-05-26 12:14_

---

_Closed by @sharkdp on 2025-05-26 12:14_

---

_Branch deleted on 2025-05-26 12:14_

---
