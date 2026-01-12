```yaml
number: 14471
title: Improve the performance of the formatter instability check job
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/format-inst
created_at: 2024-11-20T00:53:47Z
updated_at: 2024-11-20T15:00:51Z
url: https://github.com/astral-sh/ruff/pull/14471
synced_at: 2026-01-12T15:55:47Z
```

# Improve the performance of the formatter instability check job

---

_@zanieb_

We should probably get rid of this entirely and subsume it's functionality in the normal ecosystem checks? I don't think we're using the black comparison tests anymore, but maybe someone wants it?

There are a few major parts to this:

1. Making the formatter script idempotent, so it can be run repeatedly and is robust to changing commits
2. Reducing the overhead of the git operations, minimizing the data transfer
3. Parallelizing all the git operations by repository

This reduces the setup time from 80s to 16s (locally).

The initial motivation for idempotency was to include the repositories in the GitHub Actions cache. I'm not sure it's worth it yet — they're about 1GB and would consume our limited cache space. Regardless, it improves correctness for local invocations.

The total runtime of the job is reduced from ~4m to ~3m.

I also made some cosmetic changes to the output paths and such.

---

_Label `ci` added by @zanieb on 2024-11-20 00:53_

---

_Renamed from "Improve the performance of the formatter instability checks" to "Improve the performance of the formatter instability check job" by @zanieb on 2024-11-20 01:08_

---

_Comment by @github-actions[bot] on 2024-11-20 01:17_

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

_Marked ready for review by @zanieb on 2024-11-20 04:33_

---

_Comment by @MichaReiser on 2024-11-20 07:09_

> We should probably get rid of this entirely and subsume it's functionality in the normal ecosystem checks? I don't think we're using the black comparison tests anymore, but maybe someone wants it?

I'm still using them because they're not only useful to assess black compatibility (which the ecosystem checks don't give us) but it also assigns a number to how compatible the stable and preview style are. The latter is mainly useful to understand: Is this a change happening very locally in a single file or does it touch every file in a repository. Something that I find difficult to assess from the ecosystem checks (because they truncate). 

I'm also not concerned about the performance of this job. It only runs on formatter PRs and they're rare. Every formatter diff has to wait for the ecosystem check results, which tend to be much slower than any other check.

---

_@MichaReiser approved on 2024-11-20 07:09_

> The initial motivation for idempotency was to include the repositories in the GitHub Actions cache. I'm not sure it's worth it yet — they're about 1GB and would consume our limited cache space. Regardless, it improves correctness for local invocations.

I don't think it's worth it. performance isn't a concern for this job and it runs to infrequently for the caches to even be useful.

---

_Comment by @zanieb on 2024-11-20 14:55_

Good to know you're still using it.

> I'm also not concerned about the performance of this job. It only runs on formatter PRs and they're rare. Every formatter diff has to wait for the ecosystem check results, which tend to be much slower than any other check.

Not anymore! They run in 3m now — I'm following up on this because it's one of the slowest remaining jobs.

---

_Merged by @zanieb on 2024-11-20 14:55_

---

_Closed by @zanieb on 2024-11-20 14:55_

---

_Branch deleted on 2024-11-20 14:55_

---

_Comment by @MichaReiser on 2024-11-20 15:00_

I'm mainly saying that it's probably not worth your time. At least, I never waited for that job.

---
