```yaml
number: 2561
title: "support a \"reveal type\" lsp method for tool integration"
type: issue
state: open
author: KotlinIsland
labels:
  - server
  - editor
assignees: []
created_at: 2026-01-19T11:09:46Z
updated_at: 2026-01-20T05:30:17Z
url: https://github.com/astral-sh/ty/issues/2561
synced_at: 2026-01-20T05:34:07Z
```

# support a "reveal type" lsp method for tool integration

---

_@KotlinIsland_

### Summary

in PyCharm, it would be very useful to be able to ask ty for the type of an expression. retrieving this type as a string with fully qualified names is enough

we have a working prototype of this, would you be interested in incorporating this into ty?

### Version

_No response_

---

_Renamed from "support a "reveal type" lsp method" to "support a "reveal type" lsp method for tool integration" by @KotlinIsland on 2026-01-19 11:09_

---

_Label `server` added by @AlexWaygood on 2026-01-19 11:11_

---

_Comment by @MichaReiser on 2026-01-19 11:22_

I'd be open to that as it doesn't expose much internals. Can you say a little more about what Pycharm uses it for (You can also include that information in the PR or the custom LSP method's documentation). Having good documentation of the intended use case will help us understand the API stability that PyCharm expects upstream.

I'd be inclined to make the result type an object with only a full qualified name string for now. It will make it easier to expose more information later. 

Are there other LSP methods that you expect are needed for Pycharm?

---

_Label `editor` added by @dhruvmanila on 2026-01-20 05:30_

---
