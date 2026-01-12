```yaml
number: 2702
title: Add ARM build configurations to CI and release workflows
type: pull_request
state: merged
author: yelkarama
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2024-01-05T06:17:31Z
updated_at: 2024-01-06T15:21:35Z
url: https://github.com/BurntSushi/ripgrep/pull/2702
synced_at: 2026-01-12T18:23:14Z
```

# Add ARM build configurations to CI and release workflows

---

_@yelkarama_

Add the following targets:
+ armv7-unknown-linux-gnueabihf
+ armv7-unknown-linux-musleabi
+ armv7-unknown-linux-musleabihf


---

_Comment by @BurntSushi on 2024-01-05 13:15_

Could you say why you opened #2700 and #2701 too? This should only need one PR.

---

_Comment by @yelkarama on 2024-01-05 21:33_

Please ignore the other 2. I was trying to push another commit in a separate branch, but it ended up (twice) in the master branch and it was included in the PR. I had to close them and submit this one instead.

---

_Comment by @BurntSushi on 2024-01-05 21:40_

Hmm okay. Just so you're aware, you can rebase and force push.

---

_Comment by @BurntSushi on 2024-01-06 13:50_

How did you test this? You have to run the release workflow and ensure the binaries are produced in a fork. Right now, it fails because the `armv7-unknown-linux-musleabi` target doesn't seem to have a Docker image:

```
$ docker run -it rustembedded/cross:armv7-unknown-linux-musleabi bash
Unable to find image 'rustembedded/cross:armv7-unknown-linux-musleabi' locally
docker: Error response from daemon: manifest for rustembedded/cross:armv7-unknown-linux-musleabi not found: manifest unknown: manifest unknown.
See 'docker run --help'.
```

Ah okay... It looks like the Cross project changed this around. Sigh. It's now here:

```
$ docker run -it ghcr.io/cross-rs/x86_64-unknown-linux-musl:main bash
root@45607f50d74d:/# exit
```

---

_Comment by @BurntSushi on 2024-01-06 15:18_

Switching everything over to ghcr.io was a bit of a pain. It also turned out that we don't need the old custom images I was maintaining any more. But that was probably true before.

---

_Merged by @BurntSushi on 2024-01-06 15:21_

---

_Closed by @BurntSushi on 2024-01-06 15:21_

---
