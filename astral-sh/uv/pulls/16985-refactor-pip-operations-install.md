```yaml
number: 16985
title: "Refactor `pip::operations::install`"
type: pull_request
state: closed
author: EliteTK
labels:
  - internal
assignees: []
draft: true
base: main
head: tk/split-install-plan
created_at: 2025-12-04T19:11:25Z
updated_at: 2025-12-12T21:08:06Z
url: https://github.com/astral-sh/uv/pull/16985
synced_at: 2026-01-10T05:49:14Z
```

# Refactor `pip::operations::install`

---

_Pull request opened by @EliteTK on 2025-12-04 19:11_

## Summary

Move the planning stage to a separate function.

Also a small refactor of `report_dry_run`.

I don't think this is necessarily worth it. I think a better approach is just to return the plan from install either always or when dry running (currently it returns an empty change-log instead).

## Test Plan

Test suite.

---

_Review requested from @zanieb by @EliteTK on 2025-12-04 19:11_

---

_Label `internal` added by @EliteTK on 2025-12-04 19:11_

---

_Comment by @zanieb on 2025-12-04 20:06_

Regarding whether it's worth it... I think returning a different type (e.g., using `Either`) based on `DryRun` feels like an anti-pattern? I'm sure why it'd be better to always return `Plan` and `Changelog` when the `DryRun::is_enabled()` case just returns an empty `Changelog::default()` immediately? It feels like `install` just shouldn't be called when dry-run is enabled.

---

_Comment by @EliteTK on 2025-12-05 14:30_

> Regarding whether it's worth it... I think returning a different type (e.g., using `Either`) based on `DryRun` feels like an anti-pattern?

After sleeping on it, I think I agree but I also think that the alternative of making every install call site require an identical preparation step seems sub-optimal.

Also, the `Plan` seems more like an implementation detail of the install function. The `Resolution` seems more like the `Plan` and in that sense I don't think it necessarily would be bad to have a function like `install` and a function like `dry_run_install` and they both take a `Resolution` among other things and return a changelog of some sort. If we go down the route of splitting them, I think that might be the most sensible approach.

But, regardless of the option, I think fundamentally, Changelog seems like something which could be returned from either function. I will explain more in #16981.

> It feels like install just shouldn't be called when dry-run is enabled.

I mean, there is this way to look at it: the command that initiates this is `uv sync --dry-run` and not `uv dry-run-sync` so I feel like it's not entirely unusual for install to take a dry-run parameter.

I agree that it not returning a changelog in the dry-run case is weird, but I will explain more in the other PR.

---

_Comment by @EliteTK on 2025-12-12 21:07_

Superseded by #17039.

---

_Closed by @EliteTK on 2025-12-12 21:07_

---

_Branch deleted on 2025-12-12 21:08_

---
