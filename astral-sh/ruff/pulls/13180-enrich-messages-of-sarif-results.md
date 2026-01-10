```yaml
number: 13180
title: Enrich messages of SARIF results
type: pull_request
state: merged
author: RussellLuo
labels:
  - cli
assignees: []
merged: true
base: main
head: enrich-sarif-message
created_at: 2024-08-31T04:40:42Z
updated_at: 2024-09-01T12:30:25Z
url: https://github.com/astral-sh/ruff/pull/13180
synced_at: 2026-01-10T21:38:32Z
```

# Enrich messages of SARIF results

---

_Pull request opened by @RussellLuo on 2024-08-31 04:40_

Closes #13179.

After this patch, we'll get more informative messages as below:

```json
{
  "$schema": "https://json.schemastore.org/sarif-2.1.0.json",
  "runs": [
    {
      "results": [
        {
          "level": "error",
          "locations": [
            {
              "physicalLocation": {
                "artifactLocation": {...},
                "region": {
                  "endColumn": 10,
                  "endLine": 1,
                  "startColumn": 8,
                  "startLine": 1
                }
              }
            }
          ],
          "message": {
            "text": "`os` imported but unused"
          },
          "ruleId": "F401"
        },
        {
          "level": "error",
          "locations": [
            {
              "physicalLocation": {
                "artifactLocation": {...},
                "region": {
                  "endColumn": 34,
                  "endLine": 4,
                  "startColumn": 11,
                  "startLine": 4
                }
              }
            }
          ],
          "message": {
            "text": "`.format` call is missing argument(s) for placeholder(s): name"
          },
          "ruleId": "F524"
        }
      ],
      "tool": {...}
    }
  ],
  "version": "2.1.0"
}
```

---

_Comment by @codspeed-hq[bot] on 2024-08-31 04:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/RussellLuo:enrich-sarif-message)

### Merging #13180 will **not alter performance**

<sub>Comparing <code>RussellLuo:enrich-sarif-message</code> (8c25f9c) with <code>main</code> (3ceedf7)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-31 04:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-09-01 11:13_

Thanks

---

_Label `cli` added by @MichaReiser on 2024-09-01 11:13_

---

_Merged by @MichaReiser on 2024-09-01 11:13_

---

_Closed by @MichaReiser on 2024-09-01 11:13_

---

_Branch deleted on 2024-09-01 12:30_

---
