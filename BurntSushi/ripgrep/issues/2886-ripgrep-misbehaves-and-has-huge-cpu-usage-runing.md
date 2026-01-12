```yaml
number: 2886
title: Ripgrep misbehaves and has huge CPU usage runing on VSCodium from inside distrobox
type: issue
state: closed
author: MateusRodCosta
labels:
  - invalid
assignees: []
created_at: 2024-09-11T16:32:55Z
updated_at: 2024-12-16T21:00:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2886
synced_at: 2026-01-12T16:13:25Z
```

# Ripgrep misbehaves and has huge CPU usage runing on VSCodium from inside distrobox

---

_@MateusRodCosta_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

No idea, apparently it's from either VSCodium or a VSCodium extension

### How did you install ripgrep?

Same as above, it's apparently internal to either VSCodium or from a VSCodium extension

### What operating system are you using ripgrep on?

Host is Fedora Silverblue 40. ripgrep is apparently invoked by something from a VSCodium install inside a Fedora 40 distrobox.

### Describe your bug.

Ever so often when using VSCodium I will notice a random high CPU usage while I am not compiling anything or otherwise doing something that makes sense to trigger it.
By looking at any system resource monitor usage I will notice that it's usually cause by VSCodium, more precisely by a `rg`.

I usually solve this by killing it and the issue goes away for a while. It could be caused by any reason ranging from the atomic/immutable nature of the host file system or the container nature of the distrobox.

The command that is run is apparently as follow:

```
/usr/share/codium/resources/app/node_modules.asar.unpacked/@vscode/ripgrep/bin/rg --files --hidden --case-sensitive --no-require-git -g **/package.json -g !**/.git -g !**/.svn -g !**/.hg -g !**/CVS -g !**/.DS_Store -g !**/Thumbs.db -g !**/{node_modules,.vscode-test}/** --no-ignore --follow --no-config --no-ignore-global
```

### What are the steps to reproduce the behavior?

Install VSCodium inside a distrobox, use normally

### What is the actual behavior?

Huge unneeded CPU usage by ripgrep

### What is the expected behavior?

No unneeded CPU usage

---

_Comment by @MateusRodCosta on 2024-09-11 16:35_

Potentially related to https://github.com/microsoft/vscode/issues/225767, in case it's not a ripgrep issue but one in how VS Code uses it

---

_Comment by @BurntSushi on 2024-09-11 16:38_

This isn't actionable on my end. Most of your issue is explaining an interaction point with a piece of software that uses ripgrep, and you haven't identified any specific issue with ripgrep itself. Ripgrep using CPU isn't a ripgrep problem. It's just doing what has been asked of it.

You need to provide an MRE for ripgrep specifically. I'm not going to install random IDEs to guess at and debug your workflow in the hope of reproducing the same thing as you. You need to provide not just a specific ripgrep command, but a corpus along with expected and actual results.

---

_Closed by @BurntSushi on 2024-09-11 16:38_

---

_Label `invalid` added by @BurntSushi on 2024-09-11 16:39_

---

_Comment by @MateusRodCosta on 2024-09-11 17:20_

> This isn't actionable on my end. Most of your issue is explaining an interaction point with a piece of software that uses ripgrep, and you haven't identified any specific issue with ripgrep itself. Ripgrep using CPU isn't a ripgrep problem. It's just doing what has been asked of it.

Ah, my bad, yeah it's likely some weird interaction between VS Code (or one of its extensions) and ripgrep, no idea if distrobox even matters here.

> You need to provide an MRE for ripgrep specifically. I'm not going to install random IDEs to guess at and debug your workflow in the hope of reproducing the same thing as you. You need to provide not just a specific ripgrep command, but a corpus along with expected and actual results.

I am not very sure how to debug this, as I don't know if it's a native VSCode feature that triggers it or a extension. I will see if I can reproduce and then I might re-open if it turns out to be ripgrep-specific. Or, in case it's not, report to the appropriate channels.

I did find this pointing that a specific VS Code extensions (in this case one related to PHP) was able to trigger it:
https://community.devsense.com/d/877-high-cpu-usage-of-rg-process-when-symlinks-are-used
There is also this closed issue: https://github.com/microsoft/vscode/issues/186279

In any case I will investigate further and I might come back when I get more info, sorry for the inconvenience and thanks for the patience.

---

_Comment by @MateusRodCosta on 2024-09-14 15:03_

@BurntSushi I did figure out something that might be close to a "MRE", I maintain three flatpaks for Flathub (ChibiTracker, Digital and dtipbijays).
Apparently sometimes when I build one of those flatpaks, rg might start taking ~80% and basically never stopping taking 
CPU (it even starts spinning the laptop fans). When that happens apparently just deleting the `build-dir/` (where the files that will be in the final flatpak are saved) and `.flatpak-builder/` (where any compilation step actually happens) folders will just make the `rg` process go away.

I would guess then that VSCode is telling ripgrep to keep accessing files in those folder and, in case there are too many files, it takes too long to complete and uses a lot of CPU for prolonged time.

---

_Comment by @BurntSushi on 2024-09-14 15:36_

An MRE means you can provide precise instructions for another human to reproduce the issue you're seeing.

---

_Comment by @MateusRodCosta on 2024-12-16 20:54_

Ok, I do have a bit more info on what is likely to be causing this.

* This issue seems to only appear when after building a relatively complex flatpak
* This likely is because of the `.flatpak-builder/` (files used during the build) and `builddir/` (output folder) directories
* This is potentially caused by the aforementioned folders containing many files and not being on `.gitignore`
* In certain situations, usually at the same time as the high CPU usage, the VCS VS Code integration also complains it can't track that many files at once
* Always by just deleting `.flatpak-builder/` an `builddir/` the CPU usage gets lowered soon after

So, some VS Code component uses ripgrep to track the several files on the project, and the flatpak building trips it up.
It could potentially be VCS, search, file list or something of the sort.

> An MRE means you can provide precise instructions for another human to reproduce the issue you're seeing.

The issue here is that I really can't think of any MRE that wouldn't be: "download this manifest and build the flatpak using flatpak-builder".
Because I don't know yet how to fully separate it from "ripgrep has an issue with the flatpak build files".

---

_Comment by @MateusRodCosta on 2024-12-16 21:00_

For reference, https://github.com/microsoft/vscode/issues/233703 is a newer issue that seems more closely related to this, I will move to following that

---
