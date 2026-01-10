```yaml
number: 2010
title: Fails to install JAX TPU
type: issue
state: closed
author: sayakpaul
labels:
  - bug
assignees: []
created_at: 2024-02-27T10:49:24Z
updated_at: 2024-07-01T14:43:39Z
url: https://github.com/astral-sh/uv/issues/2010
synced_at: 2026-01-10T05:31:36Z
```

# Fails to install JAX TPU

---

_Issue opened by @sayakpaul on 2024-02-27 10:49_

```
python3 -m uv pip install --no-cache-dir \
        "jax[tpu]>=0.2.16,!=0.3.2" \
        -f https://storage.googleapis.com/jax-releases/libtpu_releases.html
```

Fails with:

```bash
...
      Because there is no version of libtpu-nightly==0.1.dev20231204 and jax[tpu]==0.4.21 depends on libtpu-nightly==0.1.dev20231204,
      we can conclude that jax[tpu]==0.4.21 cannot be used.
      And because we know from (36) that any of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2,<0.4.21
       cannot be used, we can conclude that any of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2,<0.4.22
       cannot be used. (37)

      Because there is no version of libtpu-nightly==0.1.dev20231213 and jax[tpu]>=0.4.22,<=0.4.23 depends on
      libtpu-nightly==0.1.dev20231213, we can conclude that jax[tpu]>=0.4.22,<=0.4.23 cannot be used.
      And because we know from (37) that any of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2,<0.4.22
       cannot be used, we can conclude that any of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2,<0.4.24
       cannot be used. (38)

      Because there is no version of libtpu-nightly==0.1.dev20240205 and jax[tpu]==0.4.24 depends on libtpu-nightly==0.1.dev20240205,
      we can conclude that jax[tpu]==0.4.24 cannot be used.
      And because we know from (38) that any of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2,<0.4.24
       cannot be used, we can conclude that any of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2,<0.4.25
       cannot be used. (39)

      Because there is no version of libtpu-nightly==0.1.dev20240224 and jax[tpu]==0.4.25 depends on libtpu-nightly==0.1.dev20240224,
      we can conclude that jax[tpu]==0.4.25 cannot be used.
      And because we know from (39) that any of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2,<0.4.25
       cannot be used, we can conclude that any of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2
       cannot be used.
      And because you require one of:
          jax[tpu]>=0.2.16,<0.3.2
          jax[tpu]>0.3.2
      we can conclude that the requirements are unsatisfiable.

      hint: libtpu-nightly was requested with a pre-release marker (e.g., libtpu-nightly==0.1.dev20230207), but pre-releases weren't
      enabled (try: `--prerelease=allow`)
```

(Full output removed for brevity)

While 

```bash
python3 -m pip install --no-cache-dir \
        "jax[tpu]>=0.2.16,!=0.3.2" \
        -f https://storage.googleapis.com/jax-releases/libtpu_releases.html
```

passes.

I am on uv 0.1.11.

---

_Label `bug` added by @charliermarsh on 2024-02-27 16:13_

---

_Comment by @charliermarsh on 2024-02-27 16:16_

Thanks, will take a look!

---

_Comment by @sayakpaul on 2024-03-01 01:56_

Same happens for:

```bash
uv pip install --no-cache-dir         torch==2.1.2         torchvision==0.16.2         torchaudio==2.1.2         onnxruntime         --extra-index-url https://download.pytorch.org/whl/cpu &&     python3 -m uv pip install --no-cache-dir         accelerate         datasets         hf-doc-builder         huggingface-hub         Jinja2         librosa         numpy         scipy         tensorboard         transformers
```

Passes if I omit `uv`. However, it passes with an older version of `uv` (0.1.11). But fails with 0.1.13. 

I am on standard Ubuntu machine. 

---

_Comment by @charliermarsh on 2024-03-01 03:52_

Does it get any further if you pass `--prerelease=allow`?

---

_Comment by @charliermarsh on 2024-03-01 03:53_

(Our Torch-related resolutions have some issues right now due to their use of local versions segments, e.g., the `+cpu` stuff. But that may not be relevant here -- can't tell from the above trace yet.)

---

_Comment by @sayakpaul on 2024-03-01 04:04_

> Does it get any further if you pass `--prerelease=allow`?

Tried with:

```bash
uv pip install --no-cache-dir         torch==2.1.2         torchvision==0.16.2         torchaudio==2.1.2         onnxruntime         --extra-index-url https://download.pytorch.org/whl/cpu &&     python3 -m uv pip install --no-cache-dir         accelerate         datasets         hf-doc-builder         huggingface-hub         Jinja2         librosa         numpy         scipy         tensorboard         transformers --prerelease=allow
```

Leads to:

```bash
  × No solution found when resolving dependencies:
  ╰─▶ Because torch==2.1.2 is unusable because no wheels are available with a matching platform and you require torch==2.1.2, we can
      conclude that the requirements are unsatisfiable.
```

---

_Comment by @ncoop57 on 2024-03-14 19:49_

Facing similar issue

---

_Comment by @charliermarsh on 2024-03-18 20:53_

Torch installs should now work as expected if you include the local tag, e.g., `uv pip install torch==2.1.2+cpu torchvision==0.16.2+cpu` and so on.

---

_Comment by @sayakpaul on 2024-03-19 02:33_

How about the GPU variants? `pip` variant:

```bash
pip3 install torch --index-url https://download.pytorch.org/whl/cu118
```

---

_Comment by @charliermarsh on 2024-03-19 02:43_

Yeah, that should all work. The only difference from `pip` is that if you want one of the non-standard versions with a `+` at the end, you need to make that explicit, like: `uv pip install torch==2.2.1+cu118 --index-url https://download.pytorch.org/whl/cu118`. If you see otherwise, let me know.


---

_Comment by @sayakpaul on 2024-03-19 02:54_

Good to know! Maybe worth adding this to the docs or somewhere relevant when you think the behaviour has gotten a little more stable. 

---

_Comment by @charliermarsh on 2024-03-19 03:06_

👍 It's explained in more detail here: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers

---

_Comment by @konstin on 2024-07-01 14:43_

Closing this since we've implemented and documented how to use uv with the local version identifiers and `uv pip install --no-cache-dir "jax[tpu]>=0.2.16,!=0.3.2" -f https://storage.googleapis.com/jax-releases/libtpu_releases.html --prerelease allow` works, feel free to reopen if jax still doesn't install for your accelerator.

---

_Closed by @konstin on 2024-07-01 14:43_

---
