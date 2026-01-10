```yaml
number: 12247
title: Make uv seed pip by default
type: issue
state: closed
author: ThermodynamicBeta
labels:
  - enhancement
assignees: []
created_at: 2025-03-17T20:26:09Z
updated_at: 2025-04-01T17:54:39Z
url: https://github.com/astral-sh/uv/issues/12247
synced_at: 2026-01-10T03:41:47Z
```

# Make uv seed pip by default

---

_Issue opened by @ThermodynamicBeta on 2025-03-17 20:26_

### Summary

Sorry if this is duplicate.
Currently, if you have a uv-created venv activated, `pip` is not available and so it points to your system pip. You can add pip to your uv venv when you create it with the --seed flag, but I think it should be this way by default and I'm curious why it isn't

Reasons:
- I would expect a venv to have pip in it. If I ssh into a shared linux server and find and activate a virtual environment, I would expect to be able to use pip. People who don't know what uv is would be very confused in this situation
- I manage a python package for ~100 engineers internally at a company. We are migrating our main project from pip to uv, and it would help to have commands that work regardless of how they set up their venv, i.e. `pip install x` `pip show x`, rather than "if you set this up with uv, do this x, otherwise y". 

Because of the above two reasons, I basically will only use and instruct others to use uv venv with the --seed flag, which makes me feel like it should be the default.

I understand seed is configurable with environment variable, but:
- We use both Windows and Linux, setting environment variables on Windows is a mess (both the UI and the PS command) and very tedious and multiplying that by 100 engineers each who each have multiple computers is a big loss. 


### Example

```
uv venv
.venv/Scripts/activate
pip install pandas
uv pip show pandas
```

---

_Label `enhancement` added by @ThermodynamicBeta on 2025-03-17 20:26_

---

_Comment by @samypr100 on 2025-03-18 03:04_

I know there's many packages out there that run pip on your behalf using sys.execuble such as [spacy](https://github.com/explosion/spaCy/blob/b3c46c315eb16ce644bddd106d31c3dd349f6bb2/spacy/cli/download.py#L175) e.g. via `python -m spacy download xyz`. 

I believe something like this could help those scenarios, although this could easily be a breaking change.

---

_Comment by @shauneccles on 2025-03-18 05:20_

I think this is a deliberate design decision to add a barrier to using pip inside uv venvs and I think that's the right choice.

What is the barrier to using native uv commands?

---

_Comment by @paveldikov on 2025-03-22 18:56_

> I understand seed is configurable with environment variable, but

I haven't tried it for the `seed` option per se, but have you considered centrally managing users' `uv.toml` configuration files? This is what we do for our corporate environment (Windows + Linux, with a tiny Mac segment), and it works great -- much better than env vars indeed.

---

_Comment by @zanieb on 2025-04-01 17:54_

Let's move this to https://github.com/astral-sh/uv/issues/12604

We don't intend to seed pip by default.

---

_Closed by @zanieb on 2025-04-01 17:54_

---
