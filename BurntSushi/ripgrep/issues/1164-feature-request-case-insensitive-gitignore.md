```yaml
number: 1164
title: "Feature request: case-insensitive .gitignore matching on Windows"
type: issue
state: closed
author: davidtorosyan
labels:
  - enhancement
assignees: []
created_at: 2019-01-16T04:58:09Z
updated_at: 2019-01-23T01:04:44Z
url: https://github.com/BurntSushi/ripgrep/issues/1164
synced_at: 2026-01-12T16:13:23Z
```

# Feature request: case-insensitive .gitignore matching on Windows

---

_@davidtorosyan_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)

#### How did you install ripgrep?

Downloaded ripgrep-0.10.0-i686-pc-windows-msvc.zip from the release page and installed rg.exe to a location in my path.

#### What operating system are you using ripgrep on?

Windows 10 Pro
Version 1803 (OS Build 17134.523)

#### Describe your feature request.

I'd like ripgrep to support case-insensitive .gitignore matching.

On Windows, the file system is case insensitive which inspired this issue #163 and the subsequent --iglob flag. However, it looks like the flag can't be used to make exclusions from the .gitignore case insensitive.

Here's an example on the Windows command line. You'll notice that `git status` doesn't list `myfile.txt`, but ripgrep does (of course on linux this isn't the case, and the file is listed twice).

```
> mkdir rgtest
> cd rgtest
> git init
> echo foo > myfile.txt
> echo MYFILE.txt > .gitignore
> git status -s
?? .gitignore
> rg -l foo
myfile.txt
```

Sorry if I'm missing something obvious, or if this is covered by another issue. I tried searching the man page and didn't find any configuration regarding .gitignore, and found *many* issues related to case insensitive file matching but none as far as I could tell that asked for this specific feature.

---

_Label `enhancement` added by @BurntSushi on 2019-01-16 12:00_

---

_Comment by @BurntSushi on 2019-01-16 12:01_

It's possible I could get on board with enabling this functionality with a flag, but I think the better solution is to probably stop relying on the case insensitivity of your file system in your `.gitignore` files. However, that's a subjective assessment on my part.

If case insensitive globbing is enabled across ignore files, then the performance of matching ignore rules could substantially decrease in the presence of a large number of rules.

---

_Comment by @davidtorosyan on 2019-01-16 16:15_

I agree that better .gitignore files is a nicer strategy, especially because that way the repo would become a little closer to cross-platform. This works well for repos I own and I can advocate for others to do the same. 

That said, I often have to search across repos that I don’t own, and as long as .gitignore is case insensitive on Windows people will continue writing them in case insensitive ways. 

Is there a way I can help find out if the performance degradation of such a flag would be bad enough to keep me from using it (to at least provide one data point)? Maybe write a command with a giant --iglob that matches the .gitignore?

---

_Comment by @BurntSushi on 2019-01-16 16:21_

I think the simplest way would be to just patch ripgrep to do case insensitive matching. I think if you just hacked globset to always enable case insensitivity, then I think that will do the trick.

---

_Comment by @davidtorosyan on 2019-01-16 16:23_

Alright, I’ll see if I can give that a try. 

---

_Comment by @davidtorosyan on 2019-01-20 01:37_

I tried my patched version on my repos and it's fast enough for me, but I thought I'd gather some benchmarking data. I used the benchsuite detailed [here](https://blog.burntsushi.net/ripgrep/) but had trouble building the kernel, so I followed instructions from the [Arch Wiki](https://wiki.archlinux.org/index.php/Kernel/Traditional_compilation) instead. I downloaded [linux-4.20.3.tar.xz](https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.20.3.tar.xz), turned it into a git repo, and built.

For the hack, I changed [this line in gitignore.rs](https://github.com/BurntSushi/ripgrep/blob/master/ignore/src/gitignore.rs#L335) to `case_insensitive: true`.

Builds of ripgrep were done using `cargo build --release`. Other packages were installed from official arch repositories and AUR.

Setup:
```
$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
$ sudo mv /usr/bin/rg /usr/bin/rg.bkup
```
For my first run, tired running the benchmark with the official package.
```
$ sudo cp /usr/bin/rg.bkup /usr/bin/rg
$ benchsuite/benchsuite --dir ~/data/corpus --allow-missing linux_literal

linux_literal (pattern: PM_RESUME)
----------------------------------
rg (ignore)          0.444 +/- 0.012 (lines: 19)
rg (ignore) (mmap)   0.895 +/- 0.026 (lines: 19)
ag (ignore) (mmap)   0.826 +/- 0.004 (lines: 19)
pt (ignore)          1.006 +/- 0.004 (lines: 19)
git grep (ignore)    0.435 +/- 0.027 (lines: 19)
rg (whitelist)       0.483 +/- 0.060 (lines: 19)
ucg (whitelist)*     0.405 +/- 0.004 (lines: 19)*

linux_literal_casei (pattern: PM_RESUME)
----------------------------------------
rg (ignore)          0.460 +/- 0.001 (lines: 430)
rg (ignore) (mmap)   0.904 +/- 0.015 (lines: 430)
ag (ignore) (mmap)   0.873 +/- 0.061 (lines: 430)
pt (ignore)          19.943 +/- 1.453 (lines: 430)
git grep (ignore)*   0.398 +/- 0.007 (lines: 430)*
rg (whitelist)       0.416 +/- 0.006 (lines: 430)
ucg (whitelist)      0.444 +/- 0.022 (lines: 430)

linux_literal_default (pattern: PM_RESUME)
------------------------------------------
rg         0.441 +/- 0.058 (lines: 19)
ag         1.059 +/- 0.232 (lines: 19)
ucg        0.401 +/- 0.002 (lines: 19)
pt         1.014 +/- 0.004 (lines: 19)
git grep*  0.350 +/- 0.005 (lines: 19)*
```
Next up, my built copy from 7cbc535.
```
$ sudo cp ~/data/ripgrep_builds/rg_built /usr/bin/rg
$ benchsuite/benchsuite --dir ~/data/corpus --allow-missing linux_literal

linux_literal (pattern: PM_RESUME)
----------------------------------
rg (ignore)          0.405 +/- 0.001 (lines: 19)
rg (ignore) (mmap)   0.902 +/- 0.050 (lines: 19)
ag (ignore) (mmap)   0.898 +/- 0.067 (lines: 19)
pt (ignore)          1.018 +/- 0.021 (lines: 19)
git grep (ignore)*   0.353 +/- 0.004 (lines: 19)*
rg (whitelist)       0.359 +/- 0.002 (lines: 19)
ucg (whitelist)      0.403 +/- 0.001 (lines: 19)

linux_literal_casei (pattern: PM_RESUME)
----------------------------------------
rg (ignore)          0.429 +/- 0.002 (lines: 430)
rg (ignore) (mmap)   0.886 +/- 0.011 (lines: 430)
ag (ignore) (mmap)   0.836 +/- 0.006 (lines: 430)
pt (ignore)          19.139 +/- 0.283 (lines: 430)
git grep (ignore)    0.402 +/- 0.006 (lines: 430)
rg (whitelist)*      0.380 +/- 0.001 (lines: 430)*
ucg (whitelist)      0.454 +/- 0.044 (lines: 430)

linux_literal_default (pattern: PM_RESUME)
------------------------------------------
rg         0.398 +/- 0.002 (lines: 19)
ag         0.841 +/- 0.019 (lines: 19)
ucg        0.405 +/- 0.002 (lines: 19)
pt         1.004 +/- 0.001 (lines: 19)
git grep*  0.352 +/- 0.009 (lines: 19)*
```
Finally, my patched copy.
```
$ sudo cp ~/data/ripgrep_builds/rg_patched /usr/bin/rg
$ benchsuite/benchsuite --dir ~/data/corpus --allow-missing linux_literal

linux_literal (pattern: PM_RESUME)
----------------------------------
rg (ignore)          0.680 +/- 0.003 (lines: 19)
rg (ignore) (mmap)   1.100 +/- 0.011 (lines: 19)
ag (ignore) (mmap)   0.830 +/- 0.010 (lines: 19)
pt (ignore)          1.006 +/- 0.004 (lines: 19)
git grep (ignore)*   0.352 +/- 0.000 (lines: 19)*
rg (whitelist)       0.363 +/- 0.015 (lines: 19)
ucg (whitelist)      0.401 +/- 0.003 (lines: 19)

linux_literal_casei (pattern: PM_RESUME)
----------------------------------------
rg (ignore)          0.706 +/- 0.006 (lines: 430)
rg (ignore) (mmap)   1.150 +/- 0.073 (lines: 430)
ag (ignore) (mmap)   0.887 +/- 0.085 (lines: 430)
pt (ignore)          19.038 +/- 0.102 (lines: 430)
git grep (ignore)    0.410 +/- 0.010 (lines: 430)
rg (whitelist)*      0.380 +/- 0.001 (lines: 430)*
ucg (whitelist)      0.430 +/- 0.003 (lines: 430)

linux_literal_default (pattern: PM_RESUME)
------------------------------------------
rg         0.671 +/- 0.002 (lines: 19)
ag         0.854 +/- 0.030 (lines: 19)
ucg        0.417 +/- 0.023 (lines: 19)
pt         1.006 +/- 0.003 (lines: 19)
git grep*  0.350 +/- 0.004 (lines: 19)*
```
Overall it looks like ripgrep takes 1.5x longer with the patch. That's a pretty big hit, but it's already so fast that I personally wouldn't mind it.

Thoughts on including this? If you're interested I can try working on the changes myself - I haven't worked in Rust before but I've been looking for a good opportunity, and just adding a flag to enable this looks like it would be a pretty minor change (famous last words).

---

_Comment by @BurntSushi on 2019-01-20 01:43_

Excellent analysis, thank you! Yes, I would be OK adding this behind a flag.

In principle, you're right, the change is simple. There is a fair bit of plumbing though. ripgrep is split up into libraries, so features like this need to be part of the public API of a library. For example, I'd probably expect it to be a new option on a [`ignore::WalkBuilder`](https://docs.rs/ignore/0.4.6/ignore/struct.WalkBuilder.html) that in turn set [`ignore::gitignore::GitignoreBuilder::case_insensitive`](https://docs.rs/ignore/0.4.6/ignore/gitignore/struct.GitignoreBuilder.html#method.case_insensitive) appropriately.

If you want to give it a try, that'd be great! If you get stuck, feel free to just throw up a PR with what you've got and we can figure out how to go from there together.

---

_Comment by @davidtorosyan on 2019-01-20 02:20_

That makes sense, thanks for the direction. I'll give it a shot!

---

_Closed by @BurntSushi on 2019-01-23 01:04_

---
