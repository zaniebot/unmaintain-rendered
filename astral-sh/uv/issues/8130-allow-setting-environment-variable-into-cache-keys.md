```yaml
number: 8130
title: Allow setting environment variable into cache-keys
type: issue
state: closed
author: gaborbernat
labels:
  - help wanted
  - configuration
assignees: []
created_at: 2024-10-11T19:23:51Z
updated_at: 2024-12-26T15:31:50Z
url: https://github.com/astral-sh/uv/issues/8130
synced_at: 2026-01-10T04:36:20Z
```

# Allow setting environment variable into cache-keys

---

_Issue opened by @gaborbernat on 2024-10-11 19:23_

So `uv sync` is governed by https://docs.astral.sh/uv/reference/settings/#cache-keys. Occasionally, you want to customize the build package based on environment variables (e.g., for a type checker no need to build a NPM distribution static files, but for the tests you might need).  Accordingly, we should allow including environment variables into the cache key.

---

_Label `configuration` added by @charliermarsh on 2024-10-12 03:47_

---

_Comment by @charliermarsh on 2024-10-12 03:47_

That seems reasonable.

---

_Label `help wanted` added by @charliermarsh on 2024-10-30 20:22_

---

_Comment by @charliermarsh on 2024-10-30 20:22_

For anyone interested, this requires adding a new variant here:

https://github.com/astral-sh/uv/blob/bed47d512a873ef2b2812b86a6cef231ba2c965d/crates/uv-cache-info/src/cache_info.rs#L224

The rest follows from there, not too hard.

---

_Comment by @gaborbernat on 2024-10-30 20:57_

I did try going down that rabbit hole, but this quickly can becoime quite complicated, especially as the test suite is not that exhaustive 🤔 so can be daunting to change a lot of the code base without that safety net 🤔 I do not see a single test in this crate here https://github.com/astral-sh/uv/tree/bed47d512a873ef2b2812b86a6cef231ba2c965d/crates/uv-cache-info 🤔 

---

_Comment by @charliermarsh on 2024-10-30 20:59_

That's because you're looking in the wrong place. We don't do much unit testing at all, uv is almost entirely tested via integration tests and snapshots. For example, here:

https://github.com/astral-sh/uv/blob/bed47d512a873ef2b2812b86a6cef231ba2c965d/crates/uv/tests/it/pip_install.rs#L3492

---

_Comment by @gaborbernat on 2024-10-30 21:00_

> That's because you're looking in the wrong place. We don't do much unit testing at all, uv is almost entirely tested via integration tests and snapshots. For example, here:

Integration tests are much slower... which in turn make development harder 😆 Having the tests far from the code does not help their discoverabilty or locallity 😮 

All I am saying is that I looked at the code base and what I've seen looked daunting enough to discourage me from doing it. Granted I am not a rust expert, but I think I am not horrible either. Not having a quick feedback loop when making changes (that unit tests give you), is on its own discouraging enough to stop me going any further. Or any obvious place to put my new tests.

---

_Comment by @zanieb on 2024-10-30 21:20_

These are trade-offs we're willing to accept for coverage of behavior as encountered by the user. I'm sorry you are feeling discouraged, but we have to balance test coverage with delivering value to our users. If you encounter something that you're not comfortable changing based on existing test coverage, it's reasonable practice to add tests in a separate pull request first. 

Please understand we are a small team and get a lot of requests, this isn't a great place to debate the merits of different testing strategies.

---

_Comment by @gaborbernat on 2024-10-30 21:24_

I totally understand, and I am not saying this as hey why are you not doing x, y. I am just pointing out what stopped me contributing when I tried to pick this up, as a feedback to the ease of contribution. In contrast I found contributing https://github.com/axodotdev/axoupdater/pull/199 much more straigh forward, but perhaps that is just a facet of that project being smaller and more focused. After all this project now has 218,281 lines of rust code, that one is only 2400.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-26 15:10_

---

_Closed by @charliermarsh on 2024-12-26 15:31_

---

_Closed by @charliermarsh on 2024-12-26 15:31_

---
