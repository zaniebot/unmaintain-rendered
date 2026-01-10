```yaml
number: 21785
title: Rule for blank line at start of file
type: issue
state: open
author: skissane-medallia
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-12-04T06:06:46Z
updated_at: 2025-12-04T09:20:15Z
url: https://github.com/astral-sh/ruff/issues/21785
synced_at: 2026-01-10T11:10:00Z
```

# Rule for blank line at start of file

---

_Issue opened by @skissane-medallia on 2025-12-04 06:06_

### Summary

I was looking for a rule which checks for a blank line at the start of a file – the first character of the file is a newline.

I can't find one... (I assume I just haven't failed to notice it.) it seems like it would be an easy thing to provide?

In a certain sense, the mirror image of [too-many-newlines-at-end-of-file](https://docs.astral.sh/ruff/rules/too-many-newlines-at-end-of-file/#too-many-newlines-at-end-of-file-w391) – except it would be triggered by any non-zero number of initial newlines, not by just one. (I guess you could also have it match whitespace followed by a newline.)

I realise the formatter will get rid of this, but I work on a code base where we are slowly introducing new ruff check rules, and we aren't yet at the point we want to enforce running ruff format on every file. But I just deleted every initial newline out of the code base, and I'd like a ruff check to ensure we don't get any coming back, but there doesn't seem to be one.

---

_Comment by @MichaReiser on 2025-12-04 09:20_

I'm surprised that this isn't a pycodestyle rule. The rule itself would be compatible with the formatter and shouldn't be too hard to implement. However, I don't think we should add it before we decided on what to do with the already existing https://github.com/astral-sh/ruff/issues/2402 rules

---

_Label `rule` added by @MichaReiser on 2025-12-04 09:20_

---

_Label `needs-decision` added by @MichaReiser on 2025-12-04 09:20_

---
