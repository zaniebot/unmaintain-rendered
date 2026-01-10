```yaml
number: 2318
title: Literal autocomplete
type: issue
state: closed
author: benomahony
labels:
  - server
  - completions
assignees: []
created_at: 2026-01-03T18:29:17Z
updated_at: 2026-01-03T18:33:36Z
url: https://github.com/astral-sh/ty/issues/2318
synced_at: 2026-01-10T01:56:41Z
```

# Literal autocomplete

---

_Issue opened by @benomahony on 2026-01-03 18:29_

ty doesn't provide autocomplete for Literal type alias values when typing inside strings. This works fine in basedpyright/pyright.

## Reproduction
```python
from pydantic_ai.models import KnownModelName

model_name: KnownModelName = ""  # cursor here, between the quotes
```

When I type between the quotes or trigger completion, I'd expect to see suggestions like:
- `"anthropic:claude-3-5-haiku-latest"`
- `"openai:gpt-5"`
- etc.

Instead, no literal suggestions appear - just generic string completions.

The same code works in basedpyright, which shows all the literal values from the type alias.

## Context

`KnownModelName` is defined as a Literal type alias in pydantic-ai:
```python
KnownModelName = Literal[
    "openai/gpt-4o",
    "anthropic/claude-sonnet-4-5-20250929",
    # ... bunch more model names
]
```

This pattern is pretty common for libraries that want type safety + IDE autocomplete for string constants.

## Environment

- ty 0.0.8
- Neovim with LazyVim
- Python 3.14

Pyright added this feature in response to microsoft/pyright#335 if that's helpful.

---

_Label `completions` added by @AlexWaygood on 2026-01-03 18:31_

---

_Label `server` added by @AlexWaygood on 2026-01-03 18:31_

---

_Comment by @AlexWaygood on 2026-01-03 18:33_

Thanks! I think this is https://github.com/astral-sh/ty/issues/938

---

_Closed by @AlexWaygood on 2026-01-03 18:33_

---
