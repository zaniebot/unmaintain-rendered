```yaml
number: 1536
title: "cli: fix typo in help description"
type: pull_request
state: merged
author: mariusschulz
labels: []
assignees: []
merged: true
base: master
head: only_matched_typo
created_at: 2020-03-30T08:53:27Z
updated_at: 2020-03-30T21:31:17Z
url: https://github.com/BurntSushi/ripgrep/pull/1536
synced_at: 2026-01-12T18:23:14Z
```

# cli: fix typo in help description

---

_@mariusschulz_

Fix typo in the help description of the `-o` (`--only-matching`) flag.

---

_Comment by @BurntSushi on 2020-03-30 13:45_

I don't know what's going on with GitHub Actions. This happened with a previous PR too. All of the CI builders are failing to checkout the forked repo. I have no idea why.

---

_Comment by @mariusschulz on 2020-03-30 18:02_

Should I abandon this PR and let you apply the fix yourself?

---

_Comment by @BurntSushi on 2020-03-30 18:21_

Nah just leave it for now. Going to give it some time. Maybe it's just a burp. Otherwise I'll eventually just merge it manually.

---

_Comment by @BurntSushi on 2020-03-30 21:10_

Ug. It looks like this is a bug with `actions/checkout@v1` and was seemingly fixed in `v2`: https://github.com/actions/checkout/issues/23#issuecomment-572688577

I've pushed a fix to master. Would you mind rebasing this PR on master and force pushing?

---

_Comment by @mariusschulz on 2020-03-30 21:15_

Done! Let's see.

---

_Comment by @BurntSushi on 2020-03-30 21:30_

Huzzah!

---

_Merged by @BurntSushi on 2020-03-30 21:31_

---

_Closed by @BurntSushi on 2020-03-30 21:31_

---
