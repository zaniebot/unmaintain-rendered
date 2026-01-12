```yaml
number: 920
title: Add details of BSD packages to README
type: pull_request
state: merged
author: wezm
labels: []
assignees: []
merged: true
base: master
head: add-bsd-packages-to-readme
created_at: 2018-05-13T21:13:51Z
updated_at: 2018-05-14T10:45:40Z
url: https://github.com/BurntSushi/ripgrep/pull/920
synced_at: 2026-01-12T18:23:13Z
```

# Add details of BSD packages to README

---

_@wezm_

_No description provided._

---

_Review comment by @BurntSushi on `README.md`:276 on 2018-05-13 23:01_

Can you use a `$` here? Or is the `#` intentional? Is it implying that the user should be `root`? Is it standard BSD convention to write it this way instead of `sudo`?

(Apologies, I am not experienced with BSDs or BSDisms. :-))

---

_@BurntSushi reviewed on 2018-05-13 23:01_

---

_@BurntSushi reviewed on 2018-05-13 23:02_

Wow this is awesome! I had no idea ripgrep was in this many BSD package repos. This basically looks good to me, but had a question on nomenclature.

---

_@wezm reviewed on 2018-05-13 23:43_

---

_Review comment by @wezm on `README.md`:276 on 2018-05-13 23:43_

`#` was intentional to suggest a root shell. `sudo` is optional on NetBSD and FreeBSD so I wrote these examples without assuming it wasn't installed. It follows the [convention used in the FreeBSD handbook](https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/book-preface.html#preface-conv-examples). OpenBSD has their `sudo` replacement `doas` in the base system so I used that in their example.

---

_Comment by @wezm on 2018-05-13 23:46_

> I had no idea ripgrep was in this many BSD package repos.

Yep it's there along with a [handful of other Rust tools](https://www.freshports.org/search.php?stype=depends_all&method=match&query=lang%2Frust&num=100&orderby=category&orderbyupdown=asc&search=Search&format=html&minimal=1&branch=head). :slightly_smiling_face: 

---

_@BurntSushi reviewed on 2018-05-14 10:45_

---

_Review comment by @BurntSushi on `README.md`:276 on 2018-05-14 10:45_

Ah awesome! I'm happy as long as this is the convention.

---

_@BurntSushi approved on 2018-05-14 10:45_

---

_Merged by @BurntSushi on 2018-05-14 10:45_

---

_Closed by @BurntSushi on 2018-05-14 10:45_

---
