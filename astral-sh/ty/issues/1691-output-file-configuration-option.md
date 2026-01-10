---
number: 1691
title: Output file configuration option
type: issue
state: open
author: kieran-ryan
labels:
  - cli
  - needs-design
assignees: []
created_at: 2025-11-30T23:26:28Z
updated_at: 2025-12-29T12:57:28Z
url: https://github.com/astral-sh/ty/issues/1691
synced_at: 2026-01-10T01:51:14Z
---

# Output file configuration option

---

_Issue opened by @kieran-ryan on 2025-11-30 23:26_

Provide a `--output-file` configuration option - matching `ruff` - to specify file to output the linter output rather than the user handling through other means. Standardises configuration across Astral toolchains.

```yaml
Ty Check:
  script:
    - ty check --output-format=gitlab --output-file=code-quality-report.json
  artifacts:
    reports:
      codequality: $CI_PROJECT_DIR/code-quality-report.json
```

```yaml
Ruff Check:
  script:
    - ruff check --output-format=gitlab --output-file=code-quality-report.json
  artifacts:
    reports:
      codequality: $CI_PROJECT_DIR/code-quality-report.json
```

Related https://github.com/astral-sh/ruff/pull/21706.

---

_Label `cli` added by @MichaReiser on 2025-12-01 07:08_

---

_Label `needs-design` added by @MichaReiser on 2025-12-01 07:08_

---

_Comment by @MichaReiser on 2025-12-01 07:10_

For context. The `--output-file` was added in https://github.com/astral-sh/ruff/issues/2388, but it's not sufficient for the use case of most users who want multiple outputs in CI. 

---
