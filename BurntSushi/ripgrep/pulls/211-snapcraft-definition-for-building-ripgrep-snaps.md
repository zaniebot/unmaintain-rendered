```yaml
number: 211
title: Snapcraft definition for building ripgrep snaps.
type: pull_request
state: closed
author: tasdomas
labels: []
assignees: []
base: master
head: snapcraft-def
created_at: 2016-11-01T22:08:39Z
updated_at: 2016-11-03T21:48:22Z
url: https://github.com/BurntSushi/ripgrep/pull/211
synced_at: 2026-01-12T18:23:12Z
```

# Snapcraft definition for building ripgrep snaps.

---

_@tasdomas_

Snaps are a new application packaging approach that allows for better isolation. This PR introduces a snapcraft.yaml file that allows building ripgrep snaps using the [snapcraft](http://snapcraft.io) tool.

---

_Comment by @BurntSushi on 2016-11-01 23:11_

Hmm. I'm honestly not sure when I should merge this or not. I know that at least as of right now, I have a hard enough time maintaining the brew formula (I'm not a Mac user), so I worry that this will bitrot easily. In particular, I've never used or heard of this tool.

Is there an established way of maintaining snaps out-of-tree? I think that's what I would prefer at the moment.


---

_Comment by @anuragsoni on 2016-11-03 02:32_

@tasdomas @BurntSushi Since snapcraft (the snap package builder) can pull source from git it can live out-of-tree. I was actually trying out building a snap package today before I saw this pull request :)

I believe the only change required to the yml file would be the following.

```
parts:
  rg:
    plugin: rust
    source: git://github.com/BurntSushi/ripgrep
    source-tag: 0.2.6

```


---

_Comment by @anuragsoni on 2016-11-03 02:36_

Also, i'm not sure how common is it for snap packages to be used outside of Ubuntu for now, but I guess now they offer installation on arch, fedora and opensuse so that should cover a lot of different flavors! 


---

_Comment by @tasdomas on 2016-11-03 12:39_

@BurntSushi, @anuragsoni - having the snap build out of tree is fine. 


---

_Comment by @BurntSushi on 2016-11-03 21:48_

@tasdomas @anuragsoni Thanks for the info. I think I'd rather take that route for now!


---

_Closed by @BurntSushi on 2016-11-03 21:48_

---
