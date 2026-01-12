```yaml
number: 2252
title: "[`pycodestyle`] Add `E201`, `E202`, `E203` Rule"
type: pull_request
state: closed
author: saadmk11
labels: []
assignees: []
draft: true
base: main
head: pycodestyle-e201-e202-e203
created_at: 2023-01-27T13:56:18Z
updated_at: 2023-01-30T02:20:37Z
url: https://github.com/astral-sh/ruff/pull/2252
synced_at: 2026-01-12T04:52:00Z
```

# [`pycodestyle`] Add `E201`, `E202`, `E203` Rule

---

_Pull request opened by @saadmk11 on 2023-01-27 13:56_

ref https://github.com/charliermarsh/ruff/issues/1073

---

_Converted to draft by @saadmk11 on 2023-01-27 14:08_

---

_Comment by @saadmk11 on 2023-01-27 15:02_

Hi @charliermarsh, Is there a way to iterate over logical lines on Ruff? We need to use logical lines here instead of physical.

---

_Comment by @charliermarsh on 2023-01-27 15:40_

Not at present but I implemented it once in #1130. We could consider reviving it. My main concern is that it's pretty slow (it should be faster now that we changed the `Locator` implementation) so I'm hesitant to turn on rules that require it by default.

We could consider changing the default ruleset to be explicitly those rules from Flake8 that are relevant when Black is enabled (instead of all `E` and `F` rules), which would help with that problem. Although most users would probably still end up enabling `E` and unknowingly pay the performance penalty. What do you think?

---

_Comment by @ngnpope on 2023-01-27 16:13_

Perhaps instead of `--select ALL` we can have a `--select ALL-EXCEPT-RULES-HANDLED-BY-AUTOFORMATTER` ™ 😆 

Yeah - struggling on naming that one...

This would be particularly useful when `ruff` grows its own autoformatting support. Perhaps, if `ruff` knows that autoformatting is enabled it could silently ignore those rules it doesn't need to run.  

---

_Comment by @saadmk11 on 2023-01-28 12:19_

> Not at present but I implemented it once in #1130. We could consider reviving it. My main concern is that it's pretty slow (it should be faster now that we changed the `Locator` implementation) so I'm hesitant to turn on rules that require it by default.
> 
Currently the PR is using physical lines to check for issues which works perfectly with one line examples. But it requires logical lines to work on all scenarios and not raise false positives. Let me know if you revive #1130, I would be happy to test it out. but for now we can either close this PR (As it is dependent on a feature that is not implemented yet) or keep it as is. :)

> We could consider changing the default ruleset to be explicitly those rules from Flake8 that are relevant when Black is enabled (instead of all `E` and `F` rules), which would help with that problem. Although most users would probably still end up enabling `E` and unknowingly pay the performance penalty. What do you think?

Checking if black is enabled may proof to be inconsistent as it might be installed globally or installed in the current users virtualenv but not used for the project.

---

_Comment by @charliermarsh on 2023-01-29 01:21_

Yeah I think we need logical lines for this to really be useful. We could try reviving that PR. I'm not sure what the performance characteristics will look like now.

---

_Comment by @charliermarsh on 2023-01-30 02:20_

Let's close for now then, we can always reopen later.

---

_Closed by @charliermarsh on 2023-01-30 02:20_

---
