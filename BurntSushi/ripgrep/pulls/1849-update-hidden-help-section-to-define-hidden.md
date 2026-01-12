```yaml
number: 1849
title: Update --hidden help section to define hidden
type: pull_request
state: merged
author: dbjorge
labels: []
assignees: []
merged: true
base: master
head: hidden-docs
created_at: 2021-04-15T22:58:31Z
updated_at: 2021-04-15T23:31:51Z
url: https://github.com/BurntSushi/ripgrep/pull/1849
synced_at: 2026-01-12T18:23:14Z
```

# Update --hidden help section to define hidden

---

_@dbjorge_

Fixes #1847

If you'd prefer to leave out the `--no-ignore-dot` part, I'm happy to update the PR accordingly

Thanks!

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1978 on 2021-04-15 23:00_

Just looking at other text, I think you just want `*not*` here instead of `**not**`. (This unfortunately isn't Markdown. It's AsciiDoc that gets converted into roff.)

---

_@BurntSushi reviewed on 2021-04-15 23:00_

---

_@dbjorge reviewed on 2021-04-15 23:01_

---

_Review comment by @dbjorge on `crates/core/app.rs`:1978 on 2021-04-15 23:01_

This is technically inaccurate in that a .ignore file with negation patterns could affect whether ripgrep ignores dotfiles, but it felt overly verbose to clarify that as part of the note. If you have a better wording proposal that is more accurate, I'd be happy to update accordingly

---

_Review comment by @dbjorge on `crates/core/app.rs`:1978 on 2021-04-15 23:02_

Oops, fixed!

---

_@dbjorge reviewed on 2021-04-15 23:02_

---

_@BurntSushi approved on 2021-04-15 23:20_

---

_Merged by @BurntSushi on 2021-04-15 23:21_

---

_Closed by @BurntSushi on 2021-04-15 23:21_

---

_Comment by @dbjorge on 2021-04-15 23:31_

Thanks very much for the quick review!

---
