---
number: 12994
title: "Add `--torch-backend=auto` Flag to `uv add torch`"
type: issue
state: open
author: NourEldin-Osama
labels:
  - enhancement
assignees: []
created_at: 2025-04-21T01:51:34Z
updated_at: 2025-11-03T13:28:28Z
url: https://github.com/astral-sh/uv/issues/12994
synced_at: 2026-01-10T01:25:27Z
---

# Add `--torch-backend=auto` Flag to `uv add torch`

---

_Issue opened by @NourEldin-Osama on 2025-04-21 01:51_

### Summary

Currently, `uv pip` can automatically install the latest compatible CUDA version of PyTorch via:

1- CLI
`uv pip install "torch" --torch-backend=auto --preview`

2- [pyproject.toml](https://docs.astral.sh/uv/reference/settings/#__tabbed_96_1)
```pyproject.toml
[tool.uv.pip]
torch-backend = "auto"
```

It would be great if the same “auto” option were available when adding the `torch` package directly with `uv add`.

Proposal:
1- CLI
`uv add torch --torch-backend=auto`

2- pyproject.toml
```pyproject.toml
[tool.uv]
torch-backend = "auto"
```

Thanks for this amazing tool!

### Example

_No response_

---

_Label `enhancement` added by @NourEldin-Osama on 2025-04-21 01:51_

---

_Comment by @FishAlchemist on 2025-04-21 03:06_

This is the PR that initially implemented ``torch-backend=auto`` in ``uv pip install``.
* https://github.com/astral-sh/uv/pull/12070
> Right now, this is only implemented in the uv pip CLI, since it doesn't quite fit into the lockfile APIs given that it relies on feature detection on the currently-running machine.

So, I think the biggest issue with implementing this feature you're proposing is probably how to represent it in the lockfile, as lockfiles typically don't include dynamic detection content.

---

_Comment by @NourEldin-Osama on 2025-04-21 15:41_

@FishAlchemist

Any ideas on how to add this feature and address the challenge of representing dynamic, machine-specific detection (like `--torch-backend=auto`) in a static lockfile?

Has anyone considered possible approaches or workarounds for this limitation?

I’d like to use `uv add` with `torch-backend=auto` so that anyone installing my project gets the appropriate torch version for their CUDA setup.

---

_Comment by @peacefulotter on 2025-08-06 06:31_

This would be great! 

---

_Comment by @intexcor on 2025-10-22 08:58_

You can use wheelnext

---

_Comment by @muhammad-fiaz on 2025-10-30 13:27_

this feature throwing me an error, when i add[ this feature](https://docs.astral.sh/uv/reference/settings/#pip_torch-backend) to `[tool.uv.pip]`

```
{"torch-backend":"auto","generate-hashes":true,"no-strip-markers":true,"output-file":"requirements.txt","prerelease":"allow","universal":true,"upgrade":true,"verify-hashes":true} is not valid under any of the schemas listed in the 'anyOf' keywordEven Better TOML
```
this is actually what vscode's warning shows!

---

_Referenced in [astral-sh/uv#16545](../../astral-sh/uv/issues/16545.md) on 2025-11-01 05:29_

---

_Comment by @FishAlchemist on 2025-11-03 13:28_

@muhammad-fiaz 
I think you can open a separate issue to handle it, as it can easily be overlooked in the comments. As for your question, I have opened an issue (https://github.com/astral-sh/uv/issues/16545) for you, and I think you can test whether it is working correctly.

---
