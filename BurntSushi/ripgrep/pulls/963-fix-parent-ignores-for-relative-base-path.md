```yaml
number: 963
title: Fix parent ignores for relative base path.
type: pull_request
state: closed
author: tmccombs
labels: []
assignees: []
base: master
head: ignore-paths
created_at: 2018-06-24T23:35:23Z
updated_at: 2019-04-06T12:08:30Z
url: https://github.com/BurntSushi/ripgrep/pull/963
synced_at: 2026-01-12T18:23:13Z
```

# Fix parent ignores for relative base path.

---

_@tmccombs_

Fixes #829 and #278.

I think I prefer this one over #962. But I'm ok with either getting merged (but we don't need both).

---

_Comment by @tmccombs on 2018-07-30 06:07_

Do I need to do anything to move this forward?

---

_Comment by @BurntSushi on 2018-07-30 11:22_

@tmccombs Other than the fact that this PR doesn't pass CI? No. I don't have the bandwidth to review subtle ignore-related problems right now. I will not merge fixes to those bugs without first very carefully thinking through the problem (again) and the fix.

---

_Comment by @Deewiant on 2018-09-08 12:01_

@tmccombs: In case you want to keep this up to date, heads up that there's a merge conflict here now.

---

_Comment by @tmccombs on 2018-09-08 23:11_

Thanks @Deewiant, merge conflicts are resolved.

---

_Comment by @BurntSushi on 2019-04-06 12:08_

@tmccombs Thanks so much for working on this, but for now, I'm going to explicitly punt on fixing these bugs for now (including #962). They're generally low priority for me, and I'd like to fix them when I do a more holistic rethink of how the `ignore` crate should be structured. Right now, the invariants with respect to file paths are just all over the place and are impossible for me to reason about.

I'm going to close out this PR and #962 in order to keep the PR review clean and to better express my intent. I'll leave #829 and #278 open with the intent of trying to address them later.

Thanks again!

---

_Closed by @BurntSushi on 2019-04-06 12:08_

---
