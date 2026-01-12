```yaml
number: 12956
title: "Using Ruff on an Online Python Playground, `CompileError: WebAssembly.instantiate(): expected magic word 00 61 73 6d, found 3c 21 64 6f @+0`"
type: issue
state: closed
author: jymchng
labels:
  - question
assignees: []
created_at: 2024-08-17T16:42:43Z
updated_at: 2024-09-09T12:02:40Z
url: https://github.com/astral-sh/ruff/issues/12956
synced_at: 2026-01-12T15:54:52Z
```

# Using Ruff on an Online Python Playground, `CompileError: WebAssembly.instantiate(): expected magic word 00 61 73 6d, found 3c 21 64 6f @+0`

---

_@jymchng_

I have a not-yet-announced [Python Playground](https://shiny-chebakia-638038.netlify.app/).

Within it, I plan to have the [Format] button formatting the `main.py` codes shown on the Left Panel.

`vite.config.js`
```
// `ruffFormatPluginVite` attempts to copy the both files into the browser's assets.
const ruffFormatPluginVite = {
  name: "vite-plugin-ruff",
  generateBundle: async () => {
    const ruffSourceDir = "node_modules/@astral-sh/ruff-wasm-web";
    const assetsDir = "dist/assets";
    const files = [
      "ruff_wasm.js",
      "ruff_wasm_bg.wasm",
    ];
    for (const file of files) {
      await copyFile(join(ruffSourceDir, file), join(assetsDir, file));
    }
  },
}

export default defineConfig({
  optimizeDeps: { exclude: ["@astral-sh"] },
  plugins: [vue(), WindiCSS(), ruffFormatPluginVite], // pyodidePluginVite],
  define: {
    __API_URL__: JSON.stringify(apiURL),
    __PY_VER__: JSON.stringify(pyVer),
    __COMMIT__: JSON.stringify(commit),
  },
});
```

`ruff.format.js`

```
import init, { Workspace } from "@astral-sh/ruff-wasm-web";

await init();

export const workspace = new Workspace({
    'line-length': 88,
    'indent-width': 4,
    format: {
        'indent-style': 'space',
        'quote-style': 'double',
    },
    lint: {
        select: [
            'E4',
            'E7',
            'E9',
            'F'
        ],
    },
});
```

Here I simply follow the example [here](https://www.npmjs.com/package/@astral-sh/ruff-wasm-web) and export the `workspace`.

`Run.vue`
```
function formatCode() {
  // const errorMsg = "\`Format\` functionality is not yet implemented!";
  // toast.error(errorMsg);
  console.log("Format action triggered");
  store.files["main.py"] = workspace.format(store.files["main.py"])
  const formattedMessage = `Codes have been formatted!`;
  toast.success(formattedMessage);
}
```

Here in `Run.vue`, `store.files["main.py"]` is the Python source codes and it is formatted by `workspace` and assigned back to `store.files["main.py"]`.

This is the error I got when executing the `npm run dev`.

```
ruff.format.js:3 `WebAssembly.instantiateStreaming` failed because your server does not serve wasm with `application/wasm` MIME type. Falling back to `WebAssembly.instantiate` which is slower. Original error:
 TypeError: Failed to execute 'compile' on 'WebAssembly': Incorrect response MIME type. Expected 'application/wasm'.
__wbg_load @ @astral-sh_ruff-wasm-web.js?v=99fcae90:432
await in __wbg_load
__wbg_init @ @astral-sh_ruff-wasm-web.js?v=99fcae90:768
await in __wbg_init
(anonymous) @ ruff.format.js:3
Show 2 more frames
Show lessUnderstand this warning
:5173/#ewB9AA==:1 Uncaught CompileError: WebAssembly.instantiate(): expected magic word 00 61 73 6d, found 3c 21 64 6f @+0
```

Any idea what's going on?

Thank you.

---

_Label `question` added by @MichaReiser on 2024-08-18 07:49_

---

_Comment by @MichaReiser on 2024-08-18 07:51_

This looks neat!

Hmm, not sure. I would try to fix 

>  TypeError: Failed to execute 'compile' on 'WebAssembly': Incorrect response MIME type. Expected 'application/wasm'.

first by double checking what mime type your server returns for the WASM file. I somewhat suspect that the WASM file might be missing and that your server returns a HTML error page. 

---

_Comment by @jymchng on 2024-08-18 08:02_

Thank you for your prompt reply.

```
// `ruffFormatPluginVite` attempts to copy the both files into the browser's assets.
const ruffFormatPluginVite = {
  name: "vite-plugin-ruff",
  generateBundle: async () => {
    const ruffSourceDir = "node_modules/@astral-sh/ruff-wasm-web";
    const assetsDir = "dist/assets";
    const files = [
      "ruff_wasm.js",
      "ruff_wasm_bg.wasm",
    ];
    for (const file of files) {
      await copyFile(join(ruffSourceDir, file), join(assetsDir, file));
    }
  },
}

export default defineConfig({
  optimizeDeps: { exclude: ["@astral-sh"] },
  plugins: [vue(), WindiCSS(), ruffFormatPluginVite], // pyodidePluginVite],
  define: {
    __API_URL__: JSON.stringify(apiURL),
    __PY_VER__: JSON.stringify(pyVer),
    __COMMIT__: JSON.stringify(commit),
  },
});
```

I actually copied this from `pyodide` and that is how they taught me on how to get the `.wasm` binary and `.js` shim to be copied into `dist/assets` and be made available at runtime within the console.

When I investigate the Sources tab of Chrome, I do realize that they are actually not copied over.

Perhaps, is there a working example, aside from the Ruff playground, to illustrate how one can deploy the Ruff wasm on the frontend?

Thank you.

---

_Comment by @MichaReiser on 2024-08-18 08:06_

> Perhaps, is there a working example, aside from the Ruff playground, to illustrate how one can deploy the Ruff wasm on the frontend?

Not that I'm aware of. I would expect vite to automatically pick up the dependencies when importing the wasm package. 

---

_Comment by @jymchng on 2024-08-18 08:21_

Yeah I saw the wasm-build command actually calls wasm-pack and dump the built artefacts directly into `src`. I guess I shall try the same later. Will update here on how it goes.

---

_Comment by @MichaReiser on 2024-09-09 12:02_

Let me know if you're still facing any issues. I close this for now.

---

_Closed by @MichaReiser on 2024-09-09 12:02_

---
