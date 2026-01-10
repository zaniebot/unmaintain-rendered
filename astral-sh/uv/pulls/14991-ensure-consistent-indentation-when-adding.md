```yaml
number: 14991
title: Ensure consistent indentation when adding dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ind
created_at: 2025-07-31T11:24:25Z
updated_at: 2025-07-31T11:50:06Z
url: https://github.com/astral-sh/uv/pull/14991
synced_at: 2026-01-10T06:53:02Z
```

# Ensure consistent indentation when adding dependencies

---

_Pull request opened by @charliermarsh on 2025-07-31 11:24_

## Summary

The basic problem here is that when we had multiple items in an inline array, and that array expanded to multiple lines, we accidentally changed the indentation part-way through due to how prefixes work in the TOML.

Here's Claude's explanation of the root cause, which I find pretty decent:

```
  Here's what happened step by step:

  1. First item ("iniconfig"): Has empty prefix "" → indentation_prefix stays None → uses default 4 spaces
  2. Second item ("ruff"): Has empty prefix "" → indentation_prefix stays None → uses default 4 spaces
  3. Third item ("typing-extensions"): Has prefix " " (single space from inline format) → indentation_prefix becomes
  Some(" ") → uses only 1 space!

  This produced:
  [dependency-groups]
  dev = [
      "iniconfig>=2.0.0",
      "ruff",
   "typing-extensions",  # ← Only 1 space instead of 4!
  ]

  Why the Third Item Had a Different Prefix

  In inline arrays like ["ruff", "typing-extensions"], the items are separated by commas and spaces. When parsed by
  the TOML library:
  - "ruff" has no prefix (it comes right after [)
  - "typing-extensions" has a single space prefix (the space after the comma)

  The Fix

  Moving the indentation calculation outside the loop ensures it's calculated only once:

  // Calculate indentation ONCE before the loop
  if let Some(first_item) = deps.iter().next() {
      let decor_prefix = /* get prefix from first item */
      indentation_prefix = (!decor_prefix.is_empty()).then_some(decor_prefix.to_string());
  }

  // Now use the same indentation for ALL items
  for item in deps.iter_mut() {
      // Apply consistent indentation to every item
  }

  This ensures all items get the same indentation (4 spaces by default when converting from inline arrays), producing
   the correct output:

  [dependency-groups]
  dev = [
      "iniconfig>=2.0.0",
      "ruff",
      "typing-extensions",  # ← Correct 4-space indentation
  ]

  The bug only affected arrays being converted from inline to multiline format, where different items might have
  different residual formatting from their inline representation.
```

Closes #14961.

---

_Label `bug` added by @charliermarsh on 2025-07-31 11:24_

---

_Marked ready for review by @charliermarsh on 2025-07-31 11:25_

---

_@zanieb approved on 2025-07-31 11:37_

---

_Merged by @zanieb on 2025-07-31 11:50_

---

_Closed by @zanieb on 2025-07-31 11:50_

---

_Branch deleted on 2025-07-31 11:50_

---
