```yaml
number: 22328
title: "Identical/duplicate rules: compare-with-tuple (SIM109) and repeated-equality-comparison (PLR1714)"
type: issue
state: open
author: GideonBear
labels:
  - rule
  - breaking
assignees: []
created_at: 2026-01-01T14:36:34Z
updated_at: 2026-01-01T15:07:21Z
url: https://github.com/astral-sh/ruff/issues/22328
synced_at: 2026-01-10T11:10:00Z
```

# Identical/duplicate rules: compare-with-tuple (SIM109) and repeated-equality-comparison (PLR1714)

---

_Issue opened by @GideonBear on 2026-01-01 14:36_

### Summary

The rules [compare-with-tuple (SIM109)](https://docs.astral.sh/ruff/rules/compare-with-tuple/#compare-with-tuple-sim109) and [repeated-equality-comparison (PLR1714)](https://docs.astral.sh/ruff/rules/repeated-equality-comparison/#repeated-equality-comparison-plr1714) do (mostly) the same, resulting in duplicate lints: [playground](https://play.ruff.rs/09286444-61e2-4492-ae08-2c201f5d5c36)

Note that on SIM109 seems to only work on variables. When using literals: [playground](https://play.ruff.rs/5ba10317-2b21-4ce6-9cdf-df058772dd69)

I propose removing one of the rules, similarly to [repeated-isinstance-calls (PLR1701)](https://docs.astral.sh/ruff/rules/repeated-isinstance-calls/#repeated-isinstance-calls-plr1701).

Alternatively, the relation between these two rules should be explained in the documentation, and the PLR1714 should be suppressed when SIM109 is also found, to avoid duplicate lints.

### Version

ruff 0.14.10

---

_Comment by @ntBre on 2026-01-01 15:07_

Thanks! I hadn't noticed these, but they do sound basically identical. I'm surprised they didn't appear in the analysis in #12481, but maybe the literal vs variable handling made them look different enough.

I'll put this on the milestone to consider deprecating one of these in the next minor release.

---

_Added to milestone `v0.15` by @ntBre on 2026-01-01 15:07_

---

_Label `rule` added by @ntBre on 2026-01-01 15:07_

---

_Label `breaking` added by @ntBre on 2026-01-01 15:07_

---
