```yaml
number: 6562
title: Allow local date for inline exclude newer
type: pull_request
state: closed
author: hauntsaninja
labels:
  - documentation
  - enhancement
assignees: []
base: main
head: exclude-nwer
created_at: 2024-08-23T23:36:46Z
updated_at: 2024-08-30T03:16:41Z
url: https://github.com/astral-sh/uv/pull/6562
synced_at: 2026-01-12T16:07:25Z
```

# Allow local date for inline exclude newer

---

_@hauntsaninja_

Resolves #6555

---

_@zanieb approved on 2024-08-23 23:40_

Thank you!

---

_@zanieb reviewed on 2024-08-23 23:41_

---

_Review comment by @zanieb on `crates/uv-resolver/src/exclude_newer.rs`:70 on 2024-08-23 23:41_

cc @BurntSushi I remember some recent discussion about local dates?

---

_Review requested from @BurntSushi by @zanieb on 2024-08-23 23:41_

---

_Label `documentation` added by @zanieb on 2024-08-23 23:41_

---

_Label `enhancement` added by @zanieb on 2024-08-23 23:41_

---

_Comment by @zanieb on 2024-08-27 02:00_

@hauntsaninja on further thought, isn't it _wrong_ to allow a local date for `exclude-newer` in files that are intended to be version controlled and distributed? It seems like a bit of a correctness problem to have the script resolve to different dependencies depending on who is running it. It might make sense to require people to use a proper timestamp here that will be correct regardless of the execution timezone.

I think this notably differs from `UV_EXCLUDE_NEWER` which is configured on a single machine and not easily distributed.

---

_Comment by @hauntsaninja on 2024-08-27 04:32_

It's a fair concern! The use case I'm thinking of is "I wrote a script a few years ago and I want it to mostly run the same, maximising convenience / bang for buck". My guess is this use case will be much more common than inter-timezone distribution. We could also resolve different dependencies for different people depending on arch or python version as well, it's why I phrased this as "improving" reproducibility.

---

_Comment by @zanieb on 2024-08-27 11:13_

I feel like if we want to maximize convenience we should allow emitting an exclude newer pin (in UTC) via a command or, even better, we should be able to write a lock to scripts. Hm.

If you're willing to separate the change from the docs, we can merge the docs right away. I think we'll wait for @BurntSushi to be back from vacation to make a decision on the local dates.

---

_Assigned to @BurntSushi by @zanieb on 2024-08-29 16:04_

---

_@BurntSushi reviewed on 2024-08-29 16:26_

I think this is making the `serde::Deserialize` trait implementation on `ExcludeNewer` consistent with its `FromStr` trait implementation.

I am still pretty iffy on supporting local dates here though. When I added support for it to `ExcludeNewer`, I was thinking about its use on the command line or in user-specific configuration files. But this seems to be expanding its scope to a point where I think it actually becomes problematic. If, for example, `exclude-newer = "2023-10-16"` is put into a script and then shared with someone else, that date may be interpreted as an entirely different instant than is intended. It might be better to just keep it restricted to RFC 3339 timestamps. Although, the difference in the `Deserialize` and `FromStr` trait implementation seems like a footgun.

---

_Comment by @BurntSushi on 2024-08-29 16:32_

For completeness, one alternative here is to parse an RFC 9557 timestamp (an extension to RFC 3339). So you could write something like `2024-08-28[America/New_York]`. That would let you write something "local" that would still (almost certainly) translate to the same instant on anyone else's machine. But it's a bit of a heavier tool to leverage and technically relies on a time zone database and so on.

---

_Comment by @BurntSushi on 2024-08-29 16:34_

Related: https://github.com/astral-sh/uv/issues/6820

---

_Comment by @hauntsaninja on 2024-08-29 20:12_

Actually one possibly silly thought, should we make the short form just be a UTC timestamp? In the short form I think it's arguably already a little unclear what the boundary semantics are (e.g. `--exclude-newer 2024-08-28` does kind of read like `2024-08-28T23:59:59` to me).

This is also nice if you have a cloud machine on UTC time and you want to reproduce a command using the short form on your local machine. It's technically a breaking change, but it seems maybe saner, and I have a hard time thinking about the user profile who would be broken by it.

---

_Comment by @hauntsaninja on 2024-08-29 20:12_

Anyway, sounds like I should split the PRs regardless: https://github.com/astral-sh/uv/pull/6831 for docs!

---

_Renamed from "Add docs for inline exclude newer, allow local date" to "Allow local date for inline exclude newer" by @hauntsaninja on 2024-08-29 21:57_

---

_Comment by @BurntSushi on 2024-08-29 23:54_

> Actually one possibly silly thought, should we make the short form just be a UTC timestamp?

This is what it was originally, but I changed it. It's generally bad juju to just assume UTC for an unadorned date. I think we either don't support an unadorned date or we assume it's in the user's local time zone. But in this case, "user's local time zone" creates a configuration that isn't self-contained.

In terms of the time for a date without a time, it's common practice to interpret it as the first instant on that date (which, interestingly, [may not be midnight](https://docs.rs/jiff/latest/jiff/struct.Zoned.html#method.start_of_day)).

It's probably best to just required an RFC 3339 timestamp here. `2024-08-28` verus `2024-08-28T00:00:00Z` isn't too onerous for persistent configuration I think. On the CLI, it's nice to support `2024-08-28` since it's easier to type, and in that context, it's perhaps more justifiable to assume the user's system time zone (and which can even be overridden by setting the `TZ` environment variable).

---

_Closed by @hauntsaninja on 2024-08-30 03:16_

---

_Branch deleted on 2024-08-30 03:16_

---

_Comment by @hauntsaninja on 2024-08-30 03:16_

Sounds good!

---
