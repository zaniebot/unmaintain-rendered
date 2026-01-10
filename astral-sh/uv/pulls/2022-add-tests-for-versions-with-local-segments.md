```yaml
number: 2022
title: add tests for versions with local segments
type: pull_request
state: closed
author: BurntSushi
labels: []
assignees: []
base: main
head: ag/local-tests
created_at: 2024-02-27T17:36:26Z
updated_at: 2024-03-13T15:14:39Z
url: https://github.com/astral-sh/uv/pull/2022
synced_at: 2026-01-10T14:49:08Z
```

# add tests for versions with local segments

---

_Pull request opened by @BurntSushi on 2024-02-27 17:36_

While trying to fix #1855, I came up with a few tests that improve
our coverage of versions with local segments. Those tests were added
to packse here: https://github.com/zanieb/packse/pull/132

None of these tests pass yet, so they are marked as ignored.


---

_@charliermarsh approved on 2024-02-27 22:47_

---

_Comment by @zanieb on 2024-02-28 02:33_

Note we should merge in packse first so they publish to PyPI and are discovered by tests

---

_Comment by @BurntSushi on 2024-02-28 14:01_

All righty, so I've got the packse repo updated with tests for local versions, but (and I should have anticipated this), [publishing to `test.pypi.org` fails](https://github.com/zanieb/packse/actions/runs/8081475053/job/22080133217) because of the local segments in the version numbers. They aren't allowed to be published. This present an interesting conundrum, because the ban isn't reflective of the reality that other indexes (such as pytorch) _do_ allow local versions to be published.

I'm not quite sure how to fix this, and I also don't know the implications of merging this PR when publishing to `test.pypi.org` fails.

---

_Comment by @zanieb on 2024-02-28 17:36_

@BurntSushi I can definitely look into this when I'm back, I'm interested in getting us using a local PyPI mirror for tests which would allow us to do things like this.

In the meantime.. if you merge with broken PyPI uploads we'll spam PyPI retrying the upload every few hours. You could exclude these scenarios from the PyPI uploads somehow then rely on our public CodeArtifact proxy to test the scenarios in CI. I'm not sure if it's worth the effort though.

---

_Comment by @charliermarsh on 2024-03-13 15:13_

Should this be closed?

---

_Comment by @zanieb on 2024-03-13 15:14_

Yep!

---

_Closed by @zanieb on 2024-03-13 15:14_

---

_Comment by @charliermarsh on 2024-03-13 15:14_

Sweet. (For posterity, these got merged into main separately.)

---

_Comment by @zanieb on 2024-03-13 15:14_

Done in https://github.com/astral-sh/uv/pull/2256

---
