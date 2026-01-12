```yaml
number: 2103
title: "add yum-utils for Red Hat & CentOS"
type: pull_request
state: closed
author: strouja
labels:
  - rollup
assignees: []
base: master
head: patch-1
created_at: 2021-12-16T02:19:53Z
updated_at: 2023-07-08T22:53:09Z
url: https://github.com/BurntSushi/ripgrep/pull/2103
synced_at: 2026-01-12T18:23:14Z
```

# add yum-utils for Red Hat & CentOS

---

_@strouja_

I also suggest removing the `$` in front of each command so end users can simply copy and paste the line without having to edit and remove the `$`
If you do not like my suggestions I won't be offended.

`yum-config-manager` will fail unless `yum-utils` is installed as `yum-config-manager` is installed with `yum-utils`.  If `yum-utils` is already installed then the `yum install -y yum-utils` is harmless

Great job with the tool and I love it.  Thanks for everything

---

_Comment by @BurntSushi on 2023-07-08 18:12_

I ended up just adding this in myself because I didn't want to bring in the changes to every other command too. Thanks for the tip!

---

_Closed by @BurntSushi on 2023-07-08 18:12_

---

_Label `rollup` added by @BurntSushi on 2023-07-08 18:12_

---

_Reopened by @BurntSushi on 2023-07-08 18:12_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
