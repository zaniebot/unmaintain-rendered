```yaml
number: 979
title: Tweak snapcraft.yaml
type: pull_request
state: merged
author: mati865
labels: []
assignees: []
merged: true
base: master
head: snap_fix
created_at: 2018-07-13T10:42:19Z
updated_at: 2018-07-21T17:28:28Z
url: https://github.com/BurntSushi/ripgrep/pull/979
synced_at: 2026-01-12T18:23:13Z
```

# Tweak snapcraft.yaml

---

_@mati865_

I know you don't have time to work with snaps, is there any way I could help you?

---

_Comment by @BurntSushi on 2018-07-13 10:54_

> I know you don't have time to work with snaps, is there any way I could help you?

Seems unlikely. My position has changed on this from "I don't have time" to "actively discourage the use of snap." I've never been able to figure out how to get it working, and moreover, the use of snap has generated so many bug reports that I can do nothing about that I've had to put a special call out about snap in the issue template and I removed it from the list of suggested installation instructions: https://github.com/BurntSushi/ripgrep/commit/6ffb4b7466c914a2683847b5fb4e30ea16749e94#diff-0b6c1416b1293274c4e2d11fb49e1795

Maybe these problems are fixable, but I just don't have the bandwidth to fix them. For example, I understand exactly nothing about the change that's being submitted here. Why is it necessary? What does it do? What does it fix? None of this is obvious to me and none of it is in the commit message.

Sorry to be so harsh sounding. I appreciate folks trying to increase the number of ways that ripgrep can be installed. But snap has been nothing but a headache for me.

---

_Comment by @mati865 on 2018-07-13 11:45_

@BurntSushi there is extremely simple reason why it didn't work before as I wrote in https://github.com/BurntSushi/ripgrep/issues/902#issuecomment-404798881

> For example, I understand exactly nothing about the change that's being submitted here. Why is it necessary? What does it do? What does it fix? None of this is obvious to me and none of it is in the commit message.

Snap by default creates wrappers for running package which can be useful for other apps to configure their dynamic libs location but totally redundant for static libs. This change gives very small benefit to startup time.
I can add commit message.

I haven't used this option but if https://dashboard.snapcraft.io/snaps/ripgrep/collaboration/ does what it looks to do I could take care of managing releases.

I'll understand if you don't want to touch it anymore, in this case feel free to close this PR.

---

_Comment by @BurntSushi on 2018-07-13 11:55_

@mati865 If you can improve the commit message and ideally include a link to the relevant snap documentation (so that I can start learning how to diagnose these problems myself), then I'm happy to merge this.

---

_Comment by @mati865 on 2018-07-13 14:04_

Snap documentation is a bit lacking, discussion about this feature is available [here](https://forum.snapcraft.io/t/telling-snapcraft-to-skip-generating-wrapper-scripts/1635).

> so that I can start learning how to diagnose these problems myself

Those issues were related to AppArmor.
As long as this snap is using `classic` confinement AppArmor will let it do everything and `ripgrep` will behave just like normal app.

This section doesn't apply for `classic` snaps:
To improve security snaps are confined and given only explicitly requested (just like Android or iOS apps). Problematic snap `rg`requested only `home` interface which gives access only to user files (hence the permission errors). List of available interfaces is [here](https://docs.ubuntu.com/core/en/reference/interfaces/).
AppArmor debugging guide is available [here](https://docs.ubuntu.com/core/en/guides/intro/security)
To make `ripgrep` confined it would need interface giving read access to everything user can read. The problem is interface like that doesn't exists ([request was filed](https://bugs.launchpad.net/snappy/+bug/1603838)). Of course one [can create interface](http://www.zygoon.pl/2016/08/creating-your-first-snappy-interface.html) like that but it wouldn't differ from `classic` confinement too much and Ubuntu developer made valid point `the request is at odds with snappy's application isolation goals`.

---

_Merged by @BurntSushi on 2018-07-21 17:28_

---

_Closed by @BurntSushi on 2018-07-21 17:28_

---
