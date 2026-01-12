```yaml
number: 301
title: Add powershell as a known file type
type: pull_request
state: merged
author: lzybkr
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-01-03T19:18:09Z
updated_at: 2017-01-06T13:18:22Z
url: https://github.com/BurntSushi/ripgrep/pull/301
synced_at: 2026-01-12T18:23:12Z
```

# Add powershell as a known file type

---

_@lzybkr_

_No description provided._

---

_@BurntSushi reviewed on 2017-01-03 22:47_

---

_Review comment by @BurntSushi on `ignore/src/types.rs` on 2017-01-03 22:47_

Would it be better to use `ps` here? I feel like `powershell` is a bit long. Is `ps` is accepted abbreviation of PowerShell that folks would recognize?

---

_@lzybkr reviewed on 2017-01-03 23:12_

---

_Review comment by @lzybkr on `ignore/src/types.rs` on 2017-01-03 23:12_

Indeed, it felt long when I tested it.

`ps` is a frequently used abbreviation *within* PowerShell (e.g. as a prefix for reserved variables or module names), but I'm not sure it's used enough with non-PowerShell tools that it should be considered the best abbreviation.

I also had a slight concern with potential confusion with postscript - one reason PowerShell doesn't use the extension `ps`.

`posh` might be a more widely accepted abbreviation, but I'm not a fan of that abbreviation, partly because that is an actual word.

This would be the second longest type, and there are many somewhat long types, e.g.:

```
markdown
asciidoc
taskpaper
vimscript
powershell
coffeescript
```

At any rate, I posted a survey on [Twitter](https://twitter.com/lzybkr/status/816421545421615105), so we'll see what the feedback looks like.

---

_@BurntSushi reviewed on 2017-01-04 00:04_

---

_Review comment by @BurntSushi on `ignore/src/types.rs` on 2017-01-04 00:04_

> I also had a slight concern with potential confusion with postscript - one reason PowerShell doesn't use the extension ps.

Yeah, I thought about that too, but postscript could use `post` I think.

> This would be the second longest type, and there are many somewhat long types, e.g.:

Yeah, I haven't been terribly conscientious with vetting each name that's added. We do at least have `md` for `markdown`.

> At any rate, I posted a survey on Twitter, so we'll see what the feedback looks like.

Funny. I retweeted it. :-)

---

_@lzybkr reviewed on 2017-01-05 03:15_

---

_Review comment by @lzybkr on `ignore/src/types.rs` on 2017-01-05 03:15_

So the vote is in and `ps` is preferred over the alternatives, with just 1 or 2 folks considering PostScript, so I updated the PR to use `ps`.

I also added a couple of less frequently used extensions that are related to PowerShell - not everyone cares to search those extensions but I will find it useful from time to time so I added them.

---

_Merged by @BurntSushi on 2017-01-06 13:18_

---

_Closed by @BurntSushi on 2017-01-06 13:18_

---

_Comment by @BurntSushi on 2017-01-06 13:18_

Looks great! Thanks so much!

---
