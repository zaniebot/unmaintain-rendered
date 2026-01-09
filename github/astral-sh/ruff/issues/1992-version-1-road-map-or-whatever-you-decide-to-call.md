---
number: 1992
title: Version 1 road map, or whatever you decide to call “stable”…
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2023-01-19T09:43:25Z
updated_at: 2024-01-10T21:13:40Z
url: https://github.com/astral-sh/ruff/issues/1992
synced_at: 2026-01-07T13:12:14-06:00
---

# Version 1 road map, or whatever you decide to call “stable”…

---

_Issue opened by @ghost on 2023-01-19 09:43_

_This is not really a bug, more a request for clarity._

First, I really like `ruff` and I do want to make it my go-to tool for linting. Currently, using it takes some effort to update as the release versions are coming fast and frequently. This is, of course, fine and understandable as it is under massive development. Kudos to everyone working on it, your efforts are much appreciated.

What I am looking for is a vague and non-binding roadmap to a “stable” version (let's call it v1!) so that I can plan a massive upgrade.

---

_Comment by @charliermarsh on 2023-01-19 12:46_

Totally hear you and thanks for the kind words. I have a bunch of thoughts here of course :joy: I'll try to write something up later today if I can!


---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-19 12:48_

---

_Comment by @Dilski on 2023-01-24 13:43_

I would like to adopt Ruff as our main tool for linting, but our main blocker is the v1/stable release. For now however, we're going to be running ruff alongside our other linting tools (flake8, bandit, etc).

---

_Comment by @charliermarsh on 2023-01-24 14:53_

I got delayed on this, but I'll post some thoughts on what I want to see in a "stable" release by the end of the week!

---

_Comment by @ghost on 2023-01-24 14:58_

>I got delayed on this, but I'll post some thoughts on what I want to see in a "stable" release by the end of the week!

No worries whatsoever. Please do take your time.

---

_Comment by @charliermarsh on 2023-01-26 19:28_

Here's what I'd like to see going into a Version 1 (which I assume would be `v0.1.0`, but let's punt that discussion for now):

- [x] Support for structural pattern matching (https://github.com/charliermarsh/ruff/issues/282)
- [x] Backwards-compatible migration to a subcommand-oriented CLI (#2190)
- [ ] Add support for "severity" levels (#1256)
- [ ] Add support for "autofix safety" levels, to let people opt-in or opt-out of potentially-dangerous fixes (#4181)
- [ ] Add backwards-compatible, first-class abstractions that don't rely on mapping back to Flake8 concepts (#1773, #1774)
    - [ ] Add support for "rule aliasing" (#2186)
- [ ] "Versioning" for Ruff configuration files (similar to Rust [editions](https://doc.rust-lang.org/edition-guide/editions/index.html))
- [x] Better documentation for rules (#2642)

As you can see, there are a few potentially-breaking changes in there. The _intent_ is for them to be fully backwards-compatible, but I can't process _complete_ backwards compatibility yet.

To put this in higher-level terms, I want Ruff to hit two milestones before bumping to `v0.1.0`:

1. We should support all known Python language features (of course!).
2. We should have an API that we're confident in -- one that we don't expect to change significantly in the future. (By API here, I mean the command-line interface and the way in which Ruff is configured.) The current API doesn't meet this bar. It totally works, but it's heavily anchored on Flake8 (see the error code system, e.g., `E501` and friends), and is missing some key concepts (like "safety levels" for autofixes). We want to implement those missing concepts, and put some new abstractions in place (e.g., users should be able to do things like enable all "correctness" or "style" rules without having to parse through the list of implemented Flake8 plugins) while maintaining compatibility with the Flake8-oriented system. (Compatibility is an important piece here. We now have a lot of users -- which is great! -- but it comes with responsibility.)

I don't know when exactly we'll be able to bump to `v0.1.0`. I mean, definitely this _year_, but not _this month_.

Here are a few things that aren't important to me, with respect to bumping to `v0.1.0`:

- A plugin API or plugin system. We need to stabilize the API (part of `v0.1.0` :)) before we consider doing this.
- A programmatic API (call into `ruff` from Python, or from Rust). Same as above.
- More lint rules. We'll continue adding rules and capabilities, but to me, they're not blockers for this kind of version bump.
- ...what else am I missing?

This is all based on current thinking and I reserve the right to deviate from this path as time goes on 😅  But hopefully this is helpful. Feedback always welcome.


---

_Comment by @charliermarsh on 2023-01-26 19:28_

I'll \cc @not-my-profile on this issue too, actually, as he's doing a lot of work related to those rule and diagnostic API improvements.

---

_Comment by @not-my-profile on 2023-01-26 19:46_

I'd add More thorough documentation for rules (#1467).

---

_Comment by @ghost on 2023-01-27 08:55_

> (which I assume would be `v0.1.0`, but let's punt that discussion for now)

My only very minor concern here is that poetry expects things to be 1.x.x before auto-updating packages. Not an unsurmountable issue, just a minor annoyance.

Otherwise, I fully support this vision. It's perfect! Thank you.

---

_Comment by @hugovk on 2023-01-27 09:53_

And using [SemVer](https://semver.org/#how-do-i-know-when-to-release-100):

> **How do I know when to release 1.0.0?**
>
> If your software is being used in production, it should probably already be 1.0.0.

There's a couple of dozen big projects listed on https://github.com/charliermarsh/ruff already using Ruff in production :)

> If you have a stable API on which users have come to depend, you should be 1.0.0. If you’re worrying a lot about backwards compatibility, you should probably already be 1.0.0.

All planned for "Version 1": https://github.com/charliermarsh/ruff/issues/1992#issuecomment-1405497643 :)

---

_Referenced in [johnthagen/python-blueprint#141](../../johnthagen/python-blueprint/issues/141.md) on 2023-01-30 12:48_

---

_Referenced in [astral-sh/ruff#2354](../../astral-sh/ruff/pulls/2354.md) on 2023-01-30 17:20_

---

_Referenced in [yt-project/yt#4320](../../yt-project/yt/pulls/4320.md) on 2023-01-31 17:54_

---

_Comment by @Sawbez on 2023-02-11 04:46_

@charliermarsh You might consider pinning this, putting this in a milestone, or putting this general information in a `ROADMAP.md` and link it in the `README`. We could also use the roadmap for version 2 and 3 (presumably 2 is formatting, 3 is type checking if we go that far) and possibly other big changes that contributors or users might want to help with/look forward to.

That might be too much maintainer time on top of all the things you have to manage, so not doing any of these would also be 100% okay.

---

_Referenced in [astral-sh/ruff#2788](../../astral-sh/ruff/issues/2788.md) on 2023-02-12 00:55_

---

_Referenced in [astral-sh/ruff#2741](../../astral-sh/ruff/pulls/2741.md) on 2023-02-12 11:37_

---

_Comment by @sbrugman on 2023-02-16 08:18_

> Add support for "autofix safety" levels, to let people opt-in or opt-out of potentially-dangerous fixes

The closest issue that we have open related to this is https://github.com/charliermarsh/ruff/issues/1997 (I think)

---

_Comment by @ghost on 2023-02-16 08:51_

[GitHub Project](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects) can help organize it all. It's fine as it goes. I am :100: sure that a fair few people would help the project if @charliermarsh was getting overwhelmed…

@charliermarsh please do not burn out.

---

_Comment by @charliermarsh on 2023-02-21 18:54_

We're making good progress on this!

Now that we support all Python 3.10 and Python 3.11 syntax, we can start seriously considering a "stable" release (that is, if we wanted to punt some of the features listed above to "post-0.1").

I think "Editions" would be a good thing to do since it gives us a mechanism to change configuration in the future in a backwards-compatible way.


---

_Referenced in [python-pillow/Pillow#6966](../../python-pillow/Pillow/pulls/6966.md) on 2023-02-25 19:45_

---

_Referenced in [conda/conda#12445](../../conda/conda/issues/12445.md) on 2023-03-06 17:38_

---

_Referenced in [PaddlePaddle/community#412](../../PaddlePaddle/community/pulls/412.md) on 2023-03-09 08:36_

---

_Comment by @pauloxnet on 2023-04-06 14:44_

I'm waiting for the first stable release to propose ruff adoption to big python project (e.g. Django).😅
Thanks for all the work you all are doing. 👍

---

_Referenced in [astral-sh/ruff#4541](../../astral-sh/ruff/issues/4541.md) on 2023-05-21 17:54_

---

_Referenced in [cookiecutter/cookiecutter-django#4383](../../cookiecutter/cookiecutter-django/issues/4383.md) on 2023-06-09 15:15_

---

_Comment by @tgross35 on 2023-06-14 21:41_

Is there any specific place you're tracking the editions concept? Quick notes:

- Editions sound awesome since there will definitely be lints that are uplifted over time
- Yearly (`edition = "2023"`) or quarterly (`edition = 2023-Q3`) editions seems like a reasonable duration (as compared to Rust's every 3 years)
- I think that it is OK to default to the latest edition if unspecified (as compared to Rust always using the earliest 2015 edition) as long as it happens with a minor version bump. Reason being, I think users are likely to have their `ruff` versions pinned for relatively long times

---

_Comment by @zanieb on 2023-08-30 00:59_

Hi! We're looking at publishing a versioning policy for `v0.1.x`. Check out the proposal at https://github.com/astral-sh/ruff/discussions/6998, we'd love your feedback.

@tgross35 We have not invested more effort into editions yet. It may need to be a `v1.0.0+` feature.

---

_Referenced in [astral-sh/ruff#7931](../../astral-sh/ruff/pulls/7931.md) on 2023-10-16 17:31_

---

_Comment by @charliermarsh on 2024-01-10 21:13_

We shipped v0.1.0 a while back, which included a clear [versioning policy](https://docs.astral.sh/ruff/versioning/) -- and we're now looking ahead to a bunch of stabilizations in v0.2.0 in the near future -- so I'm going to close this out for now.

We still have some big changes planned for the future that will likely change the API (e.g., we want to do a holistic recategorization of the rules at some point; we may add support for custom rules; we'd like to ship a programmatic API, to call Ruff from Python). But all of those changes will follow the versioning policy defined above, and will be shipped with appropriate deprecations and compatibility guarantees over time.

---

_Closed by @charliermarsh on 2024-01-10 21:13_

---
