```yaml
number: 1748
title: Crash on hover of stringified unknown type hint
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - server
  - fatal
assignees: []
created_at: 2025-12-03T21:00:44Z
updated_at: 2025-12-04T08:11:42Z
url: https://github.com/astral-sh/ty/issues/1748
synced_at: 2026-01-10T01:56:41Z
```

# Crash on hover of stringified unknown type hint

---

_Issue opened by @MeGaGiGaGon on 2025-12-03 21:00_

### Summary

Edit: After a bit more messing around, I found it's actually worse, and this crash affects any code that has an unknown item in a string annotation, ie `x: "foobar"`

This code crashes when you try to hover the `"x"`:
```py
x: "x"
```
With the error `panicked at crates/ruff_python_ast/src/token/tokens.rs:182:13: Offset 5 is inside a token range 3..6`

<details>
<summary>Full playground stack trace</summary>

```
panicked at crates/ruff_python_ast/src/token/tokens.rs:182:13:
Offset 5 is inside a token range 3..6

Stack:

Lp/r.wbg.__wbg_new_8a6f238a6ece86ea@https://play.ty.dev/assets/index-C7XW03DP.js:64:31611
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[11413]:0x92038a
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[3635]:0x76e364
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[6123]:0x865407
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[9294]:0x8fdcba
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[4678]:0x7e4159
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[9330]:0x9028b7
codeActions@https://play.ty.dev/assets/index-C7XW03DP.js:64:23094
provideCodeActions@https://play.ty.dev/assets/Editor-CINI16zY.js:1:7101
provideCodeActions@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:739:7711
C/z<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:135649
C@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:135603
_update/this._codeActionOracle.value</v<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:146660
b@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:11727
_update/this._codeActionOracle.value<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:144012
trigger@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:141115
_tryAutoTrigger/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:141340
cancelAndSet/this._token<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:15238
setTimeout handler*cancelAndSet@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:15207
_tryAutoTrigger@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:141317
g/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:141018
_deliver@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:910
_deliverQueue@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1001
fire@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1336
_attachModel/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:18484
_deliver@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:910
fire@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1229
_emitOutgoingEvents@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:111:131434
endEmitViewEvents@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:111:132130
_withViewEventsCollector/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:5059
batchChanges@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:17190
_withViewEventsCollector@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:4943
setCursorStates@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:2163
runCoreEditorCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:93402
runEditorCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:93318
_runEditorCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:70190
h/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:69771
runCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:60898
handler@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:59966
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
Fv/we<@https://play.ty.dev/assets/index-C7XW03DP.js:74:6961
Fv/<@https://play.ty.dev/assets/index-C7XW03DP.js:74:7256
br@https://play.ty.dev/assets/index-C7XW03DP.js:49:83576
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:98271
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:98253
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:99009
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:98253
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:98253
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:99009
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157


[index-C7XW03DP.js:64:30034](https://play.ty.dev/assets/index-C7XW03DP.js)
    __wbg_error_7534b8e9a36f1ab4 index-C7XW03DP.js:64
    <anonymous> ty_wasm_bg--RVj3Zek.wasm:9569365
    <anonymous> ty_wasm_bg--RVj3Zek.wasm:7791460
    <anonymous> ty_wasm_bg--RVj3Zek.wasm:8803335
    <anonymous> ty_wasm_bg--RVj3Zek.wasm:9428154
    <anonymous> ty_wasm_bg--RVj3Zek.wasm:8274265
    <anonymous> ty_wasm_bg--RVj3Zek.wasm:9447607
    codeActions index-C7XW03DP.js:64
    provideCodeActions Editor-CINI16zY.js:1
    provideCodeActions standaloneLanguages.ts:572
    z codeAction.ts:124
    C codeAction.ts:121
    v codeActionModel.ts:350
    b async.ts:24
    value codeActionModel.ts:233
    trigger codeActionModel.ts:54
    _tryAutoTrigger codeActionModel.ts:66
    _token async.ts:443
    (Async: setTimeout handler)
    cancelAndSet async.ts:441
    _tryAutoTrigger codeActionModel.ts:65
    g codeActionModel.ts:49
    _deliver event.ts:1225
    _deliverQueue event.ts:1236
    fire event.ts:1260
    _attachModel codeEditorWidget.ts:1637
    _deliver event.ts:1225
    fire event.ts:1256
    _emitOutgoingEvents viewModelEventDispatcher.ts:64
    endEmitViewEvents viewModelEventDispatcher.ts:109
    _withViewEventsCollector viewModelImpl.ts:1111
    batchChanges codeEditorWidget.ts:1572
    _withViewEventsCollector viewModelImpl.ts:1106
    setCursorStates viewModelImpl.ts:1002
    runCoreEditorCommand coreCommands.ts:1898
    runEditorCommand coreCommands.ts:1894
    _runEditorCommand coreCommands.ts:341
    h coreCommands.ts:312
    runCommand editorExtensions.ts:228
    handler editorExtensions.ts:154
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
    we index-C7XW03DP.js:74
    Fv index-C7XW03DP.js:74
    br index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
Uncaught Error: unreachable executed

@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[6123]:0x86542d
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[9294]:0x8fdcba
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[4678]:0x7e4159
@https://play.ty.dev/assets/ty_wasm_bg--RVj3Zek.wasm:wasm-function[9330]:0x9028b7
codeActions@https://play.ty.dev/assets/index-C7XW03DP.js:64:23094
provideCodeActions@https://play.ty.dev/assets/Editor-CINI16zY.js:1:7101
provideCodeActions@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:739:7711
C/z<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:135649
C@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:135603
_update/this._codeActionOracle.value</v<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:146660
b@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:11727
_update/this._codeActionOracle.value<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:144012
trigger@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:141115
_tryAutoTrigger/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:141340
cancelAndSet/this._token<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:15238
setTimeout handler*cancelAndSet@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:15207
_tryAutoTrigger@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:141317
g/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:141018
_deliver@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:910
_deliverQueue@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1001
fire@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1336
_attachModel/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:18484
_deliver@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:910
fire@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:71:1229
_emitOutgoingEvents@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:111:131434
endEmitViewEvents@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:111:132130
_withViewEventsCollector/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:5059
batchChanges@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:695:17190
_withViewEventsCollector@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:4943
setCursorStates@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:2163
runCoreEditorCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:93402
runEditorCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:93318
_runEditorCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:70190
h/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:69771
runCommand@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:60898
handler@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:660:59966
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
Fv/we<@https://play.ty.dev/assets/index-C7XW03DP.js:74:6961
Fv/<@https://play.ty.dev/assets/index-C7XW03DP.js:74:7256
br@https://play.ty.dev/assets/index-C7XW03DP.js:49:83576
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:98271
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:98253
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:99009
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:98253
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:98253
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
yg@https://play.ty.dev/assets/index-C7XW03DP.js:49:99009
an@https://play.ty.dev/assets/index-C7XW03DP.js:49:98157
    unexpectedErrorHandler errors.ts:26
    setTimeout handler*d/this.unexpectedErrorHandler errors.ts:20
    onUnexpectedExternalError errors.ts:47
    I errors.ts:64
    z codeAction.ts:143
    C codeAction.ts:121
    v codeActionModel.ts:350
    b async.ts:24
    value codeActionModel.ts:233
    trigger codeActionModel.ts:54
    _tryAutoTrigger codeActionModel.ts:66
    _token async.ts:443
    setTimeout handler*cancelAndSet async.ts:441
    _tryAutoTrigger codeActionModel.ts:65
    g codeActionModel.ts:49
    _deliver event.ts:1225
    _deliverQueue event.ts:1236
    fire event.ts:1260
    _attachModel codeEditorWidget.ts:1637
    _deliver event.ts:1225
    fire event.ts:1256
    _emitOutgoingEvents viewModelEventDispatcher.ts:64
    endEmitViewEvents viewModelEventDispatcher.ts:109
    _withViewEventsCollector viewModelImpl.ts:1111
    batchChanges codeEditorWidget.ts:1572
    _withViewEventsCollector viewModelImpl.ts:1106
    setCursorStates viewModelImpl.ts:1002
    runCoreEditorCommand coreCommands.ts:1898
    runEditorCommand coreCommands.ts:1894
    _runEditorCommand coreCommands.ts:341
    h coreCommands.ts:312
    runCommand editorExtensions.ts:228
    handler editorExtensions.ts:154
    invokeFunction instantiationService.ts:109
    executeCommand standaloneServices.ts:292
    _doDispatch abstractKeybindingService.ts:331
    _dispatch abstractKeybindingService.ts:186
    ft standaloneServices.ts:334
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
    we index-C7XW03DP.js:74
    Fv index-C7XW03DP.js:74
    br index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
    yg index-C7XW03DP.js:49
    an index-C7XW03DP.js:49
[errors.ts:26:12](https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min-maps/vs/editor/out-editor/vs/editor/file:/mnt/vss/_work/1/s/dependencies/vscode/out-editor-src/vs/base/common/errors.ts)
​```
```

</details>


### Version

playground (8ebecb2a8)

---

_Label `server` added by @AlexWaygood on 2025-12-03 21:01_

---

_Label `fatal` added by @AlexWaygood on 2025-12-03 21:02_

---

_Renamed from "Crash on hover of stringified recursive type hint" to "Crash on hover of stringified unknown type hint" by @MeGaGiGaGon on 2025-12-03 21:10_

---

_Added to milestone `Beta` by @carljm on 2025-12-03 21:44_

---

_Assigned to @Gankra by @carljm on 2025-12-03 21:45_

---

_Comment by @MichaReiser on 2025-12-03 21:50_

We should test if this only affects the playground 

---

_Comment by @Gankra on 2025-12-03 21:55_

I can reproduce this in vscode but not in hover tests...

---

_Comment by @Gankra on 2025-12-03 22:01_

Oh I wonder if quick-fixes is crashing

Edit: yep.

---

_Comment by @Gankra on 2025-12-03 22:10_

`create_suppression_fix` is handing the span of `x` to `Tokens::after`, but the the full token is `"x"` so the function panics as documented.

---

_Comment by @MichaReiser on 2025-12-03 22:15_

Hmm stringified annotations feel very fragile when it comes to fixes. Or fixes have to use the semantic model

---

_Closed by @sharkdp on 2025-12-04 08:11_

---
