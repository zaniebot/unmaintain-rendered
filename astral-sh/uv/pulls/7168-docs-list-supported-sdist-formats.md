```yaml
number: 7168
title: "docs: list supported sdist formats"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/list-supported-sdist-formats
created_at: 2024-09-07T14:22:59Z
updated_at: 2024-09-08T14:38:41Z
url: https://github.com/astral-sh/uv/pull/7168
synced_at: 2026-01-12T16:07:43Z
```

# docs: list supported sdist formats

---

_@mkniewallner_

## Summary

Explicitly list the formats and extensions that uv supports, based on [this list](https://github.com/astral-sh/uv/blob/86ee8d2c0134692dec85901bf2a7f459ea01bbd6/crates/distribution-filename/src/extension.rs#L70-L77). Not a huge fan of adding the section in `concepts/resolution.md`, but I did not find a better place. Alternatively we could maybe add a dedicated page that shortly explains Python package types (wheels, sdists), where such a section could live?

## Test Plan

Local run of the documentation.

---

_Review comment by @mkniewallner on `crates/uv-cli/src/lib.rs`:3565 on 2024-09-07 14:56_

Initially wanted to also add a link to the new section here, but the code generating markdown pages outputs all links with `<a>` HTML tags, which made it not possible to have both a correct link and `mkdocs` not complaining about the link not existing in strict mode. Since there are no other links to other internal pages here and in `settings.rs`, I assumed that this is simply not yet supported by the code generating markdown pages, so left this out for now.

---

_@mkniewallner reviewed on 2024-09-07 14:56_

---

_Marked ready for review by @mkniewallner on 2024-09-07 14:56_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-07 15:49_

---

_@zanieb reviewed on 2024-09-07 15:50_

This seems like a reasonable place to me right now. I guess we'll need like a "Distributions" concept page? Now that we have a `uv build` command it makes a bit more sense. I can open a tracking issue for that.

Should we say "i.e." or "e.g." instead of "usually" in those CLI docs?


---

_Comment by @mkniewallner on 2024-09-07 15:58_

> Should we say "i.e." or "e.g." instead of "usually" in those CLI docs?

Good idea, went with `.e.g.` since it's a subset of the actually supported formats, so thought it makes more sense.

---

_Comment by @zanieb on 2024-09-07 16:11_

Charlie will want "e.g.," haha (per our style guide)

---

_Comment by @mkniewallner on 2024-09-07 16:22_

> Charlie will want "e.g.," haha (per our style guide)

Thanks to that, TIL about yet another thing that is different between American and British English 😄 https://jakubmarian.com/comma-after-i-e-and-e-g/

---

_Assigned to @charliermarsh by @zanieb on 2024-09-07 16:40_

---

_@charliermarsh approved on 2024-09-07 19:09_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-09-07 19:10_

---

_Merged by @charliermarsh on 2024-09-07 19:16_

---

_Closed by @charliermarsh on 2024-09-07 19:16_

---

_@konstin reviewed on 2024-09-07 19:28_

---

_Review comment by @konstin on `docs/concepts/resolution.md`:301 on 2024-09-07 19:28_

We should clarify that PEP 625 has decided that `.tar.gz` is the canonical source distribution filename, all other extensions should be considered legacy or experimental.

---

_Branch deleted on 2024-09-08 14:38_

---
