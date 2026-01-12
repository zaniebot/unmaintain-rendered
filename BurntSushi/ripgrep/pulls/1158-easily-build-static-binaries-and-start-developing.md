```yaml
number: 1158
title: Easily build static binaries and start developing
type: pull_request
state: closed
author: geropl
labels: []
assignees: []
base: master
head: master
created_at: 2019-01-10T16:49:36Z
updated_at: 2019-01-22T17:00:16Z
url: https://github.com/BurntSushi/ripgrep/pull/1158
synced_at: 2026-01-12T18:23:13Z
```

# Easily build static binaries and start developing

---

_@geropl_

With this PR everyone browsing GitHub can easily try out ripgrep and start contributing by using Gitpod, a free dev environment service with rich GitHub integration.

The PR contains the config for Gitpod that automatically executes `cargo build` and `cargo test --all` and allows to build static binaries using `cargo build --release --target x86_64-unknown-linux-musl`, without the need for any setup on the user side.

Here is a screenshot:
![image](https://user-images.githubusercontent.com/32448529/50983257-6548da00-14ff-11e9-97fa-58d0549b91bf.png)


[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io#snapshot/74880e80-9911-4ca2-8b91-490228153d2b)


---

_Comment by @svenefftinge on 2019-01-22 10:33_

Gitpod / Theia is actually using ripgrep for search. Full circle! :)

---

_Comment by @BurntSushi on 2019-01-22 17:00_

I'm happy to hear that Theia is using ripgrep! :-)

With that said, I don't think this functionality is a good fit for ripgrep. I'm not aware of this being an issue in practice, and I'd prefer not to maintain the Dockerfile for this.

---

_Closed by @BurntSushi on 2019-01-22 17:00_

---
