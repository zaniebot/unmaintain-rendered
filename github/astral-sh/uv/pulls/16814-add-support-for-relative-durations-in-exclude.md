---
number: 16814
title: "Add support for relative durations in `exclude-newer`"
type: pull_request
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
base: main
head: zb/exclude-newer-delay
created_at: 2025-11-21T21:59:18Z
updated_at: 2025-12-09T19:52:15Z
url: https://github.com/astral-sh/uv/pull/16814
synced_at: 2026-01-07T13:12:19-06:00
---

# Add support for relative durations in `exclude-newer`

---

_Pull request opened by @zanieb on 2025-11-21 21:59_

Adds support for "friendly" durations like, 1 week, 7 days, 24 hours using Jiff's parser. During resolution, we calculate this relative to the current time and resolve it into a concrete timestamp for the lockfile. If the span has not changed, e.g., to another relative value, then locking again will not change the lockfile. The locked timestamp will only be updated when the lockfile is invalidated, e.g., with `--upgrade`. This prevents the lockfile from repeatedly churning when a relative value is used.

---

_Referenced in [astral-sh/uv#14992](../../astral-sh/uv/issues/14992.md) on 2025-11-26 15:52_

---

_Referenced in [PlasmaPy/PlasmaPy#3158](../../PlasmaPy/PlasmaPy/issues/3158.md) on 2025-11-26 22:37_

---

_Comment by @namurphy on 2025-11-26 22:55_

Thank you for doing this!  I'm going to start using this feature immediately after 48 hours after it's released! 😹

I'm wondering if it would be helpful to add the term "cooldown" in the documentation for `--exclude-newer` since it's been gaining traction, and might be what a lot of people would do web searches for. 🤔 


---

_Referenced in [j178/prek#1152](../../j178/prek/issues/1152.md) on 2025-11-29 19:48_

---

_Referenced in [jazzband/pip-tools#2288](../../jazzband/pip-tools/issues/2288.md) on 2025-11-30 01:25_

---

_Referenced in [pypa/pip#13674](../../pypa/pip/issues/13674.md) on 2025-11-30 11:43_

---

_@zanieb reviewed on 2025-12-06 13:58_

---

_Review comment by @zanieb on `crates/uv-resolver/src/exclude_newer.rs`:275 on 2025-12-06 13:58_

Unfortunately this error message is pretty bad. I'm not sure what the best way to fix it is though.

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3045 on 2025-12-06 15:47_

```suggestion
    /// (e.g., `2006-12-02`, resolved based on your system's configured time zone), and relative durations
```

---

_@charliermarsh approved on 2025-12-06 15:51_

This seems quite reasonable to me.

---

_@zanieb reviewed on 2025-12-09 18:58_

---

_Review comment by @zanieb on `docs/concepts/resolution.md`:694 on 2025-12-09 18:58_

@woodruffw want to review this copy?

---

_Marked ready for review by @zanieb on 2025-12-09 18:58_

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:3092 on 2025-12-09 19:03_

```suggestion
    /// number of seconds assuming that a day is 24 hours (e.g., DST transitions are ignored).
```

Since DST transitions are just one example of discontinuities or repetitions in civil time. (The main other one being much rarer: when the standard time zone for a locale changes.)

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:3088 on 2025-12-09 19:05_

```suggestion
    /// duration (e.g., `24 hours`, `1 week`, `30 days`), or an ISO 8601 duration (e.g., `PT24H`,
```

For ISO 8601, the `T` designator has to be used to denote time units. So e.g., `P1DT12H` is 1 day 12 hours.

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:3107 on 2025-12-09 19:05_

```suggestion
    /// number of seconds assuming that a day is 24 hours (e.g., DST transitions are ignored).
```

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:3103 on 2025-12-09 19:05_

```suggestion
    /// (e.g., `24 hours`, `1 week`, `30 days`), or a ISO 8601 duration (e.g., `PT24H`, `P7D`,
```

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:5598 on 2025-12-09 19:06_

```suggestion
    /// duration (e.g., `24 hours`, `1 week`, `30 days`), or an ISO 8601 duration (e.g., `PT24H`,
```

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:5602 on 2025-12-09 19:06_

```suggestion
    /// number of seconds assuming that a day is 24 hours (e.g., DST transitions are ignored).
```

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:5613 on 2025-12-09 19:06_

```suggestion
    /// (e.g., `24 hours`, `1 week`, `30 days`), or an ISO 8601 duration (e.g., `PT24H`, `P7D`,
```

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:5617 on 2025-12-09 19:06_

```suggestion
    /// number of seconds assuming that a day is 24 hours (e.g., DST transitions are ignored).
```

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:6570 on 2025-12-09 19:06_

```suggestion
    /// duration (e.g., `24 hours`, `1 week`, `30 days`), or an ISO 8601 duration (e.g., `PT24H`,
```

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:6574 on 2025-12-09 19:07_

```suggestion
    /// number of seconds assuming that a day is 24 hours (e.g., DST transitions are ignored).
```

---

_Review comment by @BurntSushi on `crates/uv-cli/src/lib.rs`:6574 on 2025-12-09 19:07_

OK, I'll stop marking these. I ran out of steam lol.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/exclude_newer.rs`:166 on 2025-12-09 19:09_

It's probably fine to make this `Copy` since `Span` is also `Copy`. That will let you drop some `.clone()` calls.

---

_Label `enhancement` added by @zanieb on 2025-12-09 19:10_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/exclude_newer.rs`:191 on 2025-12-09 19:10_

I might add a note here that calls out that this will serialize to an ISO 8601 duration and that this was specifically intended for reasons of interoperability. That is, even though the "friendly" format is arguably more readable, the ISO 8601 format enjoys broader support.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/exclude_newer.rs`:324 on 2025-12-09 19:16_

Nice

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/exclude_newer.rs`:525 on 2025-12-09 19:18_

Maybe...

```rust
let Some(other_timestamp) = other.get(package) else {
    return Some(ExcludeNewerPackageChange::PackageRemoved(package.clone()));
};
...
```

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/exclude_newer.rs`:640 on 2025-12-09 19:20_

```suggestion
            "description": "Exclude distributions uploaded after the given timestamp.\n\nAccepts both RFC 3339 timestamps (e.g., `2006-12-02T02:07:43Z`) and local dates in the same format (e.g., `2006-12-02`), as well as relative durations (e.g., `1 week`, `30 days`, `6 months`). Relative durations are resolved to a timestamp at lock time.",
```

I feel like "absolute timestamp" sounds a little funny. Not a strong opinion here though.

---

_@BurntSushi approved on 2025-12-09 19:25_

LGTM with some nits. :-) Nice work on the error messages!!!

---

_@zanieb reviewed on 2025-12-09 19:27_

---

_Review comment by @zanieb on `crates/uv-resolver/src/exclude_newer.rs`:640 on 2025-12-09 19:27_

Fair enough!

---

_Merged by @zanieb on 2025-12-09 19:52_

---

_Closed by @zanieb on 2025-12-09 19:52_

---

_Branch deleted on 2025-12-09 19:52_

---

_Referenced in [astral-sh/uv#17055](../../astral-sh/uv/pulls/17055.md) on 2025-12-09 19:57_

---

_Referenced in [Homebrew/homebrew-core#257965](../../Homebrew/homebrew-core/pulls/257965.md) on 2025-12-09 23:22_

---

_Referenced in [astropy/astropy#17788](../../astropy/astropy/issues/17788.md) on 2025-12-10 11:12_

---

_Referenced in [astral-sh/uv#17079](../../astral-sh/uv/pulls/17079.md) on 2025-12-10 21:22_

---
