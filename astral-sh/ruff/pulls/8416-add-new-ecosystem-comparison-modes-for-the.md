```yaml
number: 8416
title: Add new ecosystem comparison modes for the formatter
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/eco-format-cmp
created_at: 2023-11-01T18:07:53Z
updated_at: 2023-11-02T01:20:54Z
url: https://github.com/astral-sh/ruff/pull/8416
synced_at: 2026-01-12T15:55:26Z
```

# Add new ecosystem comparison modes for the formatter

---

_@zanieb_

Previously, the ecosystem checks formatted with the baseline then formatted again with `--diff` to get the changed files.

Now, the ecosystem checks support a new mode where we:
- Format with the baseline
- Commit the changes
- Reset to the target ref
- Format again
- Check the diff from the baseline commit

This effectively tests Ruff changes on unformatted code rather than changes in previously formatted code (unless, of course, the project is already using Ruff).

While this mode is the new default, I've retained the old one for local checks. The mode can be toggled with `--format-comparison <type>`.

Includes some more aggressive resetting of the GitHub repositories when cached.

Here, I've also stubbed comparison modes in which `black` is used as the baseline. While these do nothing here, #8419 adds support.

I tested this with the commit from #8216 and ecosystem changes appear https://gist.github.com/zanieb/a982ec8c392939043613267474471a6e

---

_Marked ready for review by @zanieb on 2023-11-01 18:17_

---

_Label `internal` added by @zanieb on 2023-11-01 18:17_

---

_Review requested from @MichaReiser by @zanieb on 2023-11-01 18:38_

---

_Review requested from @konstin by @zanieb on 2023-11-01 18:38_

---

_Comment by @github-actions[bot] on 2023-11-01 19:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---

_Review comment by @MichaReiser on `python/ruff-ecosystem/ruff_ecosystem/format.py`:138 on 2023-11-02 00:57_

Oh that's cool

---

_Review comment by @MichaReiser on `python/ruff-ecosystem/ruff_ecosystem/format.py`:240 on 2023-11-02 00:57_

Python noob question: What's the reason for using inline comments here instead of docstrings?

---

_@MichaReiser approved on 2023-11-02 00:58_

Thank you. I love the different modes for running it locally

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/format.py`:240 on 2023-11-02 01:05_

I don't think there's a formalized way to document class attributes individually. Generally what you do is create a class docstring and then document the members using an 'Attributes' section, but that felt like more work than I wanted to do here 😬 

tldr a docstring for the class is proper but I was lazy. I wish we had tooling to generate the scaffold.

---

_@zanieb reviewed on 2023-11-02 01:05_

---

_@zanieb reviewed on 2023-11-02 01:09_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/format.py`:240 on 2023-11-02 01:09_

I even just wrote it out and it feels awkward; I guess I write Rust now

```python
class FormatComparison(Enum):
    """
    The type of format comparison to use.

    Attributes:
        ruff_then_ruff: Run Ruff baseline then Ruff comparison; checks for changes in
            behavior when formatting previously "formatted" code
        ruff_and_ruff: Run Ruff baseline then reset and run Ruff comparison; checks
            changes in behavior when formatting "unformatted" code
        black_then_ruff: Run Black baseline then Ruff comparison; checks for changes in
            behavior when formatting previously "formatted" code
        black_and_ruff: Run Black baseline then reset and run Ruff comparison; checks
            changes in behavior when formatting "unformatted" code
    """
```



---

_@zanieb reviewed on 2023-11-02 01:12_

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/format.py`:240 on 2023-11-02 01:12_

Some additional resources:
- https://peps.python.org/pep-0258/#attribute-docstrings
- https://github.com/microsoft/pylance-release/issues/1576

---

_Review comment by @zanieb on `python/ruff-ecosystem/ruff_ecosystem/format.py`:240 on 2023-11-02 01:14_

Since you nudged, I switched to individual attribute docstrings — while the PEP is rejected, VSCode supports it.

---

_@zanieb reviewed on 2023-11-02 01:14_

---

_Merged by @zanieb on 2023-11-02 01:20_

---

_Closed by @zanieb on 2023-11-02 01:20_

---

_Branch deleted on 2023-11-02 01:20_

---
