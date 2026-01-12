```yaml
number: 1600
title: Ignore Rules Documentation
type: pull_request
state: closed
author: tifrel
labels:
  - rollup
assignees: []
base: master
head: rg-doc-ignore-patch
created_at: 2020-05-27T12:25:04Z
updated_at: 2021-06-01T01:51:23Z
url: https://github.com/BurntSushi/ripgrep/pull/1600
synced_at: 2026-01-12T18:23:14Z
```

# Ignore Rules Documentation

---

_@tifrel_

Added a short description on the local ignore files and how to toggle them on/off

---

_Renamed from "Ignore Rules" to "Ignore Rules Documentation" by @tifrel on 2020-05-27 12:26_

---

_Review comment by @BurntSushi on `GUIDE.md`:195 on 2020-05-28 11:55_

Could this line be wrapped to 79 columns (inclusive) please? I think there is another line above that's too long.

---

_Review comment by @BurntSushi on `GUIDE.md`:183 on 2020-05-28 12:07_

I think saying that `.rgignore` has the highest precedence isn't quite correct. And having this in list form kind of implies that it is exhaustive. But it's not. For example, this leaves out global gitignores and `.git/info/exclude` rules. It also leaves out ignore files explicitly specified on the command line via `--ignore-file`.

This is kind of why I wrote the guide the way I did, because being 100% precise here ends up making it quite a bit more verbose and more like a reference than a guide. The guide is definitely not meant to be reference. It is explicitly imprecise at the expense of leaving out every detail but at the benefit of giving users more succinct material to read.

I do think your PR here straddles a decent line. So instead of trying to make this complete, I'd suggest some phrasing changes. So instead of "Glob patterns specified in several ignore files:," I'd say, "Files and directories that match glob patterns in these three categories:" Then I'd list it like this:

1. gitignore globs (including global and repo-specific globs)
2. `.ignore` globs, which take precedence over all gitignore globs when there's a conflict.
3. `.rgignore` globs, which take precedence over all `.ignore` globs when there's a conflict.

---

_Review comment by @BurntSushi on `GUIDE.md`:195 on 2020-05-28 12:09_

Thinking this over, I kind of think these changes should be removed. I think it's too many details and is liable to overwhelm the user. ripgrep has _a lot_ of flags, and most/all of the flags you've added here are pretty niche. I think it's okay to let users discover them by reading the man page. I think the man page is probably the right place to have a more detailed section on smart filtering that weaves together all relevant flags, but that will be tricky to write well.

---

_@BurntSushi approved on 2020-05-28 12:10_

Thanks for improving the docs! I like some parts of this, and left some suggestions. I think other parts get too far into the weeds for a guide though.

---

_@BurntSushi requested changes on 2020-05-29 12:53_

Apologies, I meant to request changes here.

---

_Label `rollup` added by @BurntSushi on 2021-05-30 16:44_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
