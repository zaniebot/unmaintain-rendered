```yaml
number: 45
title: Build ABI3 wheels and expand supported platforms
type: pull_request
state: merged
author: adriangb
labels: []
assignees: []
merged: true
base: main
head: abi3-wheels
created_at: 2022-08-30T03:40:43Z
updated_at: 2023-01-28T02:46:52Z
url: https://github.com/astral-sh/ruff/pull/45
synced_at: 2026-01-12T15:55:04Z
```

# Build ABI3 wheels and expand supported platforms

---

_@adriangb_

_No description provided._

---

_Comment by @adriangb on 2022-08-30 03:51_

Gave this a quick test drive: https://github.com/adriangb/ruff/actions/runs/2953375467. There's an artifact with all of the wheels (16 total) on that page.

I do ask that you look at the release gates. I think you intend this to run and release on every tagged commit, even if it's not a commit on `main`.

---

_Comment by @charliermarsh on 2022-08-30 13:15_

Wait, this is so nice of you? Not only did you go find this repo but you PR'd a huge improvement? Thank you!

For context: I started by copying over the graphlib2 YAML, but at that point, I was still including pyo3 (I later realized this was unnecessary), which was forcing me to build version-specific wheels... so I think some of the individual wheels were failing, because I wasn't building against 3.7? And then I just figured it wasn't worth the painful debugging loop, and gave up on a lot of versions :) This is way better!

---

_Comment by @charliermarsh on 2022-08-30 13:16_

> I do ask that you look at the release gates. I think you intend this to run and release on every tagged commit, even if it's not a commit on `main`.

For now I only want to upload to PyPI when I create a new release, though I'd be open to creating (and not uploading) the artifacts for every commit.


---

_Merged by @charliermarsh on 2022-08-30 13:16_

---

_Closed by @charliermarsh on 2022-08-30 13:16_

---

_Branch deleted on 2022-08-30 13:16_

---

_Comment by @adriangb on 2022-08-30 14:04_

> I'd be open to creating (and not uploading) the artifacts for every commit

Easy enough: #46. Now you'll have artifacts on GitHub for every commit but only publish to PyPi on tags. I guess one could download and install those wheels specifically if they wanted to. Shame you can't link to them directly (so you can't point pip at them).

Sorry about that rogue lint step.

---
