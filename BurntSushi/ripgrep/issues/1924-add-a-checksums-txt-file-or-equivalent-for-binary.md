```yaml
number: 1924
title: Add a checksums.txt file or equivalent for binary/artifacts checksums on GitHub release pages
type: issue
state: closed
author: ScriptAutomate
labels:
  - enhancement
  - help wanted
  - rollup
assignees: []
created_at: 2021-07-01T19:17:58Z
updated_at: 2023-07-09T14:14:06Z
url: https://github.com/BurntSushi/ripgrep/issues/1924
synced_at: 2026-01-12T16:13:24Z
```

# Add a checksums.txt file or equivalent for binary/artifacts checksums on GitHub release pages

---

_@ScriptAutomate_

**Is your feature request related to a problem? Please describe.**

When downloading binaries from a GitHub releases page, or downloads/release pages in general, it would be beneficial to have something of a checksums file available for reference.

Examples:
- GitHub CLI: https://github.com/cli/cli/releases/tag/v1.9.2
- GitLab CLI: https://github.com/profclems/glab/releases/tag/v1.16.0

Both have a `*.txt` file for referencing hashes, such as `checksums.txt` or other naming convention.

This is a good idea in order to verify the integrity of a downloaded binary.

Examples:
- https://ubuntu.com/tutorials/how-to-verify-ubuntu#1-overview
- https://docs.hak5.org/hc/en-us/articles/360049922674-How-to-verify-the-SHA256-checksum-of-a-downloaded-file

> "...it is definitely reassuring to be able to verify that the image [or artifact] you have downloaded is not corrupted in some way, and also that it is an authentic image [or artifact] that hasn’t been tampered with."

**Describe the solution you'd like**

A release page should have a `*.txt` file for checksums of release artifacts, in the format similar to the GitHub and GitLab CLI checksum files I provided.

I use [`salt`](https://github.com/saltstack/salt/) to install packages like `ripgrep`, and by default they support checksum validation by referencing the file examples given above for GitLab/GitHub CLI downloads. :)

---

_Comment by @BurntSushi on 2021-07-01 22:59_

I would be okay with this. It seems like a good idea, probably not much work and is unlikely to be a maintenance hazard.

I doubt I'll do this myself (any time soon), so if someone wanted to take a stab at it, that would be great!

---

_Label `enhancement` added by @BurntSushi on 2021-07-01 22:59_

---

_Label `help wanted` added by @BurntSushi on 2021-07-01 22:59_

---

_Label `rollup` added by @BurntSushi on 2023-07-09 12:35_

---

_Closed by @BurntSushi on 2023-07-09 14:14_

---
