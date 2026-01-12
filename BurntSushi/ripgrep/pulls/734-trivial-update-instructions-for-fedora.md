```yaml
number: 734
title: "trivial: update instructions for Fedora"
type: pull_request
state: merged
author: igor-raits
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2018-01-07T19:32:13Z
updated_at: 2018-01-07T21:48:50Z
url: https://github.com/BurntSushi/ripgrep/pull/734
synced_at: 2026-01-12T18:23:13Z
```

# trivial: update instructions for Fedora

---

_@igor-raits_

_No description provided._

---

_Review comment by @BurntSushi on `README.md`:197 on 2018-01-07 19:45_

How come we don't need the `sudo dnf copr enable carlwgeorge/ripgrep` line here?

---

_@BurntSushi reviewed on 2018-01-07 19:46_

Thanks! Just had one question.

---

_@igor-raits reviewed on 2018-01-07 20:03_

---

_Review comment by @igor-raits on `README.md`:197 on 2018-01-07 20:03_

because it is in official repos for quite some time (but only F28+) already

---

_@igor-raits reviewed on 2018-01-07 20:05_

---

_Review comment by @igor-raits on `README.md`:197 on 2018-01-07 20:05_

```
➜  ~ sudo docker run --rm registry.fedoraproject.org/fedora:rawhide sh -c 'dnf -y install ripgrep -q && rg --version' 
ripgrep 0.7.1
-AVX -SIMD
```

---

_@igor-raits reviewed on 2018-01-07 20:08_

---

_Review comment by @igor-raits on `README.md`:197 on 2018-01-07 20:08_

I'm planning to backport it to Fedora 27 as well, but yet don't have time for it..

---

_@BurntSushi reviewed on 2018-01-07 21:48_

---

_Review comment by @BurntSushi on `README.md`:197 on 2018-01-07 21:48_

Oh I see. I think I got confused since both instructions talk about a COPR, which I just assumed was similar to something like adding a new PPA in Ubuntu. (Unfortunately, I am hopelessly unfamiliar with both Ubuntu and Fedora.) Anyway, thank you!


---

_@BurntSushi approved on 2018-01-07 21:48_

---

_Merged by @BurntSushi on 2018-01-07 21:48_

---

_Closed by @BurntSushi on 2018-01-07 21:48_

---
