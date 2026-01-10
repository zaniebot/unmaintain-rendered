```yaml
number: 16114
title: Check for backtick-quoted shortcut links in CI
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ci
assignees: []
merged: true
base: main
head: docs-backticked-shortcut-links
created_at: 2025-02-12T11:05:15Z
updated_at: 2025-02-14T08:23:54Z
url: https://github.com/astral-sh/ruff/pull/16114
synced_at: 2026-01-10T19:57:22Z
```

# Check for backtick-quoted shortcut links in CI

---

_Pull request opened by @InSyncWithFoo on 2025-02-12 11:05_

## Summary

Follow-up to #16035.

`check_docs_formatted.py` will now report backtick-quoted shortcut links in rule documentation. It uses a regular expression to find them. Such a link:

* Starts with `[`, followed by <code>\`</code>, then a "name" sequence of at least one non-backtick non-newline character, followed by another <code>\`</code>, then ends with `]`.
* Is not followed by either a `[` or a `(`.
* Is not placed within a code block.

If the name is a known Ruff option name, that link is not considered a violation.

## Test Plan

Manual.


---

_Comment by @github-actions[bot] on 2025-02-12 11:17_

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

_Label `ci` added by @AlexWaygood on 2025-02-12 12:28_

---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:20 on 2025-02-13 08:37_

Can you explain what this change is about?

---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:26 on 2025-02-13 08:38_

I'm not sure what this comment is telling me and it's also a rather long article. It would be good to write a short summary here of what trick you apply here. You can still link to the website for the curious once.

---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:254 on 2025-02-13 08:42_

I find the triple ` somewhat confusing because it wasn't clear to me if it actually matches three backticks or not. I'd probably use regualr quotes here or a colon
```suggestion
    """Check for links of the form: [`foobar`].
```



---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:361 on 2025-02-13 08:42_

```suggestion
        print("Do not use backticked shortcut links of the form: [`foobar`].")
```

---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:34 on 2025-02-13 08:43_

Maybe that's the trick. It might be worth explaining why we include the snipped regex here (I see we skip it below)

---

_@MichaReiser approved on 2025-02-13 08:44_

Thanks, this is great. I only have a few suggestions around commenting the code.

---

_@InSyncWithFoo reviewed on 2025-02-13 14:09_

---

_Review comment by @InSyncWithFoo on `scripts/check_docs_formatted.py`:20 on 2025-02-13 14:09_

This change was necessary because `SNIPPED_RE` is now part of `BACKTICKED_SHORTCUT_LINK_RE`, which is compiled using `(?x)`: `*` would be target-less otherwise, as whitespace are ignored.

---

_@InSyncWithFoo reviewed on 2025-02-13 14:12_

---

_Review comment by @InSyncWithFoo on `scripts/check_docs_formatted.py`:254 on 2025-02-13 14:12_

Changed, but this is supposed to be what docstring conventions are for.

---

_@MichaReiser reviewed on 2025-02-13 14:30_

---

_Review comment by @MichaReiser on `scripts/check_docs_formatted.py`:254 on 2025-02-13 14:30_

Can you say more about what docstring conventions you mean? It might be that this is a convention that I'm not familiar with

---

_@InSyncWithFoo reviewed on 2025-02-13 15:00_

---

_Review comment by @InSyncWithFoo on `scripts/check_docs_formatted.py`:254 on 2025-02-13 15:00_

For example, here's how I would rewrite the docstring in pseudo-ReST:

```rest
"""Check for links of the form ``[`foobar`]``.

See explanation at :ref:`#16010`.
"""
```

Note the two pairs of backticks. That's how ReST recognizes text meant to represent code. If the convention is clear, then the meaning will also be clear.

---

_Comment by @InSyncWithFoo on 2025-02-13 15:04_

With this merged, I will be able to rest easy and monkeypatch the rest of the broken links client-side (not that I think I should, though) as they will be guaranteed to be option links.

---

_Merged by @MichaReiser on 2025-02-14 07:37_

---

_Closed by @MichaReiser on 2025-02-14 07:37_

---

_Branch deleted on 2025-02-14 08:23_

---
