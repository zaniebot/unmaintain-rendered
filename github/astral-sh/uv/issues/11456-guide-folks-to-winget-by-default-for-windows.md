---
number: 11456
title: Guide folks to WinGet by default for Windows installations in README
type: issue
state: open
author: localden
labels:
  - enhancement
assignees: []
created_at: 2025-02-12T18:02:03Z
updated_at: 2025-11-14T09:41:17Z
url: https://github.com/astral-sh/uv/issues/11456
synced_at: 2026-01-07T13:12:18-06:00
---

# Guide folks to WinGet by default for Windows installations in README

---

_Issue opened by @localden on 2025-02-12 18:02_

### Summary

Currently, the `README` is guiding folks to run a PowerShell script, bypassing the local execution policy. This is _generally_ considered a bad practice because it conditions users to run arbitrary code without controls. Recommend switching to `winget` as the default for Windows, especially since this is already covered [in the documentation](https://docs.astral.sh/uv/getting-started/installation/#winget).

Happy to open a PR for this as well - should be an easy fix.

This is also related to #11021, but is just not covered in the README

### Example

_No response_

---

_Label `enhancement` added by @localden on 2025-02-12 18:02_

---

_Referenced in [astral-sh/uv#11457](../../astral-sh/uv/pulls/11457.md) on 2025-02-12 18:08_

---

_Comment by @localden on 2025-02-12 18:17_

@zanieb wanted to bring the conversation here since the PR is closed.

How can I help the team maintain an official `winget` package? For Windows, `winget` should now be the default package manager (it's built into the OS), and behind the scenes it can effectively do what your PowerShell script does.



---

_Comment by @zanieb on 2025-02-12 18:34_

Thanks for switching to the issue — seems better to chat here and I'm happy to talk about it.

A couple functional concerns (other team members may have more):

1. `uv self update` is powered by our installer, I think this is valuable to our users and I don't want to have to recommend they switch installation methods _after the fact_ to get access to the command.
2. The installer will warn if the installed executable is shadowed by another, this is important for our users who can easily end up with multiple versions of uv from different sources.

Do you have thoughts about those?

Regarding maintaining an official `winget` package, I think we'd be willing to get involved but I haven't maintained a package there before so I'm not sure how much effort this would be. cc @Gankra 

It looks like in, e.g., https://github.com/microsoft/winget-pkgs/pull/226972, it can take some iterative work to get a release out? I would be disappointed if our recommended installation method on Windows took significantly longer to get new versions than other channels.

---

_Comment by @konstin on 2025-02-12 18:55_

Sorry if this is a naive question (coming from someone only occasionally developing on windows), but how universally available is winget compared to our requirements for a powershell installation? Are there cases where a user would have to install winget manually first, for example for a server or containers?

Are there any other differences, for example related to PATH precedence or permissions we have to consider with a winget installation?

---

_Comment by @localden on 2025-02-12 19:08_

I'll tag in @crutkas here as the PM driving the Windows developer story (which includes `winget`).

I fully agree that changing installers would be _super_ jarring. I suspect that if one would use `winget` to install, then `uv self update` will not work at all in the [current implementation](https://github.com/astral-sh/uv/blob/3f6a7f987928424fa8340015b39e3ca485431514/crates/uv/src/commands/self_update.rs#L52C37-L52C48), given that the executable paths will be completely different? If the updater is able to determine reliably that a package is sourced from `winget` on Windows (although I recognize that this is likely a Windows-specific code path that might not be optimal for a generic script), would it be able to rely on the built-in package manager to update?

In terms of package maintenance, I don't want to overly trivialize it, but it's [essentially a manifest](https://github.com/microsoft/winget-pkgs/blob/master/manifests/a/astral-sh/uv/0.5.29/astral-sh.uv.installer.yaml) that needs to define the artifacts that you already provide through your releases. There is [tooling available](https://github.com/microsoft/winget-create) that simplifies the process.

To @konstin's question, `winget` should be available starting with [Windows 10 version 1809](https://devblogs.microsoft.com/commandline/windows-package-manager-1-0/) (Windows 10 October 2018 Update) and later. It is possible in certain scenarios that `winget` is not on the box and it will require the user to install it first (e.g., older versions of Windows).

I'll let the `winget` folks (@crutkas can bring in additional experts) to chime in on specific technical details, such as `PATH` precendence and permission model.



---

_Comment by @zanieb on 2025-02-12 19:18_

> ... would it be able to rely on the built-in package manager to update?

Maybe. I know, for example, that the `fly` CLI will self-update with HomeBrew — but we haven't investigated that at all yet. The installer already is implementing some platform-specific logic.

A related question: are there people on your team that would invest into making this work well enough for us to recommend it as the default? If so, that would be helpful for prioritization.

I basically agree with the premise that it'd be great to avoid bypassing system policies for the installer to work consistently.

I also worry a little about installing to a different path. Installing to a consistent location across platforms is generally a better experience for our users. But, more importantly, `uv tool install` needs to install executable entrypoints _somewhere_ and if it's the same location as `uv` itself is installed in then it's already on the `PATH`.

---

_Comment by @Gankra on 2025-02-12 19:37_

Adding context: the prebuilt "trampoline" binaries we ship on Windows currently rely on a being able to set the utf8 codepage in manifests, which is apparently [already making us require win10 1903](https://github.com/astral-sh/uv/pull/11049#issuecomment-2624563587) which I consider acceptable.

---

_Comment by @chrisrodrigue on 2025-02-12 21:13_


> 1. `uv self update` is powered by our installer, I think this is valuable to our users and I don't want to have to recommend they switch installation methods _after the fact_ to get access to the command.

Related: 
[Add uv self install](https://github.com/astral-sh/uv/issues/9421)
[Add uv self uninstall](https://github.com/astral-sh/uv/issues/9871)

I think it is worth considering `winget` as the method to download the `uv` binary, and then utilize `uv` itself to perform its own installation.

IMO, `uv self update` should not rely on an untracked/cached powershell script somewhere on the system.

`winget` plays nicely with programs that can update themselves. I've updated programs manually and noticed that `winget` then reports those new versions properly. uv may need to update a few registry keys to play nicely with this (or show an entry in the Control Panel programs list), but the `winget` devs will know way more on the subject.

---

_Comment by @chrisrodrigue on 2025-02-12 21:20_

`winget` manifests can specify several things such as the software license, installer download URL, and arguments that should be passed to the installer depending on the scope (i.e., user or machine).

In this case, `winget` should just download `uv.exe` and then call `uv.exe self install --user` if launched in an unprivileged terminal/process (user scope) and `uv.exe self install --system` if launched in a privileged terminal/process (machine scope).

Example configs:
- [Git](https://github.com/microsoft/winget-pkgs/blob/master/manifests/g/Git/Git/2.47.1.2/Git.Git.installer.yaml)
- [Python](https://github.com/microsoft/winget-pkgs/blob/master/manifests/p/Python/Python/3/13/3.13.2/Python.Python.3.13.installer.yaml)

---

_Comment by @zanieb on 2025-02-12 21:24_

> IMO, uv self update should not rely on an untracked/cached powershell script somewhere on the system.

It doesn't, but our updater relies on metadata about the installation that the installer writes.

> ... and then utilize uv itself to perform its own installation.

This is.. interesting. I'm not necessarily opposed to that, but it'd be a significant shift in how we're distributing uv (and Ruff) and  I'm not sure we have the resources to work on that right now.

---

_Comment by @chrisrodrigue on 2025-02-12 21:30_

> > IMO, uv self update should not rely on an untracked/cached powershell script somewhere on the system.
> 
> It doesn't, but our updater relies on metadata about the installation that the installer writes.
> 

Ah okay, this makes a lot more sense... thanks for correcting my assumption!

> > ... and then utilize uv itself to perform its own installation.
> 
> This is.. interesting. I'm not necessarily opposed to that, but it'd be a significant shift in how we're distributing uv (and Ruff) and I'm not sure we have the resources to work on that right now.

That's completely understandable. Overall, I love the portability of uv. An MSI or EXE installer seems like it would be overkill.


---

_Comment by @Paciupa on 2025-02-13 02:18_

WinGet packages actually are not real packages. They are yaml manifests created according to [schema](https://github.com/microsoft/winget-pkgs/tree/master/doc/manifest/schema/1.9.0) with meta information including Installer URL. And there is no repository with binary files, only tons of yamls on [GitHub](https://github.com/microsoft/winget-pkgs). To add the program or a new version in WinGet, someone (developer, bot or user) has to  just create a PR with the correct meta.

A lot of software isn't represented in WinGet, a lot of updated with bots like [uv](https://github.com/SpecterShell/Dumplings/blob/main/Tasks/astral-sh.uv/Script.ps1) that just parse software sites or regular users that use tools like [Komac](https://github.com/russellbanks/Komac) and a few are updated by developers like [vim](https://github.com/vim/vim-win32-installer/blob/master/.github/workflows/winget_stable.yml) or [yazi](https://github.com/sxyazi/yazi/blob/main/.github/workflows/publish.yml) with [winget-releaser](https://github.com/marketplace/actions/winget-releaser) GitHub action.

WinGet allows consistently upgrade all software with just one command `winget upgrade --all`. But its logic is limited: check dependencies, check new version, download and execute installer. In case of archive like uv it checks Visual C++ Redistributable (install it if needed), downloads and unpacks zip from GitHub release, adds uv and uvx in $PATH and adds records into the Registry. So I think additional uv installer logic like manipulation with Cargo stuff can be a problem.

I propose at least to publish a new version in WinGet using [winget-releaser](https://github.com/marketplace/actions/winget-releaser) by adding it in uv CI.

---

_Comment by @Trenly on 2025-02-13 09:43_

Currently, the manifest over at winget-pkgs specifies that the application is a portable application inside of a zip file. What this means is that WinGet handles the "installation" of the package by extracting the contents of the zip file, adding the executable to PATH, and (most importantly) creates the registry entries to make it appear in Control Panel / Add and Remove Programs. 

The registry entries are the most important part for users of WinGet, as they are what WinGet uses to determine the installed version of an application. 

It would be possible to change the manifest at winget-pkgs to call `uv.exe self install <--user|--machine>`, *but* that would be dependent upon 
1) `uv.exe` writing *and updating* its own registry entries
2) The updated install method passing all of the security scans at winget-pkgs
3) The behavior actually being compiled into the exe and not just the exe using a script included in the zip file (winget-pkgs has a strict no-scripts-as-installers policy)

-----

Regarding releasing new versions to / through WinGet, it is fairly straightforward as others have mentioned. The most reliable way is to have a GitHub action triggered on release that uses winget-create (Microsoft's own tool) to push a PR up to the winget-pkgs repository.

This really only requires 
1) The URLs to download the installer
2) The package version
3) A GitHub access token for creating the PR

This creates the mmanifest (i.e metadata files) and raises them as a PR into winget-pkgs.

-----

Once a PR is created at winget-pkgs, the pipelines there will run a series of validations using the new manifest to ensure the application installs successfully. This is where the iterations @zanieb mentioned come in. First, the installers must pass a SmartScreen check - if the URLs to download the application don’t have enough reputation on SmartScreen, the install fails (but, retrying every 12 hours or so means it usually succeeds eventually, once the URLs have gone through SmartScreen's process). After that, the application is installed and a number of malware checks are performed. Finally, the installed application is launched to be sure that it was installed and runs without issue, then a final system scan is performed.

If any of the validations fail, the PR is prevented from merging and the new version would not be available. If the validations succeed, a moderator must review the PR to ensure the quality of the metadata in the manifest. After passing the pipeline and being approved by a moderator, the manifest enters the publishing queue and is generally published within the next hour.

---

_Comment by @denelon on 2025-02-13 21:06_

Perfect description @Trenly. @crutkas and I work together on "Developer Experiences". I'm the PM for WinGet if there are any questions, but it looks like all the information has been shared here.

---

_Comment by @Gankra on 2025-03-08 00:51_

Minor update: we plan to integrate WinGet into our publish process for uv and ruff (with presumably [winget-releaser](https://github.com/marketplace/actions/winget-releaser)), so we can ensure the package's definitions are properly maintained and hopefully enabling us to shepherding any failures in winget-pkgs PRs so that releases don't stall out for days.

Making it the *preferred* option over the powershell installer will follow if we're happy enough with the result, and when we have time to prioritize remaining UX holes like `uv self update`.

---

_Comment by @crutkas on 2025-03-08 01:23_

@Gankra  one thing we do with PowerToys is actually manage our releases via a github action. Winget releaser is a option as well.

Here is PowerToys does: https://github.com/microsoft/PowerToys/blob/main/.github/workflows/package-submissions.yml

---

_Comment by @crutkas on 2025-03-08 01:28_

if it would be good to have a quick chat to see if we can help, please email us and happy to set up time.  crutkas@microsoft.com

---

_Comment by @localden on 2025-08-06 00:28_

Just checking in here @Gankra @crutkas - anything we can help with to get the `winget` ball rolling?

---

_Comment by @zanieb on 2025-08-06 13:50_

We just don't have the resources to dedicate to driving this forward at the moment, though we remain interested. I started prototyping `uv self install` and `uv self update` implementations which do not rely on axo / cargo-dist, but I think it'd be a long tail of work to reach feature parity.

---

_Comment by @SimonMerrett on 2025-11-14 09:41_

I'd just like to add my support for a better WinGet experience. It has been a brilliant tool for me as I dip into package-managed life. Unfortunately, I have not been able to successfully install it using WinGet it as a non-admin user, despite installing it with machine scope from the local admin account and trying to manually set Path environment variable. Not trying to add an issue to another one, but just context in case "better experience" doesn't give any clues as to what direction to take development.

---
