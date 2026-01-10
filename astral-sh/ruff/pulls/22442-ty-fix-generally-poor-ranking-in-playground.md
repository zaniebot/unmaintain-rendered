```yaml
number: 22442
title: "[ty] Fix generally poor ranking in playground completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - playground
  - server
assignees: []
merged: true
base: main
head: ag/set-is-incomplete-playground
created_at: 2026-01-07T17:14:20Z
updated_at: 2026-01-09T14:53:56Z
url: https://github.com/astral-sh/ruff/pull/22442
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Fix generally poor ranking in playground completions

---

_Pull request opened by @BurntSushi on 2026-01-07 17:14_

We enabled [`CompletionList::isIncomplete`] in our LSP server a while back
in order to have more of a say in how we rank and filter completions.
When it isn't set, the client tends to ask for completions less
frequently and will instead do its own filtering.

But... we did not enable it for the playground. Which I guess didn't
result in anything noticeably bad until we started limiting completions
to 1,000 suggestions. This meant that if the _initial_ completion
response didn't include the ultimate desired answer, then it would never
show up in the results until the client requested completions again.
This in turn led to some very poor completions in some cases.

This all gets fixed by simply enabling `isIncomplete` for Monaco.

Fixes astral-sh/ty#2340

[`CompletionList::isIncomplete`]: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionList


---

_Comment by @BurntSushi on 2026-01-07 17:14_

Demo:

https://github.com/user-attachments/assets/13f0f3a6-d011-4059-a59e-912ba35cb530



---

_Label `bug` added by @BurntSushi on 2026-01-07 17:15_

---

_Label `playground` added by @BurntSushi on 2026-01-07 17:15_

---

_Label `server` added by @BurntSushi on 2026-01-07 17:15_

---

_Review requested from @MichaReiser by @BurntSushi on 2026-01-07 17:15_

---

_Review requested from @AlexWaygood by @BurntSushi on 2026-01-07 17:15_

---

_@MichaReiser approved on 2026-01-07 17:24_

---

_Merged by @BurntSushi on 2026-01-07 17:25_

---

_Closed by @BurntSushi on 2026-01-07 17:25_

---

_Branch deleted on 2026-01-07 17:25_

---
