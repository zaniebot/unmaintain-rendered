```yaml
number: 1637
title: WinGet-based installation
type: issue
state: closed
author: zkat
labels:
  - question
assignees: []
created_at: 2020-07-07T21:49:18Z
updated_at: 2024-01-27T09:04:04Z
url: https://github.com/BurntSushi/ripgrep/issues/1637
synced_at: 2026-01-12T16:13:24Z
```

# WinGet-based installation

---

_@zkat_

#### Describe your feature request

It would be really great to be able to install ripgrep on Windows through the [windows package manager, `winget`](https://docs.microsoft.com/en-us/windows/package-manager/winget/).


---

_Comment by @BurntSushi on 2020-07-07 22:00_

I'm always in favor of adding ripgrep to more package repositories, but it is generally not something I track as part of this project since I don't maintain those packages. If it is added, then a PR adding winget installation instructions to the README would be very welcome.

One exception to this is if there is no package repo and the package manager can install directly from git repos. Cargo and brew are examples of this, and both require specific configuration files in the repo itself. I don't know if winget falls into this category. The link you gave appears to be end user documentation for the tool. (I am not a Windows user and don't know much about how its package ecosystem works these days.)

---

_Label `question` added by @BurntSushi on 2020-07-07 22:01_

---

_Comment by @ideologysec on 2020-08-24 04:44_

WinGet appears conceptually similar to something like Homebrew on first pass; the place to put the YAML file describing where to get the package installer gets added [HERE](https://github.com/microsoft/winget-pkgs).

However - from a quick lookaround, winget does not actually do much package management. It requires an installer to do the actual heavy lifting for program installation. I have not found a way to get it to manually copy files to a specific directory and add entries to the registry. As such, it looks like ripgrep would have to start building an installer for windows to support winget.

Perhaps a powershell script that downloaded the release, unpacked it, moved it, and registered things properly in the path would suffice as an 'installer' for winget? arbitrary powershell scripts for installing things from the internet seems a bad idea, though. Ideally at least a ripgrep installer could be signed, though that requires several thousand dollars to get a cert for windows last I checked.

It might be possible to package it via the new MSIX installer format as part of the CI/CD pipeline, but I don't know enough about how automated packaging works on Windows to have much more to offer.

EDIT: I see there's a chocolatey package for ripgrep, but I can't find the spec file in this repo. Based on my understanding, though, chocolately allows you to specify all kinds of things like custom powershell scripts, etc, so it will do the work of copying, etc for you whereas winget cannot directly.

---

_Comment by @BurntSushi on 2020-08-24 11:58_

@ideologysec Aye, thanks for doing that investigation! I have no plans to create an installer for ripgrep since it seems like it isn't really necessary in general. Of course, if someone else wants to add and maintain a winget package for ripgrep, then that sounds great, but it looks like it takes quite a bit of work to do it. For that reason, I don't see myself taking on that burden myself.



> EDIT: I see there's a chocolatey package for ripgrep, but I can't find the spec file in this repo. Based on my understanding, though, chocolately allows you to specify all kinds of things like custom powershell scripts, etc, so it will do the work of copying, etc for you whereas winget cannot directly.

Right. A Chocolately package exists but it is not maintained by me.

---

_Closed by @BurntSushi on 2020-08-24 11:58_

---

_Comment by @sitiom on 2023-01-26 16:03_

Winget 1.4 has just released, supporting zip installations (https://github.com/microsoft/winget-cli/releases/tag/v1.4.10173). A PR is already underway to add ripgrep to winget-pkgs:

- https://github.com/microsoft/winget-pkgs/pull/88066
- https://github.com/microsoft/winget-pkgs/pull/88067

Once the PRs are merged, a [Winget Releaser](https://github.com/vedantmgoyal2009/winget-releaser) workflow would have to be set up in this repo for automatic package updates. @BurntSushi Are you ok with that?

---

_Comment by @BurntSushi on 2023-01-26 16:23_

As I said above, I don't maintain ripgrep packages. The _only_ exception to that is for things that _require_ configuration in this repository.

I am definitely not adding any kind of CI workflow to maintain something I don't understand and therefore cannot easily fix when it inevitably breaks.

---

_Comment by @jjoao on 2023-03-17 11:39_

I Tried the "winget install burntsushi.ripgrep.msvc"
Apparently it succeeded, 
it adds a very long and strange dir to the "PATH" /usr/me/AppData/local/microsoft/winget/Burn.../.ripgrep.MSVC_microsoft.winget.Source_8wekyb3d8......bwe
BUT it always "rv is not recognized" 
is it just me?
um abraço
JJoao

---

_Comment by @BurntSushi on 2023-03-17 11:48_

@jjoao As explicitly mentioned above, I don't maintain ripgrep packages. There are dozens of them because there are dozens of different package managers and package repositories. This is "upstream" but you have a "downstream" question. You need to ask the people that maintain the winget package you're trying to install.

---

_Comment by @mgutz on 2023-05-17 08:26_

> I Tried the "winget install burntsushi.ripgrep.msvc" Apparently it succeeded, it adds a very long and strange dir to the "PATH" /usr/me/AppData/local/microsoft/winget/Burn.../.ripgrep.MSVC_microsoft.winget.Source_8wekyb3d8......bwe BUT it always "rv is not recognized" is it just me? um abraço JJoao

Seems like the ripgrep package in the winget repos is broken. There is a sub directory beneath that path which includes the `rg` executable. The archive needs to be extracted without the directory. I went back to using scoop for CLI utilities.


---

_Comment by @sitiom on 2023-05-17 09:11_

> > I Tried the "winget install burntsushi.ripgrep.msvc" Apparently it succeeded, it adds a very long and strange dir to the "PATH" /usr/me/AppData/local/microsoft/winget/Burn.../.ripgrep.MSVC_microsoft.winget.Source_8wekyb3d8......bwe BUT it always "rv is not recognized" is it just me? um abraço JJoao
> 
> Seems like the ripgrep package in the winget repos is broken. There is a sub directory beneath that path which includes the `rg` executable. The archive needs to be extracted without the directory. I went back to using scoop for CLI utilities.

@mgutz Please see https://github.com/microsoft/winget-cli/issues/2909. This is already fixed in the latest pre-release of Winget https://github.com/microsoft/winget-cli/releases

---
