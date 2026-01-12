```yaml
number: 8203
title: "Harmonize help commands' `--format` to `--output-format` with deprecation warnings"
type: pull_request
state: merged
author: akx
labels:
  - cli
assignees: []
merged: true
base: main
head: harmonize-format
created_at: 2023-10-25T06:27:23Z
updated_at: 2023-10-29T13:24:31Z
url: https://github.com/astral-sh/ruff/pull/8203
synced_at: 2026-01-12T15:55:25Z
```

# Harmonize help commands' `--format` to `--output-format` with deprecation warnings

---

_@akx_

## Summary

Since `--format` was changed to `--output-format` for `check`, it feels like it makes sense for the same to work for the auxiliary commands.

This 

* adds the same deprecation warning that used to be a thing in #7514 (and un-became a thing in #7984)

Fixes #7990.

## Test Plan

* `cargo run --bin=ruff -- rule --all --output-format=json` works
* `cargo run --bin=ruff -- rule --format=json` works with warnings


---

_Label `cli` added by @MichaReiser on 2023-10-25 06:32_

---

_Comment by @MichaReiser on 2023-10-25 06:32_

Nice. I linked the related issue. Would it be possible to log a warning when using `--format` that mention that it is deprecated in favor of `--output-format`?

---

_Review requested from @zanieb by @MichaReiser on 2023-10-25 06:32_

---

_Comment by @akx on 2023-10-25 06:42_

Ah, I didn't even know that there was an issue about this 😁

Sure, I can canonicalize it all to `output_format` and add warnings, but will it be weird that the help commands support a subset of the main `check` output formats with the same flag name?

---

_Comment by @github-actions[bot] on 2023-10-25 06:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2023-10-25 07:09_

> Sure, I can canonicalize it all to `output_format` and add warnings, but will it be weird that the help commands support a subset of the main `check` output formats with the same flag name?

I think that's fine, especially considering that it used to be the same before? @zanieb what's your take?

---

_Renamed from "Support --output-format as alias for --format for help commands" to "Harmonize help commands' `--format` to `--output-format` with deprecation warnings" by @akx on 2023-10-25 07:32_

---

_Comment by @akx on 2023-10-25 07:34_

@MichaReiser Done deal ✨ 

---

_@charliermarsh approved on 2023-10-25 17:10_

---

_@zanieb requested changes on 2023-10-25 18:07_

I think this needs a bit more discussion before it goes out.

I think it's okay for a subset of formats to be supported on the CLI, but it doesn't really make sense when using the environment variable. Like.. if you've set your output format in your environment to something not supported by another command it'll just fail?

In combination with that concern, it seems problematic to add support for `RUFF_FORMAT` to commands to that did not previously have it as all use of those commands could start to fail.

In general, I'm also not sure it makes sense to add support for deprecated options to new commands i.e. adding `--format` support to `ruff version`.



---

_Comment by @charliermarsh on 2023-10-25 18:11_

> I think it's okay for a subset of formats to be supported on the CLI, but it doesn't really make sense when using the environment variable. Like.. if you've set your output format in your environment to something not supported by another command it'll just fail?

Can you expand on this?

---

_Comment by @zanieb on 2023-10-25 18:19_

Well we have the output formats supported by `ruff check`: text, json, json-lines, junit, grouped, github, gitlab, pylint, azure

And then we have the output formats supported by "help" commands: text, json

If we fail on unknown formats and use `RUFF_FORMAT`/`RUFF_OUTPUT_FORMAT` for both of these I believe this will break existing usage and be confusing for future users.

```
export RUFF_OUTPUT_FORMAT=github
ruff check    # OK
ruff version  # FAILS with invalid output format
``` 

I don't think the help formats should use the same variable at least, and maybe they shouldn't support environment variables at all.

---

_Comment by @charliermarsh on 2023-10-25 18:33_

Hmm I see. How about leaving the `--format` and `--output-format` support for these commands (for consistency, for now -- we'd then deprecate `--format` for all of them at once), but omitting the environment variable support?

---

_Comment by @akx on 2023-10-26 08:38_

@charliermarsh @zanieb Done: removed the envvars, removed `--format` from `version` (which hadn't had it in the first place).

---

_Review requested from @zanieb by @akx on 2023-10-26 08:38_

---

_Review requested from @charliermarsh by @akx on 2023-10-26 08:38_

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:111 on 2023-10-26 15:40_

I'd prefer the shorter...

```suggestion
        warn_user!("The `--format` argument is deprecated. Use `--output-format` instead.");
```

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:110 on 2023-10-26 15:40_

I guess this doesn't matter since clap will error if you provide both? but I'd expect the `output_format` to take precedence.

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:179 on 2023-10-26 15:42_

Perhaps `resolve_help_output_format` would be a better function name? No strong feelings though.

---

_@zanieb approved on 2023-10-26 15:43_

Thank you for making the changes! I have some minor comments.

---

_@akx reviewed on 2023-10-26 16:45_

---

_Review comment by @akx on `crates/ruff_cli/src/lib.rs`:110 on 2023-10-26 16:45_

It doesn't, really, but only one of these is an `Option` type so I don't see a way to do it the other way around :)

---

_@zanieb reviewed on 2023-10-26 18:06_

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:110 on 2023-10-26 18:06_

👍 sounds good thanks!

---

_Comment by @charliermarsh on 2023-10-28 03:27_

Thank you as always @akx!

---

_Merged by @charliermarsh on 2023-10-28 03:30_

---

_Closed by @charliermarsh on 2023-10-28 03:30_

---

_Branch deleted on 2023-10-29 13:24_

---
