```yaml
number: 11353
title: "Improve the `update_schemastore` script"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: schemastore-update-fixes
created_at: 2024-05-09T17:54:49Z
updated_at: 2024-05-13T17:18:09Z
url: https://github.com/astral-sh/ruff/pull/11353
synced_at: 2026-01-10T22:05:26Z
```

# Improve the `update_schemastore` script

---

_Pull request opened by @AlexWaygood on 2024-05-09 17:54_

## Summary

- Update the script so that it works over https
- Use a shallow clone so that it doesn't take so long

## Test Plan

Before these changes, the script failed for me. Now, it works: https://github.com/SchemaStore/schemastore/pull/3772.

---

_Review requested from @zanieb by @AlexWaygood on 2024-05-09 17:54_

---

_Comment by @github-actions[bot] on 2024-05-09 18:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@zanieb reviewed on 2024-05-09 18:54_

---

_Review comment by @zanieb on `scripts/update_schemastore.py`:16 on 2024-05-09 18:54_

You can't just change it, it'll break for ssh authentication users i.e. Charlie haha

---

_@AlexWaygood reviewed on 2024-05-09 18:57_

---

_Review comment by @AlexWaygood on `scripts/update_schemastore.py`:16 on 2024-05-09 18:57_

Oof yeah, right

---

_Converted to draft by @AlexWaygood on 2024-05-09 18:57_

---

_@konstin reviewed on 2024-05-09 21:15_

---

_Review comment by @konstin on `scripts/update_schemastore.py`:16 on 2024-05-09 21:15_

I had the same idea initially :D https://github.com/astral-sh/ruff/pull/5322

---

_@konstin reviewed on 2024-05-09 21:16_

---

_Review comment by @konstin on `scripts/update_schemastore.py`:26 on 2024-05-09 21:16_

Can we still do commits with this? (I'm not familiar enough with the git internals)

---

_Label `internal` added by @AlexWaygood on 2024-05-09 21:49_

---

_@zanieb reviewed on 2024-05-09 22:01_

---

_Review comment by @zanieb on `scripts/update_schemastore.py`:16 on 2024-05-09 22:01_

A simple `--proto ssh|https` flag seems sufficient?

---

_@AlexWaygood reviewed on 2024-05-13 14:08_

---

_Review comment by @AlexWaygood on `scripts/update_schemastore.py`:26 on 2024-05-13 14:08_

Yup, I checked locally. Making a new commit and creating a PR from that commit both still work fine even with a shallow clone.

---

_@AlexWaygood reviewed on 2024-05-13 15:07_

---

_Review comment by @AlexWaygood on `scripts/update_schemastore.py`:16 on 2024-05-13 15:07_

Added a new CLI flag in https://github.com/astral-sh/ruff/pull/11353/commits/6e7b3dbc07e33f8a32b6126c2ff06e6827885db8, how's that look?

---

_Marked ready for review by @AlexWaygood on 2024-05-13 15:07_

---

_@AlexWaygood reviewed on 2024-05-13 15:08_

---

_Review comment by @AlexWaygood on `scripts/update_schemastore.py`:46 on 2024-05-13 15:08_

Pyright is unhappy with this branch locally because it's unreachable (which is kinda the point). It stops complaining if I use `typing.assert_never` instead of `assert False`, but that's only available on Python 3.11+. Are we okay with requiring Python 3.11 to run this script? The updates I've already made use pattern-matching, which already means that the script will require Python 3.10, I suppose

---

_@JonathanPlasse reviewed on 2024-05-13 15:27_

---

_Review comment by @JonathanPlasse on `scripts/update_schemastore.py`:46 on 2024-05-13 15:27_

Could `typing_extensions` be used instead?

---

_@AlexWaygood reviewed on 2024-05-13 15:32_

---

_Review comment by @AlexWaygood on `scripts/update_schemastore.py`:46 on 2024-05-13 15:32_

it definitely _could_, but I don't really wanna add a third-party dependency for a one-file script that we run as part of the release process -- it would mean you'd either need to activate a venv and have the requirements installed into it, or run the script using `uv run`, etc. Doesn't seem worth it for something as small as this :-)

---

_@AlexWaygood reviewed on 2024-05-13 15:35_

---

_Review comment by @AlexWaygood on `scripts/update_schemastore.py`:46 on 2024-05-13 15:35_

If we do care about keeping compatibility with Python 3.10, it would actually be pretty simple to do our own backport that would satisfy pyright, though: https://github.com/python/cpython/blob/main/Tools/cases_generator/_typing_backports.py. I could do that, but it seems like more boilerplate, and I'm not sure if we care about keeping compatibility with Python 3.10 😆

---

_@zanieb reviewed on 2024-05-13 16:18_

---

_Review comment by @zanieb on `scripts/update_schemastore.py`:46 on 2024-05-13 16:18_

Feel free to require Python 3.11+ for the script

---

_@konstin reviewed on 2024-05-13 16:24_

---

_Review comment by @konstin on `scripts/update_schemastore.py`:46 on 2024-05-13 16:24_

Imho our internal scripts should be latest stable only, we have it installed for dev work anyway and it's good when we get to practice the new features ourselves

---

_@AlexWaygood reviewed on 2024-05-13 16:50_

---

_Review comment by @AlexWaygood on `scripts/update_schemastore.py`:46 on 2024-05-13 16:50_

I updated it to use `assert_never`, but pyright still complains locally. I think this is a false positive; mypy specifically excludes branches that are obviously _meant_ to be unreachable from its reachability checks. I left a comment at https://github.com/microsoft/pyright/issues/7559#issuecomment-2108198425 about this.

---

_Review requested from @zanieb by @AlexWaygood on 2024-05-13 16:51_

---

_Review requested from @konstin by @AlexWaygood on 2024-05-13 16:51_

---

_@AlexWaygood reviewed on 2024-05-13 16:52_

---

_Review comment by @AlexWaygood on `scripts/pyproject.toml`:33 on 2024-05-13 16:52_

This change is no longer necessary now I'm using `assert_never` in the script rather than a raw `assert False`, but I think it makes sense to ignore this rule in the `scripts/` directory regardless

---

_@konstin approved on 2024-05-13 16:55_

---

_Merged by @AlexWaygood on 2024-05-13 17:06_

---

_Closed by @AlexWaygood on 2024-05-13 17:06_

---

_Branch deleted on 2024-05-13 17:06_

---
