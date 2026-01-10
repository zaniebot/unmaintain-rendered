```yaml
number: 2037
title: "Prefer declared type when conflicting with `Unknown` from an untyped lib"
type: issue
state: closed
author: NickCrews
labels: []
assignees: []
created_at: 2025-12-17T20:58:16Z
updated_at: 2025-12-17T21:05:46Z
url: https://github.com/astral-sh/ty/issues/2037
synced_at: 2026-01-10T01:53:59Z
```

# Prefer declared type when conflicting with `Unknown` from an untyped lib

---

_Issue opened by @NickCrews on 2025-12-17 20:58_

### Summary

This may be related to https://github.com/astral-sh/ty/issues/136, but this might be a sub-pattern that I think is more straightforward, whereas that issue looks a lot more difficult in the general case.

See this example. The `markdown` library's`Markdown` instance doesn't "look" to ty as though it has a `toc_tokens` attribute, but the ["toc" extension DOES add one](https://github.com/Python-Markdown/markdown/blob/89112c293f7b399ae8808f3a06306f46601e9684/markdown/extensions/toc.py#L414). So I want to tell ty to trust me, that this attribute exists.

So I tell it to `# type: ignore[attr-defined]`, and that solves the error, but it still infers the type as Unknown, which appears to trump my declared type.

```python
from typing import TypedDict

import markdown


class TocToken(TypedDict):
    level: int
    id: str
    name: str
    children: list[TocToken]


md = markdown.Markdown(extensions=["toc"])
md.convert("# Title\n\n## Section 1\n\nContent here.\n\n## Section 2\n\nMore content.")
# The "toc" extension monkeypatches in a "toc_tokens" attribute to the Markdown instance
# that follows the TocToken structure defined above.

toc_tokens_no_ignore: list[TocToken] = md.toc_tokens
# type inferred as Unknown, but I get the attr-defined error. So in the next version, I ignore it...
toc_tokens_ignore: list[TocToken] = md.toc_tokens  # type: ignore[attr-defined]
# still, type inferred as Unknown
```

If we are explicitly declaring a type where none exists (eg we are adding a `# type: ignore[attr-defined]`) , can we get ty to just trust us here? If we are declaring a type that is in conflict with an inferred type, then as #136 discusses, perhaps we need to be more careful.

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @carljm on 2025-12-17 21:05_

Thanks for the clearly presented report, and for searching for related issues! Yes, this is #136, and we do plan to make a fix there which will fix this case, so I'll close this as duplicate.

---

_Closed by @carljm on 2025-12-17 21:05_

---
