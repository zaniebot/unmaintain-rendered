```yaml
number: 18380
title: "Rule Request: Enforce naming convention for internal vs. exported symbols"
type: issue
state: open
author: NeilGirdhar
labels:
  - rule
  - needs-decision
  - needs-design
assignees: []
created_at: 2025-05-30T01:54:34Z
updated_at: 2025-05-30T05:45:13Z
url: https://github.com/astral-sh/ruff/issues/18380
synced_at: 2026-01-12T15:54:56Z
```

# Rule Request: Enforce naming convention for internal vs. exported symbols

---

_@NeilGirdhar_

### Summary

Detect top-level variables, functions, and classes that are **not explicitly imported from other files** (i.e., not `from some_file import some_variable`) and enforce that such items are named with a **leading underscore**, indicating they are private to the module.

Conversely, detect symbols that **are exported** (intended for external use) and ensure they are **not** prefixed with an underscore.

---

### Rationale

A leading underscore is a widely understood Python convention that signals a name is internal to the file or module. Enforcing this:

* Improves code readability and clarity of intent.
* Makes it easier to detect dead code (unused internals).
* Reduces accidental usage of internal APIs across modules.
* Encourages consistent project structure and maintenance.

If a variable is internal (i.e., not used outside the file), it should be named with a leading underscore. If it's meant to be part of the file's public interface, it should be clearly named without the underscore.

---

### Implementation Notes

This check could likely be implemented in tandem with #872, which tracks usage of top-level bindings.

Classify symbols into four groups:

1. **Imported from another module** (possibly re-exported in `__init__.py`)
2. **Exported by this file** (e.g. part of the public API)
3. **Used only within this file**
4. **Unused (dead code)**

#### Recommendations:

* **Groups 1 & 2**: Use names **without** a leading underscore.
* **Group 3**: Use names **with** a leading underscore.
* **Group 4**: Should be deleted.

For non-library code (e.g., applications), group 2 may also be considered dead code unless explicitly required elsewhere.

---

_Renamed from "Detect unmarked private variables and classes" to "RULE: Detect unmarked private variables and classes" by @NeilGirdhar on 2025-05-30 01:55_

---

_Renamed from "RULE: Detect unmarked private variables and classes" to "Rule Request: Enforce naming convention for internal vs. exported symbols" by @NeilGirdhar on 2025-05-30 02:10_

---

_Label `rule` added by @MichaReiser on 2025-05-30 05:44_

---

_Label `needs-decision` added by @MichaReiser on 2025-05-30 05:44_

---

_Label `needs-design` added by @MichaReiser on 2025-05-30 05:45_

---
