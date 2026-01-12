```yaml
number: 8553
title: "Support dates for `exclude-newer` tool setting"
type: pull_request
state: closed
author: manzt
labels: []
assignees: []
base: main
head: manzt/more-flexible-exclude-newer
created_at: 2024-10-25T03:12:28Z
updated_at: 2024-11-13T19:44:48Z
url: https://github.com/astral-sh/uv/pull/8553
synced_at: 2026-01-12T16:08:22Z
```

# Support dates for `exclude-newer` tool setting

---

_@manzt_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

hey! I was poking around with the tool support for exclude newer in juv (think this would be awesome to use to make Jupyter notebooks more standalone/reproducible) and ran into an issue.

The settings documentation for [`exclude-newer`](https://docs.astral.sh/uv/reference/settings/#exclude-newer) says that it supports both dates and timestamp. 

However, with uv v0.4.26:

```sh
uv init foo
cd foo
echo "[tool.uv]\nexclude-newer = '2022-01-01'" >> pyproject.toml
uv run hello.py
#  warning: Failed to parse `pyproject.toml` during settings discovery:
#   TOML parse error at line 9, column 17
#     |
#   9 | exclude-newer = "2022-01-01"
#     |                 ^^^^^^^^^^^^
#   failed to find time component in "2022-01-01", which is required for parsing a timestamp
```

I believe this is because the derive'd Deserialize from jiff only supports timestamps and doesn't use the more flexible FromStr implemention implemented for `ExlcudeNewer`

## Test Plan

Added a test with a date string


---

_Renamed from "Support dates for exclude-newer tool setting" to "Support dates for `exclude-newer` tool setting" by @manzt on 2024-10-25 03:15_

---

_Comment by @zanieb on 2024-10-25 04:13_

What timezone does this use? UTC?

I'm not sure if this is quite the same, but we discussed something like this in https://github.com/astral-sh/uv/pull/6562 and decided against the change.

---

_Comment by @manzt on 2024-10-25 04:31_

The docs say:

> Accepts both RFC 3339 timestamps (e.g., 2006-12-02T02:07:43Z) and local dates in the same format (e.g., 2006-12-02) in your system's configured time zone.

So I'm guessing whatever the system's time zone is.

---

_Comment by @manzt on 2024-10-25 04:31_

Looking at the `from_str` implementation for ExcludeNewer, it looks like that way:

```rust
            .and_then(|date| date.to_zoned(TimeZone::system()))
```

https://github.com/manzt/uv/blob/2e795ffc4b4976584b5bb67b6860681e6492f687/crates/uv-resolver/src/exclude_newer.rs#L34-L68

If that's not the intention, than maybe we should change [the example](https://docs.astral.sh/uv/reference/settings/#exclude-newer) in the docs.

---

_Comment by @zanieb on 2024-10-25 04:41_

Yeah I think we might want to change the docs or, if we want to accept a simple date, use UTC. I think the intent is that we want a stable time in `exclude-newer` encoded in files. For example, if someone clones your repository they shouldn't get a different resolution because they have a different system time.

@BurntSushi has the most context on this though.

---

_Comment by @manzt on 2024-10-25 04:54_

> For example, if someone clones your repository they shouldn't get a different resolution because they have a different system time.

Makes total sense. I'm thinking the main "push" for flexibility here (as mentioned in #6562) is convenience (i.e., actually typing out the timestamp is annoying), but not that someone adamantly wants ambiguity. Maybe something to consider is just a convenient high-level API for adding this setting correctly:

```sh
uv add --timestamp
uv add --script foo.py --timestamp
uv add --timestamp=2024-01-01 
```

General idea is that you could have something conveniently resolve the users local time to something correct, keeping strictness with the target. (Maybe `add` isn't the right namespace but just first thing that came to mind).


EDIT: another idea

```sh
uv init --with-timestamp --script foo.py
```

---

_Comment by @manzt on 2024-10-25 05:10_

Ahh, seeing you already suggested a command in #6562... just catching up

---

_Comment by @zanieb on 2024-10-25 13:50_

Yeah I think this kind of needs to wait for a `uv config set` command

---

_Review requested from @BurntSushi by @charliermarsh on 2024-10-28 12:43_

---

_Comment by @BurntSushi on 2024-10-28 13:58_

Yeah, I basically think that we probably should not support writing unadorned dates in a persistent configuration that is meant to be shared among folks that may be in different time zones. It just makes the date ambiguous. Even if we defaulted to UTC, it's still not immediately obvious to someone reading the config file that it would be UTC. And it seems much more natural to _assume_ it isn't UTC but actually local, which would lead to folks reading the date incorrectly in some cases.

I think a hypothetical `uv config set` command that owns the "interpretation of the date as local, but writes it as an unambiguous timestamp" is a good idea.

There are other options for writing timestamps to config files that are maybe a little more user friendly. For example, we could support `2024-10-28T00:00:00-04[US/Eastern]`. But probably just writing out `2024-10-28T00:00:00Z` is the way to go.

---

_Comment by @zanieb on 2024-10-28 14:07_

Sounds like we should update the documentation to clarify this.

---

_Comment by @manzt on 2024-10-28 14:30_

> I think a hypothetical `uv config set` command that owns the "interpretation of the date as local, but writes it as an unambiguous timestamp" is a good idea.

Sounds good to me! Not something I'd considered and appreciate the thoughtfulness and explanation by you both.

---

_Closed by @manzt on 2024-10-28 14:30_

---

_Branch deleted on 2024-10-28 14:30_

---

_Comment by @manzt on 2024-11-13 19:44_

Until a built-in command exists, I’ve added `stamp` command to [juv](https://github.com/manzt/juv) for pinning unambiguous timestamps directly into inline script metadata (in both notebooks and scripts).

```sh
uv init --script foo.py
uv add --script foo.py polars
uvx juv stamp foo.py --date=2021-07-07
```

---
