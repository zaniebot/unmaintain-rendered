```yaml
number: 1172
title: should the next release be ripgrep 11 or ripgrep 0.11?
type: issue
state: closed
author: BurntSushi
labels:
  - question
assignees: []
created_at: 2019-01-23T01:02:44Z
updated_at: 2019-04-14T23:29:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1172
synced_at: 2026-01-12T16:13:23Z
```

# should the next release be ripgrep 11 or ripgrep 0.11?

---

_@BurntSushi_

Originally, I had thought that ripgrep would reach 1.0 some day and sit there for a long time with no backwards incompatible changes. In practice, ripgrep has seen very few backwards incompatible changes and no complaints about them thus far as far as I know. Instead of trying to commit to a 1.0 release, I thought it might be nice to just move the minor version over to the major version and subsequently be more flexible about bumping the major version while retaining our fairly conservative policy with regard to backwards incompatible changes.

Thoughts?

---

_Comment by @okdana on 2019-01-23 01:41_

So like the major version would just get bumped for each significant new release, similar to how Postgres and Firefox do it, rather than using a semver-style system?

I wouldn't mind it, personally, for whatever that's worth. I don't maintain any projects that rely on ripgrep where semver might be useful, though

---

_Comment by @BurntSushi on 2019-01-23 01:45_

> So like the major version would just get bumped for each significant new release, similar to how Postgres and Firefox do it, rather than using a semver-style system?

Pretty much, yes. I would still adhere to semver though. I'd just only do breaking changes in major version releases. Basically, I want to make bumping the major version a normal occurrence. Occasionally there may be a breaking change, but I feel confident breaking changes can be kept very small. 

---

_Comment by @okdana on 2019-01-23 01:58_

Are you going to change anything about the versioning on the libraries (`ignore`, `grep`, &c.), too? Or just ripgrep itself?

---

_Comment by @BurntSushi on 2019-01-23 02:05_

@okdana Ah, no, def not. Just ripgrep itself.

---

_Label `question` added by @BurntSushi on 2019-01-24 01:10_

---

_Closed by @BurntSushi on 2019-04-14 23:29_

---
