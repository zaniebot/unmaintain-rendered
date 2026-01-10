```yaml
number: 22307
title: "[ty] Remove `TY_MAX_PARALLELISM` as conformance runs no longer panic"
type: pull_request
state: merged
author: sinon
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: remove-ty-max-parallelism
created_at: 2025-12-30T19:30:07Z
updated_at: 2025-12-30T19:42:59Z
url: https://github.com/astral-sh/ruff/pull/22307
synced_at: 2026-01-10T16:36:19Z
```

# [ty] Remove `TY_MAX_PARALLELISM` as conformance runs no longer panic

---

_Pull request opened by @sinon on 2025-12-30 19:30_

## Summary

The `# TODO` in workflow appears to be resolved running a local build of `main` with `TY_MAX_PARALLELISM` set/unset has the same behaviour (i.e no panics, same diagnostic count) except one is a bit faster ⚡ 


## Test Plan

```sh
TY_MAX_PARALLELISM=1 cargo run --bin ty check ~/dev/typing/conformance/ --output-format concise
...
Found 1324 diagnostics
```

```sh
cargo run --bin ty check ~/dev/typing/conformance/ --output-format concise
...
Found 1324 diagnostics
```

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2025-12-30 19:31_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @MichaReiser on 2025-12-30 19:42_

Thank you

---

_Label `ci` added by @MichaReiser on 2025-12-30 19:42_

---

_Label `ty` added by @MichaReiser on 2025-12-30 19:42_

---

_Merged by @MichaReiser on 2025-12-30 19:42_

---

_Closed by @MichaReiser on 2025-12-30 19:42_

---
