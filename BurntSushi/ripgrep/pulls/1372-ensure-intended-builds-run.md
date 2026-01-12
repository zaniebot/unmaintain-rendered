```yaml
number: 1372
title: Ensure intended builds run
type: pull_request
state: merged
author: jclem
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2019-09-11T12:49:33Z
updated_at: 2019-09-11T13:08:25Z
url: https://github.com/BurntSushi/ripgrep/pull/1372
synced_at: 2026-01-12T18:23:13Z
```

# Ensure intended builds run

---

_@jclem_

Since this workflow specifies `runs-on: ${{matrix.os}}`, the `os` parameter needs to be defined for every build run.

Even though `include` is defined just for `pinned-musl` and `stable`, the other `build`s will still run—`include` is just saying "when these parameters match existing jobs, include some additional ones".

I've also added `sudo` to your `apt-get` command here—when running steps directly on the VM (as opposed to Docker), you're not running as root, and so password-less sudo can be used where needed.

---

_Comment by @BurntSushi on 2019-09-11 13:01_

> Even though include is defined just for pinned-musl and stable, the other builds will still run—include is just saying "when these parameters match existing jobs, include some additional ones".

Ahhhh, I see now. I had commented out some of the builds in order to debug and build out the CI stuff incrementally. Thank you so much for the clarification and fix!

---

_Comment by @jclem on 2019-09-11 13:03_

You're welcome! I'm working on trying to get a fully passing workflow here—I think I need to install `zsh` as well.

---

_Comment by @BurntSushi on 2019-09-11 13:04_

@jclem Oh I wouldn't worry about a fully passing workflow too much. Just getting the CI running again is fantastic! There's quite a bit more work required to move ripgrep over to GA that I still have to do anyway. :-) Thank you though!

---

_Comment by @jclem on 2019-09-11 13:07_

Okay, sounds good! Obviously, feel free to just yank my changes here if that's easier than merging a PR.

---

_Merged by @BurntSushi on 2019-09-11 13:08_

---

_Closed by @BurntSushi on 2019-09-11 13:08_

---
