```yaml
number: 3021
title: "FR: add `--check-only` flag to `pip sync` command"
type: issue
state: open
author: ofek
labels:
  - enhancement
assignees: []
created_at: 2024-04-14T21:13:04Z
updated_at: 2024-08-09T09:42:34Z
url: https://github.com/astral-sh/uv/issues/3021
synced_at: 2026-01-10T04:53:49Z
```

# FR: add `--check-only` flag to `pip sync` command

---

_Issue opened by @ofek on 2024-04-14 21:13_

We have a need to check whether environments are synchronized without performing action so it would be nice to have such a flag that would produce JSON output like:

```json
{
    "in-sync": false
}
```

---

_Comment by @charliermarsh on 2024-04-15 01:50_

Can we resolve this by adding `--dry-run` to `uv pip sync` (which already exists on `uv pip install`)?

---

_Comment by @ofek on 2024-04-15 02:16_

Probably! What would the output be if everything is synchronized already?

---

_Comment by @charliermarsh on 2024-04-15 02:20_

Right now it ends with "Would make no changes" but we could consider using separate exit codes.

---

_Comment by @ofek on 2024-04-15 02:38_

I think it would be preferable to have an additional `--json` flag to not be reliant upon exit codes.

---

_Comment by @charliermarsh on 2024-04-15 11:47_

Can consider it but will take a lot more time and work given that it's committing to a public API that we'll need to build and maintain in perpetuity.

---

_Comment by @ofek on 2024-04-15 11:50_

Then I'm definitely fine with the quicker solution 🙂 

---

_Comment by @charliermarsh on 2024-04-15 11:52_

I do think we _want_ JSON output: https://github.com/astral-sh/uv/issues/411. At least for resolution. But we haven't figured it out yet :) \cc @konstin 

---

_Label `enhancement` added by @charliermarsh on 2024-04-20 01:27_

---

_Comment by @Rogdham on 2024-05-03 21:34_

This feature seems related to the recurring usecase of the CI checking that the committed `requirements.in` and `requirements.txt` files match each other. In that case, we are only interested in the exit code (the output does not matter much). This would be similar to the `--check` parameter of `ruff format`.

A workaround looks like the following:

```
uv pip compile requirements.in -o requirements.txt && git diff --exit-code requirements.txt
```

For reference, the feature has been asked for `pip-tools` in https://github.com/jazzband/pip-tools/issues/882, which might provide more context.

---

_Comment by @FlorianKoegler on 2024-08-09 09:42_

> Right now it ends with "Would make no changes" but we could consider using separate exit codes.

Hi, 
i found this issue, because I'm trying to use `uv pip sync --dry-run` to test if a virtual environment matches it's lock file.

It would be great if it would have a non-zero return code, in case there are deviations. 
This would make parsing much more reliable than evaluating the output message. :)

For comparison, `pip-sync --dry-run` also returns an non-zero return code.


---
