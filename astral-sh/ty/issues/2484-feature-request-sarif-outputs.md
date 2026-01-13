```yaml
number: 2484
title: "Feature Request: SARIF Outputs"
type: issue
state: open
author: TimoVink
labels:
  - cli
  - diagnostics
assignees: []
created_at: 2026-01-13T22:49:33Z
updated_at: 2026-01-13T22:50:19Z
url: https://github.com/astral-sh/ty/issues/2484
synced_at: 2026-01-13T23:35:16Z
```

# Feature Request: SARIF Outputs

---

_@TimoVink_

## Summary
It would be nice if `ty` supported the [SARIF](https://sarifweb.azurewebsites.net/) output format.

## Use Case
Some CI tools like GitHub and Azure DevOps have support for rendering SARIF build artifacts, making it easy to surface findings.

## Notes
This is currently supported in `ruff` via 
```sh
ruff check --output-format sarif --output-file foo.sarif
ruff format --preview --output-format sarif > foo.sarif
```

---

_Label `cli` added by @AlexWaygood on 2026-01-13 22:50_

---

_Label `diagnostics` added by @AlexWaygood on 2026-01-13 22:50_

---
