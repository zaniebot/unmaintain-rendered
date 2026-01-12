```yaml
number: 28
title: Add VB files to default type list
type: pull_request
state: merged
author: zackschuster
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2016-09-23T18:45:03Z
updated_at: 2016-09-24T02:34:21Z
url: https://github.com/BurntSushi/ripgrep/pull/28
synced_at: 2026-01-12T18:23:12Z
```

# Add VB files to default type list

---

_@zackschuster_

**Use-case**
While not a vogue technology, VB is still commonly taught in many university settings and used in many commercial environments. Working with VB files out-of-the-box would thusly provide a lot of potential value to `ripgrep` users.

**Example**
I'm working on converting a legacy app to a modern infrastructure. The legacy app mixes CS and VB files liberally, so I always need to check both when doing command line string searches. For portability, it would be nice to just be able to ask for `-tcsharp -tvb` without registering with `--type-add` first.

**Tests**
I didn't notice any coverage aimed at this part of the code, but if I'm mistaken I'll amend the PR.


---

_Merged by @BurntSushi on 2016-09-24 02:33_

---

_Closed by @BurntSushi on 2016-09-24 02:33_

---

_Comment by @BurntSushi on 2016-09-24 02:33_

There's no need to be in vogue to make it on to the default type list. :-) VB is welcome.


---

_Comment by @zackschuster on 2016-09-24 02:34_

thank you! :)


---
