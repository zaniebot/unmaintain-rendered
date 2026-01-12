```yaml
number: 10951
title: "`ruff server`: Important errors are now shown as popups"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/show-message
created_at: 2024-04-15T10:10:02Z
updated_at: 2024-04-16T21:39:29Z
url: https://github.com/astral-sh/ruff/pull/10951
synced_at: 2026-01-12T15:55:33Z
```

# `ruff server`: Important errors are now shown as popups

---

_@snowsignal_

## Summary

Fixes #10866.

Introduces the `show_err_msg!` macro which will send a message to be shown as a popup to the client via the `window/showMessage` LSP method.

## Test Plan

Insert various `show_err_msg!` calls in common code paths (for example, at the beginning of `event_loop`) and confirm that these messages appear in your editor.

To test that panicking works correctly, add this to the top of the `fn run` definition in `crates/ruff_server/src/server/api/requests/execute_command.rs`:

```rust
panic!("This should appear");
```

Then, try running a command like `Ruff: Format document` from the command palette (`Ctrl/Cmd+Shift+P`). You should see the following messages appear:


![Screenshot 2024-04-16 at 11 20 57 AM](https://github.com/astral-sh/ruff/assets/19577865/ae430da6-82c3-4841-a419-664ff34034e8)

---

_Label `server` added by @snowsignal on 2024-04-15 10:10_

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-15 10:11_

---

_@MichaReiser reviewed on 2024-04-15 10:25_

Having to call both `tracing::error` and `show_err_msg!` is a bit verbose but I could see why it is needed. I think my main challenge as a developer would be is to know where I need to use what (not all tracing sides have a `show_err_msg` call but all `show_err_msg` calls have a `tracing::error` call). 


---

_Comment by @github-actions[bot] on 2024-04-15 10:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-04-15 10:26_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/message.rs`:62 on 2024-04-15 10:26_

I think it would be valuable to document the macro with when and how to use it.

---

_@MichaReiser reviewed on 2024-04-15 10:27_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/message.rs`:7 on 2024-04-15 10:27_

The once lock is clever. The only downside is that once locks can be tricky when writing tests. Is it difficult to get access to the `ClientSender`? If not, what do you think of mirroring the `write!` macro where the first argument is the output stream:

```rust
show_message_err!(sender, "My message {var}")
```

---

_Review comment by @MichaReiser on `crates/ruff_server/src/message.rs`:32 on 2024-04-15 10:28_

Could you extend your test plan to cover the panic hook? Just to make sure that it works. 

---

_@MichaReiser reviewed on 2024-04-15 10:28_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/server/api.rs`:58 on 2024-04-15 17:03_

nit: I'd remove the "error" part

```suggestion
        show_err_msg!("Ruff failed to handle a request from the editor. Check the logs for more details.");
```

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/message.rs`:58 on 2024-04-15 17:07_

nit: I'd probably just use a function instead, I find macros a bit confusing 😅 

```rs
#[inline]
pub(crate) fn show_error_message(message: String) {
	show_message(message, lsp_types::MessageType::ERROR);
}
```

Or, does the macro do more than what I've coded above?

---

_@dhruvmanila reviewed on 2024-04-15 17:08_

---

_@MichaReiser reviewed on 2024-04-15 17:28_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/message.rs`:58 on 2024-04-15 17:28_

I think the reason for a macro here is because it supports `format_args` style formatting. The alternative is to use `show_error_mesage(format_args!("my error {error}"))` but that's a bit verbose. 

---

_@snowsignal reviewed on 2024-04-16 02:10_

---

_Review comment by @snowsignal on `crates/ruff_server/src/message.rs`:7 on 2024-04-16 02:10_

I don't think it's worthwhile to make the API more involved just for the purposes of testing. If we do add tests in the future, we can rework `show_message` to take a reference to a client sender and then test `show_message` directly with a mock sender.

---

_@snowsignal reviewed on 2024-04-16 02:12_

---

_Review comment by @snowsignal on `crates/ruff_server/src/message.rs`:58 on 2024-04-16 02:12_

Right, the goal of the macro was to support formatting like `tracing::error!` does.

---

_@snowsignal reviewed on 2024-04-16 02:29_

---

_Review comment by @snowsignal on `crates/ruff_server/src/message.rs`:32 on 2024-04-16 02:29_

Done!

---

_Review requested from @MichaReiser by @snowsignal on 2024-04-16 02:29_

---

_Review requested from @dhruvmanila by @snowsignal on 2024-04-16 02:29_

---

_@dhruvmanila approved on 2024-04-16 05:00_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/message.rs`:26 on 2024-04-16 07:25_

Nit: I think I would write the panic info to the tracing output from where a user can easily copy the output (and it doesn't get truncated). The message here can direct the user to the log and ask them to open an issue on Ruff's repository

---

_Review comment by @MichaReiser on `crates/ruff_server/src/message.rs`:37 on 2024-04-16 07:25_

Should this use `tracing`?

---

_@MichaReiser approved on 2024-04-16 07:27_

The code looks good. 

To me it's still unclear when to use `show_err_msg` and tracing:error and why/when I need to use both 

> Having to call both tracing::error and show_err_msg! is a bit verbose but I could see why it is needed. I think my main challenge as a developer would be is to know where I need to use what (not all tracing sides have a show_err_msg call but all show_err_msg calls have a tracing::error call).

---

_Comment by @snowsignal on 2024-04-16 18:24_

> To me it's still unclear when to use show_err_msg and tracing:error and why/when I need to use both

`show_err_msg` shows a visible popup in the editor, with a user-friendly message. `tracing::error` is for logging errors in the `Output` window, usually in a more verbose and technical message.

Not all errors need to be surfaced to the user as visibly as a popup, which is why only some of the error logs also show a message to the user.

---

_Merged by @snowsignal on 2024-04-16 18:32_

---

_Closed by @snowsignal on 2024-04-16 18:32_

---

_Branch deleted on 2024-04-16 18:32_

---

_Added to milestone `Ruff Server: Beta` by @snowsignal on 2024-04-16 21:39_

---
