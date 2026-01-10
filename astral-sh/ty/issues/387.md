```yaml
number: 387
title: Add a CI job that fails with a nice error message if generated files are not up to date
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - ci
assignees: []
created_at: 2025-05-14T15:44:56Z
updated_at: 2025-05-16T06:05:06Z
url: https://github.com/astral-sh/ty/issues/387
synced_at: 2026-01-10T02:34:09Z
```

# Add a CI job that fails with a nice error message if generated files are not up to date

---

_Issue opened by @AlexWaygood on 2025-05-14 15:44_

We should add a CI job that will fail on PRs like https://github.com/astral-sh/ty/pull/383 so that we can easily tell which PRs touch generated code (and therefore that we should not approve and merge). In this case I remembered that this might be generated code when I saw the PR but I'm worried I won't remember next time (the PR looked correct!). Something like what we have at https://github.com/astral-sh/ruff/blob/cf70c7863c2c011367d5fd673332f7fa56ef81ec/.github/workflows/ci.yaml#L499-L503 in Ruff.

We should also mark these files as generated in our `.gitattributes` file

---

_Label `help wanted` added by @AlexWaygood on 2025-05-14 15:44_

---

_Label `ci` added by @AlexWaygood on 2025-05-14 15:44_

---

_Closed by @MichaReiser on 2025-05-16 06:05_

---
