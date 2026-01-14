```yaml
number: 2484
title: "Feature Request: SARIF Outputs"
type: issue
state: open
author: TimoVink
labels:
  - help wanted
  - cli
  - diagnostics
assignees: []
created_at: 2026-01-13T22:49:33Z
updated_at: 2026-01-14T08:56:54Z
url: https://github.com/astral-sh/ty/issues/2484
synced_at: 2026-01-14T09:34:52Z
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

_Comment by @MichaReiser on 2026-01-14 08:56_

Thank you. We already support the github output format but adding this for Azure dev ops makes sense. 


This requires porting https://github.com/astral-sh/ruff/blob/5b1d172906c143e3650d1fe0641a6ded45b6c62b/crates/ruff_linter/src/message/sarif.rs#L48 to `ruff_db` and generalizing it, so that it can be used by both Ruff and ty. Similar to what Brent did in https://github.com/astral-sh/ruff/pull/20117 and https://github.com/astral-sh/ruff/pull/20155

---

_Added to milestone `Stable` by @MichaReiser on 2026-01-14 08:56_

---

_Label `help wanted` added by @MichaReiser on 2026-01-14 08:56_

---
