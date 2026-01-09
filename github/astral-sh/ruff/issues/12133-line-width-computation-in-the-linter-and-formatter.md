---
number: 12133
title: Line width computation in the linter and formatter
type: issue
state: closed
author: dhruvmanila
labels: []
assignees: []
created_at: 2024-07-01T08:45:59Z
updated_at: 2024-07-01T13:27:20Z
url: https://github.com/astral-sh/ruff/issues/12133
synced_at: 2026-01-07T13:12:15-06:00
---

# Line width computation in the linter and formatter

---

_Issue opened by @dhruvmanila on 2024-07-01 08:45_

Related to #12130, it turns out that computing the line width on a char-by-char basis and as a whole `str` is different. From the docs of `unicode-width`, it's [these cases where it differs specifically](https://github.com/unicode-rs/unicode-width/blob/2517d68723f4a472b2eaa5090e01f74d8af4543e/src/lib.rs#L56-L88).

In the linter, the `LineWidthBuilder` determines the width by looking at each `char` individually. But, in some places the width is computed directly on `&str` which creates the panic as in #12130. These cases are:

https://github.com/astral-sh/ruff/blob/eeb24b1a5a723680319ebffe9cd35bff11c1bd31/crates/ruff_linter/src/rules/pycodestyle/overlong.rs#L64

https://github.com/astral-sh/ruff/blob/eeb24b1a5a723680319ebffe9cd35bff11c1bd31/crates/ruff_linter/src/rules/isort/sorting.rs#L109-L109

https://github.com/astral-sh/ruff/blob/eeb24b1a5a723680319ebffe9cd35bff11c1bd31/crates/ruff_linter/src/rules/isort/sorting.rs#L160-L160

https://github.com/astral-sh/ruff/blob/eeb24b1a5a723680319ebffe9cd35bff11c1bd31/crates/ruff_linter/src/fix/snippet.rs#L35-L39

In the formatter,

https://github.com/astral-sh/ruff/blob/eeb24b1a5a723680319ebffe9cd35bff11c1bd31/crates/ruff_formatter/src/printer/mod.rs#L845-L846

https://github.com/astral-sh/ruff/blob/eeb24b1a5a723680319ebffe9cd35bff11c1bd31/crates/ruff_formatter/src/printer/mod.rs#L1520-L1522



---

_Referenced in [astral-sh/ruff#12135](../../astral-sh/ruff/pulls/12135.md) on 2024-07-01 11:30_

---

_Comment by @dhruvmanila on 2024-07-01 13:27_

I'll close this as resolved by #12135 but this is a good reference to have for the time being.

---

_Closed by @dhruvmanila on 2024-07-01 13:27_

---
