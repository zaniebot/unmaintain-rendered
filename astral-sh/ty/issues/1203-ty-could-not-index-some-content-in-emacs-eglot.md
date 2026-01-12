```yaml
number: 1203
title: Ty could not index some content in emacs (eglot)
type: issue
state: closed
author: ileixe
labels:
  - server
  - wish
assignees: []
created_at: 2025-09-18T14:46:55Z
updated_at: 2025-09-22T11:12:33Z
url: https://github.com/astral-sh/ty/issues/1203
synced_at: 2026-01-12T15:54:24Z
```

# Ty could not index some content in emacs (eglot)

---

_@ileixe_

### Question

After I changed to use ty, I found very weird cases which are

1. Some contents are not indexed (go-to-definition is not working) correctly (I can't even see what's difference between working symbols and not)
2. Newly added symbols never seems to be indexed. (Go to definition is not working)

I compared pylsp in the same code, it was working as expected. 

Eglot has some suspicious logging below

```
[stderr]  [2025-09-18T14:23:02Z INFO  emacs_lsp_booster::app] Running server "uvx" "ty" "server"
[stderr]  [2025-09-18T14:23:02Z INFO  emacs_lsp_booster::app] Will convert server json to bytecode! bytecode options: BytecodeOptions { object_type: Plist, null_value: Nil, false_value: Keyword("json-false") }
[stderr]  2025-09-18 23:23:02.184626028  INFO Version: 0.0.1-alpha.20
[stderr]  2025-09-18 23:23:02.229228647  INFO Defaulting to python-platform `linux`
[stderr]  2025-09-18 23:23:02.230516781  INFO Python version: Python 3.9, platform: linux
[stderr]  2025-09-18 23:23:02.230962178  WARN Your LSP client doesn't support file watching outside of project: You may see stale results when dependencies change
[stderr]  2025-09-18 23:23:02.239369501  INFO Indexed 178 file(s) in 0.004s
[stderr]  2025-09-18 23:23:02.767160398  WARN Received notification workspace/didChangeConfiguration which does not have a handler.
```

Do we (eglot in emacs) need some specific configuration to use ty?

### Version

_No response_

---

_Label `question` added by @ileixe on 2025-09-18 14:46_

---

_Label `server` added by @carljm on 2025-09-18 14:49_

---

_Comment by @MichaReiser on 2025-09-18 14:56_

> [stderr]  2025-09-18 23:23:02.230962178  WARN Your LSP client doesn't support file watching outside of project: You may see stale results when dependencies change

I suspect that it is due to emacs (eglot) not supporting file watching. That means, ty only gets notified about files that are changed in the editor but not when files change with git. 

We may want to fall back to our own file watcher in that case.

---

_Label `question` removed by @MichaReiser on 2025-09-18 14:56_

---

_Label `wish` added by @MichaReiser on 2025-09-18 14:56_

---

_Comment by @ileixe on 2025-09-18 15:02_

Thanks for fast response.

> I suspect that it is due to emacs (eglot) not supporting file watching. That means, ty only gets notified about files that are changed in the editor but not when files change with git.

It's most strange part because eglot support it: https://github.com/joaotavora/eglot/blob/master/eglot.el#L1013

Is there any other condition that turn off the flag?

---

_Comment by @MichaReiser on 2025-09-18 15:11_

Oh right, it does support file watching but not relative paths. There might be a bug in our fallback handling https://github.com/astral-sh/ruff/blob/25853e2377b17eb9c192c4989616d6e823b81435/crates/ty_server/src/session.rs#L802-L816

CC: @BurntSushi 

---

_Comment by @ileixe on 2025-09-22 01:12_

For my case (1,2), this fix made working again.

```
modified   crates/ty_python_semantic/src/types/infer/builder.rs
@@ -2349,7 +2349,17 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             let ty = if let Some(default_ty) = default_ty {
                 UnionType::from_elements(self.db(), [Type::unknown(), default_ty])
             } else {
-                Type::unknown()
+                // Special case: infer `self` parameter type in method contexts
+                let param_name = parameter.name().as_str();
+                if param_name == "self" {
+                    if let Some(class_type) = self.class_context_of_current_method() {
+                        Type::instance(self.db(), class_type)
+                    } else {
+                        Type::unknown()
+                    }
+                } else {
+                    Type::unknown()
+                }
             };
             self.add_binding(parameter.into(), definition, ty);
         }
```

I'm not sure it's correct fix, but at least it's working now. 

---

_Comment by @MichaReiser on 2025-09-22 07:20_

@ileixe I'm not sure how this change would help with file contents (and diagnostics) getting outdated. To me, it looks like what you attempted here is to add support for https://github.com/astral-sh/ty/issues/159, which we're working on. 

Can you clarify your reproduction steps. I suspect it's something like. Open project, add a new class, search for the new class and it doesn't show up?



---

_Comment by @ileixe on 2025-09-22 10:37_

@MichaReiser 

After some trial, I found it's not related to file content updated. 

I found this simple case does not work as expected with eglot(ty)

```
:: issue1203$ cat main.py
class A:
    def func(self):
        pass

    def func2(self):
        self.func()  # go-to-definition here is not working
```

With the change, it worked as expected. For both cases, eglot log was not different.

```
[stderr]  [2025-09-22T10:34:16Z INFO  emacs_lsp_booster::app] Running server "/home/ys/ruff/target/release/ty" "server"
[stderr]  [2025-09-22T10:34:16Z INFO  emacs_lsp_booster::app] Will convert server json to bytecode! bytecode options: BytecodeOptions { object_type: Plist, null_value: Nil, false_value: Keyword("json-false") }
[stderr]  2025-09-22 19:34:16.106946297  INFO Version: ruff/%(describe:tags) (bc89d0394 2025-09-18)
[stderr]  2025-09-22 19:34:16.134834591  INFO Defaulting to python-platform `linux`
[stderr]  2025-09-22 19:34:16.135219297  INFO Python version: Python 3.13, platform: linux
[stderr]  2025-09-22 19:34:16.135580612  WARN Your LSP client doesn't support file watching outside of project: You may see stale results when dependencies change
[stderr]  2025-09-22 19:34:16.140280020  INFO Indexed 1 file(s) in 0.001s
[stderr]  2025-09-22 19:34:16.154497507  WARN Received notification workspace/didChangeConfiguration which does not have a handler.
[stderr]  2025-09-22 19:34:33.733288824  INFO Defaulting to python-platform `linux`
[stderr]  2025-09-22 19:34:33.733503147  INFO Python version: Python 3.13, platform: linux
[stderr]  2025-09-22 19:34:33.737438525  INFO Indexed 1 file(s) in 0.004s
[stderr]  2025-09-22 19:34:33.737532226  INFO Defaulting to python-platform `linux`
[stderr]  2025-09-22 19:34:33.737647818  INFO Python version: Python 3.13, platform: linux
[stderr]  2025-09-22 19:34:33.739134390  INFO Indexed 1 file(s) in 0.001s
``` 



---

_Comment by @MichaReiser on 2025-09-22 10:38_

Yes, this is https://github.com/astral-sh/ty/issues/159

But that still sounds different from what you reported in your original issue. But if it's the same, I suggest closing the issue and subscribing to https://github.com/astral-sh/ty/issues/159

---

_Comment by @ileixe on 2025-09-22 11:12_

Ok Thanks. I think I did not differentiate possible causes, and at least it's working as expected. 

I will follow #159

---

_Closed by @ileixe on 2025-09-22 11:12_

---
