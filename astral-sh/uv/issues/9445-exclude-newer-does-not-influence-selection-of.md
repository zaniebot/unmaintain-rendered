---
number: 9445
title: "`exclude-newer` does not influence selection of Python interpreter, should it?"
type: issue
state: open
author: manzt
labels:
  - great writeup
assignees: []
created_at: 2024-11-26T15:58:52Z
updated_at: 2024-11-26T20:11:35Z
url: https://github.com/astral-sh/uv/issues/9445
synced_at: 2026-01-10T01:24:41Z
---

# `exclude-newer` does not influence selection of Python interpreter, should it?

---

_Issue opened by @manzt on 2024-11-26 15:58_

Context:
- Prior discussion on [uv Discord](https://discord.com/channels/1039017663004942429/1207998321562619954/1306363222588129392)
- Some confusion about behavior on [napari zulip](https://napari.zulipchat.com/#narrow/channel/212875-general/topic/uv.20.2B.20napari.3A.20run.20scripts.20from.20url/near/484540519)

## Motivation

Self-contained scripts with inline metadata and `exclude-newer` can fail over time when there is a new release of Python, and there are no wheels. For example:

```py
uv init --script foo.py --python=3.8
uv add --script foo.py numpy
uvx juv stamp foo.py --date=2020-01-01
uv run foo.py
```

This fails because uv on my machine defaults to Python 3.13. There is no wheel for 3.13, so it tries to build the binary but fails because numpy build from early 2020 depends on distutils, which was removed in Python 3.12. I can of course get the script to work with `--python=3.12`.

## Expected behavior

My naive expectation when using `uv  run` with `exclude-newer` would also select an interpreter that respects the upper bound set by the timestamp. However, I think this is more that I'm starting to assume certain properties about `uv run` with `exclude-newer` rather than what `exclude-newer` is specifically intended for (restricting package resolution by time).

## Considerations

As @zanieb mentioned,

> It seems pretty dubious for us to extend it to interpreters since you'd probably still want the latest patch version for whichever minor? I can see how this would be surprising though.

Which I had not considered, and agree does seem questionable to extend the semantics in this way.

Right now, the best solution I've found to make the script self-contained is to set an upper bound on the interpreter through `requires-python` field on a new script, but bringing life back old scripts with `exclude-newer` right now is a bit of "whack-a-mole" situtation to find the right python version. Also, that "pinning" with `requries-python` is coupled (implicitly) to what I've set in `exclude-newer`. 

## Proposed improvements

It seems like there is some potential to improve the user experience at a minimum. For example, like when the requested interpreter is outside the scripts Python requirement. e.g.,

> Warning: The requested interpreter resolved to Python 3.X.X, which exceeds the upper bound set by exclude-newer (Python 3.X.X). You may want to re-run with the latest patch release for Python 3.X (i.e., --python=3.X).

Alternatively, perhaps there is a way to achieve the expected behavior automatically.




---

_Comment by @zanieb on 2024-11-26 17:02_

Thanks for opening this!

I think doing _something_ here is reasonable, a first step is to add Python release date information somewhere — if someone is interested in doing so let's talk about the best place for it to go and how we'll maintain it.

---

_Comment by @manzt on 2024-11-26 17:18_

Great! Yes, I was mostly hoping to start a discussion. Perhaps [jwooder/pyversion-info-data](https://github.com/jwodder/pyversion-info-data) could be a starting point. cc: @jwodder

---

_Comment by @zanieb on 2024-11-26 17:25_

Oh that's great, yeah we can probably just download that file and convert it to Rust in the same as we do for Python download metadata.

---

_Comment by @manzt on 2024-11-26 17:27_

I’d be happy to make a PR to do so.

---

_Comment by @notatallshaw-gts on 2024-11-26 17:46_

One thing that concerns me here is when I am using `exclude-newer` with CPython pre-releases, these versions were released before the official CPython release date, and popular packages like numpy release wheels for prerelease versions of CPython, e.g. https://pypi.org/project/numpy/2.1.1/#files.

---

_Label `great writeup` added by @zanieb on 2024-11-26 17:51_

---

_Referenced in [manzt/juv#74](../../manzt/juv/issues/74.md) on 2025-01-27 20:25_

---

_Referenced in [marimo-team/marimo#5520](../../marimo-team/marimo/pulls/5520.md) on 2025-07-03 00:36_

---
