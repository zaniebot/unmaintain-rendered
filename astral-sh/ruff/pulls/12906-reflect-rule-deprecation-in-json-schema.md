```yaml
number: 12906
title: Reflect rule deprecation in JSON schema
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
draft: true
base: main
head: reflect-rule-deprecation-in-json-schema
created_at: 2024-08-15T14:12:14Z
updated_at: 2024-08-19T10:05:20Z
url: https://github.com/astral-sh/ruff/pull/12906
synced_at: 2026-01-10T21:38:32Z
```

# Reflect rule deprecation in JSON schema

---

_Pull request opened by @MichaReiser on 2024-08-15 14:12_

## Summary

This PR updates our `RuleSelector` schema generator to reflect a rule's deprecation status in the schema. 



## Test Plan
Used Zed
```toml
# :schema /home/micha/astral/ruff/ruff.schema.json

preview = true
extend-include = ["*.ipynb"]

output-format = "text"

[format]
preview = true
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"

[lint]
select = ["ALL", "E99"]
```

But neither VS code nor Zed (Both use even better toml) use any specific formatting for deprecated rules :( So not sure if it is worth it.


---

_Review requested from @zanieb by @zanieb on 2024-08-15 14:14_

---

_Comment by @codspeed-hq[bot] on 2024-08-15 14:17_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/reflect-rule-deprecation-in-json-schema)

### Merging #12906 will **improve performances by 10.78%**

<sub>Comparing <code>reflect-rule-deprecation-in-json-schema</code> (2312dc5) with <code>main</code> (b9da316)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `reflect-rule-deprecation-in-json-schema` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 911.3 µs | 822.6 µs | +10.78% |


---

_Comment by @github-actions[bot] on 2024-08-15 14:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Reflect rule deprection in JSON schema" to "Reflect rule deprecation in JSON schema" by @AlexWaygood on 2024-08-15 14:30_

---

_Closed by @MichaReiser on 2024-08-19 10:05_

---
