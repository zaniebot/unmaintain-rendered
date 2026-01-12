```yaml
number: 20419
title: Disable flamegraph uploads for walltime benchmarks
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: revert-20409-renovate/codspeedhq-action-4.x
created_at: 2025-09-15T16:50:54Z
updated_at: 2025-09-16T09:15:37Z
url: https://github.com/astral-sh/ruff/pull/20419
synced_at: 2026-01-12T15:57:01Z
```

# Disable flamegraph uploads for walltime benchmarks

---

_@AlexWaygood_

~~Reverts astral-sh/ruff#20409~~

Prior to https://github.com/astral-sh/ruff/commit/25c13ea91c7535f14bda2d23595fa3ba311752fe, the walltime benchmarks CI job took around 10-11 minutes. It now seems to be taking around 17-18 minutes to complete, which is a pretty major slowdown and somewhat annoying if you're waiting for benchmark results on a PR! See e.g. https://github.com/astral-sh/ruff/actions/runs/17738649543/job/50411336184

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-09-15 16:50_

---

_Label `internal` added by @AlexWaygood on 2025-09-15 16:50_

---

_Comment by @github-actions[bot] on 2025-09-15 17:03_

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

_Comment by @silamon on 2025-09-16 06:28_

Maybe try running with environment variable `CODSPEED_PERF_ENABLED=false`, since it has been enabled by default, while it was not before. 

---

_@MichaReiser reviewed on 2025-09-16 06:31_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:938 on 2025-09-16 06:31_

Can we keep the mode lines, it makes upgrading easier (a rennovate PR rather than having to do the upgrade manually)

---

_Comment by @MichaReiser on 2025-09-16 06:32_

I pinged the codspeed folks. I'd suggest to maybe give them a few hours to fix this on their side.

---

_@MichaReiser approved on 2025-09-16 06:32_

---

_Comment by @MichaReiser on 2025-09-16 06:35_

I think @silamon is on to something. Codspeed now enables perf profiles for wallspeed benchmarks (you now see a flamegraph for walltime benchmarks too). I can't say just yet if they'll be useful or if our flamegraphs are too large but it's certainly nice to have more than just: It got slower or faster

---

_@MichaReiser requested changes on 2025-09-16 06:35_

We should try `CODSPEED_PERF_ENABLED=false` or accept the regression in return for now having flamegraphs

---

_Comment by @AlexWaygood on 2025-09-16 08:28_

https://codspeed.io/astral-sh/ruff/branches/alex%2Fsubtyping-between-protocols?runnerMode=WallTime has some flamegraphs for https://github.com/astral-sh/ruff/pull/20368#issuecomment-3292968246 but I don't find them useful personally. The regression in CI time is very significant, however; I'm finding it very frustrating waiting for CI to finish on #20368!

> We should try `CODSPEED_PERF_ENABLED=false`

I've pushed a commit trying this out

---

_Comment by @AlexWaygood on 2025-09-16 08:41_

Nice, we're back to the walltime benchmarks completing in 10-11 minutes. Though there are now a lot of lines like this in [the logs](https://github.com/astral-sh/ruff/actions/runs/17759661197/job/50469322114?pr=20419) that I don't know the meaning of:

```
  Failed to send benchmark URI to runner: FIFO does not exist: /tmp/runner.ctl.fifo
```

---

_Renamed from "Revert "Update CodSpeedHQ/action action to v4"" to "Disable flamegraph uploads for walltime benchmarks" by @AlexWaygood on 2025-09-16 08:41_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-09-16 08:42_

---

_@MichaReiser approved on 2025-09-16 08:59_

Thank you. I'm fine disabling this for now. It might be worth adding a comment explaining why we disable the perf uploads (because they would be nice)

---

_@AlexWaygood reviewed on 2025-09-16 09:02_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:975 on 2025-09-16 09:02_

```suggestion
          # enabling flamegraphs adds ~5 minutes to the CI time,
          # and they don't appear to provide much useful data for our benchmarks right now
          # (see https://github.com/astral-sh/ruff/pull/20419)
          CODSPEED_PERF_ENABLED: false
```

---

_Merged by @AlexWaygood on 2025-09-16 09:08_

---

_Closed by @AlexWaygood on 2025-09-16 09:08_

---

_Branch deleted on 2025-09-16 09:08_

---
