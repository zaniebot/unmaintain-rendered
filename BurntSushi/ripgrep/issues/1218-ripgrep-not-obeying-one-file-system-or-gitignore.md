```yaml
number: 1218
title: ripgrep not obeying --one-file-system or .gitignore on OneDrive folder
type: issue
state: closed
author: Trucido
labels:
  - help wanted
  - question
  - icebox
assignees: []
created_at: 2019-03-11T10:07:54Z
updated_at: 2019-05-17T16:47:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1218
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep not obeying --one-file-system or .gitignore on OneDrive folder

---

_@Trucido_

#### What version of ripgrep are you using?
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
tested both versions from scoop and chocolatey:
scoop install ripgrep
choco install ripgrep

#### What operating system are you using ripgrep on?
Windows 10 Version 1809 (OS Build 17763.348)
(Same behavior on prior 1803 version)

#### Describe your question, feature request, or bug.
Running rg from home directory, or anywhere that encounters the OneDrive folder doesn't respect the `--one-file-system` option or .gitignore globs for OneDrive. (tested option in both CLI and in .ripgreprc)
Also tested adding various combinations of `Onedrive\*` to `~\.gitignore` and globs in `~\OneDrive\.gitignore` but it doesn't seem to recognize it.
The only thing that seems to work is adding this to .ripgreprc:
```
--glob
!OneDrive/*
```
Possibly somewhat related to #705 and I would consider this a bug if ripgrep is trying to read files not locally available (RECALL_ON_DATA_ACCESS) which triggers OneDrive to download the files.

Repro: Create file in `~\OneDrive` with uniquely identifiable text;  Right Click -> "Free Up Space"; run `rg --one-file-system` with that unique text and it triggers download of the file. (same with .gitignore stuff).

These files seem to have identifiable flags, but it's not entirely clear. I pieced together what I could and wrote a powershell script to parse these attributes and flags here: [Get-FileAttributesEx.ps1](https://gist.github.com/Trucido/02dc6b7f43df1eb6f0e5146f77282143)

Parsing which files ARE locally available and which are NOT would probably be a pain, but I think ripgrep should check some of these flags in the --one-file-system code somehow since they're sync-related flags (and likely used on more than just OneDrive). I'm unaware of an easier method of easily identifying such cloud and/or offline-online/sync related files,

Also, parsing attrib.exe output isn't reliable either.
External links:

- https://searchenterprisedesktop.techtarget.com/blog/Windows-Enterprise-Desktop/OneDrive-File-Attributes-Uncovered
- https://www.petri.com/managing-onedrive-files-demand-windows-10-fall-creators-update
- https://social.technet.microsoft.com/Forums/windows/en-US/375f3933-fcab-450c-bb9c-da54155549e2/how-do-i-getset-onedrive-quotfiles-on-demandquot-status-from-powershell?forum=ITCG
- https://superuser.com/questions/1214542/what-do-new-windows-8-10-attributes-mean-no-scrub-file-x-integrity-v-pinn/1287315
- https://www.tenforums.com/general-support/102804-why-these-p-u-file-attributes-suddenly-appearing.html
- https://docs.microsoft.com/en-us/windows/desktop/FileIO/file-attribute-constants
- https://docs.microsoft.com/en-us/windows/desktop/api/fileapi/nf-fileapi-createfilea

---

_Comment by @BurntSushi on 2019-03-11 11:20_

The way you're using `.gitignore` seems strange. Just do `echo '*' > ~\OneDrive\.ignore` (or whatever the Windows equivalent of that is).

The way `--one-file-system` is implemented on Windows is by getting the volume serial number containing the given file. I have no idea how that interacts with OneNote, and honestly, it's hard for me to make sense of the information you've flooded me with. I'm not a Windows user nor am I experienced with Windows development, so I think someone else will need to perform the research task of figuring out more precisely what's happening.

---

_Label `question` added by @BurntSushi on 2019-03-11 11:20_

---

_Label `icebox` added by @BurntSushi on 2019-03-11 11:20_

---

_Label `help wanted` added by @BurntSushi on 2019-03-11 11:20_

---

_Comment by @Trucido on 2019-03-12 13:26_

Sorry for the walls of text/urls, I was hoping someone could make sense of this and hopefully help rg's `--one-file-system` flag work like it does on other OS's.
――――――――――――――――

> `echo '*' > ~\OneDrive\.ignore`

This worked! Thanks, but for now is just a band-aid fix. Probably will become an issue again in the future with other sync providers and newer pseudo-file types for containers and/or cross-linked interop stuff with windows and linux containers and VMs and other special files/directories.
――――――――――――――――

Q: I thought rg parsed gitignore files? These didn't seem to work:
```
# ~\.gitignore
OneDrive/*`
```
```
# ~\OneDrive\.gitignore
*
## (or various combinations of wildcards)
```
If not, sorry for wrongly assuming. I suppose .gitignore files could be symlinked/hardlinked to a '.ignore' file.
――――――――――――――――

> The way `--one-file-system` is implemented on Windows is by getting the volume serial number containing the given file.

That works in most cases, but there's a lot more "filesystem" types than are exposed by parsing volumes. (and I won't even get into PSDrives...)
In this OneDrive issue though, it's technically a sync provider folder, which are implemented strangely. I'm not sure how other sync providers and sync services implement these things; though there must be a standardized API of some sort to determine this stuff; otherwise it would be insanity.
――――――――――――――――

> I'm not a Windows user nor am I experienced with Windows development, so I think someone else will need to perform the research task of figuring out more precisely what's happening.

Understandable - rg works great on *nix platforms, though feature-parity with other OS's would be ideal. I'm not expecting you to do all the "legwork" on platforms you don't even use - the existing work put into the windows port is much appreciated.

Q: How does rg handle *nix files like sockets, FIFOs, mounted shares, fuse/overlayfs and/or btrfs subvols? I suppose /proc exposes more easily accessible information; but on windows parsing volume serial numbers seems valid only for physical drive(s) and certain types of pseudo-block devices. (Though I think MSFT is rehauling the storage system for hopefully more sanity).
――――――――――――――――


Anyway, I too am not so experienced in windows development (or rust), but if I can help I'd be glad to; and hopefully someone else can provide some insight into the appropriate APIs (or rust libs) to expose the file/directory/filesystem information we seem to be lacking on windows platforms.
If I have some time, I'll try and look at the code of similar tools and see how they implement `--one-file-system` flags. Also maybe there's some python and nodejs modules with decent code examples of doing this properly.

Perhaps this issue title should be changed to an enhancement such as "Improve windows --one-file-system detection"?



---

_Comment by @BurntSushi on 2019-03-12 18:07_

ripgrep does handle .gitignore, but only in git repos.

> hopefully help rg's --one-file-system flag work like it does on other OS's.

It does. Looking at the volume serial number (or the device number on Unix) is the correct way to implement this. It is how every other cli tool with a similar feature implements this. You are asking for something very different. You are asking for ripgrep to become aware of N different file synchronization services that each probably have their own way of working. This isn't practical.

I'm not in principle against supporting this request, but it needs to have a simple contained solution with low maintenance burden.

---

_Comment by @Trucido on 2019-05-17 16:01_

(Sorry late reply)
> You are asking for ripgrep to become aware of N different file synchronization services that each probably have their own way of working. This isn't practical.

Sorry only bringing up issue which may get larger in future (thus harder to change codebase).
Using .ignore files works but also much clutter.
RIPGREP_CONFIG_PATH with ignore globs works without clutter but has other sideeffects. (Is there global blacklist/ignore file support?)

Maybe you're right and most unix/linux utils only check `/proc/mounts` which is equivalent to checking windows volumes but maybe this is larger issue on both platforms then... especially with containers and virtualization, snapshot type mounts, checksum/archive files and other such complex xattr stuff.

I know you're not primarily a Windows user but feel this should still be kept open in case someone else with more rust experience has some insight and the time to implement xattr/attribute checking or knows of an easy rust lib since I lack the time and knowledge to do the legwork on this 

Most windows utils I work on use the GetFileAttributes family of Cpp macros (or equivalent C and dotnet methods), there's lots of FOSS ntfs libs for doing this too.
`rg GetFileAttribute "%programfiles(x86)%\Windows Kits"`  for some context on these things (if windows SDK(s) are installed

Not ripgrep's fault MSFT botched this, the new NTFS attributes are sparsely documented and whats worse is older windows headers do not even return anything meaningful except the hex values.

A few projects I work on use these GetFileAttributes APIs to check for even simple stuff like directories or no write access or to try and catch with return codes. (i.e. file logging/creation/deleting). Maybe not relevant to rust, I don't know rust much.

P.S. (I still heavily use rg on nix, win, and various virt platforms and appreciate the support for such OS's. unlike some rust utils in 'oreutils' like bat and exa which have some larger issues (exa doesn't even compile), but rg works consistently well)

---

_Comment by @BurntSushi on 2019-05-17 16:47_

Thanks for the follow up. I'm going to close this for now. If someone wants to do the research to figure out the correct way to implement this, _and_ it's a well contained and easy to maintain solution, then we can re-open this issue.

---

_Closed by @BurntSushi on 2019-05-17 16:47_

---
