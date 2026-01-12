```yaml
number: 1584
title: "Support for `uv pip install --user`"
type: issue
state: closed
author: Chaz6
labels:
  - duplicate
  - enhancement
  - compatibility
assignees: []
created_at: 2024-02-17T12:03:06Z
updated_at: 2024-05-17T13:44:40Z
url: https://github.com/astral-sh/uv/issues/1584
synced_at: 2026-01-12T15:58:29Z
```

# Support for `uv pip install --user`

---

_@Chaz6_

`uv pip install` does not support the flag `--user`. The help for `pip install --user` is as follows:-

```
  --user                      Install to the Python user install directory for your platform. Typically ~/.local/, or
                              %APPDATA%\Python on Windows. (See the Python documentation for site.USER_BASE for full details.)
```

---

_Comment by @zanieb on 2024-02-17 19:35_

Related #1526, we do not allow installation outside of a virtual environment right now.

---

_Label `enhancement` added by @zanieb on 2024-02-17 19:35_

---

_Label `compatibility` added by @zanieb on 2024-02-17 19:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-28 22:50_

---

_Comment by @Rotonen on 2024-04-08 13:08_

For a bit more context beyond tiered-managed-yet-sans-virtualenv academic and enterprise (modified cacerts, PKI trust - not everything can use `truststore` yet) userspace deployments, this also has a practical use in building Docker containers, which assemble software from multiple ecosystems and/or have some security policies in place (only centrally curated base images can do `USER root`, all internal build time steps beyond those base images must run as uid >= 1000).

In those situations it's convenient or necessary to be able to `COPY --from=python-build /home/<user>/.local/...` the local Python eggs across build stages.

Currently one could work around this by using a virtualenv and an entrypoint script to load that virtualenv up.

But there is a tendency towards extremely minimal hardened images (for general open reference check out what Microsoft is doing with the Dotnet chiseled Ubuntu images). These kinds of runtime images do not ship a shell, thus making the entrypoint workaround impossible. That leaves setting the virtualenv up manually via ENV as the main workaround, and that has implementation time ripple effects around templating of containers when doing internal containers at scale (or leaving it as a fragile pile of copy paste snippets, which people either get right, or don't).

In general implementing `uv pip install --user` will remove a lot of workarounds needed for uv to "just work" across a wider variety of non-happy-path deployment scenarios currently out there.

---

_Comment by @zanieb on 2024-05-17 13:44_

I'm going to close this in favor of https://github.com/astral-sh/uv/issues/2077 which has a bunch more discussion in it.

---

_Closed by @zanieb on 2024-05-17 13:44_

---

_Label `duplicate` added by @zanieb on 2024-05-17 13:44_

---
