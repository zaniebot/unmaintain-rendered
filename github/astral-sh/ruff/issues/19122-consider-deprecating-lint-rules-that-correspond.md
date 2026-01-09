---
number: 19122
title: Consider deprecating lint rules that correspond to syntax errors
type: issue
state: open
author: ntBre
labels:
  - rule
assignees: []
created_at: 2025-07-03T15:13:15Z
updated_at: 2025-10-02T12:39:32Z
url: https://github.com/astral-sh/ruff/issues/19122
synced_at: 2026-01-07T13:12:16-06:00
---

# Consider deprecating lint rules that correspond to syntax errors

---

_Issue opened by @ntBre on 2025-07-03 15:13_

Now that we stabilized the semantic syntax errors in 0.12, we could consider deprecating, and eventually removing, the current lint rules that correspond to `SyntaxError`s in CPython. These currently include `SemanticSyntaxError`s:

- [late-future-import (F404)](https://docs.astral.sh/ruff/rules/late-future-import/#late-future-import-f404)
- [load-before-global-declaration (PLE0118)](https://docs.astral.sh/ruff/rules/load-before-global-declaration/#load-before-global-declaration-ple0118)
- [yield-outside-function (F704)](https://docs.astral.sh/ruff/rules/yield-outside-function/#yield-outside-function-f704)
- [return-outside-function (F706)](https://docs.astral.sh/ruff/rules/return-outside-function/#return-outside-function-f706)
- [await-outside-async (PLE1142)](https://docs.astral.sh/ruff/rules/await-outside-async/#await-outside-async-ple1142)

As seen [here](https://github.com/astral-sh/ruff/blob/1b813cd5f1933ab05ba7b96ace798199429c0bd6/crates/ruff_linter/src/checkers/ast/mod.rs#L625).

As well as rules that overlap with `ParseError`s:

- [no-indented-block (E112)](https://docs.astral.sh/ruff/rules/no-indented-block/#no-indented-block-e112)
- [unexpected-indentation (E113)](https://docs.astral.sh/ruff/rules/unexpected-indentation/#unexpected-indentation-e113)

---

_Added to milestone `v0.13` by @ntBre on 2025-07-03 15:13_

---

_Label `rule` added by @ntBre on 2025-07-03 15:13_

---

_Referenced in [astral-sh/ruff#18972](../../astral-sh/ruff/issues/18972.md) on 2025-07-07 22:12_

---

_Comment by @MeGaGiGaGon on 2025-07-07 22:13_

I found a couple more while working on #18972:
- [no-indented-block (E112)](https://docs.astral.sh/ruff/rules/no-indented-block/#no-indented-block-e112)
- [unexpected-indentation (E113)](https://docs.astral.sh/ruff/rules/unexpected-indentation/#unexpected-indentation-e113)

---

_Comment by @ntBre on 2025-07-08 02:30_

I think those are a bit different, at least from what I had in mind here, because we haven't re-implemented them as `SemanticSyntaxError`s (I guess these would actually become `ParseError`s). But that's a good reminder that there are other rules with this property!

---

_Comment by @MeGaGiGaGon on 2025-07-08 03:58_

I guess I don't understand, since those rules raise both a SyntaxError and the lint if you enable them, which to me says they've been re-implemented. https://play.ruff.rs/57648a41-214d-44cb-9c08-8069dfc9c484

---

_Comment by @ntBre on 2025-07-08 13:18_

You're right, I should have tested the examples first. I didn't realize we emitted both a lint and a syntax error. I'll add them to the body of the issue!

---

_Comment by @CodeMan62 on 2025-07-10 01:40_

@ntBre  should we deprecate all these lint rules in one PR or different different PR's ?? 

---

_Referenced in [astral-sh/ruff#19249](../../astral-sh/ruff/pulls/19249.md) on 2025-07-10 02:05_

---

_Comment by @AlexWaygood on 2025-07-10 09:03_

I know you're aware of this, but just for visibility on this issue -- we need to be careful about making PLE1142 non-suppressible, because it's not always a syntax error (it almost always is for `.py` files, but in notebook contexts it very often isn't). See https://github.com/astral-sh/ruff/issues/17395.

If a user has preview mode enabled, Ruff will issue a hard error if a deprecated rule is selected in their Ruff configuration (https://github.com/astral-sh/ruff/pull/19249#issuecomment-3055045708). But eventually we want to make it so that it's impossible to opt _out_ of these rules rather than impossible to opt _into_ them, which makes it a bit different from a normal deprecation. For preview users, would these rules be enabled by default (and reported as syntax errors) whether or not they had selected them by their deprecated lint-rule names in their configuration file?

---

_Comment by @ntBre on 2025-07-10 12:34_

Good point Alex! 

I meant the "Consider" in the title pretty literally. I think it would be good to collect more discussion and design plans before strongly considering this deprecation. 0.13 might even be too soon for that, I expected we might kick this between a couple of milestones but wanted to track the idea somewhere.

I think there are more lint rules related to `nonlocal`, for example, that we might want to include in this batch, and they aren't even implemented as syntax errors yet.

---

_Removed from milestone `v0.13` by @ntBre on 2025-09-05 13:06_

---

_Added to milestone `v0.14` by @ntBre on 2025-09-05 13:06_

---

_Removed from milestone `v0.14` by @ntBre on 2025-10-02 12:39_

---

_Added to milestone `v0.15` by @ntBre on 2025-10-02 12:39_

---
