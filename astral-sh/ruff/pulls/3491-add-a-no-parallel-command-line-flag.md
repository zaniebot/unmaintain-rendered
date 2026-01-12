```yaml
number: 3491
title: "Add a `--no-parallel` command-line flag"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/no-parallel
created_at: 2023-03-13T20:32:07Z
updated_at: 2023-03-13T20:49:42Z
url: https://github.com/astral-sh/ruff/pull/3491
synced_at: 2026-01-12T15:55:13Z
```

# Add a `--no-parallel` command-line flag

---

_@charliermarsh_

## Summary

Right now, it's tough to debug panics, since we always run in parallel, and we don't show users the checked files as we go (and we don't catch-and-report panics). You can turn off parallelism with the `RAYON_NUM_THREADS=1` environment variable, but this PR adds a CLI argument for convenience. It's not exposed in `--help`, since I see this as an advanced flag for debugging only, but I could be convinced to expose it...

Closes: #3423.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-13 20:32_

---

_Review requested from @konstin by @charliermarsh on 2023-03-13 20:32_

---

_Comment by @MichaReiser on 2023-03-13 20:41_

Is this a common ask of users that the rayon env variable isn't sufficient? I hope ruff users don't need to fix panics 😅

If they do, then catching panics on a per file level and logging the filename in the diagnostic could be more approachable than having a hidden option and asking users to manually go through logs

I would prefer a more generic option, e.g a debug option, that changes ruff's behavior overall to help pinpoint issues 

---

_Comment by @github-actions[bot] on 2023-03-13 20:44_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Comment by @charliermarsh on 2023-03-13 20:49_

Mmm, that's fair! I'll close this out.

---

_Closed by @charliermarsh on 2023-03-13 20:49_

---
