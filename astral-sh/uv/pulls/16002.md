```yaml
number: 16002
title: "Add support for explicit variants and `--all-variants` flag with `uv python list`"
type: pull_request
state: open
author: shaanmajid
labels: []
assignees: []
base: main
head: add-all-variants-flag
created_at: 2025-09-23T14:46:02Z
updated_at: 2025-12-16T01:41:22Z
url: https://github.com/astral-sh/uv/pull/16002
synced_at: 2026-01-10T05:49:14Z
```

# Add support for explicit variants and `--all-variants` flag with `uv python list`

---

_Pull request opened by @shaanmajid on 2025-09-23 14:46_

Adds `--all-variants` flag to `uv python list` to show debug Python builds that are normally hidden.

## Functional Requirements Met:
- **Default behavior**: Only default variants shown (hides debug/freethreaded)
- **--all-variants flag**: Shows all variants including debug, freethreaded, freethreaded+debug  
- **Explicit requests**: `3.13+debug` works without needing the flag
- **Flag compatibility**: Works with `--all-versions`, `--only-downloads`, etc.

## Key Behavior:
```bash
# Default - only shows default variant
uv python list 3.13  # → cpython-3.13.7-platform

# Show all variants for specific version  
uv python list 3.13 --all-variants  # → default, debug, freethreaded, freethreaded+debug

# Explicit variant request (works without flag)
uv python list 3.13+debug  # → cpython-3.13.7+debug-platform
```

Fixes #15983

---

_@zanieb reviewed on 2025-09-23 14:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/list.rs`:79 on 2025-09-23 14:55_

This looks wrong — we shouldn't drop the original request from the user. Though I see you filter using it later, then we'll need to duplicate a bunch of logic that happens in discovery. We'll also do a bunch of work querying interpreters that do not match the request. I'm not quite sure how best to achieve what you're trying to do here though.

---

_Comment by @zanieb on 2025-09-23 14:57_

I think this issue may be harder than it looks. Maybe it'd be a good place to start just adjusting the _downloads_ we show based on an `--all-variants` flag?

---

_Comment by @zanieb on 2025-09-23 15:09_

Oh it looks like you are only changing the download listing logic here, so perhaps disregard that part (though we'll need to figure out how to show all variants we discover too at some point)

---

_Comment by @zanieb on 2025-09-23 15:11_

Maybe you need to add something like 

https://github.com/astral-sh/uv/blob/dea17009455249b7c76a8575d73dd76c7796a0fc/crates/uv-python/src/downloads.rs#L163-L165

for variants?

---

_@shaanmajid reviewed on 2025-09-24 16:13_

---

_Review comment by @shaanmajid on `crates/uv/src/commands/python/list.rs`:79 on 2025-09-24 16:13_

yeah that was pretty troll mb :/

Reworked the implementation based on your feedback

---

_Review requested from @zanieb by @shaanmajid on 2025-09-24 21:35_

---

_Comment by @shaanmajid on 2025-09-24 21:36_

Testing was a bit annoying ; I didn't realize debug variants were not supported on Windows at first. Last minute split up a lot of test-cases based off target, but lmk if you think it's worth organizing this better 

---

_Comment by @shaanmajid on 2025-09-29 17:25_

Hey @zanieb, just wanted to check whether there’s anything else you’d like me to do on this PR. Happy to rebase or tweak if needed :)

---

_@zanieb reviewed on 2025-10-07 18:59_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1731 on 2025-10-07 18:59_

We generally prefer to list all the variants so if we add a new one we're forced to revisit this and see if it needs to be updated.

---

_@zanieb reviewed on 2025-10-07 18:59_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:167 on 2025-10-07 18:59_

What does "to filter by the requested variant" mean here?

---

_@zanieb reviewed on 2025-10-07 19:00_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:167 on 2025-10-07 19:00_

You mean like, one in the `VersionRequest`?

---

_@zanieb reviewed on 2025-10-07 19:01_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:465 on 2025-10-07 19:01_

I'm confused that this is an `Option` if we're not using the inner boolean value?

---

_@zanieb reviewed on 2025-10-07 19:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/list.rs`:63 on 2025-10-07 19:01_

I don't think we should use this — we're moving away from `miette`. You can see other `hint` formatting to see how we're handling this for now.

---

_@zanieb reviewed on 2025-10-07 19:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/list.rs`:94 on 2025-10-07 19:05_

Leaving a note for https://github.com/astral-sh/uv/pull/16002/files#r2411619048

---

_@zanieb reviewed on 2025-10-07 19:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/list.rs`:144 on 2025-10-07 19:05_

I'm confused that we're changing the filtering here _and_ at https://github.com/astral-sh/uv/pull/16002/files#r2411618419

Why do we need both?

---

_@shaanmajid reviewed on 2025-10-08 21:10_

---

_Review comment by @shaanmajid on `crates/uv/src/commands/python/list.rs`:63 on 2025-10-08 21:10_

Switched to use standard `hint` formatting

---

_@shaanmajid reviewed on 2025-10-08 21:10_

---

_Review comment by @shaanmajid on `crates/uv-python/src/downloads.rs`:465 on 2025-10-08 21:10_

Fixed to use an explicit boolean

---

_Renamed from "feat: add --all-variants flag to uv python list" to "Add support for explicit variants and `--all-variants` flag with `uv python list`" by @shaanmajid on 2025-10-08 22:46_

---

_@shaanmajid reviewed on 2025-10-08 22:47_

---

_Review comment by @shaanmajid on `crates/uv/src/commands/python/list.rs`:144 on 2025-10-08 22:47_

Updated and consolidated.

---

_@shaanmajid reviewed on 2025-10-08 23:01_

---

_Review comment by @shaanmajid on `crates/uv-python/src/discovery.rs`:1731 on 2025-10-08 23:01_

Updated to use explicit listing.

---

_@shaanmajid reviewed on 2025-10-08 23:02_

---

_Review comment by @shaanmajid on `crates/uv-python/src/downloads.rs`:167 on 2025-10-08 23:02_

Yeah, that was the intent. Sorry, the conflicting terminology / overload between Python's "variants" and Rust variants is a bit unfortunate here haha. Updated the comment a bit to further elaborate.

---

_Comment by @shaanmajid on 2025-10-08 23:12_

@zanieb Wanted to quickly double-check about pre-existing behavior with the display of freethreaded variants.

Currently:
- `uv python list` (i.e. no explicit request) shows both default and free-threaded variants (i.e. all non-debug variants)
- `uv python list 3.13` (i.e. explicit request) shows only the default variant

This seems intentional and aligns with treating freethreaded as non-experimental in Python 3.14+ and uv 0.9+ (per #16142). Just wanted to confirm this is the intended behavior, since this PR makes the variant filtering more explicit.

---

_Review requested from @zanieb by @shaanmajid on 2025-10-08 23:13_

---

_Comment by @shaanmajid on 2025-11-03 22:49_

Hey @zanieb, just wanted to check back on this. Is there anything else you would like changed? Also happy to just close the PR if you don't think y'all have time for it.

---
