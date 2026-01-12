```yaml
number: 1032
title: deb package is missing completions
type: issue
state: closed
author: muendelezaji
labels:
  - bug
assignees: []
created_at: 2018-08-30T11:40:15Z
updated_at: 2018-09-07T18:05:12Z
url: https://github.com/BurntSushi/ripgrep/issues/1032
synced_at: 2026-01-12T16:13:22Z
```

# deb package is missing completions

---

_@muendelezaji_

#### What version of ripgrep are you using?

ripgrep 0.9.0 (rev e86d3d95c2)
-SIMD -AVX

#### How did you install ripgrep?

ripgrep_0.9.0_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 16.04

#### Describe your question, feature request, or bug.

Thanks for the fix to #842 to which this is somewhat related. Commit 5c80e4a does not add completion files for bash, zsh etc. to the deb package. I understand this was committed after the 0.9.0 release, but as it stands the next release will still lack these completion files, despite this [FAQ entry](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#complete) suggesting otherwise.

Is there a plan to add them to the deb build or should we settle for downloading the release tar & manually copying them to the appropriate location?


---

_Comment by @BurntSushi on 2018-08-30 11:52_

@muendelezaji Ah thanks for noticing this! In theory, it should be fairly straight-forward to add completions. You can see how we manage the existing assets here: https://github.com/BurntSushi/ripgrep/blob/3edeeca6e94b4e812b4ff078cf92f3178436daa3/Cargo.toml#L89-L102

I think the only thing I really need to know here is where to put the completions themselves. They probably vary by shell and I'm not familiar with Debian's convention here.

---

_Label `bug` added by @BurntSushi on 2018-08-30 11:53_

---

_Comment by @okdana on 2018-08-30 14:54_

zsh completion functions for Debian/Ubuntu packages belong in `/usr/share/zsh/vendor-completions`.

I think bash completion functions are supposed to go in `/usr/share/bash-completion/completions`, but i'm less sure of the conventions there. Some packages also add them to `/etc/bash_completion.d`, idk.

Not sure about fish at all, but a quick glance at the package contents suggests `/usr/share/fish/completions`.

---

_Closed by @BurntSushi on 2018-09-07 18:05_

---
