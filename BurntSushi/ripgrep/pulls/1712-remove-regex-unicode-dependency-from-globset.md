```yaml
number: 1712
title: Remove regex unicode dependency from globset
type: pull_request
state: merged
author: ajeetdsouza
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2020-10-19T18:08:50Z
updated_at: 2020-10-22T06:06:47Z
url: https://github.com/BurntSushi/ripgrep/pull/1712
synced_at: 2026-01-12T18:23:14Z
```

# Remove regex unicode dependency from globset

---

_@ajeetdsouza_

I don't think globset requires Unicode support in regex for path matching (correct me if I'm wrong). I removed the dependency to make the crate lighter.

This won't affect the size of ripgrep, since it depends on Unicode in regex elsewhere. However, I've been meaning to use globset in [zoxide](https://github.com/ajeetdsouza/zoxide), and I'd like to keep the binary size as small as possible.

I tested a trivial program before and after the change, and the executable size went from 2544 KB to 2084 KB.

Thanks!

---

_@BurntSushi approved on 2020-10-19 18:14_

Hmmm, yes, I _believe_ this is indeed correct. Good catch. In particular, the glob-to-regex transform explicitly disables Unicode support in the regex, so I think that means it's impossible for the regex to make use of any of the Unicode features.

---

_Comment by @ajeetdsouza on 2020-10-19 18:25_

Yup, just saw that too. Thanks!

---

_Comment by @BurntSushi on 2020-10-19 18:27_

Aye, yeah, passing the test suite is good enough for me. ~~I expect we'll find out if this was an unwise choice after the next release! Hah. But I think it should be fine.~~

Errmm... Well... That's not true. Derp. Because the Unicode feature will still be enabled for ripgrep.

---

_Merged by @BurntSushi on 2020-10-19 18:29_

---

_Closed by @BurntSushi on 2020-10-19 18:29_

---

_Branch deleted on 2020-10-19 18:30_

---

_Comment by @BurntSushi on 2020-10-22 01:11_

Whoops, I forgot to put out a new release of globset with this change. This PR is now in `globset 0.4.6` on crates.io.

---

_Comment by @ajeetdsouza on 2020-10-22 06:06_

Awesome, thank you!

---
