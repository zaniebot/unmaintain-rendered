```yaml
number: 10008
title: "Emit index urls in `uv export` same way as `uv pip compile --emit-index-url`"
type: issue
state: open
author: Fogapod
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-12-18T18:10:18Z
updated_at: 2026-01-20T00:06:36Z
url: https://github.com/astral-sh/uv/issues/10008
synced_at: 2026-01-20T00:50:23Z
```

# Emit index urls in `uv export` same way as `uv pip compile --emit-index-url`

---

_@Fogapod_

I am exporting `uv.lock` to `requirements.txt` for bazel.
I have `torch` index overrides which aren't exported and produce impossible to insall requirements file:
```toml
[tool.uv.sources]
torch = [
    { index = "pytorch-cpu" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```
results in:
```
torch==2.5.1+cpu \
    --hash=sha256:4856f9d6925121d13c2df07aa7580b767f449dfe71ae5acde9c27535d5da4840 \
    --hash=sha256:a6b720410350765d3d77c01a5ce098a6c45af446284e45e87a98b8a16e7d564d
```

Looks like lock file contains index info so this should be possible:
```toml
[[package]]
name = "torch"
version = "2.5.1+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
```

This probably isn't as simple as writing all indexes to requirements file because ordering matters. Maybe there could be a way to manually give it indexes instead? 

---

_Comment by @ant0nsc on 2024-12-20 08:09_

+1 on this issue. We have the exactly same problem with multiple indexes that contain different flavours of `torch`. It would be great to be able to export a requirements file that contains the full URL where `uv` is taking the package from. 
The format of a `requirements.txt` file does support supplying the full URL: https://pip.pypa.io/en/stable/reference/requirements-file-format/

---

_Comment by @zanieb on 2025-01-07 19:05_

I don't think this is trivially the same as `uv pip compile` but would be interesting.

---

_Label `needs-design` added by @zanieb on 2025-01-07 19:05_

---

_Label `enhancement` added by @zanieb on 2025-01-07 19:05_

---

_Renamed from "feature request: emit index urls in `uv export` same way as `uv pip compile --emit-index-url`" to "Emit index urls in `uv export` same way as `uv pip compile --emit-index-url`" by @zanieb on 2025-01-07 19:05_

---

_Comment by @zanieb on 2025-04-02 14:20_

As a note, because this is spread out across a few threads: We can't support this because our `--index` features are not representable in `requirements.txt` files. For example, there is no way to represent index pinning or uv's index strategies. It's possible there's something we could do here, but it's an open problem.

---

_Comment by @liiight on 2025-04-02 19:46_

It's understandable why `--index` feature wouldn't be implicitly representable in `requirements.txt` but that raises two questions, but if that's the case should `uv export` appear to enable the same `--index`, `--extra-index-url` or `--index-url` flags?
At the very least this is a bit confusing.

I would like to offer an alternative though. Like you said, index pinning is not an option in `requirement.txt`, but maybe a flag named `--allow-global-indexing` could be added to `uv export` which would allow some sort of "backwards" compatible exporting when required?

---

_Comment by @zanieb on 2025-04-02 20:48_

> if that's the case should uv export appear to enable the same --index, --extra-index-url or --index-url flags?

These flags are available on all commands that may need to perform a resolution.

> maybe a flag named --allow-global-indexing could be added to uv export which would allow some sort of "backwards" compatible exporting when required?

I still am not sure there's a way we can represent the desired index priority semantics.

I think this may broadly be fixed by PEP 517?


---

_Comment by @liiight on 2025-04-04 04:30_

> These flags are available on all commands that may need to perform a resolution.

I understand that these flags are available by default globally, but the fact that they appear to be available, yet have no affect is confusing. Maybe consider hiding them somehow from commands that they have no effect on.

> I still am not sure there's a way we can represent the desired index priority semantics.

I imagine you're right and seeing that particular my use case is pretty straightforward, my proposed solution for a "global" flag is probably way too naive to represent many real world scenarios.

Maybe the best course of action at this time is to simply display a warning that using export will not respect the indexing priorities that is respected when using sync, since that is simply not possible, which will help reduce the element of surprise.

If and when enabling granular indexing is possible, then I imagine the mechanics of how to do so would be much more apparent and obvious



---

_Comment by @Kugeleis on 2025-04-17 10:45_

Is there a workaround to generate a requirements.txt with entries from [tool.uv.sources]?

---

_Comment by @anton-slashm on 2025-10-24 10:13_

> Is there a workaround to generate a requirements.txt with entries from [tool.uv.sources]?

Try `uv export | uv pip install --extra-index-url https://abc.com/ -r dev/stdin`

Worked for me.

---

_Comment by @debu-sinha on 2026-01-20 00:06_

I'm working on UV integration for MLflow ([mlflow#12478](https://github.com/mlflow/mlflow/issues/12478)) and hit this limitation when exporting dependencies for model artifacts.

I'd like to volunteer a design doc for this feature. Based on the discussion here and related issues, the key challenges seem to be:

1. **Index ordering/strategy** (`first-index` vs `unsafe-best-match`) has no pip equivalent
2. **Per-package index pinning** (via `[tool.uv.sources]`) not representable in requirements.txt

**Proposed approach:**
- Emit `--index-url` for default index (if non-PyPI)
- Emit `--extra-index-url` for additional indexes
- Add warning comment when per-package pinning detected (limitation)
- New flag: `--emit-index-url` (matching `uv pip compile` behavior)

The `uv.lock` file already contains `source = { registry = "..." }` for each package, so the data is available.

Would a design doc exploring options and trade-offs be helpful? Happy to draft one if maintainers are open to contributions here.

---
