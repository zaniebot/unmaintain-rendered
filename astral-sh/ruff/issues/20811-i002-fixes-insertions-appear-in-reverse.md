```yaml
number: 20811
title: I002 fixes’ insertions appear in reverse alphabetical order
type: issue
state: open
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-10-11T13:24:48Z
updated_at: 2025-10-13T20:27:43Z
url: https://github.com/astral-sh/ruff/issues/20811
synced_at: 2026-01-12T15:54:57Z
```

# I002 fixes’ insertions appear in reverse alphabetical order

---

_@dscorbett_

### Summary

The fix for [`missing-required-import` (I002)](https://docs.astral.sh/ruff/rules/missing-required-import/) inserts imports in reverse alphabetical order. This is probably never what anyone wants. It would be better to insert them in the order specified in the config, but if they must be sorted, then they should be inserted in alphabetical order.
```console
$ echo 1 | ruff --isolated check - --config 'lint.isort.required-imports = ["import x1", "import x3", "import x2"]' --select I002 --unsafe-fixes --fix -q
import x3
import x2
import x1
1
```

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Comment by @ntBre on 2025-10-13 20:27_

I think it makes sense to try this. We currently insert each import at the very beginning of the file:

https://github.com/astral-sh/ruff/blob/a362ddb86ca356bdb3ca97d71af106258ef51cdc/crates/ruff_linter/src/rules/isort/rules/add_required_imports.rs#L127-L130

But I think that's a little easier to deal with than #20787. An `IndexSet` might be helpful for the `required_imports` field instead of a `BTreeSet`.

---

_Label `fixes` added by @ntBre on 2025-10-13 20:27_

---
