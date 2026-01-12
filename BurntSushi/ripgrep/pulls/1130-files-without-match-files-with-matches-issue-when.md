```yaml
number: 1130
title: "--files-without-match / --files-with-matches issue when providing only a single path"
type: pull_request
state: closed
author: xervon
labels: []
assignees: []
base: master
head: issue-1106-files-without-match-error
created_at: 2018-12-01T02:45:31Z
updated_at: 2019-01-23T02:38:44Z
url: https://github.com/BurntSushi/ripgrep/pull/1130
synced_at: 2026-01-12T18:23:13Z
```

# --files-without-match / --files-with-matches issue when providing only a single path

---

_@xervon_

This should fix #1106 

When determining if file names should be output despite --with-filename not being set explicitly the length of the paths array should be larger than 0 rather than 1 to activate file name output.

---

_Review comment by @BurntSushi on `src/args.rs`:1451 on 2018-12-01 03:10_

Thanks for working on this bug! But I don't think this is right. `rg foo file` should not be printing the file path by default.

---

_@BurntSushi reviewed on 2018-12-01 03:10_

---

_Comment by @xervon on 2018-12-01 03:22_

I was actually testing that... I must have messed up that test case. I'll look at this again and update the pull request after that.

---

_Comment by @xervon on 2018-12-01 12:42_

I obviously also didn't call `cargo test` yesterday. This should be better. At least `cargo test` doesn't complain locally as it would have done yesterday.

---

_Comment by @xervon on 2018-12-01 12:59_

Should I also add another test for this?

---

_Closed by @BurntSushi on 2019-01-23 02:38_

---

_Comment by @BurntSushi on 2019-01-23 02:38_

Thanks for this attempt! Unfortunately, this wasn't quite right. The issue was a bit more subtle than this. See https://github.com/BurntSushi/ripgrep/commit/a7f2d482342eb2250b5a32ee03a8fe16990228dc for more details!

---
