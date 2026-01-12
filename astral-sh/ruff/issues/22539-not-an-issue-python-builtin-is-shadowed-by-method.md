```yaml
number: 22539
title: "\"Not an issue\": Python builtin is shadowed by method `str` A003"
type: issue
state: open
author: hunterhogan
labels:
  - question
assignees: []
created_at: 2026-01-12T22:13:29Z
updated_at: 2026-01-12T22:24:52Z
url: https://github.com/astral-sh/ruff/issues/22539
synced_at: 2026-01-12T23:24:03Z
```

# "Not an issue": Python builtin is shadowed by method `str` A003

---

_@hunterhogan_

### Summary

https://github.com/python/cpython/issues/143661#issuecomment-3732892288

> Sorry but it's not an issue. There is no shadowing involved for the end-user, only for the AST class itself but that's also not an issue. ast.Name also has an id field. Unless you write str = interp.str, in which case it's rather the user's fault, there won't be an issue. I'm closing this as not planned.

[Source](https://github.com/hunterhogan/astToolkit/blob/05c52ce9f0e5e27dc5669b90c646044552ae8ad9/astToolkit/_toolDOT.py)

`rest` and the 14 other methods above `rest` do not induce not-an-issue diagnostic messages.



```python
import ast
import builtins
import sys

type hasDOTrest = ast.MatchMapping
type hasDOTtag = ast.TypeIgnore
type hasDOTtype_comment = ast.arg | ast.Assign | ast.AsyncFor | ast.AsyncFunctionDef | ast.AsyncWith | ast.For | ast.FunctionDef | ast.With

if sys.version_info >= (3, 14):
    type hasDOTstr = ast.Interpolation

class DOT:
    """Access attributes and sub-nodes of AST elements via consistent accessor methods.

    The DOT class provides static methods to access specific attributes of different types of AST nodes in a consistent
    way. This simplifies attribute access across various node types and improves code readability by abstracting the
    underlying AST structure details.

    DOT is designed for safe, read-only access to node properties, unlike the grab class which is designed for modifying
    node attributes.

    """

    @staticmethod
    def rest(node: hasDOTrest) -> None | str:
        return node.rest

    if sys.version_info >= (3, 14):

        @staticmethod
        def str(node: hasDOTstr) -> builtins.str:
            return node.str

    @staticmethod
    def tag(node: hasDOTtag) -> str:
        return node.tag

    @staticmethod
    def type_comment(node: hasDOTtype_comment) -> None | str:
        return node.type_comment
```

## Diagnostic messages about something that is not an issue.

```json
[{
	"resource": "/C:/apps/astToolkit/astToolkit/_Z0Z_.py",
	"owner": "Ruff",
	"code": {
		"value": "A003",
		"target": {
			"$mid": 1,
			"path": "/ruff/rules/builtin-attribute-shadowing",
			"scheme": "https",
			"authority": "docs.astral.sh"
		}
	},
	"severity": 4,
	"message": "Python builtin is shadowed by method `str` from line 31",
	"source": "Ruff",
	"startLineNumber": 39,
	"startColumn": 58,
	"endLineNumber": 39,
	"endColumn": 61,
	"modelVersionId": 59,
	"origin": "extHost1"
}]

[{
	"resource": "/C:/apps/astToolkit/astToolkit/_Z0Z_.py",
	"owner": "Ruff",
	"code": {
		"value": "A003",
		"target": {
			"$mid": 1,
			"path": "/ruff/rules/builtin-attribute-shadowing",
			"scheme": "https",
			"authority": "docs.astral.sh"
		}
	},
	"severity": 4,
	"message": "Python builtin is shadowed by method `str` from line 31",
	"source": "Ruff",
	"startLineNumber": 39,
	"startColumn": 58,
	"endLineNumber": 39,
	"endColumn": 61,
	"modelVersionId": 59,
	"origin": "extHost1"
}]
```

### Version

2026.34.0

---

_Comment by @MichaReiser on 2026-01-12 22:23_

The shadowing that's happening here is that the return type annotation `str` doesn't resolve to the builtin type `str` but to `DOT.str`. As you can see e.g. by clicking on `str` in this [playground](https://play.ty.dev/87fe4a96-f251-4588-95ee-cb996f4dbfdb) or by running the following snippet in Python 3.14

```pycon
>>>
... import builtins
... import sys
...
... type hasDOTrest = ast.MatchMapping
... type hasDOTtag = ast.TypeIgnore
... type hasDOTtype_comment = ast.arg | ast.Assign | ast.AsyncFor | ast.AsyncFunctionDef | ast.AsyncWith | ast.For | ast.FunctionDef | ast.With
...
... if sys.version_info >= (3, 14):
...     type hasDOTstr = ast.Interpolation
...
... class DOT:
...     """Access attributes and sub-nodes of AST elements via consistent accessor methods.
...
...     The DOT class provides static methods to access specific attributes of different types of AST nodes in a consistent
...     way. This simplifies attribute access across various node types and improves code readability by abstracting the
...     underlying AST structure details.
...
...     DOT is designed for safe, read-only access to node properties, unlike the grab class which is designed for modifying
...     node attributes.
...
...     """
...
...     @staticmethod
...     def rest(node: hasDOTrest) -> None | str:
...         return node.rest
...
...     if sys.version_info >= (3, 14):
...
...         @staticmethod
...         def str(node: hasDOTstr) -> builtins.str:
...             return node.str
...
...     @staticmethod
...     def tag(node: hasDOTtag) -> str:
...         return node.tag
...
...     @staticmethod
...     def type_comment(node: hasDOTtype_comment) -> None | str:
...         return node.type_comment
...     print(str)
...
<staticmethod(<function DOT.str at 0x105c07270>)>
```


The shadowning warning goes away if you change all `str` return types to `builtins.str`.

---

_Label `question` added by @MichaReiser on 2026-01-12 22:24_

---
