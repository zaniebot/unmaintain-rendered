```yaml
number: 17329
title: "[red-knot] Playground crashes sometimes when clicking reset"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - playground
  - ty
assignees: []
created_at: 2025-04-10T09:39:19Z
updated_at: 2025-04-10T12:40:43Z
url: https://github.com/astral-sh/ruff/issues/17329
synced_at: 2026-01-10T11:09:58Z
```

# [red-knot] Playground crashes sometimes when clicking reset

---

_Issue opened by @MichaReiser on 2025-04-10 09:39_

### Summary

This seems related to the addition of inlay hints. I suspect it's a timing issue where the editor requests an inlay hint after we closed the open file

To reproduce: Click `Reset` a couple of times. Two times is normally enough


```
Uncaught Error: Attempt to use a moved value

inlayHints@http://localhost:5173/red_knot_wasm/red_knot_wasm.js:1238:19
provideInlayHints@http://localhost:5173/src/Editor/Editor.tsx?t=1744277842792:129:38
create/l</<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:616:66210
create/l<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:616:66176
create@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:616:66167
_update/j<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:722:107791
doRun@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:16421
onTimeout@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:16400
setTimeout handler*schedule@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:16211
_update@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:722:108469
F@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:722:106519
_createInstance@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:2108
createInstance@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:1533
_instantiateById@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:601:57816
_instantiateSome@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:601:57426
initialize/<@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:601:56496
requestIdleCallback handler*e._runWhenIdle@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:16804
w@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:122:81399
initialize@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:601:56442
z@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:693:15926
x@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:738:6922
W@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:738:9828
_createInstance@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:2108
createInstance@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:627:1533
A@https://cdn.jsdelivr.net/npm/monaco-editor@0.52.2/min/vs/editor/editor.main.js:739:13847
Ve/X<@http://localhost:5173/node_modules/.vite/deps/@monaco-editor_react.js?v=401609e3:659:65
Ve/<@http://localhost:5173/node_modules/.vite/deps/@monaco-editor_react.js?v=401609e3:665:17
react-stack-bottom-frame@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:16496:20
runWithFiberInDEV@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:980:18
commitHookEffectListMount@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:8021:122
commitHookPassiveMountEffects@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:8079:60
commitPassiveMountOnFiber@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9436:29
recursivelyTraversePassiveMountEffects@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9417:38
commitPassiveMountOnFiber@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9430:51
recursivelyTraversePassiveMountEffects@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9417:38
commitPassiveMountOnFiber@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9519:51
recursivelyTraversePassiveMountEffects@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9417:38
commitPassiveMountOnFiber@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9430:51
recursivelyTraversePassiveMountEffects@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9417:38
commitPassiveMountOnFiber@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9430:51
recursivelyTraversePassiveMountEffects@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9417:38
commitPassiveMountOnFiber@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9519:51
recursivelyTraversePassiveMountEffects@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9417:38
commitPassiveMountOnFiber@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9519:51
recursivelyTraversePassiveMountEffects@http://localhost:5173/node_modules/.vite/deps/react-dom_client.js?v=401609e3:9417:38
    unexpectedErrorHandler errors.ts:26
    setTimeout handler*d/this.unexpectedErrorHandler errors.ts:20
    onUnexpectedExternalError errors.ts:47
    I errors.ts:64
    l inlayHints.ts:84
    l inlayHints.ts:77
    create inlayHints.ts:77
    j inlayHintsController.ts:215
    doRun async.ts:559
    onTimeout async.ts:554
    setTimeout handler*schedule async.ts:533
    _update inlayHintsController.ts:250
    F inlayHintsController.ts:136
    _createInstance instantiationService.ts:162
    createInstance instantiationService.ts:128
    _instantiateById codeEditorContributions.ts:149
    _instantiateSome codeEditorContributions.ts:122
    initialize codeEditorContributions.ts:61
    requestIdleCallback handler*e._runWhenIdle async.ts:626
    w dom.ts:239
    initialize codeEditorContributions.ts:60
    z codeEditorWidget.ts:309
    x standaloneCodeEditor.ts:286
    W standaloneCodeEditor.ts:442
    _createInstance instantiationService.ts:162
    createInstance instantiationService.ts:128
    A standaloneEditor.ts:50
    X Editor.tsx:159
    Ve Editor.tsx:201
    React 18
```

---

_Label `playground` added by @MichaReiser on 2025-04-10 09:39_

---

_Label `red-knot` added by @MichaReiser on 2025-04-10 09:39_

---

_Label `bug` added by @AlexWaygood on 2025-04-10 10:46_

---

_Closed by @MichaReiser on 2025-04-10 12:40_

---
