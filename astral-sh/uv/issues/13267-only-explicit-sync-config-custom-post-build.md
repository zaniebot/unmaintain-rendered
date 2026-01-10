---
number: 13267
title: only-explicit-sync config / custom post build/install hooks?
type: issue
state: open
author: mo22
labels:
  - question
assignees: []
created_at: 2025-05-02T14:35:09Z
updated_at: 2025-05-05T10:15:06Z
url: https://github.com/astral-sh/uv/issues/13267
synced_at: 2026-01-10T01:25:31Z
---

# only-explicit-sync config / custom post build/install hooks?

---

_Issue opened by @mo22 on 2025-05-02 14:35_

### Question

Good day everyone,

I recently switched to uv and trying to incorporate the following things in my project:

a) I need to patch some dependencies after installation - mainly spicy where I need to comment out three lines to make it compatible with pyinstaller. I do not want to install scipy as editable/local project as there are only minor changes required. Ideally I'm looking for something like patch-package in the npm world. Maybe it's possible to hook into the builders for all or specific packages?

b) Currently I use a script that runs uv sync followed by applying the patches, however on each uv run the patches are reverted unless I specify --no-sync or set UV_NO_SYNC. Is there maybe a pyproject config setting to set uv to only sync if uv sync is explicitly called?

c) I need to install some dependencies with custom cmake args (both custom pip command line and environment vars). Is there a right way to do this? Alternatively hooking into the builders could also be a solution here.

Thank you very much for your time and help!

Moritz

### Platform

mac/arm

### Version

0.7.2 (481d05d8d 2025-04-30)

---

_Label `question` added by @mo22 on 2025-05-02 14:35_

---

_Comment by @charliermarsh on 2025-05-04 12:54_

> Is there maybe a pyproject config setting to set uv to only sync if uv sync is explicitly called?

Hmm, nothing beyond what you've already discovered -- I'd say `UV_NO_SYNC` is your friend here.

> I need to install some dependencies with custom cmake args (both custom pip command line and environment vars). Is there a right way to do this?

Is this covered by https://docs.astral.sh/uv/reference/settings/#config-settings?


---

_Comment by @mo22 on 2025-05-04 13:26_

> > I need to install some dependencies with custom cmake args (both custom pip command line and environment vars). Is there a right way to do this?
> Is this covered by https://docs.astral.sh/uv/reference/settings/#config-settings?

I believe those config-settings are only for the projects own build backend and cannot be used to affect a dependency :/

As an example, what I'm currently calling is:
```
FORCE_CMAKE=1 CMAKE_ARGS="-DGGML_CCACHE=OFF -DGGML_NATIVE=off -DGGML_AVX=on -DGGML_AVX2=off -DGGML_AVX512=off -DGGML_FMA=off -DGGML_F16C=off -DGGML_LASX=off -DGGML_LSX=off -DGGML_CUDA=off" \
  uv run pip install --verbose --force-reinstall --no-cache-dir --no-deps llama-cpp-python==${LLAMA_CPP_PYTHON_VERSION
```

Thank you very much!



---

_Comment by @charliermarsh on 2025-05-04 14:24_

No, those settings are passed to _all_ builds.

---

_Comment by @mo22 on 2025-05-05 10:15_

> No, those settings are passed to _all_ builds.

Ah okay I see - but the build system is not used for dependencies that are available via wheels, there seems to be no way to force building from source for a specific dependency and to specify the build command for just a single dependency, or am I missing something?

---
