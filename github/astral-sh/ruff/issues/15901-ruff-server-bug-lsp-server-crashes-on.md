---
number: 15901
title: "[ruff server] bug: LSP server crashes on initialization message"
type: issue
state: closed
author: peske
labels:
  - question
  - server
assignees: []
created_at: 2025-02-03T01:53:07Z
updated_at: 2025-02-07T10:13:02Z
url: https://github.com/astral-sh/ruff/issues/15901
synced_at: 2026-01-07T13:12:16-06:00
---

# [ruff server] bug: LSP server crashes on initialization message

---

_Issue opened by @peske on 2025-02-03 01:53_

### Description

I'm working on a LSP client app. When I send initialization message that contains (among other things) the following **client capabilities**:

```json
"capabilities": {
    "textDocument": {
        "documentSymbol": {
            "tagSupport": {}
        }
    }
}
```

The server crashes with the following `stderr` output:

```
ruff failed

Cause: invalid type: null, expected a sequence
```

It turned out that it requires `valueSet` property to be set, like:

```json
"capabilities": {
    "textDocument": {
        "documentSymbol": {
            "tagSupport": {
                "valueSet": []
            }
        }
    }
}
```

This is bad for two reasons:

- It's **not compliant** to LSP standard, which allows `valueSet` to be `null`, as you can see at https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#documentSymbolClientCapabilities
- Even if the initialization message is invalid, it **should not** crash the server - instead it should properly respond with [`ResponseError`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#responseError).

```console
$ ruff --version
ruff 0.9.3
```

---

_Comment by @dhruvmanila on 2025-02-03 04:08_

Hey, thanks for the report and all the details.

> * It's **not compliant** to LSP standard, which allows `valueSet` to be `null`, as you can see at https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#documentSymbolClientCapabilities

I think it is compliant with the protocol as for the `tagSupport` object, the `valueSet` cannot be `nil`, it's the `tagSupport` itself that can be `nil`. So, the following should work:

```json
"capabilities": {
    "textDocument": {
        "documentSymbol": {}
    }
}
```

Or, the one that you've provided where `valueSet` is added with an empty array.

> * Even if the initialization message is invalid, it **should not** crash the server - instead it should properly respond with [`ResponseError`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#responseError).

Can you say why this is the case? Can you provide a reference to it if it's specified in the protocol? I can't seem to find anything related to error handling in the initialization request. 

One way to handle this would be to fallback to using the default client capabilities but that might be confusing to the users who might've provided server settings and will note that the behavior is different. Although that could be mitigated by providing a warning notification to the user that the client capabilities were incorrect and the server would use the default capabilities, thus the behavior of the server might not be as expected.

Happy to hear your thoughts on this. I can look into what other servers does as well but I know for sure that `rust-analyzer` will also crash as it uses the same crate as us.

---

_Label `question` added by @dhruvmanila on 2025-02-03 04:08_

---

_Label `server` added by @dhruvmanila on 2025-02-03 04:08_

---

_Comment by @peske on 2025-02-03 13:23_

About the first part - `tagSupport.valueSet` cannot be `null` - you're absolutely right! My mistake.

About the second part - handling error during initialization - I don't know if `initialization` request is specifically mentioned for error handling. However, it is a general rule for `RequestMessage` -> `ResponseMessage` flow. For this reason `ResponseMessage` has `error` property of type `ResponseError`. Isn't kinda obvious? The property probably wouldn't be there if the server should crash on every _bad request_.

---

_Comment by @MichaReiser on 2025-02-03 14:13_

I think terminating the server for a malformed initialized message seems reasonable. A more graceful way than panicking would be nice but I don't consider it too much of a priority because clients tend to behave well. 

---

_Closed by @MichaReiser on 2025-02-07 10:13_

---
