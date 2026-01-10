---
number: 8047
title: "Feature: Provide a Debian package"
type: issue
state: closed
author: jondo
labels:
  - question
  - wish
assignees: []
created_at: 2024-10-09T14:16:01Z
updated_at: 2025-09-19T17:30:10Z
url: https://github.com/astral-sh/uv/issues/8047
synced_at: 2026-01-10T01:24:23Z
---

# Feature: Provide a Debian package

---

_Issue opened by @jondo on 2024-10-09 14:16_

I am on Linux Mint, and I would like to start using uv, but I have been taught not to trust arbitrary installer scripts like your https://astral.sh/uv/install.sh . Also, I would like to avoid `pip install uv`, since I cannot be sure this keeps my local Python environment stable.

So I would like to humbly ask for a Debian uv package.

(This might only make sense at a 1.0 release. Is there a road map for 1.0? According to [this closed milestone](https://github.com/astral-sh/uv/milestone/1), uv is already feature complete.)

(Update: I have now used pipx for installing uv.)

---

_Comment by @zanieb on 2024-10-09 14:22_

I don't think we can maintain this — someone from the Debian package repository would need to.

---

_Comment by @jondo on 2024-10-09 14:38_

That's a pity.

Hmm, I can only use pip within a virtualenv, otherwise I get `error: externally-managed-environment` (even with `pip install --user uv`). However I have the feeling that I should be able to use uv _instead of_ virtualenv (+piptools), not in _addition_ to it.


---

_Comment by @konstin on 2024-10-09 14:48_

By default, uv doesn't install pip into the virtualenv. You can use `uv pip` or alternatively create the venv with pip by using `uv venv --seed`.

---

_Label `question` added by @konstin on 2024-10-09 14:48_

---

_Comment by @jondo on 2024-10-09 14:53_

@konstin , sorry, I think you misunderstood. My comment was about how to use pip in a venv to install uv, not about how to use uv to create a venv with the pip package installed within.

---

_Comment by @konstin on 2024-10-09 15:00_

I'm not sure why `pip install --user uv` is failing, that is not something we control.

To come back to the original post:

> but I have been taught not to trust arbitrary installer scripts like your [astral.sh/uv/install.sh](https://astral.sh/uv/install.sh)

If you are looking for a download without running a script, you can get an archive from https://github.com/astral-sh/uv/releases/latest. You can also download and inspect the installer script before running it.

---

_Comment by @zanieb on 2024-10-09 16:23_

More details on all that in https://docs.astral.sh/uv/getting-started/installation/

I think there's also a snap available for uv on Debian. I wouldn't recommend snap personally, but it might be what you want.

---

_Comment by @mescanne on 2024-10-09 19:11_

Comment on this -

I recommend using pipx. It is distributed with debian (apt-get install pipx) and allows you to install arbitrary pip-delivered CLI tools. So pipx install uv will put uv in $HOME/.local/bin.

The benefit is that you can upgrade (pipx upgrade uv) when and how you'd like to the latest, without being constrainted by Debian's release cycle.

---

_Comment by @pszpetkowski on 2024-11-20 23:55_

> I recommend using pipx. It is distributed with debian (apt-get install pipx) and allows you to install arbitrary pip-delivered CLI tools. So pipx install uv will put uv in $HOME/.local/bin.

That's exactly been my setup for the past few months and it works fine, however uv aspires to cover pipx features: 

> A single tool to replace pip, pip-tools, **pipx**, poetry, pyenv, twine, virtualenv, and more.

Providing Debian package right now would not benefit the users so much, since it's actively developed and releases are very frequent, but imo it should be considered after uv reaches some level of maturity (e.g. after 1.0 release).

---

_Label `wish` added by @zanieb on 2024-11-21 00:39_

---

_Comment by @zanieb on 2024-11-21 00:40_

I don't think this is in scope for us, happy to support a Debian package maintainer if anything is needed — just reach out.

---

_Closed by @zanieb on 2024-11-21 00:40_

---

_Comment by @rommeswi on 2024-11-21 12:14_

I think that decision is a real pity. It is completely standard that pip install uv will complain about externally managed environments in most Linux distros. pipx is a user installation. It would be great if uv would offer at least one reasonable standardized way of installation system-wide, be that appimage, deb, or flatpak (and no, snaps don't count in that category).

---

_Comment by @collinanderson on 2025-02-23 05:26_

FYI: Here is the tracking bug on the debian side of things. https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1069776

Sounds like as of Dec 1st, most of the rust dependencies were either not packaged or out of date. https://lists.debian.org/debian-python/2024/12/msg00000.html

Also, a possible issue I see with packaging uv, is that as far as I understand, the `uv python list` is [hardcoded](https://github.com/astral-sh/uv/blob/main/crates/uv-python/download-metadata.json), so a packaged/frozen version of uv will never be able to install python bugfixes or security fixes, unless someone regularly backports the python list, rebuilds, and republishes.

---

_Comment by @linsomniac on 2025-03-18 17:18_

I'm doing a preview of a github action to build deb package, currently only for x64 Linux.  You can use this repo by doing:

```
sudo wget -O /usr/share/keyrings/uvrepo.gpg https://linsomniac.github.io/uvrepo/pubkey.gpg
sudo chmod 644 /usr/share/keyrings/uvrepo.gpg
echo "deb [signed-by=/usr/share/keyrings/uvrepo.gpg] https://linsomniac.github.io/uvrepo any main" | sudo tee /etc/apt/sources.list.d/uvrepo.list
apt update
apt install uv
```

Please provide feedback on the linsomniac/uvrepo github repo.

---

_Comment by @zanieb on 2025-03-18 18:02_

> Sounds like as of Dec 1st, most of the rust dependencies were either not packaged or out of date. https://lists.debian.org/debian-python/2024/12/msg00000.html

We're pretty on top of using the latest versions of packages. If things aren't packaged, it's usually because we're on the bleeding edge of what's available. Other downstream maintainers have opened issues tracking / requesting switching to a packaged distribution — feel free to do so for any blocking dependencies.

> Also, a possible issue I see with packaging uv, is that as far as I understand, the uv python list is [hardcoded](https://github.com/astral-sh/uv/blob/main/crates/uv-python/download-metadata.json), so a packaged/frozen version of uv will never be able to install python bugfixes or security fixes, unless someone regularly backports the python list, rebuilds, and republishes.

This is only the case for versions managed by uv — we support using system versions. We'll explore supporting dynamic loading of new managed Python versions eventually, but it will require stricter versioning of the distributions from `python-build-standalone`, i.e., if the distributions change there an old version of uv will not know how to install it. 

---

_Comment by @linsomniac on 2025-03-18 20:05_

Note: My longer term plan, once I have feedback on it, would be to submit the uvrepo work as a PR to uv, so that astral can directly publish an apt repo of their artifacts alongside the direct downloads of tar files.  Assuming Astral is interested, but I can't imagine why not.  Until then my repo is building daily off the latest available uv release.

---

_Comment by @linsomniac on 2025-03-20 02:42_

FYI: I've added i386, s390, and aarch64 to the repo.

---

_Comment by @ianbmacdonald on 2025-08-29 00:30_

This looks like a good option for Debian based platforms. 

https://dario.griffo.io/posts/how-to-install-uv-debian/
https://github.com/dariogriffo/uv-debian


---

_Comment by @dariogriffo on 2025-09-19 17:30_

Yup, that's me, updates are pushed the same day.
Added support to more architectures 3 weeks ago.
I'll also add in the near future instructions to ensure package signatures are only valid with my key pair.


> This looks like a good option for Debian based platforms.
> 
> https://dario.griffo.io/posts/how-to-install-uv-debian/ https://github.com/dariogriffo/uv-debian



---
