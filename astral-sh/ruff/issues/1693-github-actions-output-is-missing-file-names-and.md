---
number: 1693
title: GitHub Actions output is missing file names and line numbers
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-01-06T15:03:41Z
updated_at: 2023-01-06T21:09:45Z
url: https://github.com/astral-sh/ruff/issues/1693
synced_at: 2026-01-10T01:22:39Z
---

# GitHub Actions output is missing file names and line numbers

---

_Issue opened by @charliermarsh on 2023-01-06 15:03_

See: https://github.com/pypa/hatch/pull/702#issuecomment-1373727049.

Here's an example: https://github.com/pypa/hatch/actions/runs/3853184547/jobs/6565955257.

The action comments appear in the right spot in the "Files changed" tabs, but should the locations also be visible in the logs?

---

_Referenced in [pypa/hatch#702](../../pypa/hatch/pulls/702.md) on 2023-01-06 15:03_

---

_Comment by @ofek on 2023-01-06 15:16_

I think it should also be visible in the logs because the errors are from a newly released version of Ruff rather than any files I actually changed

---

_Comment by @charliermarsh on 2023-01-06 15:20_

Are you suggesting that the line numbers and such were visible before, but are no longer visible?

---

_Comment by @ofek on 2023-01-06 15:24_

Yes definitely but I don't remember how long ago

---

_Comment by @charliermarsh on 2023-01-06 16:43_

Thanks, I'll look into it today.

---

_Comment by @charliermarsh on 2023-01-06 17:07_

Relevant: https://github.com/golangci/golangci-lint-action/issues/119

---

_Referenced in [astral-sh/ruff#1696](../../astral-sh/ruff/pulls/1696.md) on 2023-01-06 17:14_

---

_Comment by @charliermarsh on 2023-01-06 17:22_

I think this is "working as intended" from the perspective of GitHub's workflow commands.

Users can disable workflow commands (so they won't get inline comments, just build logs) with `--format text` or setting `GITHUB_ACTIONS=false` when invoking Ruff.

But the other thing we can do is add explicit additional text to the logs, even if it won't be picked up by GitHub, like:

<img width="884" alt="Screen Shot 2023-01-06 at 12 22 00 PM" src="https://user-images.githubusercontent.com/1309177/211064415-b4cc3df5-0ec3-48b8-b429-32f469a869ed.png">


---

_Comment by @ofek on 2023-01-06 17:25_

Yes that would be nice! Do you know why in that example output the first error was showing the file and line numbers and the rest weren't? Was the first error just malformed and was accidentally being printed?

---

_Comment by @charliermarsh on 2023-01-06 17:26_

I'm wondering that too. Not sure yet... Debugging it now.

---

_Comment by @charliermarsh on 2023-01-06 20:59_

Two fixes here:

- The GitHub reporting is now opt-in (`ruff --format=github .`) instead of auto-detecting via the `GITHUB_ACTIONS` env var.
- When running with `--format=github`, we now preserve the location information:

![Screen Shot 2023-01-06 at 3 08 06 PM](https://user-images.githubusercontent.com/1309177/211098952-40052821-9aed-4b96-9baa-9fbea44c4c4a.png)


---

_Closed by @charliermarsh on 2023-01-06 20:59_

---

_Comment by @ofek on 2023-01-06 21:09_

Thanks!

---
