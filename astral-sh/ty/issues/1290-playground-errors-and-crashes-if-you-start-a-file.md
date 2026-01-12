```yaml
number: 1290
title: "Playground errors and crashes if you start a file name with vendored:"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - playground
assignees: []
created_at: 2025-09-30T22:38:20Z
updated_at: 2025-10-04T11:40:38Z
url: https://github.com/astral-sh/ty/issues/1290
synced_at: 2026-01-12T15:54:24Z
```

# Playground errors and crashes if you start a file name with vendored:

---

_@MeGaGiGaGon_

### Summary

Opening this playground link then trying to switch between/edit the `vendored:` files will cause a lot of fun errors. Trying to rename `vendored:/module1.py` will crash the playground if you try to rename it, switch to a different file, and switch back.
https://play.ty.dev/553e7a84-d5b1-489b-aff7-a1365d1e7636

This can also cause your playground to become almost permanently unusable if it gets saved on a `vendored:` file since the reset button stops working, so here's a link to a fresh playground so you can stop that:
https://play.ty.dev/9e2d77b7-6acf-4e16-b0ff-492f84446908

A sampling of the errors I made while messing around with this:
Opening / doing anything in `vendored:main.py`:
```
Uncaught Error: Vendored file not found: main.py: Not found
```

Opening `vendored:/module1.py`:
```
panicked at crates/ruff_db/src/vendored.rs:425:25:
Unsupported component in a vendored path: /
```

Opening / doing anything in `vendored:/module1.py`:
```
panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/parking_lot_core-0.9.11/src/thread_parker/wasm.rs:26:9:
Parking not supported on this platform

Uncaught Error: unreachable
```

Trying to rename `vendored:/module1.py`:
```
Uncaught Error: recursive use of an object detected which would lead to unsafe aliasing in rust
```

Error on crash from rename:
```
Uncaught Error: null pointer passed to rust
```

<details>

<summary>Full tracebacks</summary>

```
errors.ts:26 Uncaught Error: Vendored file not found: main.py: Not found

Error: Vendored file not found: main.py: Not found
    at Bp.l.wbg.__wbg_new_97ddeb994a38bb69 (https://play.ty.dev/assets/index-C9TmvguN.js:64:25749)
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[10049]:0x7affe6
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[8066]:0x79027b
    at go.getVendoredFile (https://play.ty.dev/assets/index-C9TmvguN.js:64:22511)
    at V.getOrCreateVendoredFileHandle (https://play.ty.dev/assets/Editor-DBCEPOc0.js:1:5165)
    at V.getFileHandleForModel (https://play.ty.dev/assets/Editor-DBCEPOc0.js:1:5330)
    at V.provideDocumentSemanticTokens (https://play.ty.dev/assets/Editor-DBCEPOc0.js:1:3188)
    at https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:622:44951
    at Array.map (<anonymous>)
    at g (https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:622:44910)
(anonymous) @ errors.ts:26
setTimeout
unexpectedErrorHandler @ errors.ts:20
onUnexpectedError @ errors.ts:41
k @ errors.ts:56
(anonymous) @ documentSemanticTokens.ts:249
Promise.then
_fetchDocumentSemanticTokensNow @ documentSemanticTokens.ts:234
(anonymous) @ documentSemanticTokens.ts:123
doRun @ async.ts:559
onTimeout @ async.ts:554
setTimeout
schedule @ async.ts:533
a @ documentSemanticTokens.ts:181
L @ documentSemanticTokens.ts:42
(anonymous) @ documentSemanticTokens.ts:69
_deliver @ event.ts:1225
_deliverQueue @ event.ts:1236
fire @ event.ts:1260
createModel @ modelService.ts:377
H @ standaloneCodeEditor.ts:598
q @ standaloneCodeEditor.ts:589
U @ standaloneEditor.ts:227
O1 @ index-C9TmvguN.js:74
wi @ index-C9TmvguN.js:74
(anonymous) @ index-C9TmvguN.js:74
ll @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
Dg @ index-C9TmvguN.js:49
Nr @ index-C9TmvguN.js:49
Mg @ index-C9TmvguN.js:49
zg @ index-C9TmvguN.js:49
wg @ index-C9TmvguN.js:49
vg @ index-C9TmvguN.js:49
Ug @ index-C9TmvguN.js:49
dl @ index-C9TmvguN.js:49
Hg @ index-C9TmvguN.js:49
(anonymous) @ index-C9TmvguN.js:49
```
```
index-C9TmvguN.js:64 panicked at crates/ruff_db/src/vendored.rs:425:25:
Unsupported component in a vendored path: /

Stack:

Error
    at Bp.l.wbg.__wbg_new_8a6f238a6ece86ea (https://play.ty.dev/assets/index-C9TmvguN.js:64:25613)
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[10066]:0x7b05ff
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[3133]:0x613c34
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[5321]:0x70a137
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[8078]:0x791013
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[1389]:0x461d90
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[1120]:0x3f67ab
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[844]:0x373709
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[8066]:0x790202
    at go.getVendoredFile (https://play.ty.dev/assets/index-C9TmvguN.js:64:22511)


Bp.l.wbg.__wbg_error_7534b8e9a36f1ab4 @ index-C9TmvguN.js:64
$func10066 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7b06ca
$func3133 @ ty_wasm_bg-C9qyJ2pt.wasm:0x613c34
$func5321 @ ty_wasm_bg-C9qyJ2pt.wasm:0x70a137
$func8078 @ ty_wasm_bg-C9qyJ2pt.wasm:0x791013
$func1389 @ ty_wasm_bg-C9qyJ2pt.wasm:0x461d90
$func1120 @ ty_wasm_bg-C9qyJ2pt.wasm:0x3f67ab
$func844 @ ty_wasm_bg-C9qyJ2pt.wasm:0x373709
$workspace_getVendoredFile @ ty_wasm_bg-C9qyJ2pt.wasm:0x790202
getVendoredFile @ index-C9TmvguN.js:64
getOrCreateVendoredFileHandle @ Editor-DBCEPOc0.js:1
getFileHandleForModel @ Editor-DBCEPOc0.js:1
provideDocumentSemanticTokens @ Editor-DBCEPOc0.js:1
(anonymous) @ getSemanticTokens.ts:53
g @ getSemanticTokens.ts:49
_fetchDocumentSemanticTokensNow @ documentSemanticTokens.ts:224
(anonymous) @ documentSemanticTokens.ts:123
doRun @ async.ts:559
onTimeout @ async.ts:554
setTimeout
schedule @ async.ts:533
a @ documentSemanticTokens.ts:181
L @ documentSemanticTokens.ts:42
(anonymous) @ documentSemanticTokens.ts:69
_deliver @ event.ts:1225
_deliverQueue @ event.ts:1236
fire @ event.ts:1260
createModel @ modelService.ts:377
H @ standaloneCodeEditor.ts:598
q @ standaloneCodeEditor.ts:589
U @ standaloneEditor.ts:227
O1 @ index-C9TmvguN.js:74
wi @ index-C9TmvguN.js:74
(anonymous) @ index-C9TmvguN.js:74
ll @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
Dg @ index-C9TmvguN.js:49
Nr @ index-C9TmvguN.js:49
Mg @ index-C9TmvguN.js:49
zg @ index-C9TmvguN.js:49
wg @ index-C9TmvguN.js:49
vg @ index-C9TmvguN.js:49
Ug @ index-C9TmvguN.js:49
dl @ index-C9TmvguN.js:49
Hg @ index-C9TmvguN.js:49
(anonymous) @ index-C9TmvguN.js:49
```
```
index-C9TmvguN.js:64 panicked at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/parking_lot_core-0.9.11/src/thread_parker/wasm.rs:26:9:
Parking not supported on this platform

Stack:

Error
    at Bp.l.wbg.__wbg_new_8a6f238a6ece86ea (https://play.ty.dev/assets/index-C9TmvguN.js:64:25613)
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[10066]:0x7b05ff
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[3133]:0x613c34
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[5321]:0x70a137
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[8078]:0x791041
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[7884]:0x76fa90
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[2294]:0x581d51
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[844]:0x373534
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[8066]:0x790202
    at go.getVendoredFile (https://play.ty.dev/assets/index-C9TmvguN.js:64:22511)


Bp.l.wbg.__wbg_error_7534b8e9a36f1ab4 @ index-C9TmvguN.js:64
$func10066 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7b06ca
$func3133 @ ty_wasm_bg-C9qyJ2pt.wasm:0x613c34
$func5321 @ ty_wasm_bg-C9qyJ2pt.wasm:0x70a137
$func8078 @ ty_wasm_bg-C9qyJ2pt.wasm:0x791041
$func7884 @ ty_wasm_bg-C9qyJ2pt.wasm:0x76fa90
$func2294 @ ty_wasm_bg-C9qyJ2pt.wasm:0x581d51
$func844 @ ty_wasm_bg-C9qyJ2pt.wasm:0x373534
$workspace_getVendoredFile @ ty_wasm_bg-C9qyJ2pt.wasm:0x790202
getVendoredFile @ index-C9TmvguN.js:64
getOrCreateVendoredFileHandle @ Editor-DBCEPOc0.js:1
getFileHandleForModel @ Editor-DBCEPOc0.js:1
provideInlayHints @ Editor-DBCEPOc0.js:1
(anonymous) @ inlayHints.ts:79
(anonymous) @ inlayHints.ts:77
create @ inlayHints.ts:77
(anonymous) @ inlayHintsController.ts:215
doRun @ async.ts:559
onTimeout @ async.ts:554
setTimeout
schedule @ async.ts:533
_update @ inlayHintsController.ts:250
(anonymous) @ inlayHintsController.ts:129
_deliver @ event.ts:1225
_deliverQueue @ event.ts:1236
fire @ event.ts:1260
setModel @ codeEditorWidget.ts:499
(anonymous) @ index-C9TmvguN.js:74
ll @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
Dg @ index-C9TmvguN.js:49
Nr @ index-C9TmvguN.js:49
Mg @ index-C9TmvguN.js:49
zg @ index-C9TmvguN.js:49
wg @ index-C9TmvguN.js:49
vg @ index-C9TmvguN.js:49
Ug @ index-C9TmvguN.js:49
dl @ index-C9TmvguN.js:49
Hg @ index-C9TmvguN.js:49
(anonymous) @ index-C9TmvguN.js:49
```
```
errors.ts:26 Uncaught Error: unreachable

RuntimeError: unreachable
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[5321]:0x70a15b
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[8078]:0x791013
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[1389]:0x461d90
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[1120]:0x3f67ab
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[844]:0x373709
    at https://play.ty.dev/assets/ty_wasm_bg-C9qyJ2pt.wasm:wasm-function[8066]:0x790202
    at go.getVendoredFile (https://play.ty.dev/assets/index-C9TmvguN.js:64:22511)
    at V.getOrCreateVendoredFileHandle (https://play.ty.dev/assets/Editor-DBCEPOc0.js:1:5165)
    at V.getFileHandleForModel (https://play.ty.dev/assets/Editor-DBCEPOc0.js:1:5330)
    at V.provideDocumentSemanticTokens (https://play.ty.dev/assets/Editor-DBCEPOc0.js:1:3188)
(anonymous) @ errors.ts:26
setTimeout
unexpectedErrorHandler @ errors.ts:20
onUnexpectedError @ errors.ts:41
k @ errors.ts:56
(anonymous) @ documentSemanticTokens.ts:249
Promise.then
_fetchDocumentSemanticTokensNow @ documentSemanticTokens.ts:234
(anonymous) @ documentSemanticTokens.ts:123
doRun @ async.ts:559
onTimeout @ async.ts:554
setTimeout
schedule @ async.ts:533
a @ documentSemanticTokens.ts:181
L @ documentSemanticTokens.ts:42
(anonymous) @ documentSemanticTokens.ts:69
_deliver @ event.ts:1225
_deliverQueue @ event.ts:1236
fire @ event.ts:1260
createModel @ modelService.ts:377
H @ standaloneCodeEditor.ts:598
q @ standaloneCodeEditor.ts:589
U @ standaloneEditor.ts:227
O1 @ index-C9TmvguN.js:74
wi @ index-C9TmvguN.js:74
(anonymous) @ index-C9TmvguN.js:74
ll @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
nn @ index-C9TmvguN.js:49
gg @ index-C9TmvguN.js:49
Dg @ index-C9TmvguN.js:49
Nr @ index-C9TmvguN.js:49
Mg @ index-C9TmvguN.js:49
zg @ index-C9TmvguN.js:49
wg @ index-C9TmvguN.js:49
vg @ index-C9TmvguN.js:49
Ug @ index-C9TmvguN.js:49
dl @ index-C9TmvguN.js:49
Hg @ index-C9TmvguN.js:49
(anonymous) @ index-C9TmvguN.js:49
```
```
index-C9TmvguN.js:64 Uncaught Error: recursive use of an object detected which would lead to unsafe aliasing in rust
    at Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf (index-C9TmvguN.js:64:29195)
    at ty_wasm_bg-C9qyJ2pt.wasm:0x7acee9
    at ty_wasm_bg-C9qyJ2pt.wasm:0x7acf05
    at ty_wasm_bg-C9qyJ2pt.wasm:0x79591b
    at go.closeFile (index-C9TmvguN.js:64:18917)
    at te (index-C9TmvguN.js:87:7507)
    at le (index-C9TmvguN.js:87:3379)
    at onRenamed (index-C9TmvguN.js:76:61377)
    at h (index-C9TmvguN.js:76:62203)
    at onBlur (index-C9TmvguN.js:76:62770)
Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf @ index-C9TmvguN.js:64
$func9945 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7acee9
$func9947 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7acf05
$workspace_closeFile @ ty_wasm_bg-C9qyJ2pt.wasm:0x79591b
closeFile @ index-C9TmvguN.js:64
te @ index-C9TmvguN.js:87
le @ index-C9TmvguN.js:87
onRenamed @ index-C9TmvguN.js:76
h @ index-C9TmvguN.js:76
onBlur @ index-C9TmvguN.js:76
Gg @ index-C9TmvguN.js:49
(anonymous) @ index-C9TmvguN.js:49
Kc @ index-C9TmvguN.js:49
As @ index-C9TmvguN.js:49
Us @ index-C9TmvguN.js:50
M_ @ index-C9TmvguN.js:50
setSelectionRange @ textAreaInput.ts:824
writeToTextArea @ textAreaState.ts:85
_setAndWriteTextAreaState @ textAreaInput.ts:626
writeNativeTextAreaContent @ textAreaInput.ts:637
_setHasFocus @ textAreaInput.ts:611
focusTextArea @ textAreaInput.ts:581
focusTextArea @ textAreaHandler.ts:681
focus @ view.ts:593
focusTextArea @ view.ts:285
D @ mouseHandler.ts:322
_onMouseDown @ mouseHandler.ts:326
(anonymous) @ mouseHandler.ts:130
(anonymous) @ editorDom.ts:165
index-C9TmvguN.js:64 Uncaught Error: null pointer passed to rust
    at Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf (index-C9TmvguN.js:64:29195)
    at ty_wasm_bg-C9qyJ2pt.wasm:0x7acee9
    at ty_wasm_bg-C9qyJ2pt.wasm:0x7acef6
    at ty_wasm_bg-C9qyJ2pt.wasm:0x796582
    at Ge.path (index-C9TmvguN.js:64:6385)
    at X1 (index-C9TmvguN.js:76:62990)
    at index-C9TmvguN.js:87:5897
    at Object.yd [as useMemo] (index-C9TmvguN.js:49:42703)
    at ge.useMemo (index-C9TmvguN.js:18:7243)
    at hv (index-C9TmvguN.js:87:5795)
Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf @ index-C9TmvguN.js:64
$func9945 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7acee9
$func9946 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7acef6
$filehandle_path @ ty_wasm_bg-C9qyJ2pt.wasm:0x796582
path @ index-C9TmvguN.js:64
X1 @ index-C9TmvguN.js:76
(anonymous) @ index-C9TmvguN.js:87
yd @ index-C9TmvguN.js:49
ge.useMemo @ index-C9TmvguN.js:18
hv @ index-C9TmvguN.js:87
gv @ index-C9TmvguN.js:87
Ru @ index-C9TmvguN.js:49
Fu @ index-C9TmvguN.js:49
Pd @ index-C9TmvguN.js:49
Tg @ index-C9TmvguN.js:49
Qm @ index-C9TmvguN.js:49
ms @ index-C9TmvguN.js:49
vg @ index-C9TmvguN.js:49
Ug @ index-C9TmvguN.js:49
dl @ index-C9TmvguN.js:49
Hg @ index-C9TmvguN.js:49
(anonymous) @ index-C9TmvguN.js:49
```
```
index-C9TmvguN.js:64 Uncaught Error: recursive use of an object detected which would lead to unsafe aliasing in rust
    at Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf (index-C9TmvguN.js:64:29195)
    at ty_wasm_bg-C9qyJ2pt.wasm:0x7acee9
    at ty_wasm_bg-C9qyJ2pt.wasm:0x7acf05
    at ty_wasm_bg-C9qyJ2pt.wasm:0x79591b
    at go.closeFile (index-C9TmvguN.js:64:18917)
    at te (index-C9TmvguN.js:87:7507)
    at le (index-C9TmvguN.js:87:3379)
    at onRenamed (index-C9TmvguN.js:76:61377)
    at h (index-C9TmvguN.js:76:62203)
    at onBlur (index-C9TmvguN.js:76:62770)
Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf @ index-C9TmvguN.js:64
$func9945 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7acee9
$func9947 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7acf05
$workspace_closeFile @ ty_wasm_bg-C9qyJ2pt.wasm:0x79591b
closeFile @ index-C9TmvguN.js:64
te @ index-C9TmvguN.js:87
le @ index-C9TmvguN.js:87
onRenamed @ index-C9TmvguN.js:76
h @ index-C9TmvguN.js:76
onBlur @ index-C9TmvguN.js:76
Gg @ index-C9TmvguN.js:49
(anonymous) @ index-C9TmvguN.js:49
Kc @ index-C9TmvguN.js:49
As @ index-C9TmvguN.js:49
Us @ index-C9TmvguN.js:50
M_ @ index-C9TmvguN.js:50
setSelectionRange @ textAreaInput.ts:824
writeToTextArea @ textAreaState.ts:85
_setAndWriteTextAreaState @ textAreaInput.ts:626
writeNativeTextAreaContent @ textAreaInput.ts:637
_setHasFocus @ textAreaInput.ts:611
focusTextArea @ textAreaInput.ts:581
focusTextArea @ textAreaHandler.ts:681
focus @ view.ts:593
focusTextArea @ view.ts:285
D @ mouseHandler.ts:322
_onMouseDown @ mouseHandler.ts:326
(anonymous) @ mouseHandler.ts:130
(anonymous) @ editorDom.ts:165
```
```
index-C9TmvguN.js:64 Uncaught Error: null pointer passed to rust
    at Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf (index-C9TmvguN.js:64:29195)
    at ty_wasm_bg-C9qyJ2pt.wasm:0x7acee9
    at ty_wasm_bg-C9qyJ2pt.wasm:0x7acef6
    at ty_wasm_bg-C9qyJ2pt.wasm:0x796582
    at Ge.path (index-C9TmvguN.js:64:6385)
    at X1 (index-C9TmvguN.js:76:62990)
    at index-C9TmvguN.js:87:5897
    at Object.yd [as useMemo] (index-C9TmvguN.js:49:42703)
    at ge.useMemo (index-C9TmvguN.js:18:7243)
    at hv (index-C9TmvguN.js:87:5795)
Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf @ index-C9TmvguN.js:64
$func9945 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7acee9
$func9946 @ ty_wasm_bg-C9qyJ2pt.wasm:0x7acef6
$filehandle_path @ ty_wasm_bg-C9qyJ2pt.wasm:0x796582
path @ index-C9TmvguN.js:64
X1 @ index-C9TmvguN.js:76
(anonymous) @ index-C9TmvguN.js:87
yd @ index-C9TmvguN.js:49
ge.useMemo @ index-C9TmvguN.js:18
hv @ index-C9TmvguN.js:87
gv @ index-C9TmvguN.js:87
Ru @ index-C9TmvguN.js:49
Fu @ index-C9TmvguN.js:49
Pd @ index-C9TmvguN.js:49
Tg @ index-C9TmvguN.js:49
Qm @ index-C9TmvguN.js:49
ms @ index-C9TmvguN.js:49
vg @ index-C9TmvguN.js:49
Ug @ index-C9TmvguN.js:49
dl @ index-C9TmvguN.js:49
Hg @ index-C9TmvguN.js:49
(anonymous) @ index-C9TmvguN.js:49
```
</details>

### Version

Playground (2b1d3c60f)

---

_Label `bug` added by @MichaReiser on 2025-10-01 05:18_

---

_Label `playground` added by @MichaReiser on 2025-10-01 05:18_

---

_Closed by @MichaReiser on 2025-10-04 11:40_

---
