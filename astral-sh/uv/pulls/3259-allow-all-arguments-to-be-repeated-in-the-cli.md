```yaml
number: 3259
title: Allow all arguments to be repeated in the CLI
type: pull_request
state: closed
author: charliermarsh
labels:
  - cli
assignees: []
base: main
head: charlie/args
created_at: 2024-04-25T03:16:47Z
updated_at: 2025-03-03T02:46:50Z
url: https://github.com/astral-sh/uv/pull/3259
synced_at: 2026-01-10T11:10:33Z
```

# Allow all arguments to be repeated in the CLI

---

_Pull request opened by @charliermarsh on 2024-04-25 03:16_

## Summary

We need to decide if we actually want to change this, but I wanted to test it out anyway.

Closes https://github.com/astral-sh/uv/issues/3248.

## Test Plan

Ran `echo "flask" | cargo run pip compile - --output-file req.txt --output-file foo.txt`, and verified that `foo.txt` was populated.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-25 03:16_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-25 03:16_

---

_Label `cli` added by @charliermarsh on 2024-04-25 03:16_

---

_Comment by @charliermarsh on 2024-04-25 03:17_

I think I'm weakly in favor given that it seems like a popular behavior. Per https://github.com/clap-rs/clap/discussions/2627, it seems like you do this in ripgrep Andrew?

---

_Comment by @zanieb on 2024-04-25 03:17_

I'm not yet convinced this is a better experience for the majority of users.

---

_Comment by @charliermarsh on 2024-04-25 03:21_

I wasn't really sold, but it seems to be the default in most tools, and every CLI I've tested since researching this issue seems to operate this way (e.g., `git`, `gh`, and so on). Given that, I suspect that it's actually more surprising to users that it _doesn't_ work than it is helpful in the rare cases that you accidentally repeat an argument.

---

_Marked ready for review by @charliermarsh on 2024-04-25 03:24_

---

_Comment by @BurntSushi on 2024-04-26 11:45_

I am loosely in favor.

It's true that this is enabled in ripgrep, and has been for a long time. I don't use Clap any more in ripgrep, but in the move off of it, I think it's fair to say that I've doubled down on this behavior. I think the original motivation for allowing args to override itself is to be more flexible in the face of different workflows. For example, a user might have `--max-columns 100` in their config file (or an alias). But maybe the user wants to change that to something else for a specific invocation. Without being able to override previous values, the user isn't allowed to do that.

Anyway, how much of this applies to `uv`? I'm not sure. I'm not sure `uv` benefits as much from being super flexible. It's hard to pick a non-contrived example, but, what's the behavior of `--index-strategy unsafe-any-match --index-strategy first-match`? Today it returns an error, but I feel like it has obvious intended semantics: the index strategy should be set to `first-match`. Now... whether that specific example will come up in the wild or not is unclear. It would probably mean having `--index-strategy unsafe-any-match` in an alias or something.

---

_Comment by @zanieb on 2024-04-26 14:44_

My complaint is arguments where it's ambiguous if we accept multiple items, e.g.`--output-file` (as discussed in #3248) which could easily mean "write to all of the provided files"

---

_Comment by @BurntSushi on 2024-04-26 15:25_

> My complaint is arguments where it's ambiguous if we accept multiple items, e.g.`--output-file` (as discussed in #3248) which could easily mean "write to all of the provided files"

Yeah that is somewhat unfortunate. I think I'd still go with the flexibility though.

Another choice is that we could do both here: we could set this option globally (to allow most flags to override themselves) while treating ambiguous flags like `--output-file` specially. Although, how to do that using Clap isn't totally clear to me. (An obvious approach is to say that it can be used multiple times and error if it's used more than once, but then I imagine the auto-generated docs for that flag will be wrong.)

---

_Comment by @charliermarsh on 2024-09-23 23:29_

I remain weakly in favor of this, I think.

---

_Comment by @zanieb on 2024-09-24 13:41_

Is anyone asking for this still?

---

_Comment by @charliermarsh on 2024-09-24 13:53_

Not beyond the original ask.

---

_Comment by @AndydeCleyre on 2024-09-24 15:03_

> Is anyone asking for this still?

I'm still very much hoping for it, and it is still behind bugs and limitations in my own project when using the optional and mostly preferred `uv` backend, where I can't currently get away with the same handling I use for overriding `pip`, `pip-compile`, and `pip-sync` options on a per-invocation basis.

---

_Comment by @AndydeCleyre on 2024-10-09 19:50_

Would this be any more palatable if it required explicit enabling via another flag?

---

_Comment by @AndydeCleyre on 2024-10-25 15:28_

What if this usage came with a warning on stderr clarifying which option is being used?

---

_Closed by @charliermarsh on 2025-03-03 02:46_

---
