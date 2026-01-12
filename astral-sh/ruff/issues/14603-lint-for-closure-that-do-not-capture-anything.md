```yaml
number: 14603
title: Lint for closure that do not capture anything ? 
type: issue
state: open
author: Carreau
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-11-26T09:20:50Z
updated_at: 2024-11-27T16:46:48Z
url: https://github.com/astral-sh/ruff/issues/14603
synced_at: 2026-01-12T15:54:54Z
```

# Lint for closure that do not capture anything ? 

---

_@Carreau_

Hi,

Thanks for ruff – sorry if I missed it in ruff docs and already existing opened and closed issues; 
but is there any hints for function closure that do not capture any variables ? 

I've came across a few project recently where the variable the function was closed over was removed; and the closure could have been moved outside of it's defining function as a util function; and was wondering if this is something ruff could detect. 

Thanks. 

---

_Label `rule` added by @MichaReiser on 2024-11-26 09:23_

---

_Comment by @MichaReiser on 2024-11-26 09:25_

Hi @Carreau 

This is something that Ruff could detect well in most cases. My concern with such a rule is that there are other reasons for nested functions. For example, nesting a function might be a deliberate choice to reduce the function's visibility (make it inaccessible from outside). I haven't written enough Python code myself to judge how common this is in Python but it's a very common pattern in JavaScript. 



---

_Comment by @Carreau on 2024-11-26 09:47_

Thanks for the quick answer; 

I do understand and I was hopping there could be a `# noqa: number` in these case; it's also – sometime – I think a matter of taste.

IMHO; reduce visibility can be achieve by making the functions private (start name with underscore; ; don't list is on `__all__`, static method on class); which is always better for unit-testing and readability anyway.

I think non-capturing closures, that are returned from function should likely also be exempt from this.

I agree that for the little JS I write; there are more often locally defined functions; but Js is also better suited at functional an have an easier way of defining multiline functions.

---

_Label `needs-decision` added by @MichaReiser on 2024-11-26 09:51_

---

_Comment by @dylwil3 on 2024-11-27 14:55_

Could you give a small code snippet illustrating the situation you're talking about? It's just helpful to have something to look at 🙂 

---

_Comment by @Carreau on 2024-11-27 16:24_

Say 

```
def check_compatible(filename: str) -> None:
    ...
    def platform_to_version(platform: str) -> str:
        return (
            platform.replace("-", "_")
            .removeprefix("emscripten_")
            .removesuffix("_wasm32")
            .replace("_", ".")
        )

    pyodide_emscripten_version = platform_to_version(get_platform())
    ...
    return ...

```

`platform_to_version` does not capture anything, and does not need to be (re)-defined at every call of `check_compatible`. 


This often (IMHO) is an artefact of removing a closed over variable; made up example for the above for simplicity.

```diff python
--- a/micropip/_utils.py
+++ b/micropip/_utils.py
@@ -157,7 +157,7 @@ 
 def check_compatible(filename: str) -> None:
    ...

-   suffix = '_wasm32'
     
    def platform_to_version(platform: str) -> str:
         return (
             platform.replace("-", "_")
             .removeprefix("emscripten_")
-             .removesuffix(suffix)
+             .removesuffix('_wasm32')
             .replace("_", ".")
         )

 pyodide_emscripten_version = platform_to_version(get_platform())
 ...
 return ...

```



---

_Comment by @dylwil3 on 2024-11-27 16:46_

Thank you, that helps me understand better!

I think this is a fairly opinionated lint, since I have often seen one-off helper functions defined in the body of another function (even if they are not using a binding from the outer function's scope). So I would be tempted to wait until we have rule categories in order to communicate to users that this would be an opinionated rule whose fit may depend on the codebase, rather than adding it as a new `RUF` rule.

---
