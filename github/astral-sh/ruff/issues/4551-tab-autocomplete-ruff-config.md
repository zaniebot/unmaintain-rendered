---
number: 4551
title: "Tab-autocomplete `ruff config`"
type: issue
state: closed
author: janosh
labels:
  - good first issue
  - cli
assignees: []
created_at: 2023-05-20T19:55:31Z
updated_at: 2025-01-27T15:09:31Z
url: https://github.com/astral-sh/ruff/issues/4551
synced_at: 2026-01-07T13:12:14-06:00
---

# Tab-autocomplete `ruff config`

---

_Issue opened by @janosh on 2023-05-20 19:55_

Would be great if `ruff config` tab autocompletes:

```
ruff config is
# tab
ruff config isort
```

and

```
ruff config isort.spl
# tab
ruff config isort.split-on-trailing-comma
```

Related: https://github.com/charliermarsh/ruff/issues/2808 hence pinging @not-my-profile 

---

_Label `cli` added by @charliermarsh on 2023-05-22 03:11_

---

_Label `good first issue` added by @charliermarsh on 2023-06-26 18:05_

---

_Comment by @MichalKacprzak99 on 2024-01-04 14:50_

@charliermarsh Hi, I would like to work on this, can you assign it to me?

---

_Assigned to @MichalKacprzak99 by @charliermarsh on 2024-01-04 15:48_

---

_Comment by @charliermarsh on 2024-01-04 15:48_

You got it!

---

_Comment by @dhruvmanila on 2024-02-12 05:53_

@MichalKacprzak99 Not in a hurry but just wondering if you've started working on this? Feel free to ask any questions if you're stuck :)

---

_Comment by @hesampakdaman on 2024-06-03 20:47_

I looked at this issue a little bit during the weekend. I initially tried to create a derived macro for `Options` that creates an `enum` that I then implement `clap::ValueEnum` for. However, this approach is not viable because Rust doesn't support reflection, and I couldn't generate the enum variants for attributes `#[option_group]` recursively.

My only other idea is to create a huge `enum` by hand, but that would be a rather big code change.


---

_Referenced in [astral-sh/ruff#15603](../../astral-sh/ruff/pulls/15603.md) on 2025-01-20 03:46_

---

_Closed by @dhruvmanila on 2025-01-27 15:09_

---

_Closed by @dhruvmanila on 2025-01-27 15:09_

---

_Unassigned @MichalKacprzak99 by @dhruvmanila on 2025-01-27 15:09_

---
