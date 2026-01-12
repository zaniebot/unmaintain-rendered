```yaml
number: 285
title: Update docs to explain use of -g and --files to search for paths.
type: pull_request
state: merged
author: YPCrumble
labels: []
assignees: []
merged: true
base: master
head: update_docs_for_path_search
created_at: 2016-12-21T18:10:11Z
updated_at: 2016-12-22T15:05:02Z
url: https://github.com/BurntSushi/ripgrep/pull/285
synced_at: 2026-01-12T18:23:12Z
```

# Update docs to explain use of -g and --files to search for paths.

---

_@YPCrumble_

@BurntSushi this updates docs to resolve #284, #193, #54, #75, #91, #48. Explains the combined use of `-g` and `--files` flags to replicate the `-g` option in ag/ack.

Looks like I used a later version of pandoc to compile the docs that perhaps did things a little differently. It complained about using sed's `-i` flag (`sed: -i may not be used with stdin`).

Please let me know if any issues/suggestions from your end.

Thank you for your patience and thanks again for building ripgrep!

---

_@BurntSushi reviewed on 2016-12-21 20:49_

---

_Review comment by @BurntSushi on `doc/rg.1.md` on 2016-12-21 20:49_

Can you change `<pattern>` to `<glob>`? I ask because I think we use `<pattern>` elsewhere to refer to "regex pattern."

---

_@BurntSushi reviewed on 2016-12-21 20:49_

---

_Review comment by @BurntSushi on `doc/rg.1.md` on 2016-12-21 20:49_

As above, change this to `<glob>`.

---

_@BurntSushi requested changes on 2016-12-21 20:49_

This looks great! I just have one nit. Thank you! :-)

---

_Comment by @BurntSushi on 2016-12-21 23:03_

Oh, and could you also put the string `Fixes #284` at the bottom of your commit message? I'm trying to keep things tidy. :-) Thanks!

---

_Comment by @YPCrumble on 2016-12-22 02:12_

@BurntSushi thank you for your review! Made those changes. Let me know if what I did with the commit message isn't what you meant? Also I escaped the < and > characters around `<glob>` in `rg.1.md` because it appeared that not escaping them made them not appear in `rg.1`. Please me know if this was a mistake?

---

_Merged by @BurntSushi on 2016-12-22 12:21_

---

_Closed by @BurntSushi on 2016-12-22 12:21_

---

_Comment by @BurntSushi on 2016-12-22 12:24_

@YPCrumble Looks good to me! I verified that `rg.1` looks right by running `man -l doc/rg.1`.

> Let me know if what I did with the commit message isn't what you meant?

Almost. Ideally, simple PRs like this should just be a single commit. You can do this by amending your previous commit and force pushing your branch, and that commit message is what should contain `Fixes #284` in it. I just went ahead and did that for you though. Thanks again!

---

_Comment by @YPCrumble on 2016-12-22 15:05_

@BurntSushi got it - thanks for the explanation and making that adjustment for me. Didn't realize I can just take a look at the docs without compiling the code. Glad to have been able to contribute!

---
