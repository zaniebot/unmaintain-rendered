```yaml
number: 5899
title: "Show build and install summaries in `uv run` and `uv tool run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - tracing
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/show-more
created_at: 2024-08-08T00:36:05Z
updated_at: 2024-08-08T15:26:43Z
url: https://github.com/astral-sh/uv/pull/5899
synced_at: 2026-01-10T13:31:54Z
```

# Show build and install summaries in `uv run` and `uv tool run`

---

_Pull request opened by @charliermarsh on 2024-08-08 00:36_

## Summary

Initially, we showed _all_ resolver and installer output in `uv run` and `uv tool run`, since it was way too much for workhorse commands. Then, we moved to showing _no_ output by default, which was way too little -- you had no idea why anything was happening, and commands appeared to hang.

This PR adds a more nuanced middle-ground. With `--verbose`, we continue to show everything. But by default, in `uv run` and `uv tool run`...

- During resolution, we show any "Building" and "Build" messages, if you need to build a source distribution. But we don't show any other output. (This _could_ be too little for expensive resolutions; we may want to show a spinner.)
- If there are no changes to be made after resolving, we don't show any other output.
- If we have to install, we show the progress bars for downloads (which disappear on completion) followed by a single summary line stating the number of packages installed.

This feels pretty good, in my limited testing. When everything is built / cached, you don't get _any_ additional output. When there's work to do, you have a sense for what's happening, and we leave you with a single summary line ("Installed X packages") at the end.

Closes https://github.com/astral-sh/uv/issues/5758.

## Test Plan

Notice that the first `tool run` ends with an install line; the second shows no additional output:

![Screenshot 2024-08-07 at 8 33 21 PM](https://github.com/user-attachments/assets/6b0c7824-f14f-4d0b-86d0-a334ba486ce4)

If you run `uv run` in a package for the first time, we _do_ tell you that we're building / built it:

![Screenshot 2024-08-07 at 8 34 02 PM](https://github.com/user-attachments/assets/6a4c7b05-3aa5-410e-af5d-916eb6e745b0)

But on the second run, there's no output:

![Screenshot 2024-08-07 at 8 34 10 PM](https://github.com/user-attachments/assets/2b32f83e-0370-45fa-9e8f-407d9b93d468)

If you add a `--with`, we'll show you all the installer progress bars (which disappear once they're done), and then a single summary line:

![Screenshot 2024-08-07 at 8 34 39 PM](https://github.com/user-attachments/assets/e975d75c-01d2-4eb6-b3ff-e4d25d5ea9e2)


---

_Review requested from @zanieb by @charliermarsh on 2024-08-08 00:40_

---

_Review requested from @konstin by @charliermarsh on 2024-08-08 00:40_

---

_Marked ready for review by @charliermarsh on 2024-08-08 00:42_

---

_Label `tracing` added by @zanieb on 2024-08-08 02:33_

---

_Label `cli` added by @zanieb on 2024-08-08 02:33_

---

_Label `preview` added by @zanieb on 2024-08-08 02:33_

---

_@konstin approved on 2024-08-08 08:37_

Love it!

---

_Merged by @charliermarsh on 2024-08-08 15:26_

---

_Closed by @charliermarsh on 2024-08-08 15:26_

---

_Branch deleted on 2024-08-08 15:26_

---
