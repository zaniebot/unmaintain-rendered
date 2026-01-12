```yaml
number: 1405
title: Add --include-zero option
type: pull_request
state: closed
author: cstyles
labels:
  - rollup
assignees: []
base: master
head: add-include-zero-option
created_at: 2019-10-17T02:45:38Z
updated_at: 2020-02-22T18:21:30Z
url: https://github.com/BurntSushi/ripgrep/pull/1405
synced_at: 2026-01-12T18:23:13Z
```

# Add --include-zero option

---

_@cstyles_

This PR addresses the suggestion in https://github.com/BurntSushi/ripgrep/issues/1370.

If `--count` or `--count-matches` is enabled, ripgrep currently only reports the number of matches if it's non-zero. [This is desired behavior](https://github.com/BurntSushi/ripgrep/issues/1317#issuecomment-507711847) because otherwise it can generate a lot of noise but sometimes users may want to print zeroes anyway. There's actually already a setting for outputting a zero if there were no matches but there's no way of turning it on from the command line interface. This PR adds the `--include-zero` option to fix that.

---

_Review comment by @BurntSushi on `grep-printer/src/summary.rs`:269 on 2020-02-15 20:30_

I'm not following why this change was made. It's a breaking API change and seems purely cosmetic.

---

_@BurntSushi requested changes on 2020-02-15 20:33_

Thank you! But there is a breaking API change here in the `grep-printer` crate that seems unnecessary.

I'm going to merge this in #1486 after reverting the breaking change.

---

_Label `rollup` added by @BurntSushi on 2020-02-15 20:46_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

_Branch deleted on 2020-02-22 18:21_

---
