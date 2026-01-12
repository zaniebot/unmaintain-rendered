```yaml
number: 3347
title: ship cross platform lock files to users as a preview feature
type: issue
state: closed
author: BurntSushi
labels:
  - preview
  - tracking
assignees: []
created_at: 2024-05-03T14:20:19Z
updated_at: 2024-08-09T14:03:04Z
url: https://github.com/astral-sh/uv/issues/3347
synced_at: 2026-01-12T15:58:43Z
```

# ship cross platform lock files to users as a preview feature

---

_@BurntSushi_

This issue tracks progress toward shipping robust cross platform lock files in `uv`. This is not something that is planned to be added to the `uv pip` CLI, but rather, as part of the workspace-aware `uv`. (Which hasn't been built out yet.)

### Work items

* [x] #3350
* [x] #3356 
* [x] #3357 
* [x] #3506
* [x] #3611 
* [x] #3880 
* [x] #3881 
* [x] #3927 
* [x] https://github.com/astral-sh/uv/issues/4307
* [x] https://github.com/astral-sh/uv/issues/4194
* [x] Integrate with `uv install`.
* [x] Integrate with `uv run`.
* [ ] Build `uv update`, which updates versions in the lock file.
* [ ] Make an assessment on the state of error handling. (e.g., What happens when one fork of the resolver fails but others do not.)
* [x] https://github.com/astral-sh/uv/issues/4893
* [x] https://github.com/astral-sh/uv/issues/4888
* [x] https://github.com/astral-sh/uv/issues/4924

### Non-blocking work items

These shouldn't block shipping the universal resolver, but I thought it'd still be good to track them.

* [x] https://github.com/astral-sh/uv/issues/4357
* [x] https://github.com/astral-sh/uv/issues/4617

---

_Label `preview` added by @BurntSushi on 2024-05-03 14:20_

---

_Label `tracking` added by @BurntSushi on 2024-05-03 14:20_

---

_Comment by @BurntSushi on 2024-07-16 16:49_

While not every issue in the list has been completed, `uv lock` is currently shipping as a preview feature that folks can use. There is still work to do before it leaves "preview," but I think that's better tracked by the release-ready project: https://github.com/orgs/astral-sh/projects/11

---

_Closed by @BurntSushi on 2024-07-16 16:49_

---

_Comment by @mishamsk on 2024-07-16 17:08_

@BurntSushi hey! thanks for doing all of this. much appreciated! Although I did notice the `lock` command, I wasn't sure how to use it to force UV to install from lock => landed on this issue to track progress. Now that you've closed it, I was wondering if there is a way for the public to track progress? The project is internal apparently...

---

_Comment by @BurntSushi on 2024-07-16 17:57_

> @BurntSushi hey! thanks for doing all of this. much appreciated! Although I did notice the `lock` command, I wasn't sure how to use it to force UV to install from lock => landed on this issue to track progress. Now that you've closed it, I was wondering if there is a way for the public to track progress? The project is internal apparently...

I think you want `uv sync`.

Apologies about the project being private. I didn't realize that. We're working on fixing that (or publishing some other way to track progress).

---

_Comment by @mishamsk on 2024-07-16 18:23_

> I think you want uv sync.

ah, yes, how did I miss that. awesome, thanks! couldn't find some feature-tracking issue, but given your other comment - I'll just continue checking issues for now. thanks!

---

_Comment by @charliermarsh on 2024-07-16 18:24_

The project should be public now: https://github.com/orgs/astral-sh/projects/11

---

_Comment by @charliermarsh on 2024-07-16 18:25_

(Thanks, it wasn't intended to be private, glad you pointed that out.)

---
