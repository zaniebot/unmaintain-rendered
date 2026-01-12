```yaml
number: 588
title: "ignore: add grouped defaults toggle"
type: pull_request
state: merged
author: durka
labels: []
assignees: []
merged: true
base: master
head: group-defaults
created_at: 2017-08-29T18:37:11Z
updated_at: 2017-09-02T16:29:00Z
url: https://github.com/BurntSushi/ripgrep/pull/588
synced_at: 2026-01-12T18:23:13Z
```

# ignore: add grouped defaults toggle

---

_@durka_

Adds a single function to toggle all the enabled-by-default options on `WalkBuilder`. I implemented this for [my crate](https://github.com/haptics-nri/bible-extractor/blob/76769934d69c6cd9857884e91e30eb2f6370973a/src/main.rs#L98) because I wanted a parallel directory iterator but I don't actually need the fancy ignore stuff. Feel free to reject if that isn't an intended use case.

---

_Comment by @BurntSushi on 2017-08-30 11:00_

@durka Hmmmm. I guess I'm in principle OK with something like this, but I think the name and/or documentation isn't quite clear enough. Can we come up with something better? For example, I think it would be clearer if instead of focusing on "defaults" we focused on "filtering." So I think the docs for this function should explicitly call out that the filters for this iterator are all enabled by default, and this this function can be used to toggle their presence as a group. I think we should also say that filters may be selectively enabled by the caller after using this.

---

_Comment by @durka on 2017-08-30 17:10_

Maybe `default_filters` or `standard_filters`?

---

_Comment by @BurntSushi on 2017-08-30 17:11_

I think I like `standard_filters`.

---

_Comment by @durka on 2017-08-30 17:43_

I changed the name and added more docs.

---

_@BurntSushi approved on 2017-08-30 17:54_

LGTM. Thanks! :-)

---

_Merged by @BurntSushi on 2017-09-02 16:29_

---

_Closed by @BurntSushi on 2017-09-02 16:29_

---
