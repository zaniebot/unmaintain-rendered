```yaml
number: 2167
title: "ci: fix failing nightly-arm build on ci workflow"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-ci
created_at: 2022-03-21T12:13:03Z
updated_at: 2022-03-21T12:59:09Z
url: https://github.com/BurntSushi/ripgrep/pull/2167
synced_at: 2026-01-12T18:23:14Z
```

# ci: fix failing nightly-arm build on ci workflow

---

_@BurntSushi_

This commit updates the Ubuntu install script to include brotli and
zstd, which are needed for tests.

We also fix the Ubuntu install script to work in environments that
don't have 'sudo'. Instead of creating a totally separate script, we
preserve a single point of truth for these things and just make the
script a bit more flexible.

NOT seen in this commit is that we have built and updated the arm Docker
image. I'm hoping this fixes the GLIBC version issues we're seeing in
CI.

Fixes #2130, Closes #2132

---

_Comment by @BurntSushi on 2022-03-21 12:49_

@arcsi42 I'm going to go ahead with this PR over #2132. I did base this off of #2132, but did however simplify it quite a bit. Namely, I just pushed an updated Docker image and that seems to have fixed things. (To be honest, I'm not quite sure how pinning it inside the `Dockerfile` would have helped. Because the `Cross.toml` file in the root of the repo should instruct `cross` to use my own image anyway.)

I've also keep just one `ubuntu-install-packages` script by making it a bit more flexible.

Thanks again for doing all of the leg work here and diagnosing the problem! It was a huge help.

---

_Merged by @BurntSushi on 2022-03-21 12:59_

---

_Closed by @BurntSushi on 2022-03-21 12:59_

---

_Branch deleted on 2022-03-21 12:59_

---
