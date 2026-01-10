```yaml
number: 65
title: "Add script to update `ty.json` in schemastore"
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: micha/schemastore-script
created_at: 2025-05-06T18:10:31Z
updated_at: 2025-05-08T11:30:06Z
url: https://github.com/astral-sh/ty/pull/65
synced_at: 2026-01-10T02:34:10Z
```

# Add script to update `ty.json` in schemastore

---

_Pull request opened by @MichaReiser on 2025-05-06 18:10_

## Summary

Adds a script to update ty's schema in the schemastore repository

Closes https://github.com/astral-sh/ty/issues/13

## Test plan

https://github.com/astral-sh/schemastore/pull/new/update-ty-52d16b44bcc060bec0a0381a1ea8052bf4479f14


---

_Label `release` added by @MichaReiser on 2025-05-06 18:10_

---

_Comment by @zanieb on 2025-05-06 18:25_

> Check that the ruff submodule has no changes or automatically update it

You could crib this from the `release.sh` script (and / or move it to a `check-submodule.sh` script). I'm unsure it should be updated though. 

---

_Comment by @MichaReiser on 2025-05-06 18:30_

> You could crib this from the release.sh script (and / or move it to a check-submodule.sh script). I'm unsure it should be updated though.

Yeah, updating feels a bit aggressive but I think it's important to warn if someone is on a stale revision. I've to figure out how to check that.

---

_Review comment by @MichaReiser on `scripts/update_schemastore.py`:1 on 2025-05-07 09:00_

This is mostly just copy paste from the ruff repository. The only real addition is in `main` where we check the state of the `ruff` submodule

---

_Marked ready for review by @MichaReiser on 2025-05-07 09:00_

---

_@MichaReiser reviewed on 2025-05-07 09:00_

---

_@zanieb reviewed on 2025-05-07 14:55_

---

_Review comment by @zanieb on `scripts/update_schemastore.py`:17 on 2025-05-07 14:55_

It's more idiomatic to use ALL_CAPS for these module level constants.

---

_@zanieb reviewed on 2025-05-07 14:55_

---

_Review comment by @zanieb on `scripts/update_schemastore.py`:19 on 2025-05-07 14:55_

It's really quite naughty to run a subprocess on import. Could you avoid that? :)

---

_@zanieb reviewed on 2025-05-07 14:56_

---

_Review comment by @zanieb on `scripts/update_schemastore.py`:19 on 2025-05-07 14:56_

Is this just `Path(__file__) / ..`?

---

_@zanieb reviewed on 2025-05-07 14:56_

---

_Review comment by @zanieb on `scripts/update_schemastore.py`:21 on 2025-05-07 14:56_

This should be relative to `Path(__file__)` which makes the script working directory agnostic. 

---

_@MichaReiser reviewed on 2025-05-07 16:30_

---

_Review comment by @MichaReiser on `scripts/update_schemastore.py`:21 on 2025-05-07 16:30_

This is a relative path to the schemastore repository and it gets concatenated with the path to the schemastore repositroy. So I think what's here is correct.

---

_@MichaReiser reviewed on 2025-05-07 16:32_

---

_Review comment by @MichaReiser on `scripts/update_schemastore.py`:19 on 2025-05-07 16:32_

Yes. This was already present in the existing script. I don't mind it using `git` here, considering how much other git operations its running anyway. 

---

_@MichaReiser reviewed on 2025-05-07 16:33_

---

_Review comment by @MichaReiser on `scripts/update_schemastore.py`:19 on 2025-05-07 16:33_

> It's really quite naughty to run a subprocess on import. Could you avoid that? :)

I don't think it matters. This is a one of script. Don't import it!

---

_Merged by @MichaReiser on 2025-05-08 11:30_

---

_Closed by @MichaReiser on 2025-05-08 11:30_

---

_Branch deleted on 2025-05-08 11:30_

---
