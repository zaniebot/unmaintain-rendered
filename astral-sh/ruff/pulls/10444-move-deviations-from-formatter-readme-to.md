```yaml
number: 10444
title: Move deviations from formatter README to documentation
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: formatter-2024-deviations
created_at: 2024-03-18T08:15:22Z
updated_at: 2024-03-18T08:22:29Z
url: https://github.com/astral-sh/ruff/pull/10444
synced_at: 2026-01-10T22:47:02Z
```

# Move deviations from formatter README to documentation

---

_Pull request opened by @MichaReiser on 2024-03-18 08:15_

## Summary

#10151 documented the deviations between Ruff and Black with the new 2024 style guide in the `ruff-python-formatter/README.md`. However, that's not the documentation shown
on the website when navigating to [intentional deviations](https://docs.astral.sh/ruff/formatter/black/).

This PR streamlines the `ruff-python-formatter/README.md` and links to the documentation on the website instead of repeating the same content.
The PR also makes the 2024 style guide deviations available on the website documentation.

## Test Plan

I built the documentation locally and verified that the 2024 style guide known deviations are now shown on the website.



---

_Label `documentation` added by @MichaReiser on 2024-03-18 08:20_

---

_Merged by @MichaReiser on 2024-03-18 08:22_

---

_Closed by @MichaReiser on 2024-03-18 08:22_

---

_Branch deleted on 2024-03-18 08:22_

---
