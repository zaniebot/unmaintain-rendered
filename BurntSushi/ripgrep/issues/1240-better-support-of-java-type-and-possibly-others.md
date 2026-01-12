```yaml
number: 1240
title: "Better support of `java` type (and possibly others)"
type: issue
state: closed
author: hupfdule
labels: []
assignees: []
created_at: 2019-04-09T14:53:02Z
updated_at: 2019-04-09T15:10:14Z
url: https://github.com/BurntSushi/ripgrep/issues/1240
synced_at: 2026-01-12T16:13:23Z
```

# Better support of `java` type (and possibly others)

---

_@hupfdule_

#### What version of ripgrep are you using?

0.10.0

#### How did you install ripgrep?

As debian package from the official debian buster repository.

#### What operating system are you using ripgrep on?

Debian Linux (Buster)

#### Describe your question, feature request, or bug.

ripgrep supports the type `java`, but it only includes `*.java` and `*.jsp`. However, properties for java files are usually stored in `*.properties` files. (https://beyondgrep.com/)[ack-grep] also supports a `--java` option, which includes this. ripgrep currently has no support for `*.properties` files at all. This should be changed and I think included in the `java` type.
See the bottom of https://beyondgrep.com/documentation/ for the type supported by ack-grep.


---

_Comment by @BurntSushi on 2019-04-09 14:58_

Thanks for the issue. Generally, I don't keep issues for file types open because they are typically trivial additions. Please feel free to submit a PR with the necessary changes to `ignore/src/types.rs`: https://github.com/BurntSushi/ripgrep/blob/308819fb1fd47ad8a27ab82401eca69f24971ca0/ignore/src/types.rs#L159

As a work-around to the next release, you can add to ripgrep's built-in file types. See the guide: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-file-types

---

_Closed by @BurntSushi on 2019-04-09 14:58_

---

_Comment by @hupfdule on 2019-04-09 15:10_

I did as you requested: #1242
:-)

---
