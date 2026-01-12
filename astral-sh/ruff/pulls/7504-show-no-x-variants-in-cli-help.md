```yaml
number: 7504
title: "Show `--no-X` variants in CLI help"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/help
created_at: 2023-09-19T02:36:13Z
updated_at: 2023-09-19T12:36:06Z
url: https://github.com/astral-sh/ruff/pull/7504
synced_at: 2026-01-12T15:55:24Z
```

# Show `--no-X` variants in CLI help

---

_@charliermarsh_

I'd really like this to render as `--preview / --no-preview`, but I looked for a while in the Clap internals and issue tracker (https://github.com/clap-rs/clap/issues/815) and I really can't figure out a way to do it -- this seems like the best we can do? It's also what they do in Orogene.

Closes https://github.com/astral-sh/ruff/issues/7486.

---

_Label `cli` added by @charliermarsh on 2023-09-19 02:36_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-19 02:36_

---

_Review requested from @konstin by @charliermarsh on 2023-09-19 02:36_

---

_@dhruvmanila approved on 2023-09-19 03:54_

Is it possible to override how the flag is being displayed with a custom text? If so, we could override the `--preview` with `--preview / --no-preview`. If not, then I think this is good for now.

---

_Comment by @github-actions[bot] on 2023-09-19 03:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-09-19 03:59_

@dhruvmanila - I wasn't able to figure it out. I agree that's a much better solution (this is what I was trying to say in the PR summary, though perhaps not very clearly). I haven't found any other Clap-dependent crates that were able to do this either.

---

_Review comment by @MichaReiser on `docs/configuration.md`:228 on 2023-09-19 06:01_

What's the motivation for moving Miscellaneous above rule and file selection?

---

_@MichaReiser approved on 2023-09-19 06:11_

---

_Comment by @MichaReiser on 2023-09-19 06:29_

Relevant clap issue https://github.com/clap-rs/clap/issues/815

---

_@konstin approved on 2023-09-19 07:58_

---

_@charliermarsh reviewed on 2023-09-19 12:15_

---

_Review comment by @charliermarsh on `docs/configuration.md`:228 on 2023-09-19 12:15_

Mistake.

---

_Merged by @charliermarsh on 2023-09-19 12:27_

---

_Closed by @charliermarsh on 2023-09-19 12:27_

---

_Branch deleted on 2023-09-19 12:27_

---
