```yaml
number: 2933
title: "ignore: fix filtering searching subdir or .ignore in parent dir"
type: pull_request
state: closed
author: WalterScottYoung
labels:
  - rollup
assignees: []
base: master
head: fix_issue_2836
created_at: 2024-11-15T11:29:51Z
updated_at: 2025-09-20T01:08:22Z
url: https://github.com/BurntSushi/ripgrep/pull/2933
synced_at: 2026-01-12T18:23:14Z
```

# ignore: fix filtering searching subdir or .ignore in parent dir

---

_@WalterScottYoung_

The previous code deleted too many parts of the path when constructing the absolute path, resulting in a shortened final path. This patch creates the correct absolute path by only removing the necessary parts.

Fixes #2836 and  #2747 and #2731

---

_Comment by @WalterScottYoung on 2024-11-15 11:44_

One thing to notice is that commit cad1f5f didn't seems to fully fix https://github.com/BurntSushi/ripgrep/issues/1757, the test in issue 1757 will fail if we add one more directory in the path. I don't know if we should reopen 1757 or something like that to note others about this?

The following test from 1757 with "onemore" directory will fail before this fix.

```bash
mkdir tmp
cd tmp
mkdir rust
mkdir -p rust/target/onemore
echo needle > rust/source.rs
echo needle > rust/target/onemore/rustdoc-output.html
echo rust/target/onemore > .ignore
rg --files-with-matches needle
rg --files-with-matches needle rust
echo target >> .ignore
rg --files-with-matches needle rust
```

---

_Comment by @gstokkink on 2025-01-07 06:29_

@BurntSushi do you have time to review/merge this?

---

_Comment by @Lucus16 on 2025-02-03 12:53_

I can confirm this fixes some bugs I was about to report where some files listed in gitignore glob rules were not excluded depending on the directory that `rg --files` was run from. Thanks!

---

_Comment by @woess on 2025-02-12 00:51_

This PR also fixes https://github.com/BurntSushi/ripgrep/issues/2778.

---

_Comment by @beeb on 2025-04-22 11:06_

@BurntSushi sorry for the mention, my project is affected by the underlying bugs fixed in this PR. Would it be possible to review it?

---

_Comment by @fenuks on 2025-05-26 12:40_

I'm also affected by this bug in ripgrep, but also in https://github.com/sharkdp/fd/issues/1506 which also depends on ignore crate.

---

_Comment by @reneleonhardt on 2025-07-01 06:32_

Great work, thank you! 👍
Just wondering, one test was enough for negation and nesting?

---

_Comment by @gstokkink on 2025-07-01 07:24_

@BurntSushi any chance of a merge & release?

---

_Comment by @WalterScottYoung on 2025-07-03 12:55_

> Great work, thank you! 👍
> Just wondering, one test was enough for negation and nesting?

It's been some time and I don't really remember the detail of this, but I'm pretty sure that the original problem was cause by the path concatenation and the target file's relative depth to where ripgrep was called be positive or 0 or negative was the boardline that this  bug would be triggered, so only tests for those boardline scenarios was added, since more layer of dir won't matter as the bug would be triggered in the first nested layer of dir. Thank you for your question and you can add some more tests that you think is valuable  :-)

---

_Label `rollup` added by @BurntSushi on 2025-07-06 14:37_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
