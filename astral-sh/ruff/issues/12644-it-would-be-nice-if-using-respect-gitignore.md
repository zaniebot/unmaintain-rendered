---
number: 12644
title: "It would be nice if using `--respect-gitignore` outside of a git repo spat out an error"
type: issue
state: open
author: jfly
labels:
  - cli
assignees: []
created_at: 2024-08-02T19:49:14Z
updated_at: 2024-08-03T07:14:38Z
url: https://github.com/astral-sh/ruff/issues/12644
synced_at: 2026-01-10T01:22:52Z
---

# It would be nice if using `--respect-gitignore` outside of a git repo spat out an error

---

_Issue opened by @jfly on 2024-08-02 19:49_

keywords: respect-gitignore

In https://github.com/astral-sh/ruff/issues/5930, ruff tried to start respecting `.gitignore` files even when outside of a git repo. Unfortunately, we [had to revert that](https://github.com/astral-sh/ruff/issues/5930#issuecomment-1666588790).

I'm not asking to reconsider that revert: I understand that it had to happen. However, I do think it would be useful if `--respect-gitignore` would error out when run outside of a git repo. I personally find the current behavior quite confusing, and lost some time today trying to understand why ruff was disrespecting my `.gitignore` even though I asked it to respect it!

I'd be happy to send in a PR implementing this if you're open to it.

Thanks for the great tool!

---

_Label `wish` added by @charliermarsh on 2024-08-02 21:47_

---

_Label `wish` removed by @charliermarsh on 2024-08-02 21:47_

---

_Label `cli` added by @charliermarsh on 2024-08-02 21:47_

---

_Comment by @charliermarsh on 2024-08-02 21:50_

Seems reasonable... Might be hard to know when we should raise this exactly. E.g., is it only when it's provided on the command-line? What if it comes from a configuration file? (I think `true` is also the default so need to differentiate between "provided" and "not provided".)

A warning could also be reasonable here.


---

_Comment by @MichaReiser on 2024-08-03 07:11_

I think there's some more complication because `respect_gitignore` should rather be named `respect_ignore` because it doesn't just toggle `.gitignore`, it also toggles whether ruff respects `.ignore`, git ignore in parent directories, git global, git exclude, and ignores hidden files. 

https://github.com/astral-sh/ruff/blob/ebe5b06c95cc958d460d7cfc520e540017d9492f/crates/ruff_server/src/session/index/ruff_settings.rs#L135-L137

[standard_filters](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.standard_filters)

To me it's also unclear when such a warning would be shown? Is it when ruff doesn't find **any** gitignore or only if there is no gitignore in the root directory? Limiting it to the former seems overly restrictive but the latter has the downside that ruff always has to walk the tree eagerly even when only linting a subset of the files.

---
