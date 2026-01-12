```yaml
number: 1637
title: "Server crash on string annotation with `float`/`complex`"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - server
assignees: []
created_at: 2025-11-25T21:20:14Z
updated_at: 2025-11-25T22:09:47Z
url: https://github.com/astral-sh/ty/issues/1637
synced_at: 2026-01-12T15:54:25Z
```

# Server crash on string annotation with `float`/`complex`

---

_@MeGaGiGaGon_

### Summary

I suspect this is an issue with the new string annotation highlighting changes cc @Gankra, since it only happens on the playground and not with the VSC extension which is still on 0.0.1-alpha27.

Typing in the code `a: "float"` or `a: "complex"` causes a panic/crash when typing the last character of `float`/`complex`. I suspect this is because of the int/float/complex special case, since other builtin types like `str` or `list` work fine, as well as user-defined unions.

From the console output, the panic happens at `crates/ty_python_semantic/src/semantic_model.rs:411:32` with the message "Expression to be part of a scope if it is from the same module", which stems from here:

https://github.com/astral-sh/ruff/blob/81c97e9e9437ae68e23c047e4c6fb2abcf0c80d6/crates/ty_python_semantic/src/semantic_index.rs#L291-L299

<details>
<summary>Full playground stack trace</summary>

```
panicked at crates/ty_python_semantic/src/semantic_model.rs:411:32:
Expression to be part of a scope if it is from the same module

Stack:

jp/r.wbg.__wbg_new_8a6f238a6ece86ea@https://play.ty.dev/assets/index-CIp1Kw3A.js:64:29855
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[11158]:0x8f1797
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[3565]:0x748cf1
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[6009]:0x838bc4
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[9094]:0x8d2425
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[7681]:0x8853d2
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[3508]:0x74160c
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[401]:0x2c10fe
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[319]:0x278706
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[319]:0x278f7d
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[283]:0x25411b
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[3953]:0x773e7c
@https://play.ty.dev/assets/ty_wasm_bg-C1B77dWS.wasm:wasm-function[9038]:0x8c0be3
semanticTokens@https://play.ty.dev/assets/index-CIp1Kw3A.js:64:22315
provideDocumentSemanticTokens@https://play.ty.dev/assets/Editor-HlBsoyil.js:1:3383
g/L<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:622:44951
g@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:622:44910
_fetchDocumentSemanticTokensNow@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:673:13999
a/this._fetchDocumentSemanticTokens<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:673:11184
doRun@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:16421
onTimeout@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:16400
setTimeout handler*schedule@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:16211
a/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:673:11547
onDidChangeContent/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:679:10482
_deliver@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:910
_deliverQueue@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1001
fire@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1336
endDeferredEmit@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:688:20997
pushEditOperations@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:686:915
_innerExecuteCommands@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:650:5674
executeCommands@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:650:5092
_executeEditOperation@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:649:14021
type/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:650:1988
_executeEdit@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:650:1069
type@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:650:1864
type/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:3475
_withViewEventsCollector/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:5024
batchChanges@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:17190
_withViewEventsCollector@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:4943
_executeCursorEdit@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:3155
type@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:3440
_type@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:9316
trigger@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:8261
runCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:97439
handler@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:59966
registerCommand/_.handler@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:622:39686
invokeFunction@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:1015
executeCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:736:1382
type@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:20351
type@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:98506
w/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:667:112645
_deliver@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:910
fire@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1229
a/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:631:40947
_deliver@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:910
_deliverQueue@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1001
fire@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1336
_@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:4905
EventListener.handleEvent*onWillAddFirstListener@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:4970
get event/this._event@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:69:1671
r@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:633:1528
w@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:667:111442
_createInstance@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:2108
createInstance@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:1533
Y@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:670:21113
_createView@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:21426
_attachModel@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:19213
setModel@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:536
w1/ne<@https://play.ty.dev/assets/index-CIp1Kw3A.js:98:3841
L/<@https://play.ty.dev/assets/Editor-HlBsoyil.js:1:2601
invokeFunction@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:1015
executeCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:736:1382
_doDispatch@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:631:22276
_dispatch@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:631:19059
et/ft/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:736:1988
EventListener.handleEvent*c@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:80199
l@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:80421
ft@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:736:1891
mt@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:736:2461
_deliver@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:910
_deliverQueue@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1001
fire@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1336
addCodeEditor@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:667:47006
z@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:17042
x@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:738:6922
W@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:738:9828
_createInstance@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:2108
createInstance@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:1533
A@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:739:13847
Gv/we<@https://play.ty.dev/assets/index-CIp1Kw3A.js:74:6961
Gv/<@https://play.ty.dev/assets/index-CIp1Kw3A.js:74:7256
yr@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:83576
pg@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98271
an@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98157
pg@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98253
an@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98157
pg@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:99009
an@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98157
pg@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98253
an@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98157
pg@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98253
an@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98157
pg@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:99009
an@https://play.ty.dev/assets/index-CIp1Kw3A.js:49:98157


[index-CIp1Kw3A.js:64:28278](https://play.ty.dev/assets/index-CIp1Kw3A.js)
    __wbg_error_7534b8e9a36f1ab4 index-CIp1Kw3A.js:64
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:9377890
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:7638257
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:8620996
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:9249829
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:8934354
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:7607820
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:2887934
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:2590470
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:2592637
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:2441499
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:7814780
    <anonymous> ty_wasm_bg-C1B77dWS.wasm:9178083
    semanticTokens index-CIp1Kw3A.js:64
    provideDocumentSemanticTokens Editor-HlBsoyil.js:1
    L getSemanticTokens.ts:53
    g getSemanticTokens.ts:49
    _fetchDocumentSemanticTokensNow documentSemanticTokens.ts:224
    _fetchDocumentSemanticTokens documentSemanticTokens.ts:123
    doRun async.ts:559
    onTimeout async.ts:554
    (Async: setTimeout handler)
    schedule async.ts:533
    a documentSemanticTokens.ts:131
    onDidChangeContent textModel.ts:195
    _deliver event.ts:1225
    _deliverQueue event.ts:1236
    fire event.ts:1260
    endDeferredEmit textModel.ts:2443
    pushEditOperations textModel.ts:1219
    _innerExecuteCommands cursor.ts:792
    executeCommands cursor.ts:750
    _executeEditOperation cursor.ts:356
    type cursor.ts:561
    _executeEdit cursor.ts:516
    type cursor.ts:550
    type viewModelImpl.ts:1056
    _withViewEventsCollector viewModelImpl.ts:1109
    batchChanges codeEditorWidget.ts:1572
    _withViewEventsCollector viewModelImpl.ts:1106
    _executeCursorEdit viewModelImpl.ts:1044
    type viewModelImpl.ts:1056
    _type codeEditorWidget.ts:1109
    trigger codeEditorWidget.ts:1039
    runCommand coreCommands.ts:2144
    handler editorExtensions.ts:154
    handler commands.ts:94
    invokeFunction instantiationService.ts:109
    executeCommand standaloneServices.ts:292
    type codeEditorWidget.ts:1735
    type viewController.ts:72
    w textAreaHandler.ts:350
    _deliver event.ts:1225
    fire event.ts:1256
    a textAreaInput.ts:393
    _deliver event.ts:1225
    _deliverQueue event.ts:1236
    fire event.ts:1260
    _ event.ts:35
    (Async: EventListener.handleEvent)
    onWillAddFirstListener event.ts:37
    get event/this._event event.ts:1132
    r textAreaInput.ts:729
    w textAreaHandler.ts:307
    _createInstance instantiationService.ts:162
    createInstance instantiationService.ts:128
    Y view.ts:129
    _createView codeEditorWidget.ts:1773
    _attachModel codeEditorWidget.ts:1675
    setModel codeEditorWidget.ts:493
    ne index-CIp1Kw3A.js:98
    L Editor-HlBsoyil.js:1
    invokeFunction instantiationService.ts:109
    executeCommand standaloneServices.ts:292
    _doDispatch abstractKeybindingService.ts:331
    _dispatch abstractKeybindingService.ts:186
    ft standaloneServices.ts:334
    (Async: EventListener.handleEvent)
    c dom.ts:143
    l dom.ts:164
    ft standaloneServices.ts:332
    mt standaloneServices.ts:366
    _deliver event.ts:1225
    _deliverQueue event.ts:1236
    fire event.ts:1260
    addCodeEditor abstractCodeEditorService.ts:52
    z codeEditorWidget.ts:372
    x standaloneCodeEditor.ts:286
    W standaloneCodeEditor.ts:442
    _createInstance instantiationService.ts:162
    createInstance instantiationService.ts:128
    A standaloneEditor.ts:50
    we index-CIp1Kw3A.js:74
    Gv index-CIp1Kw3A.js:74
    yr index-CIp1Kw3A.js:49
    pg index-CIp1Kw3A.js:49
    an index-CIp1Kw3A.js:49
    pg index-CIp1Kw3A.js:49
    an index-CIp1Kw3A.js:49
    pg index-CIp1Kw3A.js:49
    an index-CIp1Kw3A.js:49
    pg index-CIp1Kw3A.js:49
    an index-CIp1Kw3A.js:49
    pg index-CIp1Kw3A.js:49
    an index-CIp1Kw3A.js:49
    pg index-CIp1Kw3A.js:49
    an index-CIp1Kw3A.js:49
```

</details>

### Version

playground (81c97e9e9)

---

_Assigned to @Gankra by @carljm on 2025-11-25 21:28_

---

_Added to milestone `Beta` by @carljm on 2025-11-25 21:28_

---

_Label `server` added by @carljm on 2025-11-25 21:28_

---

_Comment by @Gankra on 2025-11-25 21:42_

Confirmed this happens on main, looking into it.

---

_Comment by @carljm on 2025-11-25 21:45_

@Gankra Can you coordinate with @oconnor663 if you think we should hold today's release to get a fix for this in?

---

_Closed by @Gankra on 2025-11-25 22:09_

---
