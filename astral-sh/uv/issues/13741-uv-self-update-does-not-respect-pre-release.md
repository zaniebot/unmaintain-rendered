---
number: 13741
title: "`uv self update` does not respect pre-release segments during equality checks"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-05-30T19:34:43Z
updated_at: 2025-06-04T15:33:00Z
url: https://github.com/astral-sh/uv/issues/13741
synced_at: 2026-01-10T01:25:38Z
---

# `uv self update` does not respect pre-release segments during equality checks

---

_Issue opened by @zanieb on 2025-05-30 19:34_

### Summary

i.e., trying to move from alpha to stable

```
❯ uv self update 0.7.8
info: Checking for updates...
success: You're on the latest version of uv (v0.7.8-alpha.1)
❯ uv self update 0.7.7
info: Checking for updates...
success: Downgraded uv from v0.7.8 to v0.7.7! https://github.com/astral-sh/uv/releases/tag/0.7.7
```

### Platform

n/a

### Version

0.7.8

### Python version

_No response_

---

_Label `bug` added by @zanieb on 2025-05-30 19:34_

---

_Label `good first issue` added by @konstin on 2025-06-02 08:15_

---

_Comment by @LaBatata101 on 2025-06-02 14:29_

I can work on this

---

_Comment by @gaeng02 on 2025-06-03 02:56_

I’m not entirely sure what you expects. 
Are they suggesting to take the tag from 0.7.7 (alpha.1) and apply it to 0.7.8 so that the alpha.1 prerelease is installed first, and if that tag doesn’t exist, fall back to installing the stable version?

---

_Comment by @zanieb on 2025-06-03 03:10_

The tags should be ordered as v0.7.7 -> v0.7.8-alpha.1 -> v0.7.8, but, in the above example, v0.7.8 is considered identical to v0.7.8-alpha.1. I'm not quite sure what you're asking.

I don't think we should install pre-releases by default. We should definitely upgrade from a pre-release to a stable version.

---

_Comment by @LaBatata101 on 2025-06-03 22:03_

@zanieb do you have any tips for how to test this? The self-update feature is only available when installed via the installation scripts.

---

_Comment by @zanieb on 2025-06-03 22:07_

I'm not sure. Maybe @Gankra knows? We could definitely test with the self-update feature turned on in CI, if that helps.

---

_Comment by @Gankra on 2025-06-04 14:14_

The unfortunate easiest way to test this is to copy the binaries you build over your actual uv install (which you can ultimately undo by just running our hosted install scripts as normal). The updater code needs a fair amount of overhaul to allow for running in an isolated environment (the "is this ok to actually run" checks are unfortunately quite pervasive through our own code and axoupdater, and if you don't load a receipt at all it's missing key info to do its own job).

The easiest-to-explain approach is:

* Install the version of `uv` you want to use for testing through normal means
* Build your changes
* Copy the `uv` binary over the actual installed one
* Run `uv self update` to test your changes
  * If you can reproduce the issue with `--dry-run` this will save you a lot of headache.

However this is a bit tedious, so often I would resort to hand-editing the receipt.json, which is in a platform-specific location but if on unix it will be in ~/.config/uv/uv-receipt.json

Mine is here:

```
{
    "binaries": [
        "uv",
        "uvx"
    ],
    "binary_aliases": {},
    "cdylibs": [],
    "cstaticlibs": [],
    "install_layout": "flat",
    "install_prefix": "/Users/gankra/.local/bin",
    "modify_path": true,
    "provider": {
        "source": "cargo-dist",
        "version": "0.28.4"
    },
    "source": {
        "app_name": "uv",
        "name": "uv",
        "owner": "astral-sh",
        "release_type": "github"
    },
    "version": "0.7.7"
}
```

Notable fields:

* `version` is the version the updater will believe the installed version of uv to have, regardless of the actual version the binary thinks it has. 
  * So you can change this to `0.7.8-alpha.1` to test the conditions zanie is presumably under.
  * ...actually `uv self update` prints the version in the *binary* and not the *receipt* so it's possible zanie was in this kind of state when they encountered this bug?
* `install_prefix` is where the updater will expect the binary-asking-itself-to-be-updated to be located
  * you can theoretically edit this to point at your target/debug/ dir to have the updater say "oh yeah i can update these binaries"



---

_Comment by @LaBatata101 on 2025-06-04 14:28_

I've looked into this a bit, and this may be an issue with `axoupdater`.

The code responsible for showing that message is here:
https://github.com/astral-sh/uv/blob/e7c066cf16de38008aef1a11d3041809bcc5fbf9/crates/uv/src/commands/self_update.rs#L181-L192

We only get to that code path if `axoupdater` doesn't perform the update. So in this case, the `uv` binary you were running had the version `0.7.8-alpha.1`. When `axoupdater` checked for updates, the latest version was `0.7.8`, which is [considered greater](https://docs.rs/axoupdater/latest/axoupdater/struct.Version.html#total-ordering) than `0.7.8-alpha.1`. So in theory this should have been updated, but it wasn't. Could be a bug in the code responsible for comparing the versions in `axoupdater`.

What do you guys think?



---

_Comment by @Gankra on 2025-06-04 14:34_

I agree it really looks like a bug in axoupdater but if so it's a baffling one because axoupdater converts to cargo [semver::Version](https://docs.rs/semver/latest/semver/struct.Version.html) for all comparisons and that thing absolutely knows alphas are < real versions.

It could be a bug in axotag which does some preprocessing on a tag before turning it into a version, but I definitely heavily tested that on alphas (cargo-dist is a prerelease power user).

---

_Comment by @Gankra on 2025-06-04 14:58_

I discussed this with zanie and there's no actual bugs in the normal code paths.

They installed uv in a non-standard way (using `INSTALLER_DOWNLOAD_URL=` to force https://astral.sh/uv/install.sh to install a test copy of uv -- note that `0.7.8-alpha.1` isn't a published release we have!)

But that install script has the latest actual version of uv baked in (at the time, 0.7.8), so it recorded to disk in the receipt.json 0.7.8, and so the updater """correctly""" reported that the installed version was the one requested. Then the uv binary printed its own version in the success message (0.7.8-alpha.1), and not the one the updater found, creating this extra-confusing situation.

So if there's a fix we want to make here, it's a bit more of a subtle one, and probably not a good first issue?

---

_Referenced in [astral-sh/uv#13840](../../astral-sh/uv/pulls/13840.md) on 2025-06-04 15:12_

---

_Closed by @Gankra on 2025-06-04 15:33_

---
