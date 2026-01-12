```yaml
number: 12523
title: Provide log messages for the native server
type: issue
state: closed
author: dhruvmanila
labels:
  - server
assignees: []
created_at: 2024-07-26T06:57:54Z
updated_at: 2025-01-15T07:49:18Z
url: https://github.com/astral-sh/ruff/issues/12523
synced_at: 2026-01-12T15:54:52Z
```

# Provide log messages for the native server

---

_@dhruvmanila_

There aren't many log messages emitted by the server which can help in debugging. It would be quite useful to provide some more log messages at info / debug / trace levels.

Note that these log messages shouldn't include the request response messages because that's taken care by the editor. It should be specific to the server logic.

---

_Label `server` added by @dhruvmanila on 2024-07-26 06:57_

---

_Comment by @dhruvmanila on 2024-07-29 05:39_

In addition to the log messages, it's also important for us to determine how to send these log messages to the client. Currently, all tracing logs are sent to stderr unless diverted by using the `ruff.logFile` server setting. But, as per the spec, the tracing logs are usually sent via the `$/logTrace` request. We should also consider using the `window/logMessage` request for user-facing messages.

---

_Comment by @MichaReiser on 2024-07-29 05:51_

> But, as per the spec, the tracing logs are usually sent via the $/logTrace

There was some [internal discussion around using `$logTrace`](https://discord.com/channels/1039017663004942429/1248532294084460588/1248658835015733268). I don't remember the reasoning to not use it. 

---

_Comment by @dhruvmanila on 2024-09-06 05:02_

I think this is becoming more relevant because currently the user experience around viewing log messages isn't automatic.

Currently, the default value of the `ruff.trace.server` (in VS Code) or `RUFF_TRACE` environment variable (for other editors) is `off` and the default value of `ruff.logLevel` is `info`. This means that any error / warning messages that the server sends to the client are not going to be logged unless the user explicitly turns on server log messages by updating the trace value. This doesn't seem like a good user experience. For example,

https://github.com/astral-sh/ruff/blob/a4ebe7d34407ea353762ebde1fef4a718edddb55/crates/ruff_server/src/server/api.rs#L61-L64

Here, the client will display the notification with the message as in `show_err_msg` macro but it won't log the error via the `tracing::error` call unless the trace value is `messages` or `verbose`. This means that the user will see the notification and when they open the logs they won't see anything.

The differentiating factor is that which messages should always be logged (`window/logMessage`) and which are optional (`$/logTrace`).

---

_Comment by @MichaReiser on 2024-09-06 06:25_

> This means that any error / warning messages that the server sends to the client are not going to be logged unless the user explicitly turns on server log messages by updating the trace value. This doesn't seem like a good user experience. For example,

Yeah. This doesn't seem like a good default. I think the default should be `info` similar to red knot. 

---

_Comment by @dhruvmanila on 2024-09-06 06:39_

> Yeah. This doesn't seem like a good default. I think the default should be `info` similar to red knot.

Do you mean the default trace value should be "messages"? The default value for the log level is `info` but the logs themselves are `off` by way of having the default trace value as `off`.

Regardless, if we update the default trace value to be "messages", the output channel in the VS Code will be filled with the request - response cycle messages like:
```
2024-09-06 12:08:38.040 [info] [Trace - 12:08:38 PM] Sending request 'textDocument/codeAction - (28)'.
2024-09-06 12:08:38.041 [info] [Trace - 12:08:38 PM] Received response 'textDocument/codeAction - (28)' in 1ms.
2024-09-06 12:08:38.290 [info] [Trace - 12:08:38 PM] Sending request 'textDocument/codeAction - (29)'.
2024-09-06 12:08:38.291 [info] [Trace - 12:08:38 PM] Received response 'textDocument/codeAction - (29)' in 1ms.
```
We could have a separate output channel for the trace messages (similar to rust-analyzer) but that would also include the trace messages from the Ruff server. This is why we should also consider using `window/logMessage` in addition to `$/logTrace`.

---

_Comment by @MichaReiser on 2024-09-06 08:05_

Hmm. I would need to take a closer look at how our tracing setup works. Ideally, we would show info, warning, and error messages (`tracing::info!`) by default. We could then use those levels to e.g. log what configurations we discovered. 

---

_Comment by @mpsijm on 2025-01-04 20:01_

Hi there, newcomer to NeoVim here. After installing https://github.com/ray-x/navigator.lua, I started getting the generic error message quoted above: `"Ruff failed to handle a request from the editor. Check the logs for more details."`

Nothing appears to be broken in functionality, it only `vim.notify`s the error message. I have no clue whether this is caused by the way this new plugin interacts with Ruff's language server, or something else.

Is there any way I can dig deeper into where this message is coming from? I already enabled the debug logging options as per https://github.com/astral-sh/ruff/issues/14959#issuecomment-2541783682, even with a custom `logFile = '...'` option, but I don't see the `tracing::error!("Encountered error [...]")` message appear anywhere.

---

_Comment by @dhruvmanila on 2025-01-06 10:27_

> Is there any way I can dig deeper into where this message is coming from? I already enabled the debug logging options as per [#14959 (comment)](https://github.com/astral-sh/ruff/issues/14959#issuecomment-2541783682), even with a custom `logFile = '...'` option, but I don't see the `tracing::error!("Encountered error [...]")` message appear anywhere.

I think what I've suggested _should_ provide some insights into why this is failing. Can you provide the initialization options that you're passing to the Ruff language server in Neovim?

---

_Comment by @mpsijm on 2025-01-06 10:51_

@dhruvmanila I'm currently passing the following options:
```lua
require('lspconfig').ruff.setup {
    on_attach = on_attach,
    trace = 'verbose',
    init_options = {
        settings = {
            args = {},
            logLevel = 'debug',
            logFile = '/home/mpsijm/ruff.log',
        }
    }
}
```

<details>
<summary>Collapsing the `on_attach` function because I don't think it's relevant (but sharing it anyway just in case):</summary>

```lua
local on_attach = function(client, _)
    vim.keymap.set('n', '<leader>r', vim.lsp.buf.rename, {})
    vim.keymap.set('n', '<leader>ca', vim.lsp.buf.code_action, {})
    vim.keymap.set('n', ']g', vim.diagnostic.goto_next)
    vim.keymap.set('n', '[g', vim.diagnostic.goto_prev)
    vim.keymap.set('n', 'gd', vim.lsp.buf.definition, {})
    vim.keymap.set('n', 'gi', vim.lsp.buf.implementation, {})
    vim.keymap.set('n', 'gr', require('telescope.builtin').lsp_references, {})
    vim.keymap.set('n', 'K', vim.lsp.buf.hover, {})
    vim.keymap.set('n', '<C-Q>', vim.lsp.buf.hover, {})

    --  Source: https://www.reddit.com/r/neovim/comments/10a31vo/how_does_vimlspbufformat_deal_with_multiple/
    vim.keymap.set('n', '<leader>l', function()
        vim.lsp.buf.format {
            async = true,
            filter = function()
                return client.name ~= 'ruff_lsp'
            end
        }
    end, {})

    if client.name == 'ruff_lsp' then
        -- Disable hover in favor of Pyright
        client.server_capabilities.hoverProvider = false
    end
end
```

</details>

---

_Comment by @dhruvmanila on 2025-01-06 10:58_

That looks like it should work, it does for me:
```
   0.000014375s  INFO main ruff_server::server: No workspace settings found for file:///Users/dhruv/playground/ruff, using default settings
   0.002282000s DEBUG ThreadId(04) ruff_server::session::index::ruff_settings: Ignored path via `exclude`: /Users/dhruv/playground/ruff/.vscode
   0.005479291s  INFO main ruff_server::session::index: Registering workspace: /Users/dhruv/playground/ruff
   0.007846083s  INFO ruff:main ruff_server::server: Configuration file watcher successfully registered
```

(You don't need to pass the `args` settings as that's only relevant for `ruff_lsp`.)

Are you getting nothing in `/home/mpsijm/ruff.log`? Does that file get created and is empty? 

You can also try removing the `logFile` setting which then should log the messages to the file as given by `vim.lsp.get_log_path()`.

---

_Comment by @mpsijm on 2025-01-06 14:38_

Thanks, I removed `args` 🙂 

The file `/home/mpsijm/ruff.log` did not even get created.

However, I've now found that I also had to call `vim.lsp.set_log_level('trace')` before NeoVim creates the log file, so now I do get logs (also in the default log file when I remove the `logFile` setting) 🥳 

The messages I'm getting are:
```
[DEBUG][2025-01-06 15:27:37] .../vim/lsp/rpc.lua:408    "rpc.receive"   {  jsonrpc = "2.0",  method = "window/showMessage",  params = {    message = "Ruff failed to handle a request from the editor. Check the logs for more details.",    type = 1  }}
[DEBUG][2025-01-06 15:27:37] .../vim/lsp/rpc.lua:408    "rpc.receive"   {  error = {    code = -32603,    message = "JSON parsing failure:\nInvalid request\nMethod: textDocument/codeAction\n error: missing field `range`"  },  id = 5,  jsonrpc = "2.0"}
[TRACE][2025-01-06 15:27:37] ...m/lsp/client.lua:1003   "notification"  "window/showMessage"    {  message = "Ruff failed to handle a request from the editor. Check the logs for more details.",  type = 1}
[TRACE][2025-01-06 15:27:37] ...lsp/handlers.lua:711    "default_handler"       "window/showMessage"    {  ctx = '{\n  client_id = 4,\n  method = "window/showMessage"\n}',  result = {    message = "Ruff failed to handle a request from the editor. Check the logs for more details.",    type = 1  }}
```
Where I think the most important bit is in the second line:
```
JSON parsing failure:
Invalid request
Method: textDocument/codeAction
 error: missing field `range`
```

I found https://www.reddit.com/r/neovim/comments/1g6cs0s/comment/lsmc40n/ who gets the same error, also caused by the Navigator plugin. Sounds like Navigator is trying to call the Ruff language server with incomplete arguments, which I'll pick up in their GitHub repo. If you happen to know from the top of your head where they could look to fix this, please share 🙂 Thanks for your help! ❤ 

---

_Comment by @dhruvmanila on 2025-01-08 05:12_

> I found [reddit.com/r/neovim/comments/1g6cs0s/comment/lsmc40n](https://www.reddit.com/r/neovim/comments/1g6cs0s/comment/lsmc40n/) who gets the same error, also caused by the Navigator plugin. Sounds like Navigator is trying to call the Ruff language server with incomplete arguments, which I'll pick up in their GitHub repo. If you happen to know from the top of your head where they could look to fix this, please share 🙂 Thanks for your help! ❤

Thanks, it seems that the plugin isn't passing in the `range` field which is required for the code action request (https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#codeActionParams) which is weird because it utilizes the standard library function to construct the parameters (https://github.com/ray-x/navigator.lua/blob/b1d0da438b2e0fa32e2d52a4a047b4900bb0e8ca/lua/navigator/codeAction.lua#L150-L156) so not sure why that is happening.

---

_Comment by @MichaReiser on 2025-01-15 07:40_

Should we close this and add more logs as we make changes to the LSP? 

---

_Closed by @dhruvmanila on 2025-01-15 07:49_

---
