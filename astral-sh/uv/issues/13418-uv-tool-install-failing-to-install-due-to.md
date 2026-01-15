```yaml
number: 13418
title: "`uv tool install` failing to install due to 'Permission denied'"
type: issue
state: open
author: cchristous
labels:
  - needs-mre
assignees: []
created_at: 2025-05-13T00:32:25Z
updated_at: 2026-01-15T08:51:43Z
url: https://github.com/astral-sh/uv/issues/13418
synced_at: 2026-01-15T09:45:45Z
```

# `uv tool install` failing to install due to 'Permission denied'

---

_@cchristous_

### Summary

Hi,

I am seeing a 'Permission denied' error when trying to install a tool using `uv tool install`:

```
+/tmp/script_314.sh:366> brew install uv
==> Downloading https://ghcr.io/v2/homebrew/core/uv/manifests/0.7.3
==> Fetching uv
==> Downloading https://ghcr.io/v2/homebrew/core/uv/blobs/sha256:3d300ee7ebcfa7e0ef7802ddec4fdbf0ac6e023851bad35460ba9f58dc5951d8
==> Pouring uv--0.7.3.ventura.bottle.tar.gz
==> Caveats
zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/uv/0.7.3: 17 files, 37.2MB
==> Running `brew cleanup uv`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).


+/tmp/script_314.sh:367> uv tool install <tool> --with-requirements /Users/<user-name>/requirements-ami.txt --python 3.11
Downloading cpython-3.11.12-macos-x86_64-none (download) (17.5MiB)
 Downloading cpython-3.11.12-macos-x86_64-none (download)
error: Permission denied (os error 13) at path "/var/folders/sk/1rps9wzs79zdj9f2wz8m0yf00000gn/T/.tmpjKWSI5"
Provisioning step had errors: Running the cleanup provisioner, if present...
```

I run these commands via packer to provision AWS AMIs. This last worked 5 days ago on uv 0.7.2. I've made no other changes to the AMI provisioning since the last success. It is possible that other brew package updates could have caused this issue, but that seems unlikely at this point.

### Platform

macOS 13 ARM64 and AMD64

### Version

0.7.3

### Python version

Python 3.11.12

---

_Label `bug` added by @cchristous on 2025-05-13 00:32_

---

_Comment by @konstin on 2025-05-13 06:53_

Does it work if you go back to uv 0.7.2, or was this a change in the environment? Are any `XDG` or `UV_` variables set, as `/var/folders/sk` isn't a usual build folder? 

---

_Label `bug` removed by @konstin on 2025-05-13 06:53_

---

_Label `needs-mre` added by @konstin on 2025-05-13 06:53_

---

_Comment by @cchristous on 2025-05-13 13:02_

I've switched to installing `uv` via pipx, and I've pinned it to 0.7.2 and the install works properly. I've not set any `XDG` or `UV_` variables.

---

_Comment by @cchristous on 2025-05-13 20:06_

I've continued using the pipx install method and unpinned the version, allowing 0.7.3 to be used, and it fails again.

---

_Comment by @charliermarsh on 2025-05-20 13:13_

I don't think we changed anything meaningful between those versions. I think this is probably an issue on the host?

---

_Comment by @cchristous on 2025-05-20 14:58_

Well, it consistently works with 0.7.2 and consistently fails with 0.7.3. If there is some debugging data that would be helpful, I am happy to try to collect it.

---

_Comment by @kkjdaniel on 2025-07-16 00:50_

I am having this issue as well.

---

_Comment by @zanieb on 2025-07-16 04:26_

@kkjdaniel does this also work for you on 0.7.2 and fail on 0.7.3?

---

_Comment by @Crypto69 on 2025-07-23 02:01_

I get the same issue
(base) ~ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.8.2 aarch64-apple-darwin
no checksums to verify
installing to /Users/xxxx/.local/bin
  uv
  uvx
everything's installed!
sh: line 1453: /Users/xxxxxx/.bash_profile: Permission denied
sh: line 1454: /Users/xxxxxx/.bash_profile: Permission denied
mkdir: /Users/xxxxxx/.config/fish/conf.d: Permission denied
ERROR: command failed: mkdir -p /Users/xxxxxx/.config/fish/conf.d
Same thing when I tried running with sudo

---

_Comment by @cchristous on 2025-10-02 18:50_

I'd like to report that after I upgraded to macOS 15, this issue no longer occurs.

---

_Comment by @Florin-Birgu on 2025-10-06 13:28_

installing via pip worked for me:
pip install uv

---

_Comment by @pbitzer on 2025-10-16 14:33_

> I get the same issue (base) ~ curl -LsSf https://astral.sh/uv/install.sh | sh downloading uv 0.8.2 aarch64-apple-darwin no checksums to verify installing to /Users/xxxx/.local/bin uv uvx everything's installed! sh: line 1453: /Users/xxxxxx/.bash_profile: Permission denied sh: line 1454: /Users/xxxxxx/.bash_profile: Permission denied mkdir: /Users/xxxxxx/.config/fish/conf.d: Permission denied ERROR: command failed: mkdir -p /Users/xxxxxx/.config/fish/conf.d Same thing when I tried running with sudo

Ditto running MacOS 15.1. Installing via brew works fine. 

---

_Comment by @konstin on 2025-10-16 20:31_

Are the problems you are reporting for installing uv itself, or for running `uv tool install`?

---

_Comment by @pbitzer on 2025-10-16 21:09_

> Are the problems you are reporting for installing uv itself, or for running `uv tool install`?

Ah, sorry. That output is when I run the curl command to download and install uv: `curl -LsSf https://astral.sh/uv/install.sh | sh` Using the brew instructions to install works fine. 

---

_Comment by @konstin on 2025-10-16 21:11_

This bug is about a `uv tool install` problem, could you open a separate issue with logs and other information that could be relevant, such as security software and the permissions on the paths in the error message?

---

_Comment by @salchichongallo on 2026-01-15 08:51_

I ran `sudo chown -R <my_user> ~/.local/share/uv` and I was able to run the `uv tool install` without problems.

Note: I installed uv via Homebrew

---
