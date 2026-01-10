---
number: 16713
title: Best way to ensure python in uv always uses system trust store?
type: issue
state: open
author: pboushy
labels:
  - question
assignees: []
created_at: 2025-11-12T21:36:40Z
updated_at: 2025-11-21T23:00:59Z
url: https://github.com/astral-sh/uv/issues/16713
synced_at: 2026-01-10T01:26:09Z
---

# Best way to ensure python in uv always uses system trust store?

---

_Issue opened by @pboushy on 2025-11-12 21:36_

### Question

I've recently started using `uv` to setup python environments on my Mac.
I'd like to have all environments I create automatically have access to the system trust store.

I know that I have to install `pip-system-certs`, but I don't want to have to remember to do that every time I setup a new venv for the various python projects I work on.
I also don't want to have to modify every project's toml and/or requirements.txt

Is there a way to specify that `uv venv` should always install X packages when it creates a new venv?

If not, what's the best way to have `uv` do this?

### Platform

macOS 26.0.1 arm64 (Darwin 25.0.0 arm64)

### Version

uv 0.9.7 (Homebrew 2025-10-30)

---

_Label `question` added by @pboushy on 2025-11-12 21:36_

---

_Comment by @konstin on 2025-11-13 10:20_

There is currently no support for installing a package in all venvs automatically.

---

_Comment by @pboushy on 2025-11-13 18:14_

Any recommendations on a way that might work? would simplest solution for the time being be to create an alias:
`uvenv=uv venv && uv pip install pip-system-certs`

I anticipated `native-tls=true` would have solved the issue I'm trying to solve but `requests` doesn't read the system trust in macOS when that's set and the only fix I found is to install `pip-system-certs`

---

_Comment by @zanieb on 2025-11-13 18:49_

This is really a problem with `requests`, I think?

I'm not sure it makes sense for uv to introduce a way to break reproducibility of environments.

If we did add something for this, I guess it'd be like `UV_VENV_SEED_PACKAGES=...`?



---

_Comment by @pboushy on 2025-11-21 22:31_

I don't think it's a problem with `requests`?

I haven't quite figured out what `native-tls=true` does in UV on Mac, but I interpreted that it's supposed to make pip, urllib, and requests use the OS's system trust store (macOS keychain) to verify certificates.

I didn't see that occur in mine when I run `uv venv`

On the other hand, if I install Managed Python from https://github.com/macadmins/python and use `managed_python3 -m venv venv` to create an environment, `requests` uses the Root CAs that are in the keychain.
I haven't tested what occurs if I use macOS's built-in (Xcode) python and try to use requests. I'll test that and see if it makes a difference.


---

_Comment by @zanieb on 2025-11-21 23:00_

`native-tls=true` for uv instructs uv's networking stack to read your system certs. It doesn't inject the system certs into the Python SSL context.

---
