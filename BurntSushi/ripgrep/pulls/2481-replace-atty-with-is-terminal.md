```yaml
number: 2481
title: Replace atty with is-terminal
type: pull_request
state: closed
author: tottoto
labels: []
assignees: []
base: master
head: replace-atty-with-is-terminal
created_at: 2023-03-30T14:53:09Z
updated_at: 2023-03-30T15:14:08Z
url: https://github.com/BurntSushi/ripgrep/pull/2481
synced_at: 2026-01-12T18:23:14Z
```

# Replace atty with is-terminal

---

_@tottoto_

Replaces atty with is-terminal as atty is unmaintained.

---

_Comment by @BurntSushi on 2023-03-30 15:04_

Thanks for the PR! I'm going to stick with `atty` for now. The `is-terminal` crate brings in too many dependencies IMO.

It does look like the `IsTerminal` API in std will be landing soon. Once that's stable, we can switch to that and drop `atty` completely.

---

_Closed by @BurntSushi on 2023-03-30 15:04_

---

_Branch deleted on 2023-03-30 15:11_

---

_Comment by @tottoto on 2023-03-30 15:14_

I understand. It makes sense. Thanks for your review.

---
