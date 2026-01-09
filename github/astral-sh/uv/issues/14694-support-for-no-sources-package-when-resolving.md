---
number: 14694
title: Support for --no-sources-package when resolving
type: issue
state: open
author: Panacea729
labels:
  - question
assignees: []
created_at: 2025-07-17T21:39:05Z
updated_at: 2025-07-17T22:15:50Z
url: https://github.com/astral-sh/uv/issues/14694
synced_at: 2026-01-07T13:12:18-06:00
---

# Support for --no-sources-package when resolving

---

_Issue opened by @Panacea729 on 2025-07-17 21:39_

### Question

It seems as though most of the arguments that support being applied to every package also allow package specific arguments, e.x. (--upgrade-package, --reinstall-package, --no-build-package, ...) etc. It led me to believe that intuitively there would also be a `--no-sources-package` when doing `uv run`, `uv sync`, `uv lock`, etc...

It seems like there was some distinction between, "probably should apply to all packages" (i.e. --index), and, "could apply to specific packages", (e.x. `--upgrade-package`).

I'm not sure where that line was drawn nor does it seem to be discussed in the docs.

For me personally, a `--no-sources-package` argument would be useful (or even a --no-sources-group if using groups). I have a few different dependency groups that include workspace packages. In some of those groups the packages might exist locally in the workspace, and sometimes they won't. When they won't I want to sync with `--no-sources`, but now I have to split up my dependency groups into individual groups that would exist together at the same time and then use two `uv sync`s or create a bunch of `requirements.txt` files with specific directories and install those using pip.

If this sort of scenario was specifically excluded from workspaces (seeing as the lock file gets a bit wonky) then I don't really see a need and this can be closed. I will say that, in a setup supporting multiple different developer setups all possibly having access/containing different local dependencies in their workspace, this can be useful even though the lock file might differ from the one committed to VCS.

Alternatively, if there's any plans to support conditional sources based on environment variables or some other means (outside of environment markers), that would also address my issue. I know that there are issues out there to support environment variables in the pyproject.toml (like the hatch frontend, which satisfies my situation by allowing environment variables as a source as part of environments' (they don't support PEP 735) dependencies)

An issue that recently mentioned something similar is https://github.com/astral-sh/uv/issues/14600. Which intended to use different uv config files for different sources. This request was rejected because the intention is to keep sources in the same place as the project. Without conditional sources, in my case it requires splitting up the `uv sync` calls into two separate `uv sync` and `uv sync --no-sources`, but in other instances it could necessitate people to use the pip interface, which is not ideal.

Thank you.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @Panacea729 on 2025-07-17 21:39_

---

_Comment by @Panacea729 on 2025-07-17 22:15_

Oh I was completely unaware that you can differentiate sources by dependency group until I saw this comment: https://github.com/astral-sh/uv/pull/14197#issuecomment-2994205542

Is this somewhere in the docs? I was unable to find it anywhere. This seems to resolve my issue. I can have editable paths for one group and indexed for another group.

---
