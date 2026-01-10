---
number: 19532
title: "`I` output differ with `isort` by a single blank line"
type: issue
state: closed
author: knguyen1
labels:
  - isort
assignees: []
created_at: 2025-07-24T15:40:05Z
updated_at: 2025-07-25T07:25:07Z
url: https://github.com/astral-sh/ruff/issues/19532
synced_at: 2026-01-10T01:23:00Z
---

# `I` output differ with `isort` by a single blank line

---

_Issue opened by @knguyen1 on 2025-07-24 15:40_

### Summary

Given this file: https://github.com/metabrainz/picard/blob/master/picard/script/__init__.py
`isort` is outputting NO blank line between `)` and the comment line `# Those imports are required to actually parse the code and interpret decorators`.

For `ruff`, see the blank line in this commit: https://github.com/metabrainz/picard/pull/2690/commits/efa2ceb62be95f9250b15e071a6b5ba58f3ce1da

My LLM did some investigations and gave up and concluded that it's due to "design philosophies".  I just wanted to confirm with an actual human.

Given these `isort` settings:

```
[tool.isort]
sections = [
    "FUTURE", "STDLIB", "FIRSTPARTY", "THIRDPARTY", "QT", "TEST", "PICARD", "LOCALFOLDER",
    "RESOURCES", "PICARD_UI"
]
default_section = "LOCALFOLDER"
known_first_party = []
known_picard = ["picard"]
known_picard_ui = ["picard.ui"]
known_qt = ["PyQt6"]
known_resources = ["picard.resources"]
known_test = ["test.*"]
known_third_party = ["dateutil", "fasteners", "mutagen", "yaml"]
skip_glob = ["picard/ui/forms/ui_*.py", "picard/resources.py"]
combine_as_imports = true
float_to_top = false
force_grid_wrap = 2
force_sort_within_sections = true
include_trailing_comma = true
indent = "    "
lines_after_imports = 2
multi_line_output = 3
order_by_type = true
use_parentheses = true
```

Given these `tool.ruff.lint.isort` settings:
```

[tool.ruff.lint.isort]
# Custom section order matching your isort sections
section-order = [
    "future",
    "standard-library",
    "first-party",
    "known-first-party",
    "third-party",
    "known-third-party",
    "qt",
    "test",
    "picard",
    "local-folder",
    "resources",
    "picard-ui"
]
default-section = "local-folder"

# Match other isort settings
force-sort-within-sections = true
combine-as-imports = true
lines-after-imports = 2
order-by-type = true
split-on-trailing-comma = true

# Custom section definitions
[tool.ruff.lint.isort.sections]
qt = ["PyQt6"]
test = ["test.*"]
picard = ["picard"]
resources = ["picard.resources"]
picard-ui = ["picard.ui"]

# Keep your existing known mappings for standard sections
known-first-party = []
known-third-party = ["dateutil", "fasteners", "mutagen", "yaml"]
```

Below is the LLM conclusion of the investigation.

# RooCode Output

I've successfully created a comprehensive investigation script and analyzed the root cause of the differences between `ruff check --fix` and `isort` import sorting behavior.

## Investigation Results

Using the [`investigate_import_sorting.py`](investigate_import_sorting.py:1) script, I've identified the key differences:

### Root Cause Analysis

The fundamental issue is that **ruff and isort have different philosophies about import organization and formatting**, despite similar configurations:

1. **Blank Line Handling**: 
   - **Ruff**: Maintains 2 blank lines between import groups, preserving original spacing
   - **Isort**: Reduces to 1 blank line, compacting the import section

2. **Import Formatting Style**:
   - **Ruff**: Preserves multi-line import formatting with proper indentation (32 lines total)
   - **Isort**: Compacts imports into fewer lines with different formatting (15 lines total)

3. **Import Order Logic**:
   - **Ruff**: Follows configured section order but maintains relative positioning
   - **Isort**: Reorders imports more aggressively, moving `import picard.script.functions` to the top

4. **Multi-line Import Handling**:
   - **Ruff**: Maintains original format with trailing commas and proper indentation
   - **Isort**: Compacts multi-line imports, changing from vertical to horizontal layout

### Configuration Attempts

I tested several configuration modifications in [`pyproject.toml`](pyproject.toml:203):
- `lines-between-types = 0` - No effect on blank lines between sections
- `no-lines-before = ["picard", "local-folder"]` - No effect
- `no-sections = true` - Still maintained different formatting

### Conclusion

The differences are **inherent to the tools' design philosophies** rather than configuration mismatches:

- **Ruff** prioritizes **preserving existing formatting** and being minimally invasive
- **Isort** prioritizes **aggressive optimization** and compacting imports

The blank line issue specifically occurs because ruff maintains spacing between logical import groups (regular imports vs. imports with comments), while isort removes most blank lines as part of its compacting strategy.

## Recommendation

For projects requiring consistent import formatting between tools, consider:
1. **Choose one tool** as the primary import sorter
2. **Configure pre-commit hooks** to use only the chosen tool
3. **Document the decision** for team consistency

The current configuration in [`pyproject.toml`](pyproject.toml:203) is as aligned as possible given the tools' fundamental differences.

### Version

ruff 0.12.4

---

_Renamed from "`uv` output differ by a single blank line" to "`uv` output differ with `isort` by a single blank line" by @knguyen1 on 2025-07-24 15:40_

---

_Renamed from "`uv` output differ with `isort` by a single blank line" to "`uv` rule `I` output differ with `isort` by a single blank line" by @knguyen1 on 2025-07-24 15:40_

---

_Renamed from "`uv` rule `I` output differ with `isort` by a single blank line" to "`I` output differ with `isort` by a single blank line" by @MichaReiser on 2025-07-24 15:51_

---

_Comment by @ntBre on 2025-07-24 16:07_

I can reproduce this with a small example and standard-library imports ([playground](https://play.ruff.rs/835a941b-5fa3-430c-9944-c6880429af16)):

```python
import abc
# comment
import bisect
import collections
```

I guess we're inserting a blank line before a comment, even if we're in the same section.

---

_Label `isort` added by @ntBre on 2025-07-24 16:08_

---

_Comment by @MichaReiser on 2025-07-24 16:09_

This might be due to ruff using the black-isort preset by default (just speculating)

---

_Comment by @ntBre on 2025-07-24 16:12_

Ah yep, with `--profile black` I get the same results from isort.

---

_Comment by @MichaReiser on 2025-07-24 16:14_

Nice. In that case I think this is working as intended. 

---

_Comment by @knguyen1 on 2025-07-24 16:18_

EDIT:  Got it.  `profile` on `isort`, not on `ruff`.  Thanks all!

Where do I specify that parameter?

```
uv run ruff check --fix --profile black .
error: unexpected argument '--profile' found
```

---

_Comment by @knguyen1 on 2025-07-24 16:29_

But..., then how do you make `ruff` match `isort`?  Impossible?
`isort --profile black` will match with `ruff`.

---

_Comment by @ntBre on 2025-07-24 16:53_

Yes, I think this isn't possible. I didn't see any `profile` configuration in our isort settings, and this is mentioned in our [isort FAQ section](https://docs.astral.sh/ruff/faq/#how-does-ruffs-import-sorting-compare-to-isort), so I think Ruff only matching `isort --profile black` is intentional, as Micha said.

---

_Closed by @MichaReiser on 2025-07-25 07:25_

---

_Referenced in [metabrainz/picard#2697](../../metabrainz/picard/pulls/2697.md) on 2025-08-05 22:03_

---
