```yaml
number: 21471
title: "[ty] fix ty playground initialization and vite optimization issues"
type: pull_request
state: merged
author: mtshiba
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: fix-playground-build-win
created_at: 2025-11-15T13:00:51Z
updated_at: 2025-11-25T11:27:36Z
url: https://github.com/astral-sh/ruff/pull/21471
synced_at: 2026-01-10T16:48:02Z
```

# [ty] fix ty playground initialization and vite optimization issues

---

_Pull request opened by @mtshiba on 2025-11-15 13:00_

## Summary

I tried to build the ty playground on my Windows machine and run it locally, but it didn't work. I got the following error:

```sh
$ npm start --workspace ty-playground

> ty-playground@0.0.0 prestart
> npm run dev:wasm


> ty-playground@0.0.0 dev:wasm
> wasm-pack build ../../crates/ty_wasm --dev --target web --out-dir ../../playground/ty/ty_wasm

[INFO]: Checking for the Wasm target...
[INFO]: Compiling to Wasm... 
Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.69s
[INFO]: Installing wasm-bindgen...
[INFO]: License key is set in Cargo.toml but no LICENSE file(s) were found; Please add the LICENSE file(s) to your project directory
[INFO]: :-) Done in 5.46s
[INFO]: :-) Your wasm pkg is ready to publish at ..\..\playground\ty\ty_wasm.

> ty-playground@0.0.0 start
> vite

[vite-plugin-static-copy] Error: No file was found to copy to C:\{omitted}\ruff\playground\node_modules\pyodide\*,!**/*.{md,html},!**/*.d.ts,!**/*.whl,!**/node_modules src.
[vite-plugin-static-copy] No items found.
```

Also, nothing is displayed when I access localhost:5173. The following error appears in the browser console:

```
ty_wasm.js?v=16eba020:2195 Uncaught TypeError: Cannot read properties of undefined (reading '__wbindgen_malloc')
at new Workspace (ty_wasm.js?v=16eba020:2195:51)
at Playground.tsx:33:27
```

This PR fixes the above issues.

First, for the first issue with vite-plugin-static-copy, I addressed it by changing backslashes `\` to slashes `/` in the path name. This is a common Windows-specific path handling issue.

For the other issue, I changed the `ty_wasm` package import from a relative path to a package name, and excluded `ty_wasm` from `optimizeDeps` in vite.config.js.
To be honest, I'm not familiar with vite, so I'm not sure why this worked, but I tried the workaround described here:

https://github.com/vitejs/vite/issues/8427

The above changes are for development purposes only and should not affect the content actually delivered.

## Test Plan

N/A


---

_Marked ready for review by @mtshiba on 2025-11-15 13:10_

---

_Label `playground` added by @AlexWaygood on 2025-11-15 13:15_

---

_Label `ty` added by @AlexWaygood on 2025-11-15 13:15_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-15 13:15_

---

_Review comment by @MichaReiser on `playground/ty/vite.config.ts`:26 on 2025-11-15 17:46_

This makes little sense to me, given that `path.join` uses the platform-specific path separator. Which path contains the `/` where windows expects a `\`?

---

_Review comment by @MichaReiser on `playground/ty/vite.config.ts`:19 on 2025-11-15 17:50_

We use vite for the production builds. Can you verify that the `vite build` reported sizes are the same before and after your changes

---

_@MichaReiser reviewed on 2025-11-15 17:50_

---

_@mtshiba reviewed on 2025-11-16 03:05_

---

_Review comment by @mtshiba on `playground/ty/vite.config.ts`:19 on 2025-11-16 03:05_

Sure. Build results on Ubuntu (@ WSL2):

main:
```sh
❯ dust ty/dist
 24K   ┌── Astral.png                │█                                                             │   0%
 20K   │ ┌── pyodide.mjs             │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 20K   │ ├── solidity-CVYD1GVc.js    │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 24K   │ ├── tsMode-De4b9PNo.js      │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 36K   │ ├── cssMode-cNRTyZYV.js     │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 36K   │ ├── htmlMode-BieFvPPN.js    │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 44K   │ ├── ffi.d.ts                │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 44K   │ ├── jsonMode-B6VJCXeF.js    │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 68K   │ ├── pyodide.d.ts            │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 84K   │ ├── pyodide.js.map          │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 84K   │ ├── pyodide.mjs.map         │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 92K   │ ├── codicon-B_Z2XQ3P.ttf    │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
112K   │ ├── pyodide-lock.json       │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
144K   │ ├── editor-BhPcksyq.css     │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
396K   │ ├── index-BeD-OYFt.js       │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   1%
1.0M   │ ├── pyodide.asm.js          │███░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   4%
2.3M   │ ├── python_stdlib.zip       │█████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   8%
3.5M   │ ├── editor.main-ClkAP-6o.js │████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │  12%
8.2M   │ ├── pyodide.asm.wasm        │██████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │  29%
 11M   │ ├── ty_wasm_bg-Bd9OBY4A.wasm│██████████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │  41%
 28M   ├─┴ assets                    │█████████████████████████████████████████████████████████████ │ 100%
 28M ┌─┴ dist                        │█████████████████████████████████████████████████████████████ │ 100%
```
#21471:
```sh
❯ dust ty/dist
 24K   ┌── Astral.png                │█                                                             │   0%
 20K   │ ┌── pyodide.mjs             │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 20K   │ ├── solidity-CVYD1GVc.js    │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 24K   │ ├── tsMode-Bzlyl_WG.js      │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 36K   │ ├── cssMode-BItooz1B.js     │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 36K   │ ├── htmlMode-XS2Ok6yb.js    │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 44K   │ ├── ffi.d.ts                │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 44K   │ ├── jsonMode-Bw4_M6_V.js    │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 68K   │ ├── pyodide.d.ts            │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 84K   │ ├── pyodide.js.map          │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 84K   │ ├── pyodide.mjs.map         │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
 92K   │ ├── codicon-B_Z2XQ3P.ttf    │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
112K   │ ├── pyodide-lock.json       │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
144K   │ ├── editor-BhPcksyq.css     │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   0%
396K   │ ├── index-B_J2zKKC.js       │█░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   1%
1.0M   │ ├── pyodide.asm.js          │███░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   4%
2.3M   │ ├── python_stdlib.zip       │█████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │   8%
3.5M   │ ├── editor.main-CF30gs3G.js │████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │  12%
8.2M   │ ├── pyodide.asm.wasm        │██████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │  29%
 11M   │ ├── ty_wasm_bg-C8Ofe_-K.wasm│██████████████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ │  41%
 28M   ├─┴ assets                    │█████████████████████████████████████████████████████████████ │ 100%
 28M ┌─┴ dist                        │█████████████████████████████████████████████████████████████ │ 100%
```

This result is consistent with what [the vite document](https://vite.dev/guide/dep-pre-bundling#:~:text=Dependency%20pre%2Dbundling%20only%20applies%20in%20development%20mode%2C%20and%20uses%20esbuild%20to%20convert%20dependencies%20to%20ESM.%20In%20production%20builds%2C%20%40rollup/plugin%2Dcommonjs%20is%20used%20instead.) states.

---

_@mtshiba reviewed on 2025-11-16 03:18_

---

_Review comment by @mtshiba on `playground/ty/vite.config.ts`:26 on 2025-11-16 03:18_

I found a statement about this limitation [here](https://github.com/sapphi-red/vite-plugin-static-copy#usage):

> If you are using Windows, make sure to use normalizePath after doing path.resolve or else. \ is a escape charactor in tinyglobby and you should use /.

According to this description, using `normalizePath` is a more robust approach, so I changed it to use that.

---

_@MichaReiser reviewed on 2025-11-16 07:50_

---

_Review comment by @MichaReiser on `playground/ty/vite.config.ts`:26 on 2025-11-16 07:50_

Right, this is a glob and not a path. 

I suggest then to call `normalizePath(pyodideDir)` (or normalize before assigning to `pyodideDir`) to scope the normalize exactly to where it's necessary.

---

_@MichaReiser approved on 2025-11-16 07:50_

---

_Review requested from @MichaReiser by @mtshiba on 2025-11-25 02:08_

---

_Merged by @MichaReiser on 2025-11-25 07:42_

---

_Closed by @MichaReiser on 2025-11-25 07:42_

---

_Branch deleted on 2025-11-25 11:27_

---
