```yaml
number: 1232
title: "couldn't install ripgrep with command \"sudo apt-get install ripgrep\" on ubuntu18.04"
type: issue
state: closed
author: irreallich
labels:
  - invalid
assignees: []
created_at: 2019-03-30T09:42:32Z
updated_at: 2021-01-19T15:02:08Z
url: https://github.com/BurntSushi/ripgrep/issues/1232
synced_at: 2026-01-12T16:13:23Z
```

# couldn't install ripgrep with command "sudo apt-get install ripgrep" on ubuntu18.04

---

_@irreallich_

log:

home-ubuntu:/etc/apt$ sudo apt-get install ripgrep
Reading package lists... Done
Building dependency tree       
Reading state information... Done

No apt package "ripgrep", but there is a snap with that name.
Try "snap install ripgrep"

E: Unable to locate package ripgrep
home-ubuntu:/etc/apt$ uname -a
Linux home-ubuntu 4.15.0-47-generic #50-Ubuntu SMP Wed Mar 13 10:44:52 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
home-ubuntu:/etc/apt$ 


---

_Comment by @BurntSushi on 2019-03-30 11:12_

That's because ripgrep isn't in Ubuntu 18.04, so I'm not sure why you're filing a bug report. The README says that `apt-get install ripgrep` only works for Ubuntu 18.10 or newer.

If you want to use ripgrep on an older Ubuntu, then use the snap if that's your thing. Or follow the instructions in the README and install the binary deb. Or download the raw binary release. Or compile it from source. Lots of options available to you, all of which are in the README.

---

_Closed by @BurntSushi on 2019-03-30 11:12_

---

_Label `invalid` added by @BurntSushi on 2019-03-30 11:12_

---

_Comment by @dmdque on 2019-04-02 08:43_

What would it take to add ripgrep to Ubuntu 18.04?

---

_Comment by @BurntSushi on 2019-04-02 08:45_

I don't know. My guess is that it isn't possible. This isn't the place to ask. I'm not an Ubuntu user and I don't know their packaging policies in detail.

---

_Comment by @Ynjxsjmh on 2021-01-19 15:02_

1. Download the latest `.deb` package from the [release page](https://github.com/BurntSushi/ripgrep/releases)
2. Install it via:

```
sudo dpkg -i ripgrep_x.x.x_amd64.deb  # adapt version number and architecture
```

---
