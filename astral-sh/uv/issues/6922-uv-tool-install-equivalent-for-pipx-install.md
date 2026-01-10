---
number: 6922
title: uv tool install equivalent for pipx install --include-deps
type: issue
state: closed
author: justin-yan
labels:
  - duplicate
  - enhancement
  - cli
assignees: []
created_at: 2024-09-01T20:25:32Z
updated_at: 2024-09-08T13:35:43Z
url: https://github.com/astral-sh/uv/issues/6922
synced_at: 2026-01-10T01:24:08Z
---

# uv tool install equivalent for pipx install --include-deps

---

_Issue opened by @justin-yan on 2024-09-01 20:25_

I searched through issues but didn't find anything similar - apologies if there's already an issue that I missed.

---

The `--include-deps` flag on `pipx install` will allow any CLI applications from the dependencies of a package also be added to the $PATH.

My use case may be pretty niche :-) but I use this as a pattern for an org-internal CLI, which can add dependencies on binary packages released on pip, and then have everything installed all at once with a single install command.  Installing everything into separate packages is not a huge inconvenience (copy paste a list of N commands instead of just one), but there are some niceties in being able to ensure that versions move together with a single package upgrade command.

Happy to explain the use case in more detail if it's helpful!

---

_Comment by @charliermarsh on 2024-09-02 17:15_

I swear there's an existing issue for this but @zanieb would know for sure.

---

_Label `enhancement` added by @charliermarsh on 2024-09-02 17:15_

---

_Label `cli` added by @charliermarsh on 2024-09-02 17:15_

---

_Comment by @justin-yan on 2024-09-02 18:11_

ah! perhaps https://github.com/astral-sh/uv/issues/6403

I'll leave it to your judgment if you want to close this issue and consolidate on #6403

---

_Comment by @jamesharris-garmin on 2024-09-04 22:27_

I think these issues are separate concerns, I'm thinking about the ansible official install instructions use pipx require the `--install-deps` flag.

https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#pipx-install

Also tools like `ansible-lint` need to be injected (with apps) into the ansible venv.

So supporting this would provide better support for ansible's install structure. (This the last thing I have installed in pipx)

---

_Comment by @sahensley on 2024-09-08 04:28_

There's also #6314 in regard to `--include-deps`. 

That issue references the flag `--install-deps` but the intention is `--include-deps`.

---

_Comment by @zanieb on 2024-09-08 13:35_

Yeah this is a duplicate of #6314 — we're interested in supporting this.

---

_Closed by @zanieb on 2024-09-08 13:35_

---

_Label `duplicate` added by @zanieb on 2024-09-08 13:35_

---
