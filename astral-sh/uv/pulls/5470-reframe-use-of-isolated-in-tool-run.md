```yaml
number: 5470
title: "Reframe use of `--isolated` in `tool run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/isolate
created_at: 2024-07-26T00:31:20Z
updated_at: 2024-08-23T14:40:51Z
url: https://github.com/astral-sh/uv/pull/5470
synced_at: 2026-01-10T13:09:50Z
```

# Reframe use of `--isolated` in `tool run`

---

_Pull request opened by @charliermarsh on 2024-07-26 00:31_

## Summary

This PR gets rid of the global `--isolated` flag (which serves a bunch of independent responsibilities right now) on `uv tool run` in favor of a dedicated `--isolated` flag, which tells uv to avoid re-using an existing tool environment for this invocation. We'll add the same thing to `uv run`, to avoid using the base project environment.

This will become a bit clearer in #5466, when we deprecate the `--isolated` flag on the preview APIs.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-26 00:31_

---

_Review requested from @konstin by @charliermarsh on 2024-07-26 00:31_

---

_Label `preview` added by @charliermarsh on 2024-07-26 00:31_

---

_Label `cli` added by @charliermarsh on 2024-07-26 00:31_

---

_@konstin approved on 2024-07-26 09:30_

---

_Comment by @zanieb on 2024-07-30 13:37_

What's the idea behind "isolate" vs "isolated"? What tense do we want to use in our CLI?

---

_Comment by @charliermarsh on 2024-07-30 13:38_

I suppose we could use `--isolated`. I wanted to have a hard break with the deprecated flag, otherwise in a vacuum I would've just used `--isolated`. Let me skim our CLI and see if we have examples either way...

---

_Comment by @zanieb on 2024-07-30 13:39_

Yeah I worry about using a different name than we would have just for the deprecation :/ it makes some sense pragmatically but I am also genuinely curious which tense we should use in general.

---

_Comment by @charliermarsh on 2024-07-30 13:39_

It looks like we tend to use imperative: `--refresh`, `--reinstall`, `--annotate`, `--generate-hashes`

---

_Comment by @charliermarsh on 2024-07-30 13:40_

On the other hand, we have `--locked`.

---

_Comment by @charliermarsh on 2024-07-30 13:41_

Although `--locked` isn't an action (it's not telling you to lock something), it's a description... Whereas `--isolate` is telling uv to add isolation. I don't know if I'm smart enough to describe the distinction accurately.

---

_Comment by @zanieb on 2024-07-30 14:11_

I think I feel the gist of what you're getting at.

I think if we consider `--locked` (and similar) as "descriptions of how to act" rather than "doing another action" (as would be implied by `--lock`), I think `--isolated` actually makes more sense because we're doing the same _action_ just in a different way?

---

_Comment by @charliermarsh on 2024-07-30 14:13_

Yeah that's a good description. Ok, should we just reuse `--isolated` here? It's not a huge problem since `--isolated` already _meant_ this in these contexts. It just makes the deprecation and warnings more difficult to implement...

---

_Comment by @zanieb on 2024-07-30 14:22_

I think we would use it if not for the existing flag; and, since it's the same as before and mostly in preview, I think we should go for it. Sorry it adds complexity 😬 hopefully the deprecation is short-lived? I'm also okay with no-deprecation in the preview commands.

---

_Comment by @charliermarsh on 2024-07-30 18:05_

I'm not 100% sure how to make it work since we'll now have `--isolated` as both a global and an argument on a dedicated command (assuming we want it to show up in `help`). I'll try though.

---

_Comment by @charliermarsh on 2024-07-30 18:57_

Oh interesting, Clap lets commands override global arguments. Ok, that's easier...

---

_Renamed from "Remove use of `--isolated` in `tool run`" to "Reframe use of `--isolated` in `tool run`" by @charliermarsh on 2024-07-30 19:04_

---

_Merged by @charliermarsh on 2024-07-30 19:09_

---

_Closed by @charliermarsh on 2024-07-30 19:09_

---

_Branch deleted on 2024-07-30 19:09_

---
