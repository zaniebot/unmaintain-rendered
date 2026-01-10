```yaml
number: 12942
title: Strange default owner/group when installed as root
type: issue
state: open
author: jklaiho
labels:
  - wish
assignees: []
created_at: 2025-04-17T12:55:26Z
updated_at: 2025-11-10T18:53:48Z
url: https://github.com/astral-sh/uv/issues/12942
synced_at: 2026-01-10T03:23:53Z
```

# Strange default owner/group when installed as root

---

_Issue opened by @jklaiho on 2025-04-17 12:55_

### Summary

I'm installing uv for all users on a system. This involves downloading the installer and executing it as root:

```
UV_INSTALL_DIR=/usr/local/bin INSTALLER_NO_MODIFY_PATH=1 ./install.sh
```

For some reason, the `uv` and `uvx` binaries are created with the owner/group values of `1001:117`. The same happens if I just download the release file from GitHub and extract it manually as root.

If I perform the installation with a non-root user into a path owned by them, the files are created with the owner/group of that user.

This seems like it may be a *nix feature rather than a uv "bug" as such, but I'd still expect root-installed binaries to end up as `root:root`. Is there anything the installer could do to facilitate that?

### Platform

Ubuntu 24.04 amd64

### Version

uv 0.6.14

### Python version

Python 3.12.3

---

_Label `bug` added by @jklaiho on 2025-04-17 12:55_

---

_Assigned to @Gankra by @zanieb on 2025-04-17 21:22_

---

_Comment by @Gankra on 2025-04-22 19:13_

Installing "as root" isn't really a supported workflow, no. Could you elaborate on how you did it "as root" (sudo? su?)? Also is this actually preventing the other users from running it? What are the full permissions you're ending up with?

---

_Label `wish` added by @Gankra on 2025-04-22 19:13_

---

_Label `bug` removed by @Gankra on 2025-04-22 19:13_

---

_Comment by @jklaiho on 2025-04-23 11:17_

Being a centrally managed multi-user system, it just didn't make sense to me to have everyone install a separate `uv` binary when everything seemingly works just fine with one installed by root in `/usr/local/bin`. (Except self-updates by non-root users, but that's by design in this case.)

I installed it via an Ansible task that sudoes using `become_user`. The method of becoming root doesn't matter, same result.

I changed the ownership of the extracted binaries to `root:root` with a later Ansible task. They have `0755` perms when extracted, so it didn't *particularly* matter what the ownership was before, anyone could still run them. Kinda ugly to have them owned by user 1001, who does exist on the system and has no business being the owner of the binaries, which is the main reason I wanted them changed.

Looking deeper, I checked what your GitHub release tarballs contain:

```
$ tar tvzf uv-x86_64-unknown-linux-gnu.tar.gz
drwxr-xr-x runner/docker     0 2025-04-10 00:38 uv-x86_64-unknown-linux-gnu/
-rwxr-xr-x runner/docker 41044656 2025-04-10 00:38 uv-x86_64-unknown-linux-gnu/uv
-rwxr-xr-x runner/docker   373296 2025-04-10 00:38 uv-x86_64-unknown-linux-gnu/uvx
```

So the CI process that builds the archive apparently has `runner` with UID 1001 and `docker` with GID 117, and those are just baked into the archive. And then when you extract the archive with `tar` as root, it preserves those numeric permissions, which then makes the files appear as owned by a particular user/group on the target machine, if corresponding ones exist.

Quick fix suggestions in your CI:

- `chown -R 0:0 uv-x86_64-unknown-linux-gnu` before `tar`ing the directory
- `tar --owner=0 --group=0 <etc>` directly.

I tested both. Extracting as root, the files come out as `root:root`. Extracting as a non-root user, they come out with the UID:GID of that user, just like they currently do.

Having a single `/usr/local/bin/uv` common to all users is a completely valid use case IMHO, unless I've missed something in the docs that absolutely prevents it from being a consideration, and this change of ownership inside the tarball seemingly breaks nothing that's currently working, just makes root installs nicer without the extra `chown` step to ensure that a random user on the system doesn't own the binaries.

---

_Comment by @AThePeanut4 on 2025-11-10 18:53_

I have exactly the same situation, where I want to install into `/usr/local/bin`. I found an even simpler fix - just use `--no-same-owner` in the `tar` command - which I've made PR https://github.com/axodotdev/cargo-dist/pull/2195 for in the upstream project.

---
