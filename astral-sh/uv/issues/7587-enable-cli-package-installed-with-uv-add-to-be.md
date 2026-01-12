```yaml
number: 7587
title: "Enable CLI package installed with `uv add` to be accessed from the virtual env"
type: issue
state: closed
author: PatrickAlphaC
labels:
  - question
assignees: []
created_at: 2024-09-20T15:04:21Z
updated_at: 2024-09-27T12:03:16Z
url: https://github.com/astral-sh/uv/issues/7587
synced_at: 2026-01-12T15:59:15Z
```

# Enable CLI package installed with `uv add` to be accessed from the virtual env

---

_@PatrickAlphaC_

Let's say I add the following package that is a CLI:

First, let's setup and activate a virtual environment.

```bash
uv init
uv venv
source .venv/bin/activate
```

Then, add our package that has an executable. 

```bash
uv add moccasin
```

Assuming `moccasin` has an executable named `mox`, if I do the above, I am able to run the tool with:

```bash
uv run mox --help
```

However, I cannot do:

```
mox --help
```

By itself. If I instead run:

```
uv pip install moccasin
```

I am then able to run it's executable in the virtual environment without `uv run`:

```
mox --help
```

It would be nice if `uv add` had a flag to expose the executable without calling `uv run`. Maybe like:

```
uv add moccasin --expose-script
```

Or something. 

---

_Comment by @charliermarsh on 2024-09-20 15:05_

Did you activate the virtual environment in `.venv` prior to running `mox --help`?

---

_Comment by @PatrickAlphaC on 2024-09-20 15:08_

@charliermarsh yes. I can send a video if that's helpful? 

---

_Comment by @charliermarsh on 2024-09-20 15:15_

Sure, anything to help reproduce would be appreciated.

---

_Comment by @PatrickAlphaC on 2024-09-20 16:12_


https://github.com/user-attachments/assets/21080fc0-e502-4735-bd37-c033957530bd



---

_Comment by @PatrickAlphaC on 2024-09-20 16:14_

I also updated the original issue so that the steps are more specific (included the virtual environment setup to make reproducing it more straightforward)

---

_Comment by @charliermarsh on 2024-09-20 17:28_

Thanks for this, it helps a lot.

I think the issue here is that `uv add moccasin` does not install into the activated virtual environment (hence the warning you're seeing). It installs into the `.venv` in the current directory, by design. So you need to activate the environment in the _current directory_ in order to add the executable to the PATH. It's configurable here (https://docs.astral.sh/uv/concepts/projects/#project-environments) though that's intended for deployment environments like Docker containers.

---

_Label `question` added by @charliermarsh on 2024-09-20 17:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-20 17:28_

---

_Comment by @charliermarsh on 2024-09-25 21:46_

Hopefully my answer helped but let me know if you have any follow-up questions.

---

_Closed by @charliermarsh on 2024-09-25 21:46_

---

_Comment by @PatrickAlphaC on 2024-09-27 12:03_

I still had issues here, but maybe something else is going on. Thanks for being so attentive. 

The active virtual environment was/is in the `.venv` directory, and that was the active directory. Perhaps something else was happening if you're unable to reproduce. 

---
