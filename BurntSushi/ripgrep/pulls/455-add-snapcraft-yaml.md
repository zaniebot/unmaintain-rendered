```yaml
number: 455
title: Add snapcraft.yaml
type: pull_request
state: merged
author: ChrisMacNaughton
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-04-20T19:02:03Z
updated_at: 2018-02-09T15:35:26Z
url: https://github.com/BurntSushi/ripgrep/pull/455
synced_at: 2026-01-12T18:23:13Z
```

# Add snapcraft.yaml

---

_@ChrisMacNaughton_

[Snapcraft](https://snapcraft.io/) makes Linux packaging very simple in a cross-distro
way. This adds the snapcraft.yaml file to setup a snap of ripgrep

---

_Comment by @ChrisMacNaughton on 2017-04-20 19:03_

I have already registered the snap package name ripgrep, will pass ownership of that over and am happy to help with setting up automated builds!

---

_Comment by @phrohdoh on 2017-05-03 18:33_

Why snapcraft over flatpack?

I know next to nothing about both but there are multiple offerings available and we should probably weight the pros and cons before selecting one (I believe providing both defeats the purpose).

---

_Comment by @BurntSushi on 2017-05-03 18:37_

This isn't the place to debate snapcraft vs flatpak.

While I appreciate this PR, I know nothing about snapcraft/flatpak and I don't have any plans to use them in the immediate future, which makes it hard for me to maintain. Is it necessary for `snapcraft.yml` to be in this repo?

(I did make an exception for Brew since so many people are using it, although even that will eventually go away once SIMD works on stable Rust.)

---

_Comment by @ChrisMacNaughton on 2017-05-04 12:24_

I'd rather not discuss one versus the other as well, although a benefit to snaps is that they are available by default on any Ubuntu >= 16.04.

Regarding inclusion in the repository, it is possible to use snaps without including them in the repository but it is a bit easier to deploy into a testing channel with snaps from CI if it's in tree.

---

_Merged by @BurntSushi on 2017-05-04 12:30_

---

_Closed by @BurntSushi on 2017-05-04 12:30_

---

_Comment by @BurntSushi on 2017-05-04 12:30_

All right. I'll merge this for now and we can see how it goes. Thanks!

---

_Comment by @ChrisMacNaughton on 2017-05-04 12:36_

@BurntSushi Feel free to ping me (or the snapcraft team) if things are needed to help maintaining it :)

---

_Comment by @BurntSushi on 2018-02-09 15:19_

@ChrisMacNaughton I have a question about this if you don't mind. I haven't updated ripgrep's `snapcraft.yaml` file since I merged this PR. When I went to install ripgrep via snap on my Ubuntu 17.10 machine, I was surprised to see that it installed ripgrep 0.7.1, which is the latest release. How does that work? What specific purpose does the `snapcraft.yaml` file in this repo serve? I tried reading the Snapcraft docs at snapcraft.io, but I wasn't able to discover any answers to my questions.

---

_Comment by @ChrisMacNaughton on 2018-02-09 15:22_

@BurntSushi looks like somebody is maintaining a build:
```
$ snap search ripgrep
Name       Version  Developer  Notes  Summary
rg-casept  0.5.2    casept     -      ripgrep combines the usability of ag with the raw speed of grep.
rg         0.7.1    tasdomas   -      a command line search tool
```
To me, the best solution would be to actually setup ripgrep to run on the launchpad builders automatically by setting it up at https://build.snapcraft.io

---

_Comment by @BurntSushi on 2018-02-09 15:27_

@ChrisMacNaughton I guess I'm just *super* confused:

1. The version in ripgrep's `snapcraft.yaml` file is `0.5.1`, which doesn't match either of the versions in your search results.
2. The `snapcraft.yaml` file does not seem to contain any instructions on how to actually build ripgrep. How is it working at all?
3. Earlier in this PR you mentioned that you already registered the name `ripgrep`, but the two ripgrep related packages in snap appear to be `rg-casept` and `rg`.

Maybe the answer is that I need to spend more time in the docs?

---

_Comment by @ChrisMacNaughton on 2018-02-09 15:32_

1. Anybody can create a snapcraft.yaml, and manage the building and publishing it; ideally upstreams (such as ripgrep, here) will publish their own snapcraft.yaml, and can have https://build.snapcraft.io automatically build on push
2. The magic for snapcraft to know how to build ripgrep is contained [here](https://github.com/BurntSushi/ripgrep/blob/master/snapcraft.yaml#L10), it uses snapcraft's [Rust plugin](https://docs.snapcraft.io/reference/plugins/rust)
3. As a corollary to 1, anybody can register a name and upload anything to it, I currently have ripgrep (would rather hand control of that over to you, frankly) which means that the others are named more interestingly :-)

If you'd like a more real-time discussion, I am on IRC as well as icey

---

_Comment by @BurntSushi on 2018-02-09 15:35_

@ChrisMacNaughton Awesome, OK! I'm at work, so not on IRC, but I'd be happy to take over the `ripgrep` name and get this setup. I'm not sure what that process looks like though!

---
