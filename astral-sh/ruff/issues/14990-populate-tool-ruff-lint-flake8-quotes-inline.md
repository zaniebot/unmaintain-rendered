```yaml
number: 14990
title: "Populate `[tool.ruff.lint.flake8-quotes.inline-quotes]` from `[tool.ruff.format.quote-style]`"
type: issue
state: open
author: sobolevn
labels:
  - configuration
assignees: []
created_at: 2024-12-15T18:24:42Z
updated_at: 2024-12-15T18:57:28Z
url: https://github.com/astral-sh/ruff/issues/14990
synced_at: 2026-01-10T11:09:56Z
```

# Populate `[tool.ruff.lint.flake8-quotes.inline-quotes]` from `[tool.ruff.format.quote-style]`

---

_Issue opened by @sobolevn on 2024-12-15 18:24_

When configuring `flake8-quotes` together with the formatter, it seems strange to specify the type of quotes you use twice:

```toml
[tool.ruff.format]
quote-style = "single"

[tool.ruff.lint.flake8-quotes]
inline-quotes = "single"
```

I propose using `quote-style` from `[tool.ruff.format]` as a default value for `inline-quotes` from `[tool.ruff.lint.flake8-quotes]`.

If others agree, I can send a PR.

---

_Label `configuration` added by @AlexWaygood on 2024-12-15 18:31_

---

_Comment by @MichaReiser on 2024-12-15 18:57_

Thanks for the proposal. 

We've so far been hesitant to add configuration inheritance from the formatter to the linter section but I can see how having to specify the value twice is annoying. I also think that inheriting formatter settings in the linter is better than the other way round. 

What's unclear to me is how to handle the `preserve` option because there's no equivalent for flake8-quotes.

---
