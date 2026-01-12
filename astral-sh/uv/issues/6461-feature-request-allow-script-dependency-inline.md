```yaml
number: 6461
title: "Feature request: Allow script dependency inline with file docstring"
type: issue
state: closed
author: aaronsteers
labels:
  - question
assignees: []
created_at: 2024-08-22T20:11:56Z
updated_at: 2024-08-22T21:39:15Z
url: https://github.com/astral-sh/uv/issues/6461
synced_at: 2026-01-12T15:59:04Z
```

# Feature request: Allow script dependency inline with file docstring

---

_@aaronsteers_

I am thrilled about `uv` and have been testing it with custom scripts.

One small ergonomics issue I ran into is the competing placement of a file-level docstring with the inline metadata. I think it could be cleaner to allow `///script` elements to exist inside the file-level docstring.

Today I have this:

```py
"""Simple script to download secrets from GCS.

Usage:
    pipx install uv
    uv run scripts/fetch_test_secrets.py
"""
# Inline dependency metadata for `uv`:
# /// script
# requires-python = "==3.10"
# dependencies = [
#     "airbyte",  # PyAirbyte
# ]
# ///

from __future__ import annotations

...
```

But I would love to be collapse the dependency declarations into the docstring like this:

```py
"""Simple script to download secrets from GCS.

Usage:
    pipx install uv
    uv run scripts/fetch_test_secrets.py

Inline dependency metadata for `uv`:

/// script
requires-python = "==3.10"
dependencies = [
    "airbyte",  # PyAirbyte
]
///
"""

from __future__ import annotations

...
```

Or using a markdown triple-backtick block like this:

````py
"""Simple script to download secrets from GCS.

Usage:
    pipx install uv
    uv run scripts/fetch_test_secrets.py

Inline dependency metadata for `uv`:

```toml-uv-script
requires-python = "==3.10"
dependencies = [
    "airbyte",  # PyAirbyte
]
```
"""

from __future__ import annotations

...
````


Thanks for considering! And thanks for all your work on this project!

---

_Comment by @charliermarsh on 2024-08-22 20:41_

Thanks!

The use of `# /// script` is actually codified in a standard ([PEP 723](https://peps.python.org/pep-0723/)), so we don't have control over the structure in that way, since it needs to be interoperable with other tools (e.g., maybe you're using Ruff, and want to put your Ruff configuration for the file in that block). 


---

_Comment by @aaronsteers on 2024-08-22 21:29_

@charliermarsh - Thanks for your reply. After reviewing PEP 723 more closely (I only glanced at it previously), I see what you mean.

A compromise I found below is a middle-ground that I find cleaner (at least slightly) - and my testing confirmed it still works! Feels weird to have comments inside a docstring block, but it doesn't violate the PEP and `uv` handles is just fine. 🙌 

```py
"""Simple script to download secrets from GCS.

Usage:
    pipx install uv
    uv run scripts/fetch_test_secrets.py

Inline dependency metadata for `uv`:

# /// script
# requires-python = "==3.10"
# dependencies = [
#     "airbyte",  # PyAirbyte
# ]
# ///

"""
```

---

_Comment by @aaronsteers on 2024-08-22 21:30_

Thanks again for your help - closing as resolved.

---

_Closed by @aaronsteers on 2024-08-22 21:30_

---

_Comment by @charliermarsh on 2024-08-22 21:30_

Oh, that's true! Thanks for following up.

---

_Label `question` added by @zanieb on 2024-08-22 21:39_

---
