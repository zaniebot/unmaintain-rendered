```yaml
number: 11603
title: "Require the command in `uvx <name>` to be available in the Python environment"
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: release/070
head: zb/tool-run-check
created_at: 2025-02-18T19:29:25Z
updated_at: 2025-04-18T21:57:34Z
url: https://github.com/astral-sh/uv/pull/11603
synced_at: 2026-01-12T16:09:54Z
```

# Require the command in `uvx <name>` to be available in the Python environment

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/7804

Includes a few small minor changes to the messaging, but the primary change is that in, e.g., `uvx foo`, if the `foo` package does not provide the `foo` executable we will no longer execute an arbitrary `foo` executable if present on the `PATH`. This prevents confusing and surprising behavior, such as the user reported where they did `uv tool install foobar` (which provides `foo`) then `uvx foo` (which does not provide `foo`) later falls back to the executable provided by `foobar` since it's on the `PATH`. We don't enforce this for `--from`, so things like `uvx --from foo bash -c "..."` are still totally valid. We also still allow `uvx foo` where the `foo` executable is provided by a _dependency_ of `foo` instead of `foo` itself.

Most of the diff here is consolidating the logic of the `hint_on_not_found` and `warn_executable_not_provided_by_package ` utilities.

---

_@zanieb reviewed on 2025-02-18 20:04_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:276 on 2025-02-18 20:04_

The user is already using `--from` here, I don't think we should hint it again.

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:298 on 2025-02-18 20:05_

We no longer attempt to execute this / succeed if it's on the `PATH`. Instead, we exit early.

---

_@zanieb reviewed on 2025-02-18 20:05_

---

_@zanieb reviewed on 2025-02-18 20:05_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:298 on 2025-02-18 20:05_

Arguably, this is breaking — but I think this is a bug.

---

_@zanieb reviewed on 2025-02-18 20:06_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:2206 on 2025-02-18 20:06_

I'm sort of tempted to just run the command when it matches the module-normalized package name :)

---

_@zanieb reviewed on 2025-02-18 20:07_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:342 on 2025-02-18 20:07_

I'm not super pleased with this. It'd be nice to just avoid this by construction.

---

_@zanieb reviewed on 2025-02-18 20:25_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:276 on 2025-02-18 20:25_

It's intended to be shown for like

```
❯ uvx fastapi-cli
Installed 21 packages in 46ms
The executable `fastapi-cli` was not found.
warning: An executable named `fastapi-cli` is not provided by package `fastapi-cli`.
The following executables are provided by `fastapi-cli`:
- fastapi
Consider using `uvx --from fastapi-cli <EXECUTABLE_NAME>` instead.
```

---

_@zanieb reviewed on 2025-02-18 21:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:342 on 2025-02-18 21:28_

Fixed in https://github.com/astral-sh/uv/pull/11603/commits/913dd90ad41683532f8c961b14864055483a04fe

---

_Renamed from "Improve executable checks in `uvx`" to "Require the command in `uvx <name>` to be available in the Python environment" by @zanieb on 2025-02-18 21:29_

---

_Marked ready for review by @zanieb on 2025-02-18 21:30_

---

_@zanieb reviewed on 2025-02-18 21:32_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:2203 on 2025-02-18 21:32_

Agonizing about whether or not there should be a blank newline here.

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:324 on 2025-04-18 15:14_

`Python environment`

`run in`?

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:522 on 2025-04-18 15:18_

It seems surprising that `Display` doesn't display the representation of the type but warning and error information. Should this be a separate dedicated function or method?

---

_@jtfmumm reviewed on 2025-04-18 15:19_

---

_@jtfmumm reviewed on 2025-04-18 15:21_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/tool_run.rs`:298 on 2025-04-18 15:21_

Yeah, I think as a user I wouldn't expect `uvx` to execute something just because it's on the `PATH`

---

_@jtfmumm reviewed on 2025-04-18 15:22_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/tool_run.rs`:2203 on 2025-04-18 15:22_

Maybe I tend toward no newline since it feels connected to the line above, but no strong feeling

---

_@jtfmumm reviewed on 2025-04-18 15:24_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/tool_run.rs`:2206 on 2025-04-18 15:24_

It does seem designed to confound the user. But are there cases where this could be a coincidence?

---

_Comment by @jtfmumm on 2025-04-18 15:25_

Left a few comments but generally looks good.

---

_@jtfmumm approved on 2025-04-18 15:25_

---

_@zanieb reviewed on 2025-04-18 16:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:522 on 2025-04-18 16:06_

I'm not sure, the type only exists for the purpose of rendering these messages. Previously, it was a function that called `warn_user` and `println` directly but here I want the caller to be able to make that decision and I think `Display` is the easiest way to do that. I think I'd just end up needing into introduce a separate type, like `ExecutableProviderPackagesDisplay` that implements `Display`? Or I'd need to implement like `fn message(&self)-> String` and construct a string instead of using `Formatter`?

---

_@jtfmumm reviewed on 2025-04-18 17:31_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:522 on 2025-04-18 17:31_

I was confused when I encountered `writeln!(printer.stderr(), "{providers}")?;` above (before seeing this code). I didn't understand why you would just log the providers without context. It seems pretty non-standard to have `Display` perform conditional logic and log warnings. 

I'd recommend something like `providers.log_warnings()` (or something more descriptive) to accomplish what you're doing here. You could pass in `printer`.
 

---

_@zanieb reviewed on 2025-04-18 20:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:522 on 2025-04-18 20:46_

> perform conditional logic and log warnings.

To be clear, it only logs one warning (which we could omit, but I don't think it really hurts — it's a case that should never happen). The rest of it is just writing to the formatter.

I can't use a function that takes a `printer` because I want to be able to use it in `warn_user_once`.

I see this as the same thing as implementing `Display` on an error type, which we frequently include more context on. Would it be clearer if it was named like `ExecutableProviderHints` instead?

---

_Review comment by @jtfmumm on `crates/uv/src/commands/tool/run.rs`:522 on 2025-04-18 20:48_

Yeah that would be clearer 

---

_@jtfmumm reviewed on 2025-04-18 20:48_

---

_@zanieb reviewed on 2025-04-18 21:34_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:522 on 2025-04-18 21:34_

That makes sense, thanks!

---

_Label `breaking` added by @zanieb on 2025-04-18 21:38_

---

_@zanieb reviewed on 2025-04-18 21:38_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:2206 on 2025-04-18 21:38_

I can't really think of one, no.

---

_@zanieb reviewed on 2025-04-18 21:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_run.rs`:2206 on 2025-04-18 21:50_

Tracking in #12976

---

_Merged by @zanieb on 2025-04-18 21:57_

---

_Closed by @zanieb on 2025-04-18 21:57_

---

_Branch deleted on 2025-04-18 21:57_

---
