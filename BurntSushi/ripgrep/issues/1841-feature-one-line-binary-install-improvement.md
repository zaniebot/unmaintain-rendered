```yaml
number: 1841
title: "[Feature] One-line binary install improvement"
type: issue
state: closed
author: thernstig
labels:
  - wontfix
assignees: []
created_at: 2021-04-02T15:27:35Z
updated_at: 2021-04-06T16:11:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1841
synced_at: 2026-01-12T16:13:24Z
```

# [Feature] One-line binary install improvement

---

_@thernstig_

#### Describe your feature request

I have a plethora of great statically linked binary installs in Linux (Ubuntu). These include `ripgrep`, `exa`, `bat`, `fd` etc. None shines as well as `chezmoi` when it comes to the one-line installer.

I recommend to re-implement the one-line binary installer to follow that of `chezmoi`. See this documentation: https://github.com/twpayne/chezmoi/blob/master/docs/INSTALL.md#one-line-binary-install

In effect, what it does, is to download the latest statically linked binary for your operating system and architecture into `./bin`
This means I can put myself in whatever dir I want and just call the installer and it will be installed into the bin path of my choosing. Be that `~/bin` or `.local/bin` or whatever I prefer.


---

_Comment by @BurntSushi on 2021-04-02 16:07_

Thanks for the idea. I admit that it looks convenient, but:

1. I do not want the maintenance burden of platform specific scripts.
2. I would generally prefer that folks get their ripgrep install from their friendly neighborhood package manager. Although, admittedly, in some Linux distros this might result in an ancient and unsupported version of ripgrep, that's kind of what you sign up for when using such distros.

So I think I'm going to pass on this. FWIW, I do have my own shell script for installing the latest version of ripgrep on the few Ubuntu machines I use: https://github.com/BurntSushi/dotfiles/blob/master/bin/ubuntu-install-ripgrep

---

_Label `wontfix` added by @BurntSushi on 2021-04-02 16:08_

---

_Closed by @BurntSushi on 2021-04-02 16:08_

---

_Comment by @thernstig on 2021-04-03 09:42_

Thanks for the answer @BurntSushi. I agree here the general problem is 2) and that I get ancient versions. It's so bad. I've gotten to love how chezmoi does this. I install my tools into a plethora of machines (devcontainers, VMs etc.) and it takes me ages each time. I always want the latest versions, so I always download the binaries and put them in a bin dir of my choice (as a backup I install the .deb packages if the first option does not work).

The chezmoi installer have proven to be a lifesaver/timesaver. So tired of all tools, outdated things etc. that it's now begun to be my number one way to install statically linked binaries.

Note: Other projects are looking into this (adding a chezmoi-style one-lien installer) as I posted a similar question to other places.

Maybe I should create such a project myself that works for all distros and any statically linked binaries hum hum.

In the meantime, I will steal https://github.com/BurntSushi/dotfiles/blob/master/bin/ubuntu-install-ripgrep with pride 😆 

---

_Comment by @BurntSushi on 2021-04-03 11:26_

> Maybe I should create such a project myself that works for all distros and any statically linked binaries hum hum.

Yes, I suspect that might be a good idea. There's likely enough in common among github projects that the vast majority of said script would be unchanged between them. Everyone probably has their own naming conventions though, so you might need a very light amount of project specific data. But I think most of the script can be generic.

---

_Comment by @BurntSushi on 2021-04-03 11:28_

And just to be clear, for me, (1) is probably the main problem. These types of things are features that beget features. Installation tends to be highly personal and people tend to want to be able to customize it. Maybe `./bin` isn't the right thing all the time, so you need a way to override it. What about docs and man pages? Should those get installed too? And then of course, how do I uninstall the tool when the time comes? And probably other things I'm not thinking about too.

(You could declare all of these things out of scope. But once you have users, take it from someone who has been there: it's hard to say No.)

---

_Comment by @thernstig on 2021-04-06 15:37_

@BurntSushi A question for you. The README.md recommends this for Debian/Ubuntu:

> ```sh
> $ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/12.1.1/ripgrep_12.1.1_amd64.deb
> $ sudo dpkg -i ripgrep_12.1.1_amd64.deb
> ```

But I tried and this works equally well to use and put directly in a bin path: ripgrep-12.1.1-x86_64-unknown-linux-musl.tar.gz

What are the advantages of the .deb package? Manpages?

---

_Comment by @BurntSushi on 2021-04-06 15:41_

The advantages are the same as any other deb package. Man page is one of them. Another is that the binary is available to all users on the system. Other docs are also installed. It also provides a way to easily uninstall the program.

---

_Comment by @thernstig on 2021-04-06 16:11_

@BurntSushi Thanks for the answer. Every considered recommending apt instead of using dpkg? https://unix.stackexchange.com/questions/159094/how-to-install-a-deb-file-by-dpkg-i-or-by-apt - seems to be recommended nowadays.

---
