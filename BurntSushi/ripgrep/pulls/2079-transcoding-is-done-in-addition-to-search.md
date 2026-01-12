```yaml
number: 2079
title: transcoding is done in addition to search
type: pull_request
state: merged
author: matkoniecz
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2021-11-21T06:00:21Z
updated_at: 2021-11-22T14:48:42Z
url: https://github.com/BurntSushi/ripgrep/pull/2079
synced_at: 2026-01-12T18:23:14Z
```

# transcoding is done in addition to search

---

_@matkoniecz_

Even if transcoding would be faster than search it would still incur performance penalty

---

_@BurntSushi requested changes on 2021-11-22 14:17_

Ah yes, exactly correct! Thank you for the clarification.

Could you please reflow the text so that it wraps to 79 columns? Thanks!

---

_Review requested from @BurntSushi by @matkoniecz on 2021-11-22 14:25_

---

_@BurntSushi approved on 2021-11-22 14:28_

Thanks!

---

_Comment by @BurntSushi on 2021-11-22 14:29_

I wish there were a way to have CI bleat about 79 column violations, but it's not a rule I enforce 100% strictly. e.g., URLs and sometimes even code going over 79 columns is okay when that's the clearer path. But alas, such cases call for human judgment.

---

_Comment by @matkoniecz on 2021-11-22 14:31_

Maybe some bot that would comment on PR "you added new lines in XXX file longer than N characters, typically this is unwanted, see this contributing.md section for more info. You probably should reflow text so that it wraps to 79 columns."?

---

_Comment by @BurntSushi on 2021-11-22 14:33_

That's a nice idea. Thank you! Love it actually.

---

_Comment by @matkoniecz on 2021-11-22 14:45_

I just checked and I see no CONTRIBUTING.md file at all (if that docs exist somewhere then making link-only CONTRIBUTING.md is still a good idea to make easy to find them).

For bot itself - I have seen some code coverage monitor bots posting, Visual Studio Code has some bot actions on issue tracker (automatically and triggered). See https://github.com/microsoft/vscode/issues/137641#issuecomment-975481667 https://github.com/microsoft/vscode-github-triage-actions

But I have no direct experience with maintaining, making or using something like that.

---

_Comment by @BurntSushi on 2021-11-22 14:48_

Aye thanks, I created #2080 to track your idea.

---

_Merged by @BurntSushi on 2021-11-22 14:48_

---

_Closed by @BurntSushi on 2021-11-22 14:48_

---
