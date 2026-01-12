```yaml
number: 2236
title: "[Breaking] Install in `~/.local/bin` instead of cargo home"
type: pull_request
state: closed
author: konstin
labels:
  - enhancement
  - do-not-merge
assignees: []
base: main
head: konsti/install-in-local-bin
created_at: 2024-03-06T09:48:54Z
updated_at: 2024-05-21T17:34:32Z
url: https://github.com/astral-sh/uv/pull/2236
synced_at: 2026-01-12T16:04:56Z
```

# [Breaking] Install in `~/.local/bin` instead of cargo home

---

_@konstin_

`uv` is not part of the rust toolchain nor is it usually installed or managed by cargo. Similarly, i expect that the majority of users does not have cargo installed. We should instead use a not-rust-associated directory for binaries.

I chose `~/.local/bin`, which is mentioned in [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) and also used by pipx.

>  User-specific executable files may be stored in $HOME/.local/bin. Distributions should ensure this directory shows up in the UNIX $PATH environment variable, at an appropriate place.

As testing strategy, i'd create a prerelease and test windows and linux(es), while i'd need someone to test mac.

---

_Label `enhancement` added by @konstin on 2024-03-06 09:48_

---

_Comment by @charliermarsh on 2024-03-06 13:32_

How does this work across platforms? Like what happens on Windows?

---

_Comment by @konstin on 2024-03-06 15:57_

Windows currently uses `C:\Users\Ferris\.cargo\bin`, i expect this to switch to `C:\Users\Ferris\.local\bin`.

---

_@zanieb approved on 2024-03-06 15:58_

---

_Comment by @charliermarsh on 2024-03-06 20:47_

For what it's worth, I don't have a `~\.local\bin` on my Windows machine, but it does look like pipx uses that. Will the installer add it to PATH?

---

_@charliermarsh approved on 2024-03-06 20:49_

---

_Comment by @charliermarsh on 2024-03-06 20:56_

I'm cool with the directory but will it cause _problems_ when used alongside `pipx`?

---

_Comment by @charliermarsh on 2024-03-06 20:57_

Should it be `~/.astral/bin`...?

---

_Comment by @konstin on 2024-03-07 09:50_

> Should it be ~/.astral/bin...?

If everybody did that it would require a PATH entry per vendor, which imo would be bad. Normally installing into `~/.astral` and symlinking into PATH would be an option, but that doesn't work on windows. I could see installing it into `~/.cargo-dist/bin` or `~/.axo/bin` if that helps with the updater. Note that i'm not opposed to putting stuff into `$XDG_*/astral` - we should do that - i'm only advocating for this PATH convention.

---

_Comment by @konstin on 2024-03-07 14:21_

I've realized this will break people who rely on the current installation location. I'd bundle this with other bigger changes (v0.2 probably) if possible.

---

_Renamed from "Install in `~/.local/bin` instead of cargo home" to "[Breaking] Install in `~/.local/bin` instead of cargo home" by @konstin on 2024-03-07 14:21_

---

_Comment by @charliermarsh on 2024-03-07 14:24_

Yeah I think this has some upside but also has the potential to be pretty disruptive.

---

_Label `do-not-merge` added by @charliermarsh on 2024-03-07 14:24_

---

_Comment by @woutervh on 2024-03-08 00:58_

I don't care whatever *default* location is choosen, as long as you can choose to override it yourself.
see https://github.com/astral-sh/uv/issues/2087

My personal preference is `~/bin` because I don't want executables hidden away in hidden folders multiple levels deep.
Why?

If I need to support non technical people on windows with a non-working python-executable in some virtualenv, 
I would often need to start explaining them  to "Open File Explorer from the taskbar. Select View > Options > Change folder and search options. Select the View tab and, in Advanced settings, select Show hidden files, folders, and drives and OK."  Just to be able to start seeing a folder.

`~/.local/bin` is just another personal preference.  In 20 years I only encountered a handful installers that use it. i never actively used pipx.  It's OK as a *default*.




---

_Comment by @sondr3 on 2024-03-08 12:31_

Just chiming in to say that I dislike tools that don't use XDG directories for caches, binaries and configs, they are a standard for a reason. `~/.local/bin` is _not_ personal preference, it's where binaries should be installed on Linux (and macOS). My `config.fish` is littered with directory checks to add binaries to my `$PATH` for umpteen tools and languages. The same issue in [`cargo`](https://github.com/rust-lang/cargo/issues/1734) is one of the issues with the most upvotes. `ghcup` (`rustup` for Haskell) solves this by symlinking binaries into `~/.local/bin` from it's own `~/.ghcup/bin` which could be a alternative solution? This way the old setup keeps working but for new users you don't need to check and add a new path to your machine.

---

_Comment by @mitsuhiko on 2024-03-08 12:56_

For what it's worth with rye i went through various different choices and I ended up with one path for all platforms instead of messing with the XDG stuff or trying to find suitable locations on all platforms. It's super confusing when you need to look into different places depending on which machine you are on. I also with with `~/.rye/shims` as a place on the path since rye is installing further tools. I fully expect that a future uv will be in the same position and will need to bring it's own folder onto the path.

---

_Comment by @konstin on 2024-03-08 13:56_

@woutervh Is `.local` a hidden folder for you on windows? It looks non-hidden on mine.

![grafik](https://github.com/astral-sh/uv/assets/6826232/eda3659b-58cb-41ff-9c06-7736226320ed)

@mitsuhiko What made you choose that over `.local/bin`?


---

_Comment by @woutervh on 2024-03-08 14:12_

Rye is a good example:  via RYE_HOME, a user can still override the default directory and choose their own location
(and I use it, many thanks @mitsuhiko)

@konstin  .dotfiles are not hidden on windows? You are right, my mistake. 

> Just chiming in to say that I dislike tools that don't use XDG directories for caches, binaries and configs,
 For system-managed applications I agree,  but for virtualenv-based tools even XDG should only be a default that can be overridden.

the goal of using venv is isolation from other venvs, 
then imagine a venv-based tool insisting taking its config from the hardcoded location:

- venv-app-v1/bin/app 
- venv-app-v2/bin/app  
- venv-app-v3/bin/app

all reading ~/config/<app>/config

---

_Comment by @mitsuhiko on 2024-03-08 14:26_

> @mitsuhiko What made you choose that over `.local/bin`?

`rye tools install` installs additional binaries. Those need to be placed somewhere. If I were to put them into `~/.local/bin` I would start to muck around in a folder that is not fully under rye's control. Similar motivation to `~/.cargo/bin` I assume.

---

_Comment by @konstin on 2024-03-08 16:37_

What would you think about (sym)linking those binaries to `~/.local/bin`, from the background that it's the XDG standard that users are expected to have in PATH while rye's shim directory requires modifying dotfiles?

---

_Comment by @zanieb on 2024-03-13 19:18_

We might want to take into account toolchain management design considerations e.g. `rustup` / `cargo` do _not_ have shims in `~/.local/bin`.

---

_Comment by @konstin on 2024-03-13 19:31_

For me it comes down to a somewhat philosophical distinction: Do we consider `uv` a tool that gets installed, so it goes where all the installed tools go, or do we consider it a "system" application manager with its own domain that can reasonably expect people to change their PATH for it. Note that people have asking cargo to [use the xdg directories](https://github.com/rust-lang/cargo/issues/1734).



---

_Comment by @alldevic on 2024-05-11 12:08_

In Windows, why not use %APPDATA% folder for specific user and Program Files for everyone?

---

_Closed by @konstin on 2024-05-21 16:03_

---

_Comment by @zanieb on 2024-05-21 16:07_

I think we should change this someday still

---

_Comment by @konstin on 2024-05-21 16:26_

I'm also strongly in favor of getting out of the cargo brand. If we want to change it, i believe we need to do it in breaking release before `uv tool` support lands, the sooner the better before external dependencies on the uv install location are created.

---

_Comment by @zanieb on 2024-05-21 17:12_

@charliermarsh Why were you opposed to doing it in 0.2.0?

---

_Comment by @charliermarsh on 2024-05-21 17:14_

I just didn't consider it an important change and hadn't had time to think through the consequences. How will this interact with tool installs? Do we know?

---

_Comment by @charliermarsh on 2024-05-21 17:15_

By "important", I mean "pressing" or "time-sensitive".

---

_Comment by @zanieb on 2024-05-21 17:32_

I don't see how it interacts with tool installs. I agree it's not pressing but I agree with Konsti that there is some time sensitivity in that the longer we wait there may be increased reliance on this install location downstream.

---

_Comment by @charliermarsh on 2024-05-21 17:33_

The interaction point is: where will binaries _installed by uv_ be placed?

---

_Comment by @zanieb on 2024-05-21 17:34_

True that's a consideration and an open question.

---
