```yaml
number: 9632
title: Generate custom JSON schema for dynamic setting
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dyn
created_at: 2024-01-24T03:32:20Z
updated_at: 2024-01-24T04:31:00Z
url: https://github.com/astral-sh/ruff/pull/9632
synced_at: 2026-01-12T15:55:29Z
```

# Generate custom JSON schema for dynamic setting

---

_@charliermarsh_

## Summary

If you paste in the TOML for our default configuration (from the docs), it's rejected by our JSON Schema:

![Screenshot 2024-01-23 at 10 08 09 PM](https://github.com/astral-sh/ruff/assets/1309177/7b4ea6e8-07db-4590-bd1e-73a01a35d747)

It seems like the issue is with:

```toml
# Set the line length limit used when formatting code snippets in
# docstrings.
#
# This only has an effect when the `docstring-code-format` setting is
# enabled.
docstring-code-line-length = "dynamic"
```

Specifically, since that value uses a custom Serde implementation, I guess Schemars bails out? This PR adds a custom representation to allow `"dynamic"` (but no other strings):

![Screenshot 2024-01-23 at 10 27 21 PM](https://github.com/astral-sh/ruff/assets/1309177/ab7809d4-b077-44e9-8f98-ed893aaefe5d)

This seems like it should work but I don't have a great way to test it.

Closes https://github.com/astral-sh/ruff/issues/9630.

---

_Label `bug` added by @charliermarsh on 2024-01-24 03:32_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-24 03:32_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-01-24 03:32_

---

_Comment by @charliermarsh on 2024-01-24 03:34_

Oh actually, I was able to test it with https://www.jsonschemavalidator.net/.

Valid integer:

![Screenshot 2024-01-23 at 10 33 26 PM](https://github.com/astral-sh/ruff/assets/1309177/f54bf934-c44d-42d9-b26d-28bfad4394a4)

Valid string:

![Screenshot 2024-01-23 at 10 33 32 PM](https://github.com/astral-sh/ruff/assets/1309177/73bab2ea-f235-46c5-b022-985f389f5130)

Invalid string:

![Screenshot 2024-01-23 at 10 33 29 PM](https://github.com/astral-sh/ruff/assets/1309177/fa3e53aa-f814-405e-857b-c2061bf73ddc)


---

_@BurntSushi approved on 2024-01-24 03:47_

Thank you. :-)

---

_Comment by @charliermarsh on 2024-01-24 04:20_

No prob!

---

_Merged by @charliermarsh on 2024-01-24 04:26_

---

_Closed by @charliermarsh on 2024-01-24 04:26_

---

_Branch deleted on 2024-01-24 04:26_

---

_Comment by @github-actions[bot] on 2024-01-24 04:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
