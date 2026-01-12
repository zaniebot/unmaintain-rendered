```yaml
number: 13179
title: Enrich messages of SARIF results
type: issue
state: closed
author: RussellLuo
labels: []
assignees: []
created_at: 2024-08-31T04:37:18Z
updated_at: 2024-09-01T11:13:23Z
url: https://github.com/astral-sh/ruff/issues/13179
synced_at: 2026-01-12T15:54:52Z
```

# Enrich messages of SARIF results

---

_@RussellLuo_

Given the following code snippet:

```python
# test.py
import os

def greet():
    print('hello {name}'.format())
```

This is the default output:

```bash
$ ruff check test.py
test.py:1:8: F401 [*] `os` imported but unused
  |
1 | import os
  |        ^^ F401
2 | 
3 | def greet():
  |
  = help: Remove unused import: `os`

test.py:4:11: F524 `.format` call is missing argument(s) for placeholder(s): name
  |
3 | def greet():
4 |     print('hello {name}'.format())
  |           ^^^^^^^^^^^^^^^^^^^^^^^ F524
  |

Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

If we try to get the SARIF output:

```bash
ruff check --output-format=sarif test.py
```
As shown below, the result messages (e.g. `UnusedImport` and `StringDotFormatMissingArguments`) are too short to be informative:

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
            "text": "UnusedImport"
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
            "text": "StringDotFormatMissingArguments"
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

Also per [the SARIF example](https://github.com/microsoft/sarif-tutorials/blob/main/docs/1-Introduction.md#a-simple-example), the message is as informative as Ruff's default (non-SARIF) one:

```json
"message": {
  "text": "'x' is assigned a value but never used."
}
```


---

_Closed by @MichaReiser on 2024-09-01 11:13_

---
