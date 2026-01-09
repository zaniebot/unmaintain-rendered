---
number: 4601
title: Blanket opt-in to nursery rules possible?
type: issue
state: closed
author: ThiefMaster
labels:
  - configuration
assignees: []
created_at: 2023-05-23T13:56:00Z
updated_at: 2023-06-05T02:26:16Z
url: https://github.com/astral-sh/ruff/issues/4601
synced_at: 2026-01-07T13:12:14-06:00
---

# Blanket opt-in to nursery rules possible?

---

_Issue opened by @ThiefMaster on 2023-05-23 13:56_

When converting my [flake8 config](https://raw.githubusercontent.com/indico/indico/master/.flake8) using `flake8-to-ruff`, I got some warnings about unsupported options, one of them being `E265`. But it turned out that these rules are in fact supported, just considered "beta" quality and thus in nursery (if i understood its purpose correctly).

Having an existing project where flake8 passes with no warnings, I obviously do not want to lose functionality. On the other hand, I'd be perfectly fine with the occasional false positive due to a bug in a nursery rule.

I DO NOT want to manually build a list of all flake8 rules and list them explicitly in the config file. This would be ugly and cumbersome to maintain.

Something like this would be great:

- `enable_nursery = true` setting which simply enables all nursery rules as long as their category (e.g. `E`) is selected.
- something like `select = ['E!']` with the `!` enabling nursery rules for that category. Probably more work to implement but also more flexible.

---

_Comment by @charliermarsh on 2023-05-23 14:38_

I totally understand why the current nursery setup is inconvenient but I think I'd rather focus on elevating rules out of the nursery category rather than improving support for enabling nursery rules.

---

_Comment by @ThiefMaster on 2023-05-23 14:51_

Wouldn't such a setting help with this though?

People are lazy, and they often don't even install prereleases of libraries to test against them - but having to opt into into many rules individually just means that there are even fewer people testing them, making it more likely that bugs only show up later when they leave nursery, kind of defeating its purpose!

---

_Comment by @charliermarsh on 2023-05-23 14:56_

Yeah, some kind of nursery flag would help! I would support its existence, probably. I'm just conveying the sentiment that every ask has to be prioritized given infinite scope and limited time, so I'm unlikely to work on this myself.


---

_Label `configuration` added by @charliermarsh on 2023-05-23 14:59_

---

_Comment by @ThiefMaster on 2023-05-23 14:59_

Got any pointers? Never wrote Rust code beyond a little bit of playing around a year or two ago, but might be a nice thing to give it a try!

---

_Comment by @charliermarsh on 2023-05-24 02:48_

@ThiefMaster - Thank you so much for asking :) Honestly part of the reason I'm hesitant is that it's not trivial to do, and I'm not actually sure how we would implement it. The "rule selector" stuff is very macro heavy and can be difficult to reason about (we want to change this), plus it also tends to require that all the various rule selectors are defined at compile time and not at runtime.

(Defining all this stuff at compile time does have some benefits, e.g., we accept `--select E265` on the CLI, via autocomplete, and in the JSON schema, but not `--select E26`.)

---

_Comment by @charliermarsh on 2023-06-05 02:26_

We added a `NURSERY` selector in #4852, so you can do `--select NURSERY` or add it to your pyproject.toml's `select` field.

---

_Closed by @charliermarsh on 2023-06-05 02:26_

---
