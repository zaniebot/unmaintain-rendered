```yaml
number: 19624
title: "Revert `license` and `license-files` changes in `pyproject.toml`"
type: pull_request
state: merged
author: ntBre
labels: []
assignees: []
merged: true
base: main
head: brent/revert-license-changes
created_at: 2025-07-29T21:16:54Z
updated_at: 2025-08-01T14:29:39Z
url: https://github.com/astral-sh/ruff/pull/19624
synced_at: 2026-01-10T17:52:17Z
```

# Revert `license` and `license-files` changes in `pyproject.toml`

---

_Pull request opened by @ntBre on 2025-07-29 21:16_

Summary
--

This partially reverts commit 13634ff433310fa4b718c57cb8ea43ab334e2637 after issues in the release today.

Test Plan
--

```shell
uv build --sdist
tar -tzf dist/ruff-0.12.6.tar.gz | grep ruff-0.12.6/LICENSE
```

which finds the license now.


---

_@zanieb approved on 2025-07-29 21:26_

---

_Merged by @ntBre on 2025-07-29 21:27_

---

_Closed by @ntBre on 2025-07-29 21:27_

---

_Branch deleted on 2025-07-29 21:27_

---

_Comment by @DimitriPapadopoulos on 2025-07-31 09:14_

I have read #19599 and this PR, and don't understand the te~~x~~st plan. Is the issue that `uv build` does not support PEP 639 yet and pushes license-less packages to PiPY?

---

_Comment by @ntBre on 2025-07-31 12:32_

I saw you got an answer on #19599, but you're right. I forgot all of the conversation around this was on Discord and could have included more context here. It might be helpful in the future.

During the 0.12.6 release, the sdist upload was rejected from PyPI for not containing the `LICENSE` file. I was able to verify that locally with the commands in the test plan. After the changes in this PR, the same commands confirmed that the `LICENSE` was back in the sdist before we tried another release.

As mentioned on #19599, the root cause seems to be a maturin bug, so we should be able to revert this revert at some point in the future.

---

_Comment by @DimitriPapadopoulos on 2025-07-31 12:38_

Thank you so much. I have created draft PR #19661 that attempts to explain all known issues.

---

_Comment by @DimitriPapadopoulos on 2025-08-01 14:29_

Putting aside the main issue (the maturin bug):

It turns out [pip-licences](https://github.com/raimon49/pip-licenses) is not much maintained and has been forked into [pip-licenses-cli](https://github.com/stefan6419846/pip-licenses-cli) which does support PEP 639. Also `pip show` has been updated and does support PEP 639 according to [What's new in pip 25.0](https://ichard26.github.io/blog/2025/01/whats-new-in-pip-25.0/#pep-639-spdx-license-expressions).

---
