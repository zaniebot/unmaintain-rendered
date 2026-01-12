```yaml
number: 18483
title: "[ty] Track the origin of the `environment.python` setting for better error messages"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/track-python-setting-origin
created_at: 2025-06-05T18:17:58Z
updated_at: 2025-06-11T10:50:29Z
url: https://github.com/astral-sh/ruff/pull/18483
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Track the origin of the `environment.python` setting for better error messages

---

_@AlexWaygood_

## Summary

This PR tracks the origin of the `environment.python` setting more precisely, so that we don't refer to the user's "`--python` argument" in error messages if the setting was actually specified in a configuration file.

The first commit is a pretty simple change that just tracks _whether_ the setting came from the CLI or a config file. The second commit adds annotations to some `site-packages` error messages so that we show the user exactly where in the config file the setting was specified. Most of the diff comes from the second commit; I can drop this change, if we feel it adds too much complexity. I'm not totally sure if it's worth it or not.

## Test Plan

Snapshot tests and manual testing.


---

_Label `ty` added by @AlexWaygood on 2025-06-05 18:17_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-05 18:18_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-05 18:18_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-05 18:18_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-06-05 18:18_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:394 on 2025-06-06 05:54_

This should not be necessary. `RelativePathBuf` wrapps `RangedValue` internally. But you might need to add a few methods to get the value (or decide that the current wrapping doesn't make any sense and rewrite all uses of `RelativePathBuf` to `RangedValue<RelativePathBuf>` and remove the `RangedValue` from `RelativePathBuf`)

---

_Review comment by @MichaReiser on `crates/ty/tests/cli.rs`:1112 on 2025-06-06 05:58_

We should display the file name here. It's otherwise unclear where this snippet comes from. E.g. I don't understand what the message shows here.

---

_@MichaReiser reviewed on 2025-06-06 05:59_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/site_packages.rs`:651 on 2025-06-06 06:01_

Hmm, It definetely doesn't make me excited that we're using annotation snippet here directly. 

---

_@MichaReiser reviewed on 2025-06-06 06:01_

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-06 07:22_

---

_@AlexWaygood reviewed on 2025-06-06 10:51_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:394 on 2025-06-06 10:51_

Thanks, I totally missed that

---

_Comment by @github-actions[bot] on 2025-06-06 11:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-06 11:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2025-06-06 11:58_

---

_@AlexWaygood reviewed on 2025-06-06 12:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/site_packages.rs`:651 on 2025-06-06 12:01_

Agreed, but it's useful to have snippet rendering capabilities in this module for other reasons too. It might be nice in the future to render annotations if we fail to parse pyvenv.cfg files, for example. (This _would_ be a good "help wanted" task.)

---

_Merged by @AlexWaygood on 2025-06-06 12:36_

---

_Closed by @AlexWaygood on 2025-06-06 12:36_

---

_Branch deleted on 2025-06-06 12:36_

---

_Comment by @AlexWaygood on 2025-06-06 12:43_

Demo of what the error looks like now:

![image](https://github.com/user-attachments/assets/ac453cbb-7757-4c84-b5e9-1f43b3140d3f)


---

_Label `diagnostics` added by @dhruvmanila on 2025-06-11 10:50_

---
