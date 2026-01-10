```yaml
number: 1295
title: Go-to-definition not working within classes
type: issue
state: closed
author: mjscriba
labels: []
assignees: []
created_at: 2025-10-01T14:56:49Z
updated_at: 2025-10-01T15:05:03Z
url: https://github.com/astral-sh/ty/issues/1295
synced_at: 2026-01-10T02:06:25Z
```

# Go-to-definition not working within classes

---

_Issue opened by @mjscriba on 2025-10-01 14:56_

### Summary

Version: ty 0.0.1-alpha.21
IDE: Zed

I can't for the life of me get 'go to definition' to work *within* classes. It works to external classes, and for functions, just not for `self` referenced classes. 

Here's a minimal reproduction case:
```py
class Test:
    def __init__():
        pass

    def first_method(self):
        pass

    def second_method(self):
        self.first_method()
```

Ctrl+click on `self.first_method()` in Zed (latest version) does nothing. The logs show this:

```
// Send:
{"jsonrpc":"2.0","id":943,"method":"textDocument/definition","params":{"textDocument":{"uri":"file:///class_test.py"},"position":{"line":7,"character":14}}}

// Receive:
{"jsonrpc":"2.0","id":943,"result":[{"originSelectionRange":{"end":{"character":20,"line":7},"start":{"character":8,"line":7}},"targetRange":{"end":{"character":12,"line":8},"start":{"character":4,"line":7}},"targetSelectionRange":{"end":{"character":20,"line":7},"start":{"character":8,"line":7}},"targetUri":"file:///class_test.py"}]}

// Send:
{"jsonrpc":"2.0","id":944,"method":"textDocument/definition","params":{"textDocument":{"uri":"file:///class_test.py"},"position":{"line":11,"character":24}}}

// Receive:
{"jsonrpc":"2.0","id":944,"result":null}

// Send:
{"jsonrpc":"2.0","id":945,"method":"textDocument/documentHighlight","params":{"textDocument":{"uri":"file:///class_test.py"},"position":{"line":12,"character":0}}}

// Receive:
{"jsonrpc":"2.0","id":945,"result":null}

// Send:
{"jsonrpc":"2.0","id":946,"method":"textDocument/documentHighlight","params":{"textDocument":{"uri":"file:///class_test.py"},"position":{"line":12,"character":0}}}

// Receive:
{"jsonrpc":"2.0","id":946,"result":null}
```

### Version

ty 0.0.1-alpha.21

---

_Comment by @AlexWaygood on 2025-10-01 15:05_

Thanks for the report!

This is a duplicate of #159. You can see #1283, and other similar issues in our tracker, for an explanation for why

---

_Closed by @AlexWaygood on 2025-10-01 15:05_

---
