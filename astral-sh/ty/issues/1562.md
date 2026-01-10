```yaml
number: 1562
title: "`from <module>` and `import <module>` detection for completions is not precise enough"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
  - server
  - completions
assignees: []
created_at: 2025-11-14T19:17:13Z
updated_at: 2025-11-21T15:51:53Z
url: https://github.com/astral-sh/ty/issues/1562
synced_at: 2026-01-10T01:58:59Z
```

# `from <module>` and `import <module>` detection for completions is not precise enough

---

_Issue opened by @BurntSushi on 2025-11-14 19:17_

### Summary

Currently, if you do `import collections a<CURSOR>`, then completions for _module_ names will be shown. Instead, we should, at minimum, show scope completions. _Ideally_ we'd only show `as` here.

Additionally, if you do `import collections.abc, collect<CURSOR>`, you'll actually get scope based completions instead of module completions.

This all stems from our `import <module>` and `from <module>` detection being not quite accurate:

https://github.com/astral-sh/ruff/blob/8599c7e5b3b301b2352d30302d74b72e2123347f/crates/ty_ide/src/completion.rs#L810-L854

It's pretty basic and doesn't try to tease apart the different import names.

There are a few other functions that recognize different forms of import statements. Perhaps we should try to unify these into one state machine that tries hard to recognize all forms of import statements and then returns what "kind" it is so that we can choose how we want to offer completions (if at all).

Originally reported here: https://github.com/astral-sh/ruff/pull/21460#pullrequestreview-3466201669

### Version

_No response_

---

_Added to milestone `Stable` by @BurntSushi on 2025-11-14 19:17_

---

_Label `bug` added by @BurntSushi on 2025-11-14 19:17_

---

_Label `server` added by @BurntSushi on 2025-11-14 19:17_

---

_Label `completions` added by @BurntSushi on 2025-11-14 19:17_

---

_Closed by @BurntSushi on 2025-11-17 14:58_

---

_Reopened by @BurntSushi on 2025-11-17 14:58_

---

_Comment by @BurntSushi on 2025-11-17 14:59_

https://github.com/astral-sh/ruff/pull/21484 partially addresses this issue, but `as` was just one example. This issue is meant to capture work toward making `import` detection more precise.

---

_Assigned to @BurntSushi by @BurntSushi on 2025-11-17 14:59_

---

_Comment by @BurntSushi on 2025-11-21 15:51_

Closed by https://github.com/astral-sh/ruff/pull/21547

---

_Closed by @BurntSushi on 2025-11-21 15:51_

---
