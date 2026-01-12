```yaml
number: 2880
title: fix the mistake of overriding mode
type: pull_request
state: closed
author: QWZeng
labels: []
assignees: []
base: master
head: qingwen/fix
created_at: 2024-09-03T11:10:36Z
updated_at: 2025-07-26T15:02:14Z
url: https://github.com/BurntSushi/ripgrep/pull/2880
synced_at: 2026-01-12T18:23:14Z
```

# fix the mistake of overriding mode

---

_@QWZeng_

a non-searching mode can override non-searching mode.
    It’s likely just a mistake or typo :
    if !matches!(*self, Mode::Search(_));
    it should be：
    if !matches!(new, Mode::Search(_))

---

_Comment by @BurntSushi on 2024-09-03 11:29_

I think the docs are wrong here.

Also, why are you opening multiple PRs? I see this one and #2879.

---

_Comment by @QWZeng on 2024-09-04 07:32_

> I think the docs are wrong here.
> 
> Also, why are you opening multiple PRs? I see this one and #2879.



> I think the docs are wrong here.
> 
> Also, why are you opening multiple PRs? I see this one and #2879.

Excuse me, I'm not sure what did you mean "the docs wrong here",  the comments are wrong in code?
I've closed the #2879 PR, sorry for that.

---

_Comment by @BurntSushi on 2025-07-26 15:02_

From what I can tell, the code on master is correct, so I'm going to close this PR.

I may still be wrong. But you'll need to demonstrate that through actual examples. Also, this PR changes the results of existing tests, including things that were specifically commented on as intended behavior. So that would need to be justified as well.

Finally, please don't make spurious updates to dependencies. PRs should be as small as possible. There's no reason to include a `cargo update` in the patch.

---

_Closed by @BurntSushi on 2025-07-26 15:02_

---
