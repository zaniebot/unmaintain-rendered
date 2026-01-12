```yaml
number: 15735
title: Add knot.toml schema
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/knot-schema
created_at: 2025-01-24T23:55:14Z
updated_at: 2025-02-07T09:59:43Z
url: https://github.com/astral-sh/ruff/pull/15735
synced_at: 2026-01-12T15:55:52Z
```

# Add knot.toml schema

---

_@MichaReiser_

## Summary

Adds a JSON schema generation step for Red Knot. This PR doesn't yet add a publishing step because it's still a bit early for that


## Test plan

I tested the schema in Zed, VS Code and PyCharm:

* PyCharm: You have to manually add a schema mapping (settings JSON Schema Mappings)
* Zed and VS code support the inline schema specification

```toml
#:schema /Users/micha/astral/ruff/knot.schema.json


[environment]
extra-paths = []


[rules]
call-possibly-unbound-method = "error"
unknown-rule = "error"

# duplicate-base = "error"
```

```json
{
    "$schema": "file:///Users/micha/astral/ruff/knot.schema.json",

    "environment": {
        "python-version": "3.13",
        "python-platform": "linux2"
    },

    "rules": {
        "unknown-rule": "error"
    }
}
```

https://github.com/user-attachments/assets/a18fcd96-7cbe-4110-985b-9f1935584411


The Schema overall works but all editors have their own quirks:

* PyCharm: Hovering a name always shows the section description instead of the description of the specific setting. But it's the same for other settings in `pyproject.toml` files 🤷 
* VS Code (JSON): Using the generated schema in a JSON file gives exactly the experience I want
* VS Code (TOML): 
  * Properties with multiple possible values are repeated during auto-completion without giving any hint how they're different. ![Screen Shot 2025-02-06 at 14 05 35 PM](https://github.com/user-attachments/assets/d7f3c2a9-2351-4226-9fc1-b91aa192a237)
  * The property description mushes together the description of the property and the value, which looks sort of ridiculous.  ![Screen Shot 2025-02-06 at 14 04 40 PM](https://github.com/user-attachments/assets/8b72f04a-c62a-49b5-810f-7ddd472884d0)
  * Autocompletion and documentation hovering works (except the limitations mentioned above)
* Zed:
	* Very similar to VS Code with the exception that it uses the description attribute to distinguish settings with multiple possible values ![Screen Shot 2025-02-06 at 14 08 19 PM](https://github.com/user-attachments/assets/78a7f849-ff4e-44ff-8317-708eaf02dc1f)


I don't think there's much we can do here other than hope (or help) editors improve their auto completion. The same short comings also apply to ruff, so this isn't something new. For now, I think this is good enough 

---

_Label `red-knot` added by @MichaReiser on 2025-01-24 23:57_

---

_Comment by @github-actions[bot] on 2025-01-25 00:11_

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

_@MichaReiser reviewed on 2025-02-06 13:10_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_knot_schema.rs`:15 on 2025-02-06 13:10_

This is mostly copied over from what we have for ruff. I don't think it's worth abstracting it in any form.

---

_@MichaReiser reviewed on 2025-02-06 13:10_

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/combine.rs`:9 on 2025-02-06 13:10_

I added support for struct with unnamed fields.

---

_Marked ready for review by @MichaReiser on 2025-02-06 13:11_

---

_Review requested from @carljm by @MichaReiser on 2025-02-06 13:11_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-06 13:11_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-06 13:11_

---

_@dhruvmanila approved on 2025-02-07 09:42_

Thank you for testing it out in various editors and providing video/screenshots of the same.

It's unfortunate that each editor has their own quirks and I wonder if it would be worthwhile to use editor specific schema fields (https://github.com/SchemaStore/schemastore/blob/master/CONTRIBUTING.md#language-server-features) and generate multiple schemas. This is not a priority and could be looked into in the future if we receive multiple complains or we decide that it would be useful for the editor to provide a fix and we work with them instead.

---

_Comment by @MichaReiser on 2025-02-07 09:59_

I like the idea with editor specific schemas but agree, that's polish we can do a bit later :)

---

_Merged by @MichaReiser on 2025-02-07 09:59_

---

_Closed by @MichaReiser on 2025-02-07 09:59_

---

_Branch deleted on 2025-02-07 09:59_

---
