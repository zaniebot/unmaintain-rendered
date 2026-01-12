```yaml
number: 9687
title: "Replace `--show-source` and `--no-show-source` with `--output_format=<full|concise>`"
type: pull_request
state: merged
author: snowsignal
labels:
  - cli
  - preview
assignees: []
merged: true
base: release/0.2.0
head: jane/cli/replace-show-sources-with-format
created_at: 2024-01-29T19:21:34Z
updated_at: 2024-02-01T20:17:20Z
url: https://github.com/astral-sh/ruff/pull/9687
synced_at: 2026-01-12T15:55:30Z
```

# Replace `--show-source` and `--no-show-source` with `--output_format=<full|concise>`

---

_@snowsignal_

Fixes #7350

## Summary

* `--show-source` and `--no-show-source` are now deprecated.
* `output-format` supports two new variants, `full` and `concise`. `text` is now a deprecated variant, and any use of it is treated as the default serialization format.
* `--output-format` now default to `concise`
* In preview mode, `--output-format` defaults to `full`
* `--show-source` will still set `--output-format` to `full` if the output format is not otherwise specified.
* likewise, `--no-show-source` can override an output format that was set in a file-based configuration, though it will also be overridden by `--output-format`

## Test Plan

A lot of tests were updated to use `--output-format=full`. Additional tests were added to ensure the correct deprecation warnings appeared, and that deprecated options behaved as intended.


---

_Label `cli` added by @snowsignal on 2024-01-29 19:21_

---

_Review requested from @charliermarsh by @snowsignal on 2024-01-29 19:21_

---

_Review requested from @zanieb by @snowsignal on 2024-01-29 19:21_

---

_Renamed from "Replace `--show-source` and `--no-show-source` with `output_format=<full|concise>`" to "Replace `--show-source` and `--no-show-source` with `--output_format=<full|concise>`" by @snowsignal on 2024-01-29 19:21_

---

_Comment by @zanieb on 2024-01-29 19:24_

Woo! Maybe we should merge this into `release/0.2.0` instead?

---

_@zanieb reviewed on 2024-01-29 19:29_

---

_Review comment by @zanieb on `crates/ruff/src/printer.rs`:274 on 2024-01-29 19:29_

Is it feasible to warn during CLI parsing instead of during display? I'm not sure if that's much better but then we can easily do things like throw an error if preview mode is enabled (instead of warning).

---

_@zanieb reviewed on 2024-01-29 19:30_

---

_Review comment by @zanieb on `crates/ruff/src/args.rs`:600 on 2024-01-29 19:30_

Seems safe to call this something simpler like `resolve_output_format` instead

---

_@zanieb reviewed on 2024-01-29 19:31_

---

_Review comment by @zanieb on `docs/configuration.md`:533 on 2024-01-29 19:31_

Can we include the default format here?

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:607 on 2024-01-29 19:44_

Nit: consider inlining the `o` into the format string? Assuming the macro supports it.

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:638 on 2024-01-29 19:45_

This is very nicely done.

---

_Review comment by @charliermarsh on `crates/ruff/src/printer.rs`:276 on 2024-01-29 19:46_

I'm wondering if we should get rid of this flag, and instead match against `SerializationFormat::Full`?

---

_Review comment by @charliermarsh on `docs/configuration.md`:533 on 2024-01-29 19:47_

I wish Clap would do this but I think we have to use an `Option` here to detect whether it was provided at all.

---

_@charliermarsh reviewed on 2024-01-29 19:48_

This is great work -- really thorough and well-done. My only concern is: should we merge with `concise` as the default for now, so that it's not a change in behavior and just a deprecation?

---

_@snowsignal reviewed on 2024-01-29 19:55_

---

_Review comment by @snowsignal on `docs/configuration.md`:533 on 2024-01-29 19:55_

I can include the default format as part of the description string for `CheckCommand::output_format`, if that works 😄 

---

_Comment by @snowsignal on 2024-01-29 19:59_

> My only concern is: should we merge with `concise` as the default for now, so that it's not a change in behavior and just a deprecation?

That's a good question! I thought it would be useful to solve two issues at once, but if it's too big of a change I'd be OK with splitting the changes into a separate PR. I'm curious to know your thoughts on this @zanieb.

---

_Comment by @zanieb on 2024-01-29 21:29_

So I feel like the concern is that someone doesn't like the new output then is upset that the flag to toggle it back is deprecated? Perhaps it's just that there are too many changes in a single release?

My suggestion would be to gate the default with preview if feasible e.g. if `--preview` is passed then use `--output-format=full` and if not then use `--output-format=concise`. This would allow us to roll out the change in default separately in `0.3.0` instead.

Curious for @charliermarsh's thoughts though before we go changing things.

---

_Comment by @snowsignal on 2024-01-29 22:00_

I added a new bundle of tests that ensure the deprecated options behave as expected.

> So I feel like the concern is that someone doesn't like the new output then is upset that the flag to toggle it back is deprecated? Perhaps it's just that there are too many changes in a single release?

I understand that concern, but doesn't `--output-format=concise` solve that for them? Anyone using the deprecated flags will ideally get pointed in the right direction with the warning messages.

That being said, I'm not against gating this behind `preview` (though it would mean more work to split up these changes)

---

_Review requested from @charliermarsh by @snowsignal on 2024-01-29 22:15_

---

_Comment by @charliermarsh on 2024-01-29 22:22_

Hmm yeah, introspecting a bit, I think my concern is that I wanted more user feedback before deciding that the `full` output format should be the default (which is a change from main, though was intended to be a settled decision per the relevant issue). So I was hoping to ship it to preview first to give us a chance to react to user feedback on the change. But I'm willing to give up on that, perhaps I'm being too cautious?


---

_Comment by @zanieb on 2024-01-29 23:07_

I think we're pretty settled on wanting to move that direction as our default output because we can build a lot on top of it.

---

_Comment by @snowsignal on 2024-01-29 23:27_

I've reverted us back to using `concise` as a default value. I'm fine with gating this behind `preview` until `0.3.0`.

---

_Review requested from @zanieb by @snowsignal on 2024-01-29 23:37_

---

_@charliermarsh approved on 2024-01-29 23:55_

Excellent! I'm still open to changing the default in v0.2.0, but we don't have to decide that to merge this PR.

---

_@charliermarsh reviewed on 2024-01-29 23:55_

---

_Review comment by @charliermarsh on `crates/ruff/src/args.rs`:135 on 2024-01-29 23:55_

Nit: should we document the preview difference? (Up to you, not a strong opinion.)

---

_@charliermarsh reviewed on 2024-01-29 23:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/printer.rs`:437 on 2024-01-29 23:56_

Should this now vary on preview?

---

_@snowsignal reviewed on 2024-01-30 00:02_

---

_Review comment by @snowsignal on `crates/ruff/src/printer.rs`:437 on 2024-01-30 00:02_

It should, good catch!

---

_Merged by @snowsignal on 2024-01-30 00:57_

---

_Closed by @snowsignal on 2024-01-30 00:57_

---

_Branch deleted on 2024-01-30 00:57_

---

_Label `preview` added by @zanieb on 2024-01-30 01:20_

---

_@AlexWaygood reviewed on 2024-02-01 20:10_

---

_Review comment by @AlexWaygood on `crates/ruff/src/lib.rs`:324 on 2024-02-01 20:10_

Does this respect `preview = true` in configuration files? I _think_ this might only respect `--preview` when it's set on the command line -- is that deliberate?

---

_@snowsignal reviewed on 2024-02-01 20:15_

---

_Review comment by @snowsignal on `crates/ruff/src/lib.rs`:324 on 2024-02-01 20:15_

No that was not deliberate, good catch.

---

_@AlexWaygood reviewed on 2024-02-01 20:17_

---

_Review comment by @AlexWaygood on `crates/ruff/src/lib.rs`:324 on 2024-02-01 20:17_

No worries! Was trying to pin down the build failures on #9599 after merging in `main`, and spotted this :)

---
