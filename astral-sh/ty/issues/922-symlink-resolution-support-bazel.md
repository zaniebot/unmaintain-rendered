```yaml
number: 922
title: "Symlink resolution support (`bazel`)"
type: issue
state: closed
author: nroman-openai
labels: []
assignees: []
created_at: 2025-07-31T21:46:32Z
updated_at: 2025-08-01T23:24:55Z
url: https://github.com/astral-sh/ty/issues/922
synced_at: 2026-01-12T15:54:24Z
```

# Symlink resolution support (`bazel`)

---

_@nroman-openai_

I was trying to get `ty check` running under `bazel test` but I ran into an issue where it could not locate the source files because the bazel execution sandbox uses a symlink forest. The walk in `crates/ty_project/src/walk.rs` ignores symlinks completely.

I was able to hack up a change small change that includes the symlink path if it canonicalizes to a file. Everything seemed to work well after that.

I'd be happy to send a PR. Thoughts? 

---

_Comment by @varunchopra on 2025-07-31 21:49_

Please do share a PR, it can't hurt. I'm using Bazel as well and I suspect there are more.

---

_Comment by @nroman-openai on 2025-07-31 22:19_

Draft PR: https://github.com/astral-sh/ruff/pull/19674

---

_Comment by @varunchopra on 2025-08-01 23:23_

Lovely, thank you!

---

_Comment by @charliermarsh on 2025-08-01 23:24_

Optimistically closing since https://github.com/astral-sh/ruff/pull/19674 merged. Thanks for the contribution!

---

_Closed by @charliermarsh on 2025-08-01 23:24_

---
