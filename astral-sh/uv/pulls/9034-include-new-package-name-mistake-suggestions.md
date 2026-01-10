```yaml
number: 9034
title: Include new package name mistake suggestions
type: pull_request
state: open
author: nathanjmcdougall
labels: []
assignees: []
base: main
head: new-pkg-name-mistake-suggestions
created_at: 2024-11-11T23:58:33Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/9034
synced_at: 2026-01-10T11:10:34Z
```

# Include new package name mistake suggestions

---

_Pull request opened by @nathanjmcdougall on 2024-11-11 23:58_

## Summary

This list of known discrepancies between package vs. import package name is missing a few very popular packages:

- PIL
- GitPython
- PyYAML

I have added these and a number of others I am aware of. These projects are of varying popularity, so I would understand if you don't want to include all of these, lest it gets out of control. These are just some suggestions to start a conversation.

## Test Plan

No tests, since this is just adding to a JSON file. The list should be manually reviewed.


---

_@charliermarsh reviewed on 2024-11-12 02:24_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/suggestions.json`:9 on 2024-11-12 02:24_

What's this one?

---

_Comment by @charliermarsh on 2024-11-12 02:26_

Thank you! As-is, I think these are only shown when a build _failures_, and not when a resolution fails. So we might need to change that for this to take effect.

---

_@zanieb reviewed on 2024-11-12 03:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/suggestions.json`:9 on 2024-11-12 03:10_

Interesting

https://pymupdf.readthedocs.io/en/latest/module.html
https://github.com/kastman/fitz?tab=readme-ov-file

---

_Comment by @nathanjmcdougall on 2024-11-12 03:26_

Good point. There are a variety of ways things could go wrong:

- Mistake name doesn't exist on PyPI at all (e.g. `git`, `arcpy`)
- Mistake name does exist on PyPI but doesn't have a >0.0 release (e.g. `absl`)
- Mistake name does exist on PyPI and will build but it is a dummy package designed to avoid typosquatting (e.g. `bs4`)
- Mistake name does exist but release is so old that it will almost always fail (e.g. `pil`)

The last case should probably be the focus of this PR for now.

There's also the case of two, well used and maintained packages competing over the same import name, but I don't think we could do anything about that.

---

_@nathanjmcdougall reviewed on 2024-11-12 03:31_

---

_Review comment by @nathanjmcdougall on `crates/uv/src/commands/suggestions.json`:9 on 2024-11-12 03:31_

[See here](https://pymupdf.readthedocs.io/en/latest/tutorial.html#note-on-the-name-fitz)

This is an older thing that was changed at some point - in the past the import name `fitz` was the only one exposed by `pymupdf`, now it looks like they have moved away from that except as a fallback.

---
