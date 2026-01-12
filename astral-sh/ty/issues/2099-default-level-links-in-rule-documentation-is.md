```yaml
number: 2099
title: "\"Default level\" links in rule documentation is broken"
type: issue
state: closed
author: DetachHead
labels: []
assignees: []
created_at: 2025-12-19T05:32:14Z
updated_at: 2025-12-25T06:23:25Z
url: https://github.com/astral-sh/ty/issues/2099
synced_at: 2026-01-12T15:54:26Z
```

# "Default level" links in rule documentation is broken

---

_@DetachHead_

https://docs.astral.sh/ty/reference/rules/#:~:text=Default%20level%3A%20error

these link to https://docs.astral.sh/ty/reference/rules.md#rule-levels which is a 404

i suggest running `mkdocs build --strict` with all the rules enabled in CI which should catch broken links

---

_Comment by @MichaReiser on 2025-12-19 07:14_

Thank you. 

It seems we already run mkdocs with `--strict` in CI

https://github.com/astral-sh/ty/blob/a00dcee85b933e38a8d6b1bf05353889e6331e7e/.github/workflows/ci.yaml#L112-L113

---

_Closed by @MichaReiser on 2025-12-19 07:25_

---

_Comment by @DetachHead on 2025-12-25 06:23_

hmm, looks like mkdocs doesn't validate links in html `a` tags, only markdown links.

---
