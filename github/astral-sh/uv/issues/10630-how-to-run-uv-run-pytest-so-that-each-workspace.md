---
number: 10630
title: "How to run `uv run pytest` so that each workspace member uses only it's own dependencies?"
type: issue
state: closed
author: justinas-kazanavicius
labels:
  - question
assignees: []
created_at: 2025-01-15T11:49:59Z
updated_at: 2025-01-16T12:09:43Z
url: https://github.com/astral-sh/uv/issues/10630
synced_at: 2026-01-07T13:12:18-06:00
---

# How to run `uv run pytest` so that each workspace member uses only it's own dependencies?

---

_Issue opened by @justinas-kazanavicius on 2025-01-15 11:49_

When running `uv run pytest`, all tests are run against the current virtual environment, so running `uv sync --all-packages` is necessary for the tests not to fail due to missing dependencies. However, if one forgets to add a dependency to a workspace member, the tests might still pass even though they shouldn't. Ideally, before running tests the environment would be set up for each workspace member independently. E.g.

If I have workspace members A, B, C, the tests should automatically run like this:
- `uv sync --package A`
- `uv run pytest /path/to/A`
- `uv sync --package B`
- `uv run pytest /path/to/B`
- `uv sync --package C`
- `uv run pytest /path/to/C`

---

_Comment by @blueraft on 2025-01-15 15:41_

perhaps `uv run --exact --pacakage A pytest /path/to/A`?

---

_Comment by @charliermarsh on 2025-01-15 16:36_

Yeah I think you want `--exact`. (I don't think that uv itself should be iterating over the workspace members like that, if that's the suggestion.)

---

_Label `question` added by @charliermarsh on 2025-01-15 16:36_

---

_Comment by @justinas-kazanavicius on 2025-01-16 12:09_

Thanks, I was not aware `--exact` was recently introduced though it does not solve my problem. As a workaround, I have created a custom script to iteratively do independent testing of each package. I agree; maybe adding it to `uv` introduces too much complexity.

---

_Closed by @justinas-kazanavicius on 2025-01-16 12:09_

---
