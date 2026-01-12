```yaml
number: 698
title: Is there any way to not exclude .gitignore, while respecting .rgignore?
type: issue
state: closed
author: Kyuuhachi
labels:
  - question
assignees: []
created_at: 2017-11-29T18:10:38Z
updated_at: 2017-11-29T19:00:52Z
url: https://github.com/BurntSushi/ripgrep/issues/698
synced_at: 2026-01-12T16:13:22Z
```

# Is there any way to not exclude .gitignore, while respecting .rgignore?

---

_@Kyuuhachi_

I've got a project which has a large directory gitignored, since it contains copyrighted information. However, I'd like my searches to include it. Is this possible?

---

_Comment by @BurntSushi on 2017-11-29 18:12_

Why doesn't the `--no-ignore-vcs` flag do what you want?

---

_Comment by @Kyuuhachi on 2017-11-29 18:52_

I just didn't see it when I looked. It does exactly what I wanted.

---

_Closed by @Kyuuhachi on 2017-11-29 18:52_

---

_Label `question` added by @BurntSushi on 2017-11-29 19:00_

---
