```yaml
number: 2619
title: Release using current regex crates.
type: issue
state: closed
author: plugwash
labels: []
assignees: []
created_at: 2023-10-03T21:48:17Z
updated_at: 2023-10-03T21:51:28Z
url: https://github.com/BurntSushi/ripgrep/issues/2619
synced_at: 2026-01-12T16:13:24Z
```

# Release using current regex crates.

---

_@plugwash_

Hi.

We are currently in the process of upgrading the regex crates (regex, regex-syntax, regex-automata, aho-corasick etc). However the current release of grep-regex depends on an old version of regex-syntax.

I've looked at the commit history to investigate the possibility of cherry-picking the changes, but the number of patches involved seems significant and it's not clear to me if things are in a release-worthy state. 

Is there any chance of a release of the grep crates that uses the latest version of the regex crates in the not too distant future?


---

_Comment by @BurntSushi on 2023-10-03 21:51_

https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#when-is-the-next-release

I am actually working on the ripgrep 14 release. Things are still shifting quite a bit, so pulling patches probably isn't the greatest idea but is doable if necessary.

---

_Closed by @BurntSushi on 2023-10-03 21:51_

---
