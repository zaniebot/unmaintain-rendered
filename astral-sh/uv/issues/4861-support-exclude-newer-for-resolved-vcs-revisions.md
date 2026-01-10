---
number: 4861
title: "Support `--exclude-newer` for resolved VCS revisions"
type: issue
state: open
author: edgarrmondragon
labels: []
assignees: []
created_at: 2024-07-07T18:55:30Z
updated_at: 2025-02-21T03:42:49Z
url: https://github.com/astral-sh/uv/issues/4861
synced_at: 2026-01-10T01:23:42Z
---

# Support `--exclude-newer` for resolved VCS revisions

---

_Issue opened by @edgarrmondragon on 2024-07-07 18:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

It'd be nice if `--exclude-newer` supported resolved git tags. For example, if `UV_EXCLUDE_NEWER` is set to `2024-06-01` and the `v1` tag of a project was created on `2024-06-12`, then requesting this would fail:

```
UV_EXCLUDE_NEWER=2024-06-01 uv pip install 'my-package @ git+https://github.com/my-org/my-package.git@v1'
```

Similarly, when requesting branches, the resolved revision would only be older than the requested date.

Let me know if this makes sense or whether it's even possible 😅.


---

_Comment by @patcon on 2025-02-21 03:42_

Just learned about the `exclude-newer` flag of UV and it won over my heart.

Came here to suggest the same thing. From my perspective, allowing commit resolution for branches seems more widely useful compared to gut-checking whether git commits were re-tagged. (Everyone commits to branches; very few users move tags.)

Specifically, I'm interested in this for when working in jupyer notebooks, so that a version-controlled jupyter notebook will still work in colab if checked out. (assuming it's set to match the date of the checked out commit)

Thanks for posting this @edgarrmondragon!

---
