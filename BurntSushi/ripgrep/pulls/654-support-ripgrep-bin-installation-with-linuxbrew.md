```yaml
number: 654
title: Support ripgrep-bin installation with linuxbrew
type: pull_request
state: merged
author: francoisferrand
labels: []
assignees: []
merged: true
base: master
head: ripgrep-bin-linuxbrew
created_at: 2017-10-25T11:36:53Z
updated_at: 2017-12-21T08:21:41Z
url: https://github.com/BurntSushi/ripgrep/pull/654
synced_at: 2026-01-12T18:23:13Z
```

# Support ripgrep-bin installation with linuxbrew

---

_@francoisferrand_

_No description provided._

---

_Comment by @BurntSushi on 2017-10-25 11:47_

Could you tell me how to test this? I don't know what linuxbrew is.

---

_@BurntSushi reviewed on 2017-10-25 11:49_

---

_Review comment by @BurntSushi on `pkg/brew/ripgrep-bin.rb`:15 on 2017-10-25 11:49_

Could you please remove the `i686` option? I don't want to devote resources to something people probably won't use. I realize CI produces builds for it, but only because it doesn't cause any trouble. However, if I support this on linuxbrew and decide one day to remove support for it, people might get upset.

---

_@francoisferrand reviewed on 2017-10-25 12:56_

---

_Review comment by @francoisferrand on `pkg/brew/ripgrep-bin.rb`:15 on 2017-10-25 12:56_

done.

---

_Comment by @francoisferrand on 2017-10-25 13:09_

Linuxbrew is a port of homebrew for Linux.
You can install it on a linux host with:
```
$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
```

Then install the `ripgrip-bin` package like with homebrew:
```
$ brew tap burntsushi/ripgrep https://github.com/BurntSushi/ripgrep.git
$ brew install burntsushi/ripgrep/ripgrep-bin
```

---

_@BurntSushi approved on 2017-10-25 13:12_

Hmm, OK, that will be tricky for me to test since I don't want to put a package manager on my system.

I'll accept this for now, and I'll do the "obvious" update whenever I put out a new release of ripgrep and hopefully that will be good enough.

---

_Comment by @BurntSushi on 2017-10-25 13:13_

(I do incidentally test this tap on a Mac, but that's because my Mac is strictly for testing this sort of thing. I otherwise don't use it.)

---

_Merged by @BurntSushi on 2017-11-01 11:11_

---

_Closed by @BurntSushi on 2017-11-01 11:11_

---

_Branch deleted on 2017-12-21 08:21_

---
