```yaml
number: 21361
title: Rebuild ruff binary instead of sharing it across jobs
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/stop-sharing-ruff-binary-across-jobs
created_at: 2025-11-10T10:24:24Z
updated_at: 2025-11-10T13:27:09Z
url: https://github.com/astral-sh/ruff/pull/21361
synced_at: 2026-01-12T15:57:21Z
```

# Rebuild ruff binary instead of sharing it across jobs

---

_@MichaReiser_

## Summary

Similar to ty's ecosystem check, build ruff from source during the ecosytem check intsead of downloading the binary from the `test` job.

Building ruff from source is relatively fast and waiting for the test job before starting the ecosystem check also only delays it unnecessarily and has the downside that the job fails if `main` hasn't uploaded the `ruff` binary yet (because the test job is still running or failed).

This also simplifies the CI pipeline.

---

_Label `ci` added by @MichaReiser on 2025-11-10 10:24_

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 10:35_


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

_Marked ready for review by @MichaReiser on 2025-11-10 12:05_

---

_Converted to draft by @MichaReiser on 2025-11-10 12:15_

---

_Marked ready for review by @MichaReiser on 2025-11-10 12:27_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:484 on 2025-11-10 12:32_

if you run the script without the `--test-executable` flag set, it defaults to building a binary itself using `cargo build --profile=profiling`. So you could possibly simplify this job even more by omitting this flag, and not even building Ruff as a separate step.

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:534 on 2025-11-10 12:33_

Is this checkout for the baseline version? For mypy_primer, the typing conformance suite diff, and the ty fuzzer we checkout the baseline version using this logic:

https://github.com/astral-sh/ruff/blob/ada3208de50d3dce3aacb393ea23b35416bed593/.github/workflows/ci.yaml#L660-L661

Not sure if there's any difference between the two, but I have a lot of confidence in the logic we're using elsewhere because we've been using it for a while and I haven't observed any problems with it 😄

---

_@AlexWaygood approved on 2025-11-10 12:38_

---

_@MichaReiser reviewed on 2025-11-10 12:53_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:534 on 2025-11-10 12:53_

Possibly, The idea was to migrate the workflow while keeping the same semantics. However, using `MERGE_BASE` would fix the issue where someone merges changes to main when your PR isn't updated yet

---

_@MichaReiser reviewed on 2025-11-10 12:54_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:534 on 2025-11-10 12:54_

I'll leave this for now

---

_@AlexWaygood reviewed on 2025-11-10 12:55_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:470 on 2025-11-10 12:55_

now that the script is doing the build itself (and using `--profile=profiling` internally), the debug build won't be a good cache key anymore

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:470 on 2025-11-10 12:59_

Yeah, I just wanted to say that I revert this change as it also switches to profiling builds

---

_@MichaReiser reviewed on 2025-11-10 12:59_

---

_@AlexWaygood reviewed on 2025-11-10 13:00_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:470 on 2025-11-10 13:00_

sounds good

---

_@MichaReiser reviewed on 2025-11-10 13:00_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:484 on 2025-11-10 13:00_

I won't make this change because it also results in switching from debug to profiling builds. The idea of this PR is to not change behavior other than switching to freshly built ruff binaries over downloading them

---

_Comment by @AlexWaygood on 2025-11-10 13:01_

Cool that the ecosystem check now completes before some of the tests 😃

---

_Merged by @MichaReiser on 2025-11-10 13:27_

---

_Closed by @MichaReiser on 2025-11-10 13:27_

---

_Branch deleted on 2025-11-10 13:27_

---
