```yaml
number: 5209
title: Add a style guide
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: zb/style
created_at: 2024-07-19T02:07:50Z
updated_at: 2024-07-23T17:57:34Z
url: https://github.com/astral-sh/uv/pull/5209
synced_at: 2026-01-10T13:37:23Z
```

# Add a style guide

---

_Pull request opened by @zanieb on 2024-07-19 02:07_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-07-19 02:07_

---

_Label `internal` added by @zanieb on 2024-07-19 02:07_

---

_@charliermarsh reviewed on 2024-07-19 02:13_

---

_Review comment by @charliermarsh on `STYLE.md`:8 on 2024-07-19 02:13_

I think you tend to put spaces around em dashes.

---

_@charliermarsh reviewed on 2024-07-19 02:13_

---

_Review comment by @charliermarsh on `STYLE.md`:57 on 2024-07-19 02:13_

Unless the message spans more than a single sentence.

---

_Review comment by @konstin on `STYLE.md`:8 on 2024-07-19 13:45_

Could we stay with ascii in input files where possible?

---

_Review comment by @konstin on `STYLE.md`:26 on 2024-07-19 13:46_

```suggestion
1. Avoid language that patronizes the reader, e.g., "simply do this".
```

---

_Review comment by @konstin on `STYLE.md`:38 on 2024-07-19 13:46_

We need to define what previous knowledge exists, we're targeting python programmers, and the guides specifically we target a subset of them with a certain background we need to match.

---

_Review comment by @konstin on `STYLE.md`:47 on 2024-07-19 13:47_

If we have a style guide, it should define whether we're doing second- or third-person.

---

_Review comment by @konstin on `STYLE.md`:78 on 2024-07-19 13:51_

If we have an error message, are the hints before or after the error message? Do we include urls in messages? When do we (not) capitalize?

---

_Review comment by @konstin on `STYLE.md`:83 on 2024-07-19 13:51_

How and when do we use indentation? What about unicode symbols?

---

_Review comment by @konstin on `STYLE.md`:99 on 2024-07-19 13:51_

Aren't those dependent on the number of `-v`s? Do we allow `{:?}` in log messages, and if yes, in which levels? Which log levels do we consider user serviceable parts and which are for devs only?

---

_Review comment by @konstin on `STYLE.md`:85 on 2024-07-19 13:52_

When do we `warn_user` and when do we `warn`?

---

_Review comment by @konstin on `STYLE.md`:87 on 2024-07-19 13:54_

These two sound like implementation behavior, not things that you should need to consider when writing user-facing messages

---

_Review comment by @konstin on `STYLE.md`:21 on 2024-07-19 13:57_

Even at the beginning of a sentence?

---

_Review comment by @konstin on `STYLE.md`:79 on 2024-07-19 14:03_

I'd say must, piping to a text file should not lose any information. 

---

_@konstin reviewed on 2024-07-19 14:04_

---

_Review comment by @zanieb on `STYLE.md`:21 on 2024-07-19 14:05_

```suggestion
1. Do not capitalize, e.g., "Uv", even at the beginning of a sentence.
```

---

_@zanieb reviewed on 2024-07-19 14:05_

---

_@zanieb reviewed on 2024-07-19 14:06_

---

_Review comment by @zanieb on `STYLE.md`:87 on 2024-07-19 14:06_

Idk it seems worth codifying while we're talking about style? Sometimes you are not using our standard printer styling e.g. in the `help` pager.

---

_@zanieb reviewed on 2024-07-19 14:07_

---

_Review comment by @zanieb on `STYLE.md`:99 on 2024-07-19 14:07_

Not unless that changed recently? It's dependent entirely on the `RUST_LOG` level right now. https://github.com/astral-sh/uv/issues/1569

Great prompts for the rest!

---

_@zanieb reviewed on 2024-07-19 14:08_

---

_Review comment by @zanieb on `STYLE.md`:47 on 2024-07-19 14:08_

Yes I agree. I'm on the fence here. Basically I think it's okay for guides to be second-person but so far I've written everything in third-person.

---

_@zanieb reviewed on 2024-07-19 14:08_

---

_Review comment by @zanieb on `STYLE.md`:8 on 2024-07-19 14:08_

What's an input file?

---

_@charliermarsh reviewed on 2024-07-19 14:10_

---

_Review comment by @charliermarsh on `STYLE.md`:85 on 2024-07-19 14:10_

Not sure I can answer that cleanly, but one useful criterion: is the message user-actionable?

---

_@charliermarsh reviewed on 2024-07-19 14:10_

---

_Review comment by @charliermarsh on `STYLE.md`:79 on 2024-07-19 14:10_

Yes, sorry, must.

---

_Review comment by @zanieb on `STYLE.md`:85 on 2024-07-19 14:14_

Yeah no clean answer, that's a great guideline — it's about severity and actionability imo.

---

_@zanieb reviewed on 2024-07-19 14:14_

---

_@konstin reviewed on 2024-07-19 15:19_

---

_Review comment by @konstin on `STYLE.md`:8 on 2024-07-19 15:19_

Our plain text files, e.g. markdown files, as opposed to rich output files such as html. For example, latex has `\textemdash` and converts that to a real em dash in the pdf output.

---

_@konstin reviewed on 2024-07-19 15:21_

---

_Review comment by @konstin on `STYLE.md`:87 on 2024-07-19 15:21_

Could we have an abstraction that codifies those rules in one place?

---

_@konstin reviewed on 2024-07-19 15:24_

---

_Review comment by @konstin on `STYLE.md`:99 on 2024-07-19 15:24_

On any level, i'm only seeing trace logs when `RUST_LOG=trace` is set. We probably should link to #1569 saying that the above is how it currently is but this should change with #1569

---

_@zanieb reviewed on 2024-07-19 16:46_

---

_Review comment by @zanieb on `STYLE.md`:99 on 2024-07-19 16:46_

I added a note. Hesitant to link to the issue, but now the issue is linked to this pull request and we should update the style guide when we change `-vv` behavior.

---

_@zanieb reviewed on 2024-07-19 19:46_

---

_Review comment by @zanieb on `STYLE.md`:8 on 2024-07-19 19:46_

In documentation? I have a pretty strong preference for being able to use em dashes in the markdown.

---

_@zanieb reviewed on 2024-07-19 19:54_

---

_Review comment by @zanieb on `STYLE.md`:87 on 2024-07-19 19:54_

I mean yeah but you still need to use the abstraction. Why the opposition here? I don't think these are harmful to note.

---

_@konstin reviewed on 2024-07-19 20:28_

---

_Review comment by @konstin on `STYLE.md`:87 on 2024-07-19 20:28_

I'm not against having them in the style guide as long as long as we generally have abstractions for this

---

_@konstin reviewed on 2024-07-19 20:34_

---

_Review comment by @konstin on `STYLE.md`:8 on 2024-07-19 20:34_

Non-unicode characters are a nuisance to enter, esp. across keyboard layouts, and when they look like an ascii characters they tend to cause unnecessary debugging

---

_@samypr100 reviewed on 2024-07-22 00:44_

---

_Review comment by @samypr100 on `STYLE.md`:22 on 2024-07-22 00:44_

I assume the exception to this is with env vars?

---

_Review comment by @zanieb on `STYLE.md`:22 on 2024-07-22 17:31_

Thanks, that seems like a good thing to note .
```suggestion
1. Do not uppercase, e.g., "UV", unless referring to an environment variable, e.g., `UV_PYTHON`.
```

---

_@zanieb reviewed on 2024-07-22 17:31_

---

_@charliermarsh approved on 2024-07-22 18:10_

I'm fine to merge this whenever and iterate.

---

_Comment by @zanieb on 2024-07-23 17:57_

Let's send it — lots more to do here though.

---

_Marked ready for review by @zanieb on 2024-07-23 17:57_

---

_Merged by @zanieb on 2024-07-23 17:57_

---

_Closed by @zanieb on 2024-07-23 17:57_

---

_Branch deleted on 2024-07-23 17:57_

---
