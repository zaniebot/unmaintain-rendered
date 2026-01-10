```yaml
number: 6742
title: Provide system-level configuration files
type: issue
state: closed
author: pradyunsg
labels:
  - configuration
assignees: []
created_at: 2024-08-28T10:32:06Z
updated_at: 2024-10-22T15:04:00Z
url: https://github.com/astral-sh/uv/issues/6742
synced_at: 2026-01-10T04:45:09Z
```

# Provide system-level configuration files

---

_Issue opened by @pradyunsg on 2024-08-28 10:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## What

This is an ask for a ["Global" layer of configuration](https://pip.pypa.io/en/stable/topics/configuration/#location) to be provided, living in [`/etc`](https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/etc.html). Unlike pip, I think uv should have only one file under that hierarchy.

## Why

This would enable system administrators of shared machines to configure uv to use the correct [index-url](https://docs.astral.sh/uv/reference/settings/#index-url) and [link-mode](https://docs.astral.sh/uv/reference/settings/#link-mode) without needing to affect the runtime shell environments used by developers.

My specific use case is for being able to configure the index-url to point to the internal index on shared machines, which helps reduce workflow friction for newcomers which in turn reduces the number of users using workarounds to hit the "wrong" package indexes.

For context, this is the current configuration I've got for pip with the URLs redacted:

```ini
$ cat /etc/pip.conf
[global]
index-url = https://[internal-url]/simple
require-virtualenv = true

[search]
index = https://[internal-url]/search
```

## How

uv already has logic for handling merging of project-level and user-level configuration. This would add another layer to that configuration merging -- with the preference order being project overrides user (current) which in turn overrides global.


---

_Comment by @pradyunsg on 2024-08-28 10:34_

_sigh_ That's malware.

---

_Label `configuration` added by @charliermarsh on 2024-08-28 12:54_

---

_Comment by @charliermarsh on 2024-08-28 12:54_

This makes sense to me.

---

_Comment by @charliermarsh on 2024-08-28 14:59_

Is there an obvious home for this, e.g., following XDG?

---

_Comment by @pradyunsg on 2024-08-29 09:49_

I think a top-level file (`/etc/uv.toml`) would be fine. I think pip inherits the XDG support from appdirs (now platformdirs) and I am not fully familiar with the "Why" on this.

---

_Comment by @pradyunsg on 2024-08-29 09:54_

Actually... I take that back. Looking at what platformdirs does today... XDG is an obvious home for this.

https://github.com/platformdirs/platformdirs/blob/4.2.2/src/platformdirs/unix.py#L79-L83

So... the location would be `/etc/xdg/uv/uv.toml` and involve respecting the relevant XDG env var if set.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-29 12:56_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-29 13:52_

---

_Comment by @pelson on 2024-09-13 10:02_

I honestly believe that system-level configuration is an anti-pattern (but common!), and if I were prioritising I would say that executable-level configuration is actually more useful for shared machines.

To clarify my meaning:

**executable-level** / **prefix** config is config that lives in the prefix root of the executable being run. So if I have my `uv` installed in `/opt/bin/uv`, I would want `uv` to look for config in `/opt` (I don't really care where, given what @pradyunsg proposed, `/opt/xdg/uv/uv.toml` is fine).

By extension, if I have my uv in `/home/user/my-own-environment/bin/uv`, then the config would come from `/home/user/my-own-environment/xdg/uv/uv.toml`.

My rationale for making the opening statement ("executable-level configuration is actually more useful for shared machines") is that if a sys admin is providing `uv` then they should absolutely be able to configure that `uv` (project and user levels don't let them do that). However, if a sys admin isn't providing `uv`, then what interest do they actually have in configuring `uv` for you? Are they sure that the config that they specify is actually valid for the _arbitrary_ `uv` version that I am going to install?

On the contrary, if a sys admin wants to provide multiple versions of `uv` (e.g. to aid migration to a newer version), they have the ability to configure each individually and adapt the config to the specific version. This would be handy for example if there are config options that aren't valid in one version that are in another.

As a final remark, if `uv` isn't found in a `bin` directory, it isn't being managed (and probably was downloaded directly into a homespace), then no `prefix-level` config should be taken.



---

_Comment by @pradyunsg on 2024-09-26 15:13_

Hi folks! Is there anything I can do to help move this forward? Would a PR adding support be the best way forward here or are there any higher level concerns around implementing this?

---

_Comment by @charliermarsh on 2024-09-26 16:14_

I think I'm fine with this (\cc @zanieb in case you disagree), in which case a PR adding support would be the best path forward!

---

_Comment by @charliermarsh on 2024-09-30 17:29_

I can also do it @pradyunsg if you haven't started on it yet.

---

_Comment by @zanieb on 2024-09-30 18:32_

I'm fine with this if we have a clear standard we can follow regarding the location.

I don't really know what to do about toggling this behavior based on where the uv binary is. That seems quite complicated, though I share your concerns about the configuration not being valid for your version. Are we going to have a separate way to opt-out of this? e.g. `--no-system-config` and `system-config = false`?

---

_Comment by @gaby on 2024-10-17 13:23_

This should be the same way pip does it. I manage a fleet of systems and we configure all of then by creating an `/etc/pip.conf` file. When ansible runs to install python apps, it just works by using that, no extra config needed. Users don't have sudo access.

---

_Comment by @charliermarsh on 2024-10-17 13:30_

Are you objecting to something specific? We have an open PR (#7851) that supports `/etc/uv/uv.toml` in that way.

---

_Comment by @gaby on 2024-10-17 13:35_

> Are you objecting to something specific? We have an open PR (#7851) that supports `/etc/uv/uv.toml` in that way.

Yeah I just literally saw the PR. It does solve this problem 💪 That would allow for easy configuration via `ansible`. 

---

_Comment by @pradyunsg on 2024-10-22 14:59_

Given that #7851 has landed, I guess we should close this out? Or do the maintainers wanna wait until they've cut a release with this. 😅

---

_Comment by @charliermarsh on 2024-10-22 15:03_

Yes thank you!

---

_Closed by @charliermarsh on 2024-10-22 15:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-22 15:04_

---
