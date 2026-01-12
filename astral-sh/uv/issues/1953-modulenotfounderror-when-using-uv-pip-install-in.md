```yaml
number: 1953
title: "`ModuleNotFoundError` when using `uv pip install` in GitHub Actions"
type: issue
state: closed
author: kevinhu
labels: []
assignees: []
created_at: 2024-02-24T18:43:16Z
updated_at: 2024-02-24T22:21:22Z
url: https://github.com/astral-sh/uv/issues/1953
synced_at: 2026-01-12T15:58:33Z
```

# `ModuleNotFoundError` when using `uv pip install` in GitHub Actions

---

_@kevinhu_

Hi there! I'm using uv in a GitHub action but ran into `ModuleNotFoundError` when importing `click` and `tqdm`.

I'm using the latest version of `uv`. When I use `uv pip install`, I get several import errors, but when I use vanilla `pip install`, the action runs without issue. An example run: https://github.com/kevinhu/uv-gh-action/actions/runs/8032491324/job/21941942800

Here is a minimal example that reproduces these errors on Python 3.9 through 3.12: https://github.com/kevinhu/uv-gh-action. I've set up two jobs, `uv` and `pip`, that differ only in their install command: `uv pip install ...` vs `pip install ...`

---

_Comment by @samypr100 on 2024-02-24 19:34_

As far as I'm aware the virtual environment won't remain activated across steps in GHA since the shell reloads per step. In your pip workflow you end up installing dependencies outside the virtualenv, hence why you can import them. uv defaults to installing them on the .venv which is why you see uv failing. Hope this helps 🙏 

---

_Comment by @kevinhu on 2024-02-24 20:38_

Thanks! I thought the PATH modification would work but I guess not.

---

_Closed by @kevinhu on 2024-02-24 20:38_

---

_Comment by @charliermarsh on 2024-02-24 22:21_

Thanks @samypr100!

---
