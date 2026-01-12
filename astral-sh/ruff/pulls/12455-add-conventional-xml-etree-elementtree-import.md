```yaml
number: 12455
title: add conventional xml.etree.ElementTree import alias
type: pull_request
state: merged
author: edhinard
labels:
  - breaking
  - configuration
assignees: []
merged: true
base: ruff-0.6
head: ET
created_at: 2024-07-22T16:14:35Z
updated_at: 2024-08-12T09:23:35Z
url: https://github.com/astral-sh/ruff/pull/12455
synced_at: 2026-01-12T15:55:41Z
```

# add conventional xml.etree.ElementTree import alias

---

_@edhinard_

## Summary

Adding a conventional alias for [ElementTree](https://docs.python.org/3/library/xml.etree.elementtree.html). I looked for other conventional aliases in standard library in order to generalize the point, but found none.
There is still an issue with this. It conflicts with `N817 CamelCase 'ElementTree' imported as acronym 'ET'`. I have to ignore locally:
`import xml.etree.ElementTree as ET # noqa: N817`
too bad

## Test Plan

only test as a `lint.flake8-import-conventions.extend-aliases` configuration option


---

_Comment by @charliermarsh on 2024-07-22 16:31_

You need to add this to `CONVENTIONAL_ALIASES` in `crates/ruff_linter/src/rules/flake8_import_conventions/settings.rs` too. We may need to include this in the next minor version.

---

_Added to milestone `v0.6` by @charliermarsh on 2024-07-25 20:57_

---

_Label `configuration` added by @charliermarsh on 2024-07-25 20:57_

---

_@charliermarsh approved on 2024-07-25 20:57_

Thanks. I think this is reasonable to include but it'll have to wait until the next minor version.

---

_Label `breaking` added by @MichaReiser on 2024-07-26 06:42_

---

_Merged by @MichaReiser on 2024-08-12 09:23_

---

_Closed by @MichaReiser on 2024-08-12 09:23_

---
