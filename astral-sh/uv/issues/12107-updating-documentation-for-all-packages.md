---
number: 12107
title: "Updating Documentation for `--all-packages`"
type: issue
state: open
author: uhurusurfa
labels:
  - documentation
assignees: []
created_at: 2025-03-11T00:09:38Z
updated_at: 2025-09-03T12:12:27Z
url: https://github.com/astral-sh/uv/issues/12107
synced_at: 2026-01-10T01:25:15Z
---

# Updating Documentation for `--all-packages`

---

_Issue opened by @uhurusurfa on 2025-03-11 00:09_

The --all-packages was added in 0.4 but took me a while to figure out that it was available because it has not yet made it to the documentation so had to trawl the issues and eventually found it in this very long issue:
https://github.com/astral-sh/uv/issues/7143

Is a doxcumentation update currently in the pipeline for this feature or should I raise a PR with an initial attempt  to provide documentation on this feature so that others using the workspace feature won't need to go the same route as I did?

---

_Comment by @charliermarsh on 2025-03-12 23:06_

Did you consider looking in the CLI help? I think we could put something for `--package` and `--all-packages` in `sync.md` thoiugh @zanieb.

---

_Label `documentation` added by @charliermarsh on 2025-03-12 23:06_

---

_Comment by @uhurusurfa on 2025-03-13 01:46_

No I did not think to look in the command line cli though I should have - thanks.

---

_Renamed from "Updating Documentation for --all-packages" to "Updating Documentation for `--all-packages`" by @charliermarsh on 2025-03-15 00:59_

---

_Assigned to @zanieb by @zanieb on 2025-03-20 22:50_

---

_Comment by @zanieb on 2025-03-20 22:51_

If you want to add some docs there I'm happy to review it!

---

_Referenced in [astral-sh/uv#12390](../../astral-sh/uv/pulls/12390.md) on 2025-03-22 14:08_

---

_Comment by @xixixao on 2025-09-03 12:12_

What is --all-packages supposed to do when there are no workspace members specified? It seems to be installing the current project, even with


`uv sync --no-install-workspace --no-dev --no-editable --all-packages --frozen`

which is suprising.

Apologies for piling on, but the sync options are really need better documentation (in CLI, on the web). It's really hard to tell what each does with all the possible permutations.

---
