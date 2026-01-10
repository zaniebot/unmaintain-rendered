```yaml
number: 11823
title: "`ruff server`: Improve error message when a command is run on an unavailable document"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/exe-command-warn
created_at: 2024-06-10T14:14:23Z
updated_at: 2024-06-11T19:00:24Z
url: https://github.com/astral-sh/ruff/pull/11823
synced_at: 2026-01-10T21:56:00Z
```

# `ruff server`: Improve error message when a command is run on an unavailable document

---

_Pull request opened by @snowsignal on 2024-06-10 14:14_

## Summary

Fixes #11744.

We now show a distinct popup message when we fail to get a document snapshot during command execution. This message more clearly communicates the issue to the user, instead of a generic "ruff encountered an error" message.

## Test Plan

Try running `Fix all auto-fixable problems` on an incompatible file (for example: `settings.json`). You should see the following popup message:
<img width="456" alt="Screenshot 2024-06-11 at 11 47 16 AM" src="https://github.com/astral-sh/ruff/assets/19577865/3a28e3d7-3896-4dd0-b117-f87300dd3b68">



---

_Label `server` added by @snowsignal on 2024-06-10 14:14_

---

_Comment by @codspeed-hq[bot] on 2024-06-10 14:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/server/exe-command-warn)

### Merging #11823 will **not alter performance**

<sub>Comparing <code>jane/server/exe-command-warn</code> (94efad9) with <code>main</code> (4e9d771)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-06-10 14:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-06-10 14:34_

Have you looked into how other LSPs handle this? I'm kind of surprised that VS Code sends us this request in the first place, considering that we only subscribed for Python files. 

---

_Comment by @snowsignal on 2024-06-10 16:34_

@MichaReiser VS Code makes a command available for every file, unless you add a filter (which isn't possible for us).

Here's how other langauge servers handle this:
* ES Lint simply ignores the command (see https://github.com/astral-sh/ruff-vscode/pull/487#pullrequestreview-2098123258)
* With `rust-analyzer`, some commands are ignored, while others show a very visible error message:
<img width="253" alt="Screenshot 2024-06-10 at 9 29 41 AM" src="https://github.com/astral-sh/ruff/assets/19577865/4ec65064-9729-4a6c-ad2f-86000933311b">

* `insta` shows a visible error message:
<img width="451" alt="Screenshot 2024-06-10 at 9 33 06 AM" src="https://github.com/astral-sh/ruff/assets/19577865/2998d847-88bc-4973-93c5-f518c3947b29">

Maybe we should just show a more descriptive error message, then? I'm not too opinionated either way.

---

_Comment by @MichaReiser on 2024-06-10 17:31_

Hmm, interesting. My main concern is that not finding the document is, in most cases, a valid error. For example, it was the root cause for untitled files, and having the server fail instead of silently ignoring the error was a good case. It makes me worried that it will be harder to debug such kind of errors in the future. So yes, maybe showing a more user visible error would help us to still catch the error that something went wrong. 

Disclaimer: It's not entirely clear when and how often this kind of requests are sent by VS code. Is it only when a user manually requests e.g. import sorting or does it also happen when they have organize imports enabled on save? Is there a way for us to narrow the requests for which we should apply the more lenient handling?

---

_Comment by @snowsignal on 2024-06-10 17:51_

> Disclaimer: It's not entirely clear when and how often this kind of requests are sent by VS code. Is it only when a user manually requests e.g. import sorting or does it also happen when they have organize imports enabled on save?

It's whenever a command gets run, which usually has to be done through the command palette (so, manual execution). I don't think there's a way to run a command on save built-in to VS Code - usually, those are done with source actions, and are only enabled for a specific language.

> Is there a way for us to narrow the requests for which we should apply the more lenient handling?

Not really. I think it would make more sense to just show a descriptive error message that allows the user to narrow down the problem themselves.

---

_@MichaReiser approved on 2024-06-11 06:19_

> It's whenever a command gets run, which usually has to be done through the command palette (so, manual execution).

I would then prefer if we would also show a user facing error in addition to logging a warning to insta similar to insta. It would ensure that we still surface real errors and I don't think it would be annoying, considering that triggering requires a manual user interaction. 


---

_Renamed from "`ruff server`: Silently handle unavailable file" to "`ruff server`: Improve error message when a command is run on an unavailable document" by @snowsignal on 2024-06-11 18:47_

---

_Merged by @snowsignal on 2024-06-11 18:50_

---

_Closed by @snowsignal on 2024-06-11 18:50_

---

_Branch deleted on 2024-06-11 18:50_

---
