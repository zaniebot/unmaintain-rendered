```yaml
number: 17466
title: Add playground workspace package-lock.json to gitignore
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - internal
  - playground
assignees: []
base: main
head: playground-gitignore
created_at: 2025-04-18T17:44:28Z
updated_at: 2025-04-25T20:54:38Z
url: https://github.com/astral-sh/ruff/pull/17466
synced_at: 2026-01-10T19:33:02Z
```

# Add playground workspace package-lock.json to gitignore

---

_Pull request opened by @MatthewMckee4 on 2025-04-18 17:44_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Sometimes these lockfiles auto generate for me and i accidentally commit them. Seems like i should add them to the gitignore

---

_Label `internal` added by @AlexWaygood on 2025-04-18 18:07_

---

_Label `playground` added by @AlexWaygood on 2025-04-18 18:07_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-04-18 18:07_

---

_Comment by @MichaReiser on 2025-04-21 10:18_

This can happen if you run `npm install` in the wrong directory (at least, I'm not aware of any other way). That's why I'm unsure if we should ignore them in gitignore. I'm leaning towareds leaving the ignore file as is, unless there are other cases where you run into this.

---

_Comment by @MatthewMckee4 on 2025-04-21 12:44_

Often they just "randomly" generate for me. Probably due to one of my vscode extensions.

---

_Comment by @MatthewMckee4 on 2025-04-22 11:42_

https://github.com/astral-sh/ruff/pull/17533#discussion_r2053937717

---

_Comment by @sharkdp on 2025-04-22 12:37_

This never happened to me. Seems to me like a pretty clear case for a user-defined Git ignore pattern (e.g. in `.git/info/exclude`).

---

_Comment by @MatthewMckee4 on 2025-04-22 13:03_

Okay I can do that. Can I ask what the reason is for not adding it to the gitignore if it's something we actively don't want to be committed?

---

_Comment by @sharkdp on 2025-04-22 14:49_

> Can I ask what the reason is for not adding it to the gitignore if it's something we actively don't want to be committed?

I don't know our general strategy here, but from my personal experience, it's always good to keep `.gitignore` patterns to a minimum, and to keep patterns as specific as possible. Not talking about your PR here, but more generally. I had way more problems caused by overly-broad/wrong `.gitignore` patterns, then I had with files accidentally being committed. Better to accidentally commit a wrong file (and find out in review) than to lose actual changes that were not committed because you didn't realize they were gitignored by a rogue pattern.

---

_Comment by @MichaReiser on 2025-04-22 15:38_

Our practice has been to add files to `.gitignore` that we expect to get created when running scripts that are checked in, e.g. our dev commands because we know that everyone wants to ignore those. 

We have refrained form adding files to the checked in `.gitignore` that are specific to your editor, your setup or your own dev workflow (e.g. I like to use samply and it creates a `profile.json`). These should be put into your user gitignore instead.

I haven't been able to reproduce the creation of a `package-lock.json` in the sub-packages except when I accidentally ran `npm install` in a sub-package folder. So I suspect that you may have some specific VS code extension (or something else) that triggers the installs which falls into the *your setup* category. However, if it turns out that this is something that impacts everyone working on the playground, adding it to our commited `.gitignore` is correct.

---

_Comment by @MatthewMckee4 on 2025-04-22 15:44_

Okay thank you, that sounds good to me.

---

_Closed by @MatthewMckee4 on 2025-04-22 15:44_

---

_Branch deleted on 2025-04-25 20:54_

---
