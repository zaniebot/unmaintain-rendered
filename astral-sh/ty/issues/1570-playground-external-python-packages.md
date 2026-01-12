```yaml
number: 1570
title: "[playground] External Python packages"
type: issue
state: open
author: mtshiba
labels:
  - wish
  - playground
assignees: []
created_at: 2025-11-15T14:01:02Z
updated_at: 2025-11-19T16:59:37Z
url: https://github.com/astral-sh/ty/issues/1570
synced_at: 2026-01-12T15:54:25Z
```

# [playground] External Python packages

---

_@mtshiba_

Being able to use external Python packages in the ty playground would be very useful for things like bug reports.

The ty playground uses pyodide to run scripts. pyodide has a lightweight package manager called micropip, which I think we can leverage. Installed packages appear to be accessible via `pyodide.FS`.

https://micropip.pyodide.org/en/stable/project/api.html#module-micropip

```js
await pyodide.loadPackage("micropip");
const micropip = pyodide.pyimport("micropip");
await micropip.install('snowballstemmer');
```

It seems that micropip does not have as full-featured package management as pip/uv, but being able to download Python code would be sufficient.

The interface I imagine would allow dependencies to be specified by adding a "dependencies" section to ty.json, or by uploading a uv.lock file. Of course, micropip doesn't understand uv.lock, but dependencies can be specified via a pyodide-lock.json, a pyodide-specific lock file. In other words, all we need to do is implement a conversion from uv.lock to pyodide-lock.json.

---

_Label `playground` added by @AlexWaygood on 2025-11-15 14:09_

---

_Comment by @AlexWaygood on 2025-11-15 14:10_

This would indeed be a very cool and useful feature. https://mypy.app/, which is a pyodide-based alternative mypy playground, has this feature -- the source code for the playground is at https://github.com/emmatyping/mypy.app.

---

_Label `wish` added by @AlexWaygood on 2025-11-15 14:11_

---

_Comment by @silamon on 2025-11-19 16:59_

The source code kind of suggest that ty will need to be published as a pyodide wheel to make this all work. Importing a package into the playground is not a big thing to add, but making the imports of that package visible to ty is not straightforward. 

---
