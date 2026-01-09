---
number: 873
title: Publish to Homebrew
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-11-22T18:29:16Z
updated_at: 2022-11-26T17:59:37Z
url: https://github.com/astral-sh/ruff/issues/873
synced_at: 2026-01-07T13:12:14-06:00
---

# Publish to Homebrew

---

_Issue opened by @charliermarsh on 2022-11-22 18:29_

_No description provided._

---

_Label `releases` added by @charliermarsh on 2022-11-22 18:29_

---

_Referenced in [astral-sh/ruff#884](../../astral-sh/ruff/pulls/884.md) on 2022-11-23 01:11_

---

_Comment by @messense on 2022-11-24 05:31_

Per https://docs.brew.sh/Acceptable-Formulae#niche-or-self-submitted-stuff

> be stable (e.g. not declared “unstable” or “beta” by upstream)

The 0.0.x versioning of ruff might be an issue? Perhaps a 0.1.x should be released before submitting formulae to homebrew-core. It doesn't matter if you just want to maintain a [tap](https://docs.brew.sh/Taps).

---

_Comment by @charliermarsh on 2022-11-24 05:36_

I think I have a working recipe here, but I need Homebrew to update to Rust 1.65.0. (See: https://github.com/Homebrew/homebrew-core/pull/116480.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-25 03:44_

---

_Comment by @charliermarsh on 2022-11-25 18:40_

Maybe it'll get rejected, we'll see!

https://github.com/Homebrew/homebrew-core/pull/116712

---

_Comment by @charliermarsh on 2022-11-26 17:59_

Good to go!

<img width="591" alt="Screen Shot 2022-11-26 at 12 58 50 PM" src="https://user-images.githubusercontent.com/1309177/204102601-699b8eec-c829-49c9-8e22-b011008e9e43.png">


---

_Closed by @charliermarsh on 2022-11-26 17:59_

---
