```yaml
number: 13812
title: "Add support for `# fmt: nowrap` and `# fmt: wrap` comments"
type: issue
state: closed
author: ngnpope
labels: []
assignees: []
created_at: 2024-10-18T13:33:36Z
updated_at: 2024-10-18T14:12:14Z
url: https://github.com/astral-sh/ruff/issues/13812
synced_at: 2026-01-10T11:09:55Z
```

# Add support for `# fmt: nowrap` and `# fmt: wrap` comments

---

_Issue opened by @ngnpope on 2024-10-18 13:33_

Would it be possible to introduce a pair of formatting suppression comments that only disable wrapping?

e.g. I might have a mapping with some long lists in it:

```python
MY_MAPPING = {
    "Item 1": ["this", "is", "a", "very", "very", "very", "very", "very", "very", "very", "very", "very", "long", "list"],
    "Item 2": ["this", "is", "another", "very", "very", "very", "very", "very", "very", "very", "very", "very", "long", "list"],
    ...
}
```

Running the formatter over this will produce:

```python
MY_MAPPING = {
    "Item 1": [
        "this",
        "is",
        "a",
        "very",
        "very",
        "very",
        "very",
        "very",
        "very",
        "very",
        "very",
        "very",
        "long",
        "list",
    ],
    "Item 2": [
        "this",
        "is",
        "another",
        "very",
        "very",
        "very",
        "very",
        "very",
        "very",
        "very",
        "very",
        "very",
        "long",
        "list",
    ],
    ...
}
```

I can do the following:

```python
# fmt: off
MY_MAPPING = {
    "Item 1": ["this", "is", "a", "very", "very", "very", "very", "very", "very", "very", "very", "very", "long", "list"],
    "Item 2": ["this", "is", "another", "very", "very", "very", "very", "very", "very", "very", "very", "very", "long", "list"],
    ...
}
# fmt: on
```

But that disables the formatter entirely. It would be nice to have something like the following to only disable wrapping and allow the formatter to ignore line length and fix other whitespace, quotes, etc.

```python
# fmt: nowrap
MY_MAPPING = {
    "Item 1": ["this", "is", "a", "very", "very", "very", "very", "very", "very", "very", "very", "very", "long", "list"],
    "Item 2": ["this", "is", "another", "very", "very", "very", "very", "very", "very", "very", "very", "very", "long", "list"],
    ...
}
# fmt: wrap
```

Am also open to spelling this in other ways, e.g. `# fmt: disable-line-wrapping` and `# fmt: enable-line-wrapping`.

---

_Comment by @MichaReiser on 2024-10-18 14:11_

Thanks for raising this idea.

Disabling wrapping, unfortunately, isn't that easy because you want to preserve the line breaks exactly as is. That would require ruff to check before every token if there's a line break in the source and insert if there is. That would lead to very hard to maintain code and close to impossible to ensure we don't miss one token. 

My take on array formatting is still that we should find a better layout for long arrays rather than investing time in working around them. I just need to find time to work on it. 

I'll close this because implementing it would require tremendous effort that outweigh the benefit of the feature.



---

_Closed by @MichaReiser on 2024-10-18 14:11_

---
