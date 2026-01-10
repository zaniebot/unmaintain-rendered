```yaml
number: 1781
title: "Server: renaming a property getter should also cause the setter and deleter to be renamed (if they're present)"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-05T18:00:28Z
updated_at: 2025-12-22T12:53:49Z
url: https://github.com/astral-sh/ty/issues/1781
synced_at: 2026-01-10T01:56:41Z
```

# Server: renaming a property getter should also cause the setter and deleter to be renamed (if they're present)

---

_Issue opened by @AlexWaygood on 2025-12-05 18:00_

### Summary

For this class:

```py
class Foo:
    @property
    def bar(self) -> str:
        return "baz"
    
    @bar.setter
    def bar(self, value: str) -> None:
        pass

    @bar.deleter
    def bar(self) -> None:
        pass
```

If you have Pylance disabled in VSCode and the ty-vscode extension installed, right-clicking on the first `bar` function and renaming it to `spam` will apply this diff to your code:

```diff
  class Foo:
      @property
-     def bar(self) -> str:
+     def spam(self) -> str:
          return "baz"
    
-     @bar.setter
+     @spam.setter
      def bar(self, value: str) -> None:
          pass

-     @bar.deleter
+     @spam.deleter
      def bar(self) -> None:
          pass
```

But if you enable Pylance and do the rename, then this edit is applied instead from the same operation:

```diff
  class Foo:
      @property
-     def bar(self) -> str:
+     def spam(self) -> str:
          return "baz"
    
-     @bar.setter
-     def bar(self, value: str) -> None:
+     @spam.setter
+     def spam(self, value: str) -> None:
          pass

-     @bar.deleter
-     def bar(self) -> None:
+     @spam.deleter
+     def spam(self) -> None:
          pass
```

The Pylance behaviour is how I would want a property rename to work; the ty behaviour should match that.

Failing tests for this were added in https://github.com/astral-sh/ruff/pull/21810; a fix for this issue should aim to resolve the TODOs added in that PR.

### Version

_No response_

---

_Label `server` added by @AlexWaygood on 2025-12-05 18:00_

---

_Label `bug` added by @AlexWaygood on 2025-12-05 18:02_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-05 18:03_

---

_Renamed from "LSP: renaming a property getter should also cause the setter and deleter to be renamed (if they're present)" to "Server: renaming a property getter should also cause the setter and deleter to be renamed (if they're present)" by @AlexWaygood on 2025-12-22 12:53_

---
