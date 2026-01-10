---
number: 5185
title: "Feature request: option to not update `uv.lock` file"
type: issue
state: closed
author: kdeldycke
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-07-18T13:12:57Z
updated_at: 2024-07-19T05:13:56Z
url: https://github.com/astral-sh/uv/issues/5185
synced_at: 2026-01-10T01:23:46Z
---

# Feature request: option to not update `uv.lock` file

---

_Issue opened by @kdeldycke on 2024-07-18 13:12_

Using the latest `uv`:
```shell-session
$ uv --version
uv 0.2.26 (fe403576c 2024-07-17)
```

## Description

When I call a `uv run`, I get the `uv.lock` updated and kept in sync automatically. I would like to have a way to tell `uv` to not change the `uv.lock` file at all.

## Context

I am maintaining several Python-based CLI. To keep their Sphinx-based documentation up-to-date, I rely on a [reusable GitHub workflow that calls `uv run sphinx-apidoc`](https://github.com/kdeldycke/workflows/blob/5e7bc19e5d56453c42dd97979bcb773a0c412e3b/.github/workflows/docs.yaml#L264-L308).

The problem is that while `uv run sphinx-apidoc` is doing its job producing auto-generated documentation, that way of invoking it is also updating the `uv.lock` file at the base of the checked-out repository.

And because my [last step of the workflow](https://github.com/kdeldycke/workflows/blob/5e7bc19e5d56453c42dd97979bcb773a0c412e3b/.github/workflows/docs.yaml#L292-L308) is creating a PR with all local changes, I end up with a noisy PR that not only contain the changes from `sphinx-apidoc`, but also unexpected updates in `uv.lock` file.

My quick fix was to add an [extra step to force its exclusion](https://github.com/kdeldycke/workflows/blob/7a42f59feacdb0b398a5614a541384d3b6792f14/.github/workflows/docs.yaml#L305-L309):
```yaml
      - name: Reset uv.lock
        # Exclude `uv.lock` file which might be auto-updated by calls to `uv run`.
        run: |
          git checkout -- uv.lock
```

And that's how I end up with a clean PR with only the changes produced by `sphinx-apidoc`.

Now I just wonder if it might be a good idea to add a new option to `uv`, to let it restrain itself and keep `uv.lock` as it found it. Something like `uv run --no-lock-update`.

## Discussion

If keeping the `uv.lock` in sync is good for everyday run, the reason I do not need that behavior in automated workflows is because I already have [another dedicated job to keep it in sync](https://github.com/kdeldycke/workflows/blob/7a42f59feacdb0b398a5614a541384d3b6792f14/.github/workflows/autofix.yaml#L264-L304).

---

_Comment by @charliermarsh on 2024-07-18 13:23_

We have these options on uv sync already, I meant to add them to uv run.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-18 13:56_

---

_Label `enhancement` added by @charliermarsh on 2024-07-18 13:56_

---

_Label `preview` added by @charliermarsh on 2024-07-18 13:56_

---

_Comment by @charliermarsh on 2024-07-18 18:26_

I'll likely ship this today.

---

_Referenced in [astral-sh/uv#5196](../../astral-sh/uv/pulls/5196.md) on 2024-07-18 18:47_

---

_Closed by @charliermarsh on 2024-07-18 18:55_

---

_Closed by @charliermarsh on 2024-07-18 18:55_

---

_Comment by @kdeldycke on 2024-07-19 05:13_

Oh wow, that was fast! Thanks @charliermarsh for the quick addition! 🤗

---
