```yaml
number: 1582
title: uv does not install dynamic build dependencies before preparing metadata
type: issue
state: closed
author: sbidoul
labels:
  - bug
assignees: []
created_at: 2024-02-17T11:43:13Z
updated_at: 2024-02-19T19:59:16Z
url: https://github.com/astral-sh/uv/issues/1582
synced_at: 2026-01-12T15:58:29Z
```

# uv does not install dynamic build dependencies before preparing metadata

---

_@sbidoul_

It seems `get_requires_for_build_wheel` is not invoked before `prepare_metadata_for_build_wheel`.

This means dynamic build dependencies are not installed before preparing metadata.

Here is a reproducer.

`uv pip  install -v --no-deps "odoo-addon-mis_builder @ git+https://github.com/OCA/mis-builder@16.0#subdirectory=setup/mis_builder"`

This fails about an invalid Name metadata, where it should succeed. This is most likely because the dynamic build dependency `setuptools-odoo` which is declared in [setup.py](https://github.com/OCA/mis-builder/blob/3aea4235697bac0f74d446e610e2b934b0994e06/setup/mis_builder/setup.py#L4) has not been installed in the build environment.

With setuptools, `setup_requires` entries are returned by `get_requires_for_build_wheel` and must be installed before preparing metadata.

---

_Comment by @halflings on 2024-02-17 11:58_

This is maybe related to an issue I'm encountering (trying to install the LLM finetuning lib axolotl):

```
(.venv) ➜  axolotl-finetuning uv pip install "axolotl[flash-attn,deepspeed] @ git+https://github.com/OpenAccess-AI-Collective/axolotl"
 Updated https://github.com/OpenAccess-AI-Collective/axolotl (5a5d474)
 Updated https://github.com/huggingface/peft.git (8db74d4)                      error: Failed to download and build: flash-attn==2.5.0
  Caused by: Failed to build: flash-attn==2.5.0
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "/tmp/.tmpSjTNqD/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 366, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/tmp/.tmpSjTNqD/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 480, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/tmp/.tmpSjTNqD/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 9, in <module>
ModuleNotFoundError: No module named 'packaging'
---
```

Running "uv pip install packaging" separately doesn't resolve this.

---

_Comment by @charliermarsh on 2024-02-17 12:42_

@sbidoul - Thank you for this. I thought it was safe to skip `get_requires_for_build_wheel` when using the default backend but clearly not! Will fix.

---

_Label `bug` added by @charliermarsh on 2024-02-17 12:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-17 12:44_

---

_Comment by @stillmatic on 2024-02-17 14:00_

the minimal repro for this is `uv pip install flash-attn` (https://github.com/Dao-AILab/flash-attention/tree/main?tab=readme-ov-file#installation-and-features)

---

_Comment by @charliermarsh on 2024-02-17 14:12_

I think the solutions may be different. `mis_builder` I can fix trivially by ensuring we call `get_requires_for_build_wheel` in all cases. But `flash-attn` is still failing...

`python -m build .` also fails on `flash-attn`, which makes me think it's a problem with the package's declared metadata. And in fact, it looks like their setup script requires `packaging` (https://github.com/Dao-AILab/flash-attention/blob/5cdabc2809095b98c311283125c05d222500c8ff/setup.py#L9C6-L9C15), but that's not going to be available at the time the script runs (since the script itself declares it as a dependency at the very bottom).

---

_Closed by @charliermarsh on 2024-02-17 14:24_

---

_Comment by @ilkersigirci on 2024-02-17 23:08_

The official way to install flash-attn is using `pip install flash-attn --no-build-isolation`. Will there be similar feature for `uv`? I can't use the current state of `uv` for my ML projects, because it doesn't allow me to install `flash-attn`

---

_Comment by @zanieb on 2024-02-18 07:04_

We might want to track that use-case in a separate issue.

---

_Comment by @Taytay on 2024-02-19 19:59_

@zanieb : Done here: #1715 

---
