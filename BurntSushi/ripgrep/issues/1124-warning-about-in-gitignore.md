```yaml
number: 1124
title: "Warning about ** in .gitignore"
type: issue
state: closed
author: sgraham
labels:
  - duplicate
assignees: []
created_at: 2018-11-27T21:03:08Z
updated_at: 2018-11-28T00:47:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1124
synced_at: 2026-01-12T16:13:23Z
```

# Warning about ** in .gitignore

---

_@sgraham_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)

#### How did you install ripgrep?

From github releases page.

#### What operating system are you using ripgrep on?

Ubuntu 18.04-ish

#### Describe your question, feature request, or bug.

When `rg`ing in a large project that pulls https://github.com/catapult-project/catapult/blob/master/telemetry/bin/.gitignore#L2 in as a dependency, I get:

```
./third_party/catapult/telemetry/bin/.gitignore: line 2: error parsing glob '!**.sha1': invalid use of **; must be one path component
```

in my output. I didn't scrutinize the .gitignore spec (is there one?) to decide whether that project is wrong or `rg` is wrong, but it'd be nice if it didn't clutter the output.

---

_Comment by @sgraham on 2018-11-27 21:04_

Oops, sorry, I see this is an old duplicate of https://github.com/BurntSushi/ripgrep/issues/507 and others now. Well, +1 I guess. :)

---

_Closed by @sgraham on 2018-11-27 21:04_

---

_Label `duplicate` added by @BurntSushi on 2018-11-28 00:47_

---
