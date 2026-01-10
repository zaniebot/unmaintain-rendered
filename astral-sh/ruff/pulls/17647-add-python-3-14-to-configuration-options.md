```yaml
number: 17647
title: Add Python 3.14 to configuration options
type: pull_request
state: merged
author: dylwil3
labels:
  - python314
assignees: []
merged: true
base: main
head: config-py314
created_at: 2025-04-26T20:51:31Z
updated_at: 2025-04-28T21:29:01Z
url: https://github.com/astral-sh/ruff/pull/17647
synced_at: 2026-01-10T19:03:00Z
```

# Add Python 3.14 to configuration options

---

_Pull request opened by @dylwil3 on 2025-04-26 20:51_

A small PR that just updates the various settings/configurations to allow Python 3.14. (At the moment selecting that target version will have no impact compared to Python 3.13).

One question (maybe for @ntBre): should I update `PythonVersion::latest` to point to 3.14 as well? Or should that wait until we have full Python 3.14 support ready to go (i.e. is that used as a default anywhere?)

It would also be good for someone to verify that this won't impact red knot in an unforseen way!


---

_Label `python314` added by @dylwil3 on 2025-04-26 20:51_

---

_Comment by @github-actions[bot] on 2025-04-26 21:01_

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

_Comment by @ntBre on 2025-04-27 02:24_

I _think_ `latest` is currently only used in tests. Most of the references are clearly in tests, but I'm not 100% sure about `LinterSettings::for_rule` and `for_rules`, which also call it.

Still, it might be worth holding off until 3.14 is our latest, fully-supported Python version, like you said.

#17529 is currently using it too, but I don't think that will end up being the final design. And I think it will have to wait for a minor release anyway.

---

_Marked ready for review by @dylwil3 on 2025-04-27 20:27_

---

_@MichaReiser reviewed on 2025-04-28 07:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:37 on 2025-04-28 07:44_

It's unfortunate that adding `Py314` makes it appear in the JSON schema and clap documentation without the possibility of mentioning that `Py314` support is in preview.

I think we should log a warning or even abort if a user specifies 3.14 and hasn't `preview` enabled.

---

_@dylwil3 reviewed on 2025-04-28 15:36_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/settings/types.rs`:37 on 2025-04-28 15:36_

I went with a warning, feel free to bikeshed the message which is currently:

> Support for Python 3.14 is under development and may be unstable. Enable `preview` to remove this warning.

---

_Merged by @dylwil3 on 2025-04-28 21:29_

---

_Closed by @dylwil3 on 2025-04-28 21:29_

---
