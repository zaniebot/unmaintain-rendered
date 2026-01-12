```yaml
number: 19812
title: Update end-of-file ranges to point to the actual last line
type: pull_request
state: closed
author: ntBre
labels:
  - bug
  - diagnostics
assignees: []
draft: true
base: main
head: brent/ruff-eof-ranges
created_at: 2025-08-07T17:29:08Z
updated_at: 2025-08-08T12:17:02Z
url: https://github.com/astral-sh/ruff/pull/19812
synced_at: 2026-01-12T15:56:47Z
```

# Update end-of-file ranges to point to the actual last line

---

_@ntBre_

## Summary

See https://github.com/astral-sh/ruff/pull/19806#discussion_r2260365462. In short, we would previously report EOF ranges as being on a line that didn't actually exist:

```
try.py:5:1: SyntaxError: unexpected EOF while parsing
  |
3 | 'x'
4 | )
  |  ^
  |
```

Note the final line is 4 in the file and in the diagnostic, but the header reports 5:1 as the location.

I split off this PR because I think this is basically a bug fix in Ruff. I'll leave the ty diagnostic change in #19806.

## Test Plan

I was a bit worried that some of the output formats might differ in the way they calculate line locations, so I added a copy of the `output_format` CLI test that runs on a snippet minimized from the `ISC_syntax_error.py` case that also changed in this PR. I guess my fears have been allayed, so we don't necessarily have to keep the CLI test if we don't want to.


---

_Label `bug` added by @ntBre on 2025-08-07 17:33_

---

_Label `diagnostics` added by @ntBre on 2025-08-07 17:33_

---

_Comment by @github-actions[bot] on 2025-08-07 17:40_

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

_Marked ready for review by @ntBre on 2025-08-07 17:49_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-07 17:49_

---

_@MichaReiser reviewed on 2025-08-08 07:59_

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:5809 on 2025-08-08 07:59_

Okay, now I'm a bit confused. 

I assumed that the issue here was that we showed 5:1 for a file with no line break at the end of the file:


```
(
'''unterminated string
'x'
)
```

Here, I expect Ruff to use 4:2 as the location. 

But I'm not so sure if this is the right fix if the file does end with a new line. I tested both textmate and VS code and both allow me to navigate to `5:1` for

```
(
'''unterminated string
'x'
)

```

which is why I think it's more accurate if we do report 5:1 if the offset points to the end of file and we should instead fix the rendering in annotated snippet.


I'm sorry that I misunderstood this in your other PR.

---

_@ntBre reviewed on 2025-08-08 11:51_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:5809 on 2025-08-08 11:51_

No worries, I realized that I was a bit confused yesterday afternoon too. I think I also thought there was no trailing newline initially and then somehow convinced myself it still made sense to do this when the newline was present.

I'll look into the rendering instead.

---

_Converted to draft by @ntBre on 2025-08-08 11:54_

---

_Closed by @ntBre on 2025-08-08 12:16_

---

_Branch deleted on 2025-08-08 12:17_

---
