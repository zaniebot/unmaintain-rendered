```yaml
number: 19665
title: Add support for using uv as an alternative formatter backend
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
  - server
assignees: []
merged: true
base: main
head: zb/format-uv
created_at: 2025-07-31T17:45:28Z
updated_at: 2025-09-09T15:09:55Z
url: https://github.com/astral-sh/ruff/pull/19665
synced_at: 2026-01-10T17:46:21Z
```

# Add support for using uv as an alternative formatter backend

---

_Pull request opened by @zanieb on 2025-07-31 17:45_

This adds a new `backend: internal | uv` option to the LSP `FormatOptions` allowing users to perform document and range formatting operations though uv. The idea here is to prototype a solution for users to transition to a `uv format` command without encountering version mismatches (and consequently, formatting differences) between the LSP's version of `ruff` and uv's version of `ruff`.

The primarily alternative to this would be to use uv to discover the `ruff` version used to start the LSP in the first place. However, this would increase the scope of a minimal `uv format` command beyond "run a formatter", and raise larger questions about how uv should be used to coordinate toolchain discovery. I think those are good things to explore, but I'm hesitant to let them block a `uv format` implementation. Another downside of using uv to discover `ruff`, is that it needs to be implemented _outside_ the LSP; e.g., we'd need to change the instructions on how to run the LSP and implement it in each editor integration, like the VS Code plugin.

---

_Comment by @github-actions[bot] on 2025-07-31 18:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2025-08-01 15:59_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/format.rs`:277 on 2025-08-01 15:59_

We should probalby show a message to the user if that happens see `Client`

---

_@zanieb reviewed on 2025-08-01 16:09_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:277 on 2025-08-01 16:09_

Can we differentiate between failures due to syntax errors in an easy way?

Will look at the `Client` for sending the message. Thank you!

---

_@zanieb reviewed on 2025-08-01 17:20_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:277 on 2025-08-01 17:20_

See https://github.com/astral-sh/ruff/pull/19665/commits/d23af97c3946e9e74a727fde26ed6381a0393670

My understanding is this will propagate upstream as the `format_internal` errors do. We could show a message directly to the user with more context, but I think we can defer that for now?

---

_Comment by @zanieb on 2025-08-22 19:26_

@dhruvmanila would you mind helping me ship this? I'm not sure what all needs to happen :)

---

_Marked ready for review by @zanieb on 2025-08-22 19:26_

---

_Comment by @dhruvmanila on 2025-08-25 08:35_

> would you mind helping me ship this? I'm not sure what all needs to happen :)

Sure! I'll look into this in a bit

---

_Comment by @zanieb on 2025-08-25 12:23_

I'll look into the test failures.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:182 on 2025-08-25 12:37_

Should this use `self.options.line_width()` and` self.options.indent_width()` instead?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:201 on 2025-08-25 12:40_

Same as above, `self.options.indent_style()` ?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:212 on 2025-08-25 12:44_

I'm a bit surprised that this works, is this equivalent to `--config format.quote-style = 'single'` (notice there are no quotes to the `--config` value):

```console
$ ruff format --config format.quote-style = 'single' --diff isolated.py               
error: invalid value 'format.quote-style' for '--config <CONFIG_OPTION>'

  tip: A `--config` flag must either be a path to a `.toml` configuration file
       or a TOML `<KEY> = <VALUE>` pair overriding a specific configuration
       option

For more information, try '--help'.
```

I think the entire value would need to be quoted, so the following should work:
```console
$ ruff format --config "format.quote-style = 'single'" --diff isolated.py
```

Same goes for other `--config` flags.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:170 on 2025-08-25 12:50_

This will always use the `uv` command that comes the earliest on PATH as seen from within the editor. Do you see any issues that could occur when doing so?

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:171 on 2025-08-25 12:50_

Yeah, we'd also need to provide a better error message when the `uv` command itself isn't available.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:139 on 2025-08-25 12:53_

Returning `Result` type here would mean that any errors resulted would be counted as internal server error and will be shown to the user. I think this is fine for now, but we might want to categorize a subset of them with better error message for example the TODO comment to check whether the subcommand is available or not.

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/options.rs`:291 on 2025-08-25 13:09_

Including it directly in the `format` table might indicate that this is a stable option, I think there are two ways to communicate the experimental nature of this feature:
1. Make it explicit in the documentation
2. Include it under `experimental.format.backend` similar to ty's `experimental` namespace (https://docs.astral.sh/ty/reference/editor-settings/#experimental)

---

_@dhruvmanila reviewed on 2025-08-25 13:13_

Thanks!

I read the corresponding Notion document for this in which you've mentioned:

> Micha and I talked quite a bit offline about the IDE experience, and there are a couple possible approaches that we can take to alleviate those problems.

~Is there any mention of other approaches apart from the one that's implemented in this PR?~ Nevermind, I see it in the PR description.

There are couple of follow-ups that would need to be done:
1. Updating the documentation to include the new option, this needs to be done manually for editor settings
2. Updating the VS Code extension to expose this option in VS Code settings and pass it to the server

Can you confirm whether this was tested in the editor? If so, which editor did you test this with, I can try it out in some other editor. 

---

_Comment by @dhruvmanila on 2025-08-25 13:19_

> The primarily alternative to this would be to use uv to discover the `ruff` version used to start the LSP in the first place. However, this would increase the scope of a minimal `uv format` command beyond "run a formatter", and raise larger questions about how uv should be used to coordinate toolchain discovery. I think those are good things to explore, but I'm hesitant to let them block a `uv format` implementation. Another downside of using uv to discover `ruff`, is that it needs to be implemented _outside_ the LSP; e.g., we'd need to change the instructions on how to run the LSP and implement it in each editor integration, like the VS Code plugin.

Does this mean something like `uv server` ?

Yeah, I think I'd be a bit hesitant to make the change as described here mainly because we only control the client side implementation for VS Code while for other editors we'd need to explicitly specify on how to start the server and what does it mean to start it from `uv` vs `ruff`.


---

_@zanieb reviewed on 2025-08-26 13:18_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:182 on 2025-08-26 13:18_

Yep, thanks!

---

_@zanieb reviewed on 2025-08-26 13:20_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:212 on 2025-08-26 13:20_

When using `Command::arg` the outer quotes (`"`) are implicit. Those outer quotes are being parsed by your shell into a single argument. I think providing them here would actually break things, as they'd be included in the value.

---

_@zanieb reviewed on 2025-08-26 13:21_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:170 on 2025-08-26 13:21_

Yes. It could be the wrong version.

I don't think I expect multiple uv versions to be available on people's machines — and when it happens it's usually an accident. We can provide a way to set the uv path in the future though. We'll want it for other things.

---

_@zanieb reviewed on 2025-08-26 13:21_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:171 on 2025-08-26 13:21_

What were you thinking for that case?

---

_@zanieb reviewed on 2025-08-26 13:22_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:139 on 2025-08-26 13:22_

That makes sense, thanks for clarifying!

---

_@zanieb reviewed on 2025-08-26 13:22_

---

_Review comment by @zanieb on `crates/ruff_server/src/session/options.rs`:291 on 2025-08-26 13:22_

I don't have strong feelings. (2) seems nice but is weird to transition off of?

---

_Comment by @zanieb on 2025-08-26 13:23_

> Does this mean something like uv server ?

Yeah, basically.

> ... while for other editors we'd need to explicitly specify on how to start the server and what does it mean to start it from uv vs ruff.

Yeah, this was the same concern Micha and I had.

---

_Comment by @zanieb on 2025-08-26 13:24_

The test failures on Windows are due to concurrent-installs being unsafe in uv. I'll fix that upstream.

---

_@dhruvmanila reviewed on 2025-08-26 14:59_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:212 on 2025-08-26 14:59_

Ah I see, thanks for clarifying

---

_@dhruvmanila reviewed on 2025-08-26 15:01_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:171 on 2025-08-26 15:01_

Like, what the error message would be?

---

_@dhruvmanila reviewed on 2025-08-26 15:11_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/format.rs`:170 on 2025-08-26 15:11_

Yeah, that makes sense

---

_@dhruvmanila reviewed on 2025-08-26 15:14_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/options.rs`:291 on 2025-08-26 15:14_

Thinking about this more, I think the current structure is fine (including it in the existing `format` namespace).

---

_@zanieb reviewed on 2025-08-26 16:23_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:171 on 2025-08-26 16:23_

Yeah, but I just made a small improvement here for now.

---

_Label `configuration` added by @dhruvmanila on 2025-09-08 05:16_

---

_Label `server` added by @dhruvmanila on 2025-09-08 05:16_

---

_@dhruvmanila approved on 2025-09-09 15:09_

---

_Merged by @dhruvmanila on 2025-09-09 15:09_

---

_Closed by @dhruvmanila on 2025-09-09 15:09_

---

_Branch deleted on 2025-09-09 15:09_

---
