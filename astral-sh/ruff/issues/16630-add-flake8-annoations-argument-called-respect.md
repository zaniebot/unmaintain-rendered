```yaml
number: 16630
title: Add flake8-annoations argument called respect-type-ignore
type: issue
state: open
author: martinResearch
labels:
  - suppression
assignees: []
created_at: 2025-03-11T15:38:10Z
updated_at: 2025-03-12T09:12:01Z
url: https://github.com/astral-sh/ruff/issues/16630
synced_at: 2026-01-12T15:54:55Z
```

# Add flake8-annoations argument called respect-type-ignore

---

_@martinResearch_

### Summary

 flake8-annotations supports an options `--respect-type-ignore`. See [here](https://github.com/sco1/flake8-annotations?tab=readme-ov-file#--respect-type-ignore).
It would be great to have support for it in Ruff.


---

_Label `configuration` added by @dylwil3 on 2025-03-11 19:55_

---

_Label `suppression` added by @dylwil3 on 2025-03-11 19:55_

---

_Label `configuration` removed by @MichaReiser on 2025-03-12 09:07_

---

_Comment by @MichaReiser on 2025-03-12 09:11_

I don't think we want to support rule-specific suppression comments. That would become rather confusing, especially once we remove the linter groups as they exist today (https://github.com/astral-sh/ruff/issues/1774). 

Our in-work type checker will support `type: ignore` comments and it's somewhat likely that we'll support it as well in Ruff once we transition Ruff to use the same infrastructure but with an option to *ignore* `type: ignore` suppression comments. 

However, I could also see us to disallow the use of `type: ignore` for lint like rules. That's why I think we should wait with adding support for `type: ignore` until we have a better understanding where we'll go with the Ruff/Red Knot integration


Related: https://github.com/astral-sh/ruff/issues/1203

---
