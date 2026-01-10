```yaml
number: 2290
title: "Go to definition cannot solve relative import in third-party files when using a virtual env location other than `.venv`"
type: issue
state: closed
author: winlaic
labels:
  - bug
  - server
  - imports
assignees: []
created_at: 2025-12-31T02:31:48Z
updated_at: 2026-01-10T10:17:07Z
url: https://github.com/astral-sh/ty/issues/2290
synced_at: 2026-01-10T11:36:40Z
```

# Go to definition cannot solve relative import in third-party files when using a virtual env location other than `.venv`

---

_Issue opened by @winlaic on 2025-12-31 02:31_

When I ctrl click into the source of some installed packages, it turns out that if there are relative import in the source code, `ty` could not infer where the definition of the symbol. Here is a example of the `transformers` library:

<img width="783" height="574" alt="Image" src="https://github.com/user-attachments/assets/ff9d7704-f237-4621-8e05-385631e30429" />

You can see code `from .cache_utils import Cache, EncoderDecoderCache`, the `Cache` and `EncoderDecoderCache` are infered as `Unknown` and I can't ctrl click it. But I am sure the definition exists in `transformers.cache_utils`.

---

_Comment by @winlaic on 2025-12-31 02:46_

BTW, I am using this version of ty vscode extension:

<img width="384" height="165" alt="Image" src="https://github.com/user-attachments/assets/183ef0a6-49cb-4a4b-90eb-dcf860c31b26" />

---

_Label `server` added by @MichaReiser on 2025-12-31 08:19_

---

_Label `imports` added by @MichaReiser on 2025-12-31 08:19_

---

_Comment by @MichaReiser on 2025-12-31 08:36_

Hmm, I'm unable to reproduce this. What I tried is:

* Install `torch` with uv, using Python 3.10 (`uv add torch --python 3.10`)
* Select that environment in VS Code
* Click on `modeling_outputs` in `from transformers.modeling_outputs import TokenClassifierOutput`
* Click on `Cache`. 

Can you say more about how you installed the `torch` dependency and on what platform you're on?

Can you also tell me what ty version you're using (Click on `{}` next to `Python` in the bottom toolbar)

<img width="2192" height="656" alt="Image" src="https://github.com/user-attachments/assets/c08824b4-ea58-41b9-9a40-ff84ff55384b" />




---

_Label `needs-info` added by @MichaReiser on 2025-12-31 08:36_

---

_Comment by @winlaic on 2026-01-05 02:22_

Sorry for the late reply...

I am using VS Code `1.102.1` (universial) on macOS and it remote connect to a Ubuntu 20.04.6 LTS server.

The `ty` version is `0.0.8`

<img width="379" height="121" alt="Image" src="https://github.com/user-attachments/assets/c06f8195-be09-46f4-a97f-93e2a1d7fe4c" />

and I used `uv pip` to install the `torch` and `transformers` library, not using `uv add`
I used `uv venv --relocatable --python 3.10` to create the environment, at `/dev/shm/$USER/venv/ZipVoice`.

as of now, I still can't ctrl click it.

---

_Comment by @MichaReiser on 2026-01-08 09:12_

Thanks. I suspect this is related to using `--relocatable` but I was also able to reproduce it by installing the dependencies using `pip` instead of `uv`. What's interesting is that renaming the virtual env from `venv` to `.venv` fixes the issue! I'm not sure why this is happening

---

_Comment by @MichaReiser on 2026-01-08 09:29_

What's interesting is that changing the import to an absolute import makes the imports work:

```
from transformers.cache_utils import Cache, EncoderDecoderCache
from .cache_utils import Cache, EncoderDecoderCache
from .utils import ModelOutput
```

The first line works, the second does not. So something's off with resolving relative imports

---

_Renamed from "Go to definition can not solve relative import in source code." to "Go to definition cannot solve relative import in source code." by @AlexWaygood on 2026-01-08 09:47_

---

_Comment by @MichaReiser on 2026-01-08 09:56_

Okay, I think there are two issues:

* `file_to_module` matches on any search path instead of using the best matching search path
* `SearchPathIter` doesn't include site packages???

---

_Comment by @MichaReiser on 2026-01-08 10:39_

Okay, I figured out what the root cause is here. 

The issue is that we open the torch file using the [default database](https://github.com/astral-sh/ruff/blob/e60586f3b960aa952cd9c135c632abe570051805/crates/ty_server/src/session.rs#L1262-L1277), which fails to automatically discover the `venv`, so site packages are not included in the search paths in `file_to_module`. 

I'm not exactly sure what the solution here is but I'm leaning towards removing the default database concept and just using the first project instead (or using a single database for all projects, see the mono repository discussion)

---

_Label `needs-info` removed by @MichaReiser on 2026-01-08 10:39_

---

_Renamed from "Go to definition cannot solve relative import in source code." to "Go to definition cannot solve relative import in third-party files when using a virtual env location other than `.venv`" by @MichaReiser on 2026-01-08 10:39_

---

_Label `bug` added by @MichaReiser on 2026-01-08 10:39_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-08 10:39_

---

_Comment by @MichaReiser on 2026-01-08 11:17_

With https://github.com/astral-sh/ruff/pull/22455, I'm not sure what the use case for the default database is and I think we can simply always use the default database. The way I remember it is that the main reason for adding it in the first place was that we don't want to show diagnostics for non-project files, but that's handled if we respect excludes. @dhruvmanila / @Gankra do you see cases where we would still need the default database?


I think the main downside of making this change is that we incorrectly assume the same virtual env for files from outside the project. This doesn't seem terrible to me, and we already accidentally picked up the project's `.venv` today. 

Edit: I can confirm that removing all the default database stuff fixes this issue

---

_Comment by @Gankra on 2026-01-08 17:27_

Yeah I think the main Correct Benefit of the default database is "you opened a random python file from outside your worktree in your IDE session and it is nonsense for us to assume the worktree's venv is in scope" but also in that case yeah i don't think assuming the worktree's venv is particularly harmful...

---

_Assigned to @MichaReiser by @Gankra on 2026-01-09 04:09_

---

_Closed by @MichaReiser on 2026-01-09 08:30_

---

_Comment by @winlaic on 2026-01-10 10:12_

Thanks guys! Looking forward to updating to the new version!

---
