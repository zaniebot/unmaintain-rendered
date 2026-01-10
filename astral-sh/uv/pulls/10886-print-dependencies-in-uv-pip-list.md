```yaml
number: 10886
title: Print dependencies in uv pip list.
type: pull_request
state: open
author: tjni
labels: []
assignees: []
base: main
head: uv-pip-list-dependencies
created_at: 2025-01-23T07:00:21Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/10886
synced_at: 2026-01-10T11:10:34Z
```

# Print dependencies in uv pip list.

---

_Pull request opened by @tjni on 2025-01-23 07:00_

## Summary

This is controlled by the `--requires` and `--required-by` flags and only works when the output format is JSON.

This addresses part of https://github.com/astral-sh/uv/issues/4711, but I agree with https://github.com/astral-sh/uv/issues/4711#issuecomment-2205099775 that it belongs in `uv pip list` instead of `uv pip tree`. A few notes about this:

1. I picked `--requires` and `--required-by` to match the `Requires` and `Required-By` fields that appear in the output of `uv pip show`.
2. I decided to return the dependencies as objects with a single field called `name`. This leaves the door open to add more fields to each dependency in the future, such as installed version and requested versions.

I'm hoping to get some early feedback on this PR to test the temperature of this approach, before I proceed. The remaining work as I see it currently includes:

- [ ] Add tests for this feature.
- [ ] Add documentation for this feature.
- [ ] Better error messages (e.g. if used with the default column format, perhaps suggest `uv pip tree` instead).
- [ ] Any code review suggestions (I'm a Rust novice).

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @Gankra by @Gankra on 2025-01-23 14:22_

---

_Comment by @tjni on 2025-01-23 20:30_

As I was doing more research about this, I noticed that https://github.com/pypa/pip/pull/11097 is prior art, where incorporating arbitrary metadata into the output of `pip list` was rejected in favor eventually of `pip inspect`. This brings up the question to us too about where to draw the line about what to include under this command. I'm not sure.

---

_Comment by @tjni on 2025-01-31 08:16_

Hi team, wondering if you’ve had a chance to think about if this is the right form factor for exposing this information. Thank you!

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1982 on 2025-02-03 22:30_

Based on the semantics you've implemented this seems like this should also include `conflicts_with = "required_by"`.

---

_Review comment by @Gankra on `crates/uv-cli/src/lib.rs`:1985 on 2025-02-03 22:30_

Hmm what's the usecase for having this opt out flag too?

---

_Review comment by @Gankra on `crates/uv/src/commands/pip/list.rs`:163 on 2025-02-03 22:33_

I'd prefer to structure this as

```
let requires_map = requires.then(|| { ... });
let required_by_map = required_by.then(|| { ... });
```

As-is it's weird to have this shared variable for two conceptually independent results just because they happen to have the same type. It's also not clear to me that these need to be mutually exclusive outputs? (And if that's the case, ignore the note above about the cli conflicts_with)

---

_@Gankra reviewed on 2025-02-03 22:56_

Apologies for the delay! I think the basic idea looks reasonable, I mostly have a few quibbles about the structure.

---

_Comment by @zanieb on 2025-02-03 22:59_

I'm also wondering about how this fits into our CLI... like `uv pip list` has flags to show dependencies but this overlaps with `uv tree` right? There are also requests for things like `uv license` (https://github.com/astral-sh/uv/pull/10292) to show license information. I'm sort of in favor of `uv inspect` / `uv show` to see metadata about packages instead? 
 

---

_Comment by @zanieb on 2025-02-03 23:01_

I guess more broadly it's unclear if we should have `uv list` which is an alternative to `uv tree` somehow? I find it confusing already that we have both `pip tree` and `pip list` commands.

---
