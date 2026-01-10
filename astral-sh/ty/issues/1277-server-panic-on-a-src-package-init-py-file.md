```yaml
number: 1277
title: "Server panic on a `src/package/__init__.py` file containing just `fr`"
type: issue
state: closed
author: AlexWaygood
labels:
  - server
  - fatal
  - completions
assignees: []
created_at: 2025-09-29T13:34:06Z
updated_at: 2025-10-03T12:18:04Z
url: https://github.com/astral-sh/ty/issues/1277
synced_at: 2026-01-10T02:06:25Z
```

# Server panic on a `src/package/__init__.py` file containing just `fr`

---

_Issue opened by @AlexWaygood on 2025-09-29 13:34_

### Summary

I just hit this panic in the playground, which appears to be server-specific (and also specific to it being an `__init__.py` file):

https://github.com/user-attachments/assets/f8c925ca-8421-4c0c-804c-36b440e91a77

Right-clicking on the webpage and selecting "Inspect", I see two stacktraces: initially this one:

```
panicked at crates/ty_python_semantic/src/module_resolver/list.rs:78:18:
System search path should have a registered root

Stack:

Bp/l.wbg.__wbg_new_8a6f238a6ece86ea@https://play.ty.dev/assets/index-IVeoMaQp.js:64:25613
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[10069]:0x7b45f6
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[3133]:0x61aca6
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[5324]:0x70d654
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[8097]:0x796599
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[6798]:0x74e0a2
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[921]:0x3ada21
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[524]:0x2ccb9a
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[512]:0x2c3f37
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[521]:0x2caf9f
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[2274]:0x584e67
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[8024]:0x77f66b
completions@https://play.ty.dev/assets/index-IVeoMaQp.js:64:21219
provideCompletionItems@https://play.ty.dev/assets/Editor-DvbeuRwl.js:1:3621
h/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:28999
h@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:28760
trigger@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:726:8465
_doTriggerQuickSuggest/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:726:7331
cancelAndSet/this._token<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:15238


[index-IVeoMaQp.js:64:24316](https://play.ty.dev/assets/index-IVeoMaQp.js)
    __wbg_error_7534b8e9a36f1ab4 index-IVeoMaQp.js:64
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:8079041
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:6401190
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:7394900
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:7955865
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:7659682
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:3856929
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:2935706
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:2899767
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:2928543
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:5787239
    <anonymous> ty_wasm_bg-CmMjrFpL.wasm:7861867
    completions index-IVeoMaQp.js:64
    provideCompletionItems Editor-DvbeuRwl.js:1
    h suggest.ts:302
    h suggest.ts:288
    trigger suggestModel.ts:495
    _doTriggerQuickSuggest suggestModel.ts:432
    _token async.ts:443
```

and then this one (repeated several times):

```
panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/fetch.rs:275:21:
dependency graph cycle when querying list_modules(Id(c400)), set cycle_fn/cycle_initial to fixpoint iterate.
Query stack:
[
    list_modules(Id(c400)),
    list_modules_in(Id(c801)),
    check_file_impl(Id(2801)),
    infer_scope_types(Id(1002g1)),
    suppressions(Id(2801)),
    ClassLiteral < 'db >::try_mro_(Id(8c4f)),
    infer_definition_types(Id(65b3)),
    infer_expression_types_impl(Id(2c68)),
    infer_expression_types_impl(Id(2c38)),
    Type < 'db >::member_lookup_with_policy_(Id(30a8)),
    infer_expression_types_impl(Id(2c37)),
    infer_definition_types(Id(1488)),
    infer_definition_types(Id(150b)),
    infer_expression_types_impl(Id(2c30)),
    all_narrowing_constraints_for_expression(Id(2ec2)),
    imported_modules(Id(ca4)),
    infer_definition_types(Id(ac59)),
    file_to_module(Id(c8f)),
    source_text(Id(c8f)),
]

Stack:

Bp/l.wbg.__wbg_new_8a6f238a6ece86ea@https://play.ty.dev/assets/index-IVeoMaQp.js:64:25613
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[10069]:0x7b45f6
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[3133]:0x61aca6
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[5324]:0x70d654
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[8097]:0x796599
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[6160]:0x73a6bc
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[2274]:0x584a3a
@https://play.ty.dev/assets/ty_wasm_bg-CmMjrFpL.wasm:wasm-function[8024]:0x77f66b
completions@https://play.ty.dev/assets/index-IVeoMaQp.js:64:21219
provideCompletionItems@https://play.ty.dev/assets/Editor-DvbeuRwl.js:1:3621
h/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:28999
h@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:28760
trigger@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:726:8465
_doTriggerQuickSuggest/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:726:7331
cancelAndSet/this._token<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:15238
```

### Version

_No response_

---

_Label `server` added by @AlexWaygood on 2025-09-29 13:34_

---

_Label `fatal` added by @AlexWaygood on 2025-09-29 13:34_

---

_Comment by @AlexWaygood on 2025-09-29 13:36_

cc. @BurntSushi -- this looks like it may be autocompletions-related?

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:48_

---

_Comment by @DanielKongsgaard on 2025-10-01 21:39_

I get a similar panic on autocomplete in the beginning of imports. With the beginning I mean that I get it on `import pac|`, but not on `import package.sub|` (where `|` is the cursor). With `ty.experimental.autoImport = true` I also consistently get the panic in code. Here is my stack trace, though it seems less useful:
```
2025-10-01 23:30:33.541873000 ERROR An error occurred with request ID 7: request handler panicked at crates/ty_python_semantic/src/module_resolver/list.rs:78:18:
System search path should have a registered rootquery stacktrace:
   0: DatabaseKeyIndex(IngredientIndex(81), Id(20803))
   1: DatabaseKeyIndex(IngredientIndex(79), Id(20400))


Backtrace:    0: __mh_execute_header
   1: __mh_execute_header
   2: __mh_execute_header
   3: __mh_execute_header
   4: __mh_execute_header
   5: __mh_execute_header
   6: __mh_execute_header
   7: __mh_execute_header
   8: __mh_execute_header
   9: __mh_execute_header
  10: __mh_execute_header
  11: __mh_execute_header
  12: __mh_execute_header
  13: __mh_execute_header
  14: __mh_execute_header
  15: __mh_execute_header
  16: __mh_execute_header
  17: __mh_execute_header
  18: __mh_execute_header
  19: __mh_execute_header
  20: __mh_execute_header
  21: __mh_execute_header
  22: __mh_execute_header
  23: __mh_execute_header
  24: __mh_execute_header
  25: __mh_execute_header
  26: __mh_execute_header
  27: __mh_execute_header
  28: __mh_execute_header
  29: __pthread_deallocate
```

---

_Assigned to @BurntSushi by @BurntSushi on 2025-10-02 12:46_

---

_Comment by @BurntSushi on 2025-10-02 17:55_

Thanks for the report! This turned out to be a bug in the `FileRoots` matching when the root is `/`. I'm surprised we hadn't hit this before or otherwise observed deleterious effects because of it. I put up https://github.com/astral-sh/ruff/pull/20684 which should fix this.

---

_Closed by @BurntSushi on 2025-10-03 12:18_

---
