---
number: 12144
title: release 0.5.0 is different, was it intentional?
type: issue
state: closed
author: alexeagle
labels:
  - question
assignees: []
created_at: 2024-07-02T05:09:00Z
updated_at: 2024-07-02T16:40:57Z
url: https://github.com/astral-sh/ruff/issues/12144
synced_at: 2026-01-10T01:22:52Z
---

# release 0.5.0 is different, was it intentional?

---

_Issue opened by @alexeagle on 2024-07-02 05:09_

I see that https://github.com/astral-sh/ruff?tab=readme-ov-file#usage still references `v0.5.0` so I wonder if the following was inadventent:

- tag is `0.5.0` where all prior releases used a `v` prefix to denote "this tag is a version"
- the artifacts uploaded to the GitHub release no longer have the version number in the filename
- the tar files now contain an extra directory segment named `ruff-[platform]` and the `ruff` binary is nested in that directory.

---

_Comment by @dhruvmanila on 2024-07-02 05:33_

Relevant discussion: https://github.com/astral-sh/ruff/discussions/12078. But, to answer your question, yes it was intentional with the new release pipeline. It's mentioned in the [breaking changes section of the changelog](https://github.com/astral-sh/ruff/blob/main/CHANGELOG.md#breaking-changes) and the release notes. I'll quote it here:

> * The released archives now include an extra level of nesting, which can be removed with `--strip-components=1` when untarring.
> * The release artifact's file name no longer includes the version tag. This enables users to install via `/latest` URLs on GitHub.

---

> I see that [astral-sh/ruff#usage](https://github.com/astral-sh/ruff?tab=readme-ov-file#usage) still references `v0.5.0` so I wonder if the following was inadventent:

Do you mean the reference in the pre-commit config? If so, then that uses the [version with `v` prefix](https://github.com/astral-sh/ruff-pre-commit/releases/tag/v0.5.0). I agree that this is a bit confusing. Maybe we should keep the deprecate the `v` prefix in pre-commit version to maintain consistency. /cc @charliermarsh 




---

_Label `question` added by @dhruvmanila on 2024-07-02 05:33_

---

_Comment by @alexeagle on 2024-07-02 15:39_

ah, thank you, I went looking through the breaking changes section and failed to find it. Will adjust our downstream scripts that install the release into Bazel.

---

_Closed by @MichaReiser on 2024-07-02 16:39_

---

_Comment by @zanieb on 2024-07-02 16:40_

Let us know if you think there's something we can do to make things easier in the future (though I think these are unlikely to change again for a while!)

---
