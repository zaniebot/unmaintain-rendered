```yaml
number: 17743
title: "Allow passing a virtual environment to `ruff analyze graph`"
type: pull_request
state: merged
author: ntBre
labels:
  - analyze
assignees: []
merged: true
base: main
head: brent/ruff-analyze-workspace
created_at: 2025-04-30T17:26:28Z
updated_at: 2025-05-01T15:29:54Z
url: https://github.com/astral-sh/ruff/pull/17743
synced_at: 2026-01-12T15:56:05Z
```

# Allow passing a virtual environment to `ruff analyze graph`

---

_@ntBre_

Summary
--

Fixes #16598 by adding the `--python` flag to `ruff analyze graph`, which adds a `PythonPath` to the `SearchPathSettings` for module resolution. For the [albatross-virtual-workspace] example from the uv repo, this updates the output from the initial issue:

```shell
> ruff analyze graph packages/albatross
{
  "packages/albatross/check_installed_albatross.py": [
    "packages/albatross/src/albatross/__init__.py"
  ],
  "packages/albatross/src/albatross/__init__.py": []
}
```

To include both the the workspace `bird_feeder` import _and_ the third-party `tqdm` import in the output:

```shell
> myruff analyze graph packages/albatross --python .venv
{
  "packages/albatross/check_installed_albatross.py": [
    "packages/albatross/src/albatross/__init__.py"
  ],
  "packages/albatross/src/albatross/__init__.py": [
    ".venv/lib/python3.12/site-packages/tqdm/__init__.py",
    "packages/bird-feeder/src/bird_feeder/__init__.py"
  ]
}
```

Note the hash in the uv link! I was temporarily very confused why my local tests were showing an `iniconfig` import instead of `tqdm` until I realized that the example has been updated on the uv main branch, which I had locally.

Test Plan
--

A new integration test with a stripped down venv based on the `albatross` example.

[albatross-virtual-workspace]: https://github.com/astral-sh/uv/tree/aa629c4a54c31d6132ab1655b90dd7542c17d120/scripts/workspaces/albatross-virtual-workspace


---

_Label `analyze` added by @ntBre on 2025-04-30 17:26_

---

_Comment by @github-actions[bot] on 2025-04-30 17:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-04-30 17:33_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-30 17:33_

---

_@MichaReiser approved on 2025-04-30 17:37_

Neat!

Can you test what happens if you provide an invalid path?

---

_Comment by @ntBre on 2025-04-30 18:55_

Ah good idea, I ran into a couple of these while trying to set up the working test. Is this what you expected?

```
----- stderr -----                                                                                                     
ruff failed                                                                                                            
  Cause: Invalid search path settings                                                                                  
  Cause: Failed to discover the site-packages directory: Invalid `--python` argument: `none` could not be canonicalized
```

I think that looks pretty reasonable. Something like ``venv dir `none` does not exist`` could possibly be nicer, but I think the `canonicalized` message is bubbling up from red-knot. I could try to check manually a bit earlier if we wanted, though.

It's also important that the flag name matches red-knot, which I'm guessing you were also thinking about here!

---

_Comment by @MichaReiser on 2025-05-01 06:07_

I mainly wanted to make sure that ruff doesn't panic (I wasn't sure if there are any unwraps). I assume `none` was what you provided on the CLI. This looks good.

> It's also important that the flag name matches red-knot, which I'm guessing you were also thinking about here!

You give me too much credit 😅 

---

_Merged by @ntBre on 2025-05-01 15:29_

---

_Closed by @ntBre on 2025-05-01 15:29_

---

_Branch deleted on 2025-05-01 15:29_

---
