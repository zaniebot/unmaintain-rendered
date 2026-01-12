```yaml
number: 1693
title: "deps: use fs-err instead of std::fs in walkdir"
type: pull_request
state: closed
author: hoodie
labels: []
assignees: []
base: master
head: feature/fs-err
created_at: 2020-09-30T22:14:03Z
updated_at: 2020-10-01T14:21:15Z
url: https://github.com/BurntSushi/ripgrep/pull/1693
synced_at: 2026-01-12T18:23:14Z
```

# deps: use fs-err instead of std::fs in walkdir

---

_@hoodie_

Hi there, thanks so much for ripgrep, I use it every day!
I just stumbled upon this little crate called [fs-err](https://github.com/andrewhickman/fs-err) which produces nicer fs errors but is basically a drop in replacement for `std::err`.

I thought I'd give it a try in ripgrep et voilá

before:
```bash
~  /usr/bin/rg password /root
/root: Permission denied (os error 13)
```

after:
```bash
~  rg password /root 
/root: failed to read directory `/root`
```

thank you for your consideration

---

_Comment by @okdana on 2020-09-30 22:39_

Is it nicer? It seems like it provides less useful information

---

_Comment by @hoodie on 2020-09-30 22:46_

@okdana actually you might be right 😀 

---

_Closed by @hoodie on 2020-09-30 22:47_

---

_Comment by @BurntSushi on 2020-10-01 12:57_

@hoodie I had the same reaction as @okdana. And ripgrep already tries pretty hard to add file paths to error messages, which appears to be the primary benefit of something like `fs-err`.

---

_Comment by @hoodie on 2020-10-01 14:21_

It was mainly an exercise for me, I was so surprised that it was so easy to integrate that I didn't validate the results thoroughly enough

---
