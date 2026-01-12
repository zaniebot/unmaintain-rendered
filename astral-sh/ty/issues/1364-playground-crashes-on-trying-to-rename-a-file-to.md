```yaml
number: 1364
title: Playground crashes on trying to rename a file to nothing
type: issue
state: open
author: MeGaGiGaGon
labels:
  - help wanted
  - playground
  - fatal
assignees: []
created_at: 2025-10-15T22:59:16Z
updated_at: 2025-11-14T08:31:44Z
url: https://github.com/astral-sh/ty/issues/1364
synced_at: 2026-01-12T15:54:25Z
```

# Playground crashes on trying to rename a file to nothing

---

_@MeGaGiGaGon_

### Summary

See attached video, the process is
1. Delete a file's name
2. Press enter

The playground will error in the console and be in an unstable state, where then navigating from and back to that file crashes the playground.

While doing this, the playground will first error with `Uncaught Error: Is a directory` and then crash with `Uncaught Error: null pointer passed to rust`

Edit: I did some more testing, and this same crash also happens if you try renaming the file to `.` or `..` or similar things (ie, `./a` works but `./..` crashes)

https://github.com/user-attachments/assets/ccaf02f1-e416-41e2-bd41-f602a3e8bd87

<details>
<summary>Full console errors when causing the crash</summary>

Error on trying to rename the file to nothing:
```
index-FQR4vDKH.js:64 Uncaught Error: Is a directory
    at Bp.l.wbg.__wbg_new_97ddeb994a38bb69 (index-FQR4vDKH.js:64:25749)
    at ty_wasm_bg-Uf3CeSIQ.wasm:0x81c9e1
    at ty_wasm_bg-Uf3CeSIQ.wasm:0x764aaa
    at ty_wasm_bg-Uf3CeSIQ.wasm:0x7e18a4
    at go.openFile (index-FQR4vDKH.js:64:18602)
    at ne (index-FQR4vDKH.js:87:7690)
    at le (index-FQR4vDKH.js:87:3379)
    at onRenamed (index-FQR4vDKH.js:76:63643)
    at h (index-FQR4vDKH.js:76:64469)
    at onBlur (index-FQR4vDKH.js:76:65036)
Bp.l.wbg.__wbg_new_97ddeb994a38bb69 @ index-FQR4vDKH.js:64
$func10208 @ ty_wasm_bg-Uf3CeSIQ.wasm:0x81c9e1
$func5269 @ ty_wasm_bg-Uf3CeSIQ.wasm:0x764aaa
$workspace_openFile @ ty_wasm_bg-Uf3CeSIQ.wasm:0x7e18a4
openFile @ index-FQR4vDKH.js:64
ne @ index-FQR4vDKH.js:87
le @ index-FQR4vDKH.js:87
onRenamed @ index-FQR4vDKH.js:76
h @ index-FQR4vDKH.js:76
onBlur @ index-FQR4vDKH.js:76
Gg @ index-FQR4vDKH.js:49
(anonymous) @ index-FQR4vDKH.js:49
Kc @ index-FQR4vDKH.js:49
As @ index-FQR4vDKH.js:49
Us @ index-FQR4vDKH.js:50
M_ @ index-FQR4vDKH.js:50
onKeyDown @ index-FQR4vDKH.js:76
Gg @ index-FQR4vDKH.js:49
(anonymous) @ index-FQR4vDKH.js:49
Kc @ index-FQR4vDKH.js:49
As @ index-FQR4vDKH.js:49
Us @ index-FQR4vDKH.js:50
M_ @ index-FQR4vDKH.js:50
```

Error on playground crash:
```
index-FQR4vDKH.js:64 Uncaught Error: null pointer passed to rust
    at Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf (index-FQR4vDKH.js:64:29195)
    at ty_wasm_bg-Uf3CeSIQ.wasm:0x819901
    at ty_wasm_bg-Uf3CeSIQ.wasm:0x81990e
    at ty_wasm_bg-Uf3CeSIQ.wasm:0x805f9b
    at Ge.path (index-FQR4vDKH.js:64:6385)
    at Xv (index-FQR4vDKH.js:76:65256)
    at index-FQR4vDKH.js:87:5887
    at Object.yd [as useMemo] (index-FQR4vDKH.js:49:42702)
    at ge.useMemo (index-FQR4vDKH.js:18:7241)
    at h1 (index-FQR4vDKH.js:87:5785)
Bp.l.wbg.__wbg_wbindgenthrow_4c11a24fca429ccf @ index-FQR4vDKH.js:64
$func10107 @ ty_wasm_bg-Uf3CeSIQ.wasm:0x819901
$func10108 @ ty_wasm_bg-Uf3CeSIQ.wasm:0x81990e
$filehandle_path @ ty_wasm_bg-Uf3CeSIQ.wasm:0x805f9b
path @ index-FQR4vDKH.js:64
Xv @ index-FQR4vDKH.js:76
(anonymous) @ index-FQR4vDKH.js:87
yd @ index-FQR4vDKH.js:49
ge.useMemo @ index-FQR4vDKH.js:18
h1 @ index-FQR4vDKH.js:87
g1 @ index-FQR4vDKH.js:87
Ru @ index-FQR4vDKH.js:49
Fu @ index-FQR4vDKH.js:49
Pd @ index-FQR4vDKH.js:49
Tg @ index-FQR4vDKH.js:49
Qm @ index-FQR4vDKH.js:49
ms @ index-FQR4vDKH.js:49
vg @ index-FQR4vDKH.js:49
Ug @ index-FQR4vDKH.js:49
dl @ index-FQR4vDKH.js:49
Hg @ index-FQR4vDKH.js:49
(anonymous) @ index-FQR4vDKH.js:49
```

</details>

### Version

Playground (73520e4ac)

---

_Label `playground` added by @AlexWaygood on 2025-10-15 23:01_

---

_Label `fatal` added by @AlexWaygood on 2025-10-15 23:01_

---

_Label `help wanted` added by @MichaReiser on 2025-11-14 08:31_

---
