```yaml
number: 1947
title: "feat: bump actions/{download,upload}-artifact"
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: upgrade-upload-download-actions
created_at: 2024-02-24T03:33:46Z
updated_at: 2024-02-26T13:57:32Z
url: https://github.com/astral-sh/uv/pull/1947
synced_at: 2026-01-12T16:04:48Z
```

# feat: bump actions/{download,upload}-artifact

---

_@samypr100_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #1943

Makes sure `build-binaries` and `publish-pypi` workflows are compatible with `actions/{download,upload}-artifact@v4`. In nature, this PR is very similar to the changes in https://github.com/astral-sh/ruff/pull/10105. This PR also updates cargo-dist.

## Test Plan

I ran a small non-dry-run [smoke test](https://github.com/samypr100/uv/actions/runs/8027864059) on my own fork CI with only linux builds (for speed) and those jobs seem to work at a glance.  

---

_Marked ready for review by @samypr100 on 2024-02-24 03:34_

---

_Label `internal` added by @zanieb on 2024-02-24 03:42_

---

_Comment by @samypr100 on 2024-02-24 03:49_

> curl: (28) Failed to connect to github.com port 443 after 130461 ms: Connection timed out

Github seems to be having a bad day, and I can't re-run the 1 that failed due to Github hiccups 😅 

Can someone with permissions re-run `Build binaries / linux-arm`? 🙏 

---

_Comment by @charliermarsh on 2024-02-24 19:13_

Awesome, thank you!

---

_Comment by @charliermarsh on 2024-02-24 19:13_

I know @MichaReiser wanted some input on https://github.com/astral-sh/ruff/pull/10105 so I'm gonna wait until that gets merged, then merge this one to follow.

---

_Comment by @samypr100 on 2024-02-25 15:59_

Sounds good, wanted to make sure releases could continue since I think they're blocked due to the earlier cargo dist update in 5a50a753bdad69f3fd2b7b6e0527314e9b268871 and this PR fixes that. Although we can unblock releases by reverting that commit too.

---

_Comment by @charliermarsh on 2024-02-25 19:35_

Ooh thank you, I didn't realize that!

---

_@charliermarsh approved on 2024-02-26 01:08_

---

_Merged by @charliermarsh on 2024-02-26 01:09_

---

_Closed by @charliermarsh on 2024-02-26 01:09_

---

_Branch deleted on 2024-02-26 01:29_

---

_Comment by @konstin on 2024-02-26 13:57_

Sorry, i also missed that cargo-dist change

---
