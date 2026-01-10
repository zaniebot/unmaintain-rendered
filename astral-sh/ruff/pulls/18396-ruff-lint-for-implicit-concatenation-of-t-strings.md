```yaml
number: 18396
title: "[`ruff`] Lint for implicit concatenation of t-strings and f-strings"
type: pull_request
state: closed
author: dylwil3
labels:
  - rule
  - preview
  - python314
assignees: []
base: main
head: implicit-t-string-concat
created_at: 2025-05-30T21:06:23Z
updated_at: 2025-07-14T21:45:56Z
url: https://github.com/astral-sh/ruff/pull/18396
synced_at: 2026-01-10T17:58:13Z
```

# [`ruff`] Lint for implicit concatenation of t-strings and f-strings

---

_Pull request opened by @dylwil3 on 2025-05-30 21:06_

As advertised!

Feel free to push back about the name, the decision to put this in the AST checker instead of working harder and putting it in the tokens checker, and anything else 😄 


---

_Label `rule` added by @dylwil3 on 2025-05-30 21:06_

---

_Label `preview` added by @dylwil3 on 2025-05-30 21:06_

---

_Label `python314` added by @dylwil3 on 2025-05-30 21:06_

---

_Comment by @MichaReiser on 2025-05-30 21:10_

I think we should wait with adding this rule until it's clear that it isn't a syntax error. We otherwise risk having a deprecated rule even before 3.14 is released 

---

_Comment by @dylwil3 on 2025-05-30 21:11_

> I think we should wait with adding this rule until it's clear that it isn't a syntax error. We otherwise risk having a deprecated rule even before 3.14 is released

That's fine with me! Just thought I'd knock it out before I forgot - happy to close/turn this to draft until then.

---

_Comment by @github-actions[bot] on 2025-05-30 21:14_

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

_Comment by @MichaReiser on 2025-05-30 21:15_

I love the rule itself. Thanks for implementing it so quickly 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/implicit_concatenation_format_and_template_string.rs`:41 on 2025-05-30 21:16_

I don't see much harm to run the rule even if the target version doesn't support t strings 

---

_@MichaReiser reviewed on 2025-05-30 21:16_

---

_@dylwil3 reviewed on 2025-05-30 21:43_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/implicit_concatenation_format_and_template_string.rs`:41 on 2025-05-30 21:43_

I guess the issue is that the user would also see a version specific syntax error. But that's fine I guess?

---

_Comment by @dylwil3 on 2025-05-30 21:44_

I think the mkdocs test fails because it's testing the syntax of the docs using Python 3.13

---

_Comment by @MichaReiser on 2025-06-20 06:39_

You can add it to the `KNOWN_PARSE_ERRORS` exclusion list (I think)?

---

_Comment by @dylwil3 on 2025-07-14 21:45_

Closing as this will now be incorporated into the parser

---

_Closed by @dylwil3 on 2025-07-14 21:45_

---
