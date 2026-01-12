```yaml
number: 1385
title: Upload WASM packages for each release
type: issue
state: closed
author: charliermarsh
labels:
  - playground
assignees: []
created_at: 2022-12-26T19:00:46Z
updated_at: 2024-07-18T20:21:03Z
url: https://github.com/astral-sh/ruff/issues/1385
synced_at: 2026-01-12T15:54:41Z
```

# Upload WASM packages for each release

---

_@charliermarsh_

_No description provided._

---

_Label `playground` added by @charliermarsh on 2022-12-26 19:01_

---

_Comment by @mattrunyon on 2024-05-16 00:03_

I was interested in uploading the wasm to npm and could contribute a PR to add this.

What would the desired npm package name be? `@astral-sh/ruff`?

---

_Comment by @MichaReiser on 2024-05-27 15:37_

Charlie created a new [`astral-sh`](https://www.npmjs.com/org/astral-sh) organisation. 

I would call it `@astral-sh/ruff-api` just to give us the option to publish the ruff binary to NPM in the future (we don't plan on doing this but you can never know). 

What's important to me is that we very clearly mark the package as experimental. I suspect significant API changes to the linter API with the work of red-knot (multifile analysis). 

@mattrunyon I suspect you need me to initialize an empty package to get started?

---

_Comment by @mattrunyon on 2024-05-28 16:43_

I don't think an empty package needs to be created.

The main question would be how does this get handled w/ your organization's versioning/changelog tool. It looks like the package version is not available to the Rust workspaces, but is updated in just the published files (I think via some internal tool?). Without knowing how to tell that tool to update the cargo file version field for `ruff_wasm`, it could just be a step in the CI job that replaces the version (e.g. with `sed`) before running `wasm-pack`. The cargo version is used in the generated `package.json`.

---

_Comment by @MichaReiser on 2024-05-29 08:24_

We use [rooster](https://github.com/zanieb/rooster/) for our release process and it can bump the version number for you. 

All you need to do is to list the `ruff_wasm/Cargo.toml` in the `version_files` section

https://github.com/astral-sh/ruff/blob/49a5a9ccc2b6b8f69bb64f87d3a915aff47eb02a/pyproject.toml#L107-L113

---

_Comment by @vshymanskyy on 2024-07-12 11:40_

I'm really interested in this. While working on ViperIDE for MicroPython ( https://viper-ide.org ) I integrated the `mpy-cross` compiler using WebAssembly and it does report syntax errors. But it's not nearly as cool as ruff :)
Being able to install ruff as an NPM package would simplify things a lot.

I'm using CodeMirror 6 editor and bundling it with Rollup.

---

_Comment by @vshymanskyy on 2024-07-13 17:41_

Just some feedback. It was really easy to integrate ruff, the Workspace API is simple and clear, bundling with Rollup works like a charm. **Kudos to the ruff team!**

If anyone needs a CodeMirror 6 linter, see https://github.com/vshymanskyy/ViperIDE/blob/main/src/editor.js#L203

![image](https://github.com/user-attachments/assets/f02d4290-1e5b-4875-b1e2-dff6526c328b)


---

_Closed by @MichaReiser on 2024-07-17 06:50_

---

_Comment by @dhruvmanila on 2024-07-18 18:32_

The first version of the packages are up!
* Node.js: https://www.npmjs.com/package/@astral-sh/ruff-wasm-nodejs
* Bundler: https://www.npmjs.com/package/@astral-sh/ruff-wasm-bundler
* Web: https://www.npmjs.com/package/@astral-sh/ruff-wasm-web

---

_Comment by @vshymanskyy on 2024-07-18 20:21_

Awesome, thank you

---
