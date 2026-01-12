```yaml
number: 5635
title: Wrap documentation at 100 characters
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: zb/wrap-docs
created_at: 2024-07-30T20:52:51Z
updated_at: 2024-08-01T18:30:04Z
url: https://github.com/astral-sh/uv/pull/5635
synced_at: 2026-01-12T16:06:55Z
```

# Wrap documentation at 100 characters

---

_@zanieb_

Basically sick of dealing with mixed formatting here. Going with the number at https://github.com/astral-sh/uv/blob/7c08e61b73d54bb91130041859dcedc74f7a8e0c/.editorconfig#L20

---

_Label `documentation` added by @zanieb on 2024-07-30 20:52_

---

_Label `preview` added by @zanieb on 2024-07-30 20:52_

---

_Comment by @zanieb on 2024-07-30 20:59_

Note that I prefer it to be unwrapped and for people to use soft-wrap in their editors because the diff is nonsense when you hard-wrap but 🤷‍♀️ feel like I'm alone in that.

---

_Review requested from @BurntSushi by @zanieb on 2024-07-30 20:59_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-30 20:59_

---

_Comment by @zanieb on 2024-07-30 21:00_

I used the VSCode Rewrap plugin for this, I'd love to hear about alternative tooling as that isn't particularly maintained and I don't love that it split inline code.

---

_@charliermarsh approved on 2024-07-30 21:41_

---

_Merged by @zanieb on 2024-07-30 22:17_

---

_Closed by @zanieb on 2024-07-30 22:17_

---

_Branch deleted on 2024-07-30 22:17_

---

_Comment by @BurntSushi on 2024-07-31 12:34_

> Note that I prefer it to be unwrapped and for people to use soft-wrap in their editors because the diff is nonsense when you hard-wrap but 🤷‍♀️ feel like I'm alone in that.

Yeah diff quality is one of the downsides of hard-wrap. Back in my days as an academic, when collaborating on a paper, we would always try to insert a line break after a sentence. (Although we still hard-wrapped at 80 columns.) This tended to help with diffs but it's difficult to automate.

Here's a screenshot of what soft-wrap looks like in my editor:

![soft-wrap-one-window](https://github.com/user-attachments/assets/36dace28-3369-4b93-8002-3cc3f63eddbe)

And here's what it looks like when I open a second window next to it. The text kinda smushes and runs together with no real smarts about where to insert breaks (vim will just happily insert breaks right in the middle of words):

![soft-wrap-two-window](https://github.com/user-attachments/assets/2681980a-b16f-4155-a63a-b2ebd4938327)

And in particular, one of the very annoying things about soft-wrap, at least in vim, is that it _looks_ like, at a glance, that, for example, "Python does not publish" and "distributions" are on two different lines. And so if your cursor is on "publish," it's easy to be misled that moving down a line should have you land on "distributions." But of course, it doesn't. And so navigating to different parts of the text winds up being really annoying.

And finally, compare the above with what hard-wrap looks like:

![hard-wrap](https://github.com/user-attachments/assets/f89f3524-9a56-4c7c-b9e2-767700efe098)

Now the test is a bit easier to read (for me) even when I have one window stretched across the screen because the text is wrapped at a reasonable point. And moving the cursor around is consistent with the appearance. And words aren't split arbitrarily.

This is to some extent a tooling deficiency, but not _completely_ so IMO.

This is also largely why I find long lines of code unwieldy as well.

> I used the VSCode Rewrap plugin for this, I'd love to hear about alternative tooling as that isn't particularly maintained and I don't love that it split inline code.

I use [`par`](http://www.nicemice.net/par/) for line wrapping. But I doubt you'd be able to run it on entire Markdown documents and have it produce a desirable result. I guess for that you'd either need a Markdown-aware line wrapper or something that knows how to parse Markdown and only asks the line wrapper to do its work for "paragraph" nodes or whatever.

---

_Comment by @konstin on 2024-08-01 18:30_

> Note that I prefer it to be unwrapped and for people to use soft-wrap in their editors because the diff is nonsense when you hard-wrap but 🤷‍♀️ feel like I'm alone in that.

Same, there's so much noise with the reflowing

---
