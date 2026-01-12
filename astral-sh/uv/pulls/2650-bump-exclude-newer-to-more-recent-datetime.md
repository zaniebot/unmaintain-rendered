```yaml
number: 2650
title: bump EXCLUDE_NEWER to more recent datetime
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/bump-exclude-newer
created_at: 2024-03-25T15:14:41Z
updated_at: 2024-03-25T15:29:06Z
url: https://github.com/astral-sh/uv/pull/2650
synced_at: 2026-01-12T16:05:09Z
```

# bump EXCLUDE_NEWER to more recent datetime

---

_@BurntSushi_

While writing tests for a new flag (`--emit-marker-expression`) for `uv
pip compile`, I noticed that one of my test cases (`pendulum 3.0.0`)
was published in Dec 2023. I wanted to include this package in my
tests, but since it comes after our `EXCLUDE_NEWER` constant, it wasn't
visible to `uv`.

In this PR, I chose to resolve this by bumping `EXCLUDE_NEWER` to
`2024-03-25T00:00:00Z`. I also considered a couple other options:

* For a specific test, override and provide a custom `--exclude-newer`
flag. I felt like this would maybe be okay, but we could easily wind
up in a situation where we do this a lot and have a bunch of different
`--exclude-newer` flags in our tests. I'm not sure if this is a huge
problem in practice. Maybe it's fine.
* Find another package (or invent one) with a similarly interesting
configuration. It seemed easier to just bump `EXCLUDE_NEWER`.

The way I did this was to run `cargo insta test` after bumping `EXCLUDE_NEWER`.
I then reviewed the snapshot diffs, and if they looked reasonable, I accepted them.
There was only one case where I changed the test to preserve what I thought it
was trying to test. That's isolated in its own commit.

---

_Review requested from @zanieb by @BurntSushi on 2024-03-25 15:14_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-03-25 15:15_

---

_Review requested from @konstin by @BurntSushi on 2024-03-25 15:15_

---

_@zanieb approved on 2024-03-25 15:28_

I'm hesitant about this approach in general but this seems fine :)

---

_Merged by @BurntSushi on 2024-03-25 15:29_

---

_Closed by @BurntSushi on 2024-03-25 15:29_

---

_Branch deleted on 2024-03-25 15:29_

---
