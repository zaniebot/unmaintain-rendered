---
number: 3886
title: Is uv supposed to use hard links when installing dependencies in virtualenvs?
type: issue
state: closed
author: pawamoy
labels: []
assignees: []
created_at: 2024-05-28T18:40:16Z
updated_at: 2024-05-28T19:01:37Z
url: https://github.com/astral-sh/uv/issues/3886
synced_at: 2026-01-07T13:12:17-06:00
---

# Is uv supposed to use hard links when installing dependencies in virtualenvs?

---

_Issue opened by @pawamoy on 2024-05-28 18:40_

The README says:

> Disk-space efficient, with a global cache for dependency deduplication.

I always interpreted this as uv using hard links (or other more efficient platform-specific techniques) to avoid wasting space when installing the same dependencies in many different virtualenvs.

But in practice, it looks like the same module, `yaml_env_tag.py`, installed in many venvs, for the same Python version (3.11), has a different inode number in each venv:

```console
% ls -i */.venv/lib/*/site-packages/yaml_env_tag.py                       
27449386 ansito/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27954769 catalog/.venv/lib/python3.11/site-packages/yaml_env_tag.py
28483068 copier-uv/.venv/lib/python3.11/site-packages/yaml_env_tag.py
32066757 duty/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27450383 fastui/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27463092 fulfill/.venv/lib/python3.11/site-packages/yaml_env_tag.py
30738026 git-changelog/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27609526 griffe2md/.venv/lib/python3.11/site-packages/yaml_env_tag.py
28209604 griffe-inherited-docstrings/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27588071 griffe/.venv/lib/python3.11/site-packages/yaml_env_tag.py
33464782 handler-template/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27529488 markdown-exec/.venv/lib/python3.11/site-packages/yaml_env_tag.py
30676935 mkdocs-autorefs/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27932068 mkdocs-gallery/.venv/lib/python3.11/site-packages/yaml_env_tag.py
28015846 mkdocs-manpage/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27566322 mkdocs-pygments/.venv/lib/python3.11/site-packages/yaml_env_tag.py
34976719 mkdocs-spellcheck/.venv/lib/python3.11/site-packages/yaml_env_tag.py
27672260 mkdocstrings-python/.venv/lib/python3.11/site-packages/yaml_env_tag.py
34634104 mkdocstrings-shell/.venv/lib/python3.11/site-packages/yaml_env_tag.py
30750852 mkdocstrings/.venv/lib/python3.11/site-packages/yaml_env_tag.py
28156683 mono/.venv/lib/python3.11/site-packages/yaml_env_tag.py
28203268 qtile/.venv/lib/python3.11/site-packages/yaml_env_tag.py
28722205 textual/.venv/lib/python3.11/site-packages/yaml_env_tag.py
```

Same for any file really:

```console
% ls -i */.venv/lib/*/site-packages/griffe/__init__.py
28470384 ansito/.venv/lib/python3.11/site-packages/griffe/__init__.py
27929408 catalog/.venv/lib/python3.11/site-packages/griffe/__init__.py
32619592 duty/.venv/lib/python3.11/site-packages/griffe/__init__.py
30556482 fastapi/.venv/lib/python3.12/site-packages/griffe/__init__.py
28382439 fastui/.venv/lib/python3.11/site-packages/griffe/__init__.py
28274305 fulfill/.venv/lib/python3.11/site-packages/griffe/__init__.py
30922324 git-changelog/.venv/lib/python3.11/site-packages/griffe/__init__.py
30436256 griffe2md/.venv/lib/python3.11/site-packages/griffe/__init__.py
28209421 griffe-inherited-docstrings/.venv/lib/python3.11/site-packages/griffe/__init__.py
27962824 markdown-exec/.venv/lib/python3.11/site-packages/griffe/__init__.py
30695448 mkdocs-autorefs/.venv/lib/python3.11/site-packages/griffe/__init__.py
28720467 mkdocs-gallery/.venv/lib/python3.11/site-packages/griffe/__init__.py
29250701 mkdocs-manpage/.venv/lib/python3.11/site-packages/griffe/__init__.py
33986747 mkdocs-pygments/.venv/lib/python3.11/site-packages/griffe/__init__.py
34981811 mkdocs-spellcheck/.venv/lib/python3.11/site-packages/griffe/__init__.py
27666376 mkdocstrings-python/.venv/lib/python3.11/site-packages/griffe/__init__.py
34639730 mkdocstrings-shell/.venv/lib/python3.11/site-packages/griffe/__init__.py
30723121 mkdocstrings/.venv/lib/python3.11/site-packages/griffe/__init__.py
28185249 qtile/.venv/lib/python3.11/site-packages/griffe/__init__.py
32963180 textual/.venv/lib/python3.11/site-packages/griffe/__init__.py
```

So I wonder: is caching enabled by default, and supported on my platform? I'm on ArchLinux.

Coming from PDM, which supports different methods for this (recursive symlinking, hardlinks, etc.), I was happy to see that uv supported it too. Byt maybe I misunderstood?

---

_Comment by @charliermarsh on 2024-05-28 18:42_

Yes, we recursively hardlink by default on Linux. On macOS, we recursively reflink.

Is your cache on the same filesystem as your environment?

---

_Comment by @pawamoy on 2024-05-28 18:44_

Thanks for the astral-fast answer. Ah, that might be the issue :thinking: (facepalm) No, uv's cache is on a different disk and partition than my projects. Closing! :slightly_smiling_face: Is there a recommendation here? Should I just move the cache? Or can I have two (or more) cache locations (one per partition or something)?

---

_Closed by @pawamoy on 2024-05-28 18:44_

---

_Comment by @charliermarsh on 2024-05-28 18:46_

No prob! Thanks for asking, it made me double-check that we're doing it right. I tested on macOS with `--link-mode=hardlink` and it does seem like the inodes line up:

```
❯ ls -i venv1/lib/python3.12/site-packages/flask/app.py venv2/lib/python3.12/site-packages/flask/app.py
268196813 venv1/lib/python3.12/site-packages/flask/app.py 268196813 venv2/lib/python3.12/site-packages/flask/app.py
```

(They don't line up on macOS _by default_ since we default to reflinks.)


---

_Comment by @charliermarsh on 2024-05-28 18:46_

Hmm, I'd probably recommend setting `UV_CACHE_DIR` to something in the same disk / partition as your projects.


---

_Comment by @pawamoy on 2024-05-28 18:47_

I'll do just that. Thanks a lot!

---

_Comment by @charliermarsh on 2024-05-28 18:48_

Thank you! Always appreciate your questions.

---

_Comment by @pawamoy on 2024-05-28 19:01_

(Just free'd 81GB :flushed:)

---
