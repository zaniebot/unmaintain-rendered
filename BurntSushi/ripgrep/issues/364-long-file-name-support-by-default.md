```yaml
number: 364
title: long file name support by default?
type: issue
state: closed
author: davidziman
labels:
  - help wanted
  - question
  - icebox
  - rollup
assignees: []
created_at: 2017-02-14T03:04:35Z
updated_at: 2023-09-28T11:26:22Z
url: https://github.com/BurntSushi/ripgrep/issues/364
synced_at: 2026-01-12T16:13:21Z
```

# long file name support by default?

---

_@davidziman_

Seems long file names may not work in the Windows x64 build of ripgrep 0.4.0.

In cmd on windows 10 (Version 1607 OS Build 14393.693) do the following:

```batch 
cd c:\
mkdir ar
cd ar
echo "foobar" > abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrs
```

Execute rg:
```batch
rg "foobar"
```

Result:
`
./abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrs: The system cannot find the path specified. (os error 3)
`

Try again:
```batch
rg "foobar" \\?\%cd%
```

Result:
`
\\?\c:\ar\abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrs
1:"foobar"
`

It would be nice to have long file name support by default without having to add `\\?\%cd%` .





---

_Comment by @BurntSushi on 2017-02-14 03:11_

Someone will have to explain how to achieve to this in enough detail for me to do it, or someone else will need to submit a PR.

---

_Comment by @JFLarvoire on 2017-02-15 10:15_

I have a solution:
I've published on https://github.com/JFLarvoire/SysToolsLib/tree/master/C/MsvcLibX a Microsoft C library extension, with support for long file names >> 260 characters, UTF-8 file names, symbolic links, etc.
By building ripgrep with MsvcLibX, you should automatically inherit all these capabilities.
Note that MsvcLibX currently has support for many file and directory access functions, but not all. I've never tried it with programs using memory map functions like mmap(). Anyway, it should be relatively easy to add any missing function.

---

_Comment by @BurntSushi on 2017-02-15 12:01_

@JFLarvoire Thanks for the pointer!

Allow me to be clearer: I am **not** a Windows programmer. I know almost nothing about it or its ecosystem. Anything that complicates the build process is something I'm unlikely to want to add. I would rather not add additional C libraries (for Windows or Linux).

If standard Windows tooling doesn't support long file paths, then I'm not sure what to do. Someone will need to explain the actual problem in depth. If it's fixable in Rust, then we should do that. Pointing to a giant C library doesn't really help me understand. Sorry. :-( Maybe you could point specifically to where long file paths are handled?

---

_Comment by @JFLarvoire on 2017-02-15 13:28_

A bit of history:
The WIN32 API was created for Windows 95. In Windows 95, the FAT32 file system was limited to 260 characters paths. When they introduced the NTFS file system in NT, even though this file system had no such limitation anymore,  for backwards compatibility reasons, they kept that 260-character limitation in the WIN32 APIs. (Educated guess: Some old version of Word broke due to fixed size buffers overflowing when longer pathnames were encountered?)
Still, to allow long-path-aware programs to use pathname longer than that, they documented that these programs should prefix pathnames with the special string "\\\\?\\".

This is a real problem: I have many systems at work with deep directory trees, with paths longer than 260 characters. Most publicly available WIN32 ports of Unix tools (like find or ag) break on these systems. (And as an aside, they usually also break on pathnames containing Unicode characters > \u00FF; And misbehave if there are symbolic links.)

My MsvcLibX library redefines open(), fopen(), exec(), etc, and prepends that special "\\\\?\\" string to all pathnames it receives, before passing it on the Windows' CreateFile(). It also defines Standard C library directory access routines missing in MS libc.
I use it for all my system management tools, and this ensures they work reliably on all our systems at work. Including the ones with deep directory trees. Including the ones localized in Russian or Chinese. Including the new ones with symbolic links. Including the old XP systems that do NOT support the symbolic link APIs. Etc.

Give me a few days: I'll try to build ripgrep with MsvcLibX, and I'll report about the difficulties and benefits.


---

_Comment by @BurntSushi on 2017-02-15 13:37_

@JFLarvoire Thanks for the explanation! It sounds like the key problem will be that ripgrep uses Rust's standard library for file system stuff, so you can't "just" hook up fopen/exec/etc. I do believe that ripgrep should be able to handle symbolic links and Unicode characters in paths on Windows.

> and prepends that special `\\?\` string to all pathnames it receives

Interesting. I wonder if this is something Rust's standard library should be doing. @retep998 thoughts?

---

_Comment by @retep998 on 2017-02-15 13:57_

MsvcLibX won't fix anything automatically for Rust because Rust doesn't use libc for IO so redefining libc functions is worthless. What would fix things is detecting non-verbatim paths that are over `MAX_PATH` and automatically converting them into verbatim paths, which would necessitate calling `GetFullPathNameW` because paths have to be normalized first (naively prepending `\\?\` would break on relative paths or paths with `..` or `.` in them). There's already an open issue for that https://github.com/rust-lang/rust/issues/32689

---

_Comment by @JFLarvoire on 2017-02-15 14:32_

Well good luck fixing that in Rust's library. This will be really useful. I'm looking forward to getting a ripgrep version for Windows with the fix.
Note that Rust is not alone: I've just started to work with Python, and despite its widespread use, guess what, the Windows version fails to open files in deep paths too!

---

_Comment by @BurntSushi on 2017-02-15 15:11_

FWIW, I probably won't be fixing this myself any time soon. If there were a simple way to patch ripgrep for some cases, then I'd be willing to accept a PR for that, even if it doesn't correctly handle every case. But I can't think of anything simple off the top of my head.

---

_Comment by @retep998 on 2017-02-15 21:42_

Fortunately since Windows 10 version 1607 you can now enable long path support for all paths, not just verbatim paths, as long as you're calling certain W functions. All you need to do is either set a flag in your manifest to enable it for the application or edit your registry to enable it globally. I believe std calls all the right functions, but it may be worth verifying what various crates use.

The allowed functions: 
* CopyFile2
* CopyFileExW
* CopyFileW
* CreateDirectoryExW
* CreateDirectoryW
* CreateFile2
* CreateFileW
* CreateHardLinkW
* CreateSymbolicLinkW
* DeleteFileW
* FindFirstFileExW
* FindFirstFileNameW
* FindFirstFileW
* FindFirstStreamW
* FindNextFileNameW
* FindNextFileW
* FindNextStreamW
* GetCompressedFileSizeW
* GetCurrentDirectoryW
* GetFileAttributesExW
* GetFileAttributesW
* GetFinalPathNameByHandleW
* GetFullPathNameW
* GetLongPathNameW
* MoveFileExW
* MoveFileW
* MoveFileWithProgressW
* RemoveDirectoryW
* ReplaceFileW
* SearchPathW
* SetCurrentDirectoryW
* SetFileAttributesW

---

_Comment by @JFLarvoire on 2017-02-16 13:22_

@retep998 
> naively prepending \\\\?\\ would break on relative paths or paths with .. or . in them

Indeed. My subroutine that does it is [there](https://github.com/JFLarvoire/SysToolsLib/blob/master/C/MsvcLibX/src/mb2wpath.c), and it handles all the cases you mention plus a few others. (And conversely, if you know about cases I forgot, please tell me!)

>Fortunately since Windows 10 version 1607 you can now enable long path support for all paths

This is a workaround for the technical users who know how to enable this option in the registry. For all others who are running Windows 10 with default settings, or Windows 7 or 8 anyway, the \\\\?\\ method is the only one that will work.


---

_Label `help wanted` added by @BurntSushi on 2017-02-18 17:39_

---

_Label `question` added by @BurntSushi on 2017-02-18 17:39_

---

_Label `icebox` added by @BurntSushi on 2017-03-12 21:09_

---

_Comment by @BurntSushi on 2018-02-01 12:06_

@chrmarti @roblourens Do either of you know how to enable long path name support on a per-application basis? Is there something I can do to the ripgrep release artifact that will fix it?

---

_Comment by @andyleejordan on 2018-02-01 16:55_

@retep998 I do not believe the app manifest is capable of enabling opt-in long path support. I interpreted the docs the same way, but try as I might, it didn't work. It seems it can only be used to _disable_ long path support per-app on a system that has opted in via the registry (or group policy).

The approach Mesos and CMake took was to prepend `\\?\` to absolute paths longer than 248 characters (the "minimal" max path due to the `CreateDirectory`API). If there is a better way, I am all ears.

It's probably only marginally useful as it's C++ not Rust (so like, this is what the Rust standard libraries should probably implement), but the implementation is [here](https://github.com/apache/mesos/blob/master/3rdparty/stout/include/stout/internal/windows/longpath.hpp).

---

_Comment by @BurntSushi on 2018-02-01 17:04_

@andschwa Interesting, thanks! How do you deal with relative paths? That is, what if ripgrep is already deep in a directory hierarchy and you run `rg foo ./`. It will try to open files using relative paths, but from my experimenting, it actually looks like the path limit is applied to the fully expanded canonical path even if the relative path is very short. In that case, how do you canonicalize without subjecting yourself to the path limit?

---

_Comment by @andyleejordan on 2018-02-01 17:46_

The dumb way: we use absolute paths "everywhere." I know, it's not a great answer, and it's not entirely true either. But when it breaks, we fix it to use an absolute path, since you can't use `\\?\` on relative paths. Honestly, I'm not sure there's a great solution to problems arising from the use of relative paths inside a directory which is already exceeding the path limit.

---

_Comment by @BurntSushi on 2018-02-01 18:13_

@andschwa Ah OK, great. And yeah no worries, honestly, I'm just trying to make sure I understand the problem and what people are doing to fix it. :-) That would definitely be a tough fix to make though. I was hoping for that application manifest trick to bail me out!

---

_Comment by @andyleejordan on 2018-02-01 18:50_

I hoped for the same thing. It's possible I screwed up; but I had disabling (`false`) working, and swapping it to `true` did _nothing_ to enable it. The docs sure imply it ought to work though, maybe @retep998 knows the secret to doing it via the manifest 😉 

---

_Comment by @tedfordgif on 2018-03-16 00:05_

@andschwa this may not be that helpful, but at least on Windows 10 it looks like you need both the (registry or GP) and the manifest setting. See "second gate" language in [this blog post](https://blogs.msdn.microsoft.com/jeremykuhne/2016/07/30/net-4-6-2-and-long-paths-on-windows-10/), which is contra [the documentation](https://msdn.microsoft.com/en-us/library/aa365247(VS.85).aspx#maxpath). But, there is more real-world confirmation in a forum if you read far enough to [this comment](https://social.msdn.microsoft.com/Forums/en-US/fc85630e-5684-4df6-ad2f-5a128de3deef/260-character-explorer-path-length-limit?forum=windowsgeneraldevelopmentissues#821ddf37-c2bc-431e-ad8b-9bcfeaf8951b).

@BurntSushi I haven't invested enough time to know if ripgrep builds for Windows even have the requisite manifest metadata, but it seems theoretically possible that it would be possible for Windows 10 users to enable long path support if ripgrep can include the manifest setting.


---

_Comment by @BurntSushi on 2018-03-16 01:09_

@tedfordgif Unfortunately I'm not much help here. The Windows build is basically the bare minimum that I know how to do, and I'm pretty sure I just copied it from someone else. I know so little that I don't even know how to guide anyone to solve this problem. Whoever does it is going to need to get their hands dirty. :-)

---

_Comment by @tedfordgif on 2018-03-26 07:16_

@BurntSushi, it looks like this probably belongs in rustc: [librustc_trans/back/linker.rs](https://github.com/rust-lang/rust/blob/d3518058e2d47ec04aa8b9756b5d4398bce19faf/src/librustc_trans/back/linker.rs#L412). You need to pass [/MANIFEST](https://docs.microsoft.com/en-us/cpp/build/reference/manifest-create-side-by-side-assembly-manifest) and [/MANIFESTINPUT:manifest.xml](https://msdn.microsoft.com/en-us/library/dn195770.aspx) to link.exe.

The manifest.xml file should look something like this:

```
<?xml version='1.0' encoding='utf-8' standalone='yes'?>
<assembly
    xmlns="urn:schemas-microsoft-com:asm.v1"
    manifestVersion="1.0"
    >
  <application xmlns="urn:schemas-microsoft-com:asm.v3">
    <windowsSettings>
      <longPathAware xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">true</longPathAware>
    </windowsSettings>
  </application>
</assembly>
```

---

_Comment by @BurntSushi on 2018-03-26 10:52_

@tedfordgif Is there any simple way to hack that manifest in, even if it's just to test, without modifying rustc's internal linker?

---

_Comment by @retep998 on 2018-03-26 15:45_

`cargo rustc -- -Clink-arg "/MANIFESTINPUT:manifest.xml"`

---

_Comment by @tedfordgif on 2018-04-03 04:27_

Glad to report that this is a very easy fix. Prerequisite: Set registry key Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem: LongPathsEnabled to 1. Then start a new powershell (it needs to be able to find the registry value you just set in order to create the test path).

```
PS> git clone git@github.com:BurntSushi/ripgrep.git ; cd ripgrep
PS> # create manifest.xml according to my previous post.
PS> cargo rustc -- -Clink-arg="/MANIFESTINPUT:manifest.xml" -Clink-arg="/MANIFEST:EMBED"
PS> $str = "a" * 250
PS> mkdir $str ; mkdir $str\$str ; mkdir $str\$str\$str
PS> echo reallylongpathtest > $str\$str\$str\test.txt
PS> rg -ic reallylongpathtest
./aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa: The system cannot find the path specified. (os error 3)
PS> target\debug\rg -ic reallylongpathtest
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\test.txt:1
PS>
```

There's probably a better way to set the rustflags in Cargo.toml, but I didn't discover the recipe just yet. Maybe this will help: https://users.rust-lang.org/t/solved-rust-project-how-build-like-gcc-mwindow/5168/8

---

_Comment by @BurntSushi on 2018-04-03 10:47_

@tedfordgif Oh wow, that's awesome!! It seems like I should be able to include a manifest in the release binaries at least. Needing to edit a registry key is still unfortunate, but perhaps good enough for a workaround for now for those that need it.

---

_Comment by @tedfordgif on 2018-04-03 12:59_

@BurntSushi It isn't a workaround, it is the Right Way, at least for now, 'cuz Windows.

---

_Comment by @BurntSushi on 2018-04-03 13:06_

@tedfordgif Anything that requires editing the registry is a work around IMO. There are ways to fix this without editing the registry, but require more pervasive changes to the code, e.g., by using the `\\?\` prefix everywhere.

---

_Comment by @andyleejordan on 2018-04-03 16:56_

@tedfordgif You don't actually even need the manifest to enable long path support if you've enabled it in the registry. Enabling it in the registry enables it "globally," and the manifest is really only useful to opt-out (and disable it for an application running on a computer where it's been enabled).

I'd be very happy to be disproved about this, but as I said earlier, it did not appear possible when I tried to enable long path support through the manifest on a per-app basis (that is, without enabling it in the registry or through GPO).

But hey, the next version of the "unversioned" Windows 10 is coming out in a couple weeks, maybe things will improve!

---

_Comment by @tedfordgif on 2018-04-11 03:04_

@andschwa, see my previous PS session that makes it pretty clear the manifest is a requirement. Also see the links to blog posts that clarify that you need _both_ the registry and the manifest. Happy to make you very happy!

Given that the official documentation seems to be saying you only need one of the two, I wouldn't be surprised if they actually make that the case in a Windows 10 update, but it is certainly not the case now.

---

_Comment by @tedfordgif on 2018-04-11 03:16_

@BurntSushi I agree it is a workaround, didn't really mean to quibble. Another important consideration is that only some of the API supports the long path names, at least according to the [documentation](https://msdn.microsoft.com/en-us/library/aa365247(VS.85).aspx#maxpath), which as we've seen is not always right.

---

_Comment by @andyleejordan on 2018-08-20 22:29_

@tedfordgif I had to do further testing of this, and indeed, it is wishy-washy. Some APIs (like `SetCurrentDirectory`) require _both_ the manifest and the registry change, and don't support opt-in via `\\?\`, other APIs are happy with just the opt-in prefix, other APIs are happy without the prefix and just the registry key enabled. It's all a mess. One API that is never happy is `CreateProcess`.

Fun thing to break: use `SetCurrentDirectory` to set your process's cwd to a long path, and then try to spawn a new process with `CreateProcess`, it'll fail with invalid parameter 😭 AFAICT there is no way around this. It's broken broken.

Sorry, this isn't totally ripgrep related, but it sure was interesting.

---

_Comment by @tedfordgif on 2018-08-27 15:21_

@andschwa Thanks for the follow-up and details.

---

_Comment by @thargor on 2021-10-14 13:51_

@BurntSushi I really hate this bug. Would you accept merge requests that implement the ```\\?\``` prefix?

>  There are ways to fix this without editing the registry, but require more pervasive changes to the code, e.g., by using the \\?\ prefix everywhere.

---

_Comment by @BurntSushi on 2021-10-14 14:18_

Unlikely, because such a patch is non-trivial and has consequences for filtering. What I would like is a patch that fixes this via a manifest file. If that still requires a registry edit, then oh well.

---

_Comment by @santagada on 2021-11-01 17:15_

I really think filtering being potentially affected is a smaller problem than not being able to use ripgrep for long paths (and can be just a note on the help "instead of matching string start for file names, match on \\?\ because windows is weird").

---

_Comment by @thargor on 2021-11-04 12:30_

#2049 embeds the manifest file on windows. It still needs to be enabled in the registry.

Maybe ripgrep could detect long paths on windows and print a warning if long paths are not enabled in the registry?

---

_Comment by @Tastaturtaste on 2023-03-16 01:10_

I think this issue can be closed. I just tested ripgrep 13.0.0 with a path of length >750 with the following powershell code already presented in a comment above:
```
PS> $str = "a" * 250
PS> mkdir $str ; mkdir $str\$str ; mkdir $str\$str\$str
PS> echo reallylongpath > $str\$str\$str\test.txt
PS> rg -ic reallylongpath
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\test.txt:1
PS>
```
I did not modify ripgrep in any way, this is the normal `cargo install ripgrep` installation. I also disabled the registry key for long path support I normally have enabled before testing.
It seems to work with long paths by default without any manifest file needed. This could be because starting with rust 1.58 the file system functions automatically convert paths to the verbatim format without the need to canonicalize manually: https://github.com/rust-lang/rust/pull/89174

Someone else should probably test it also on their machine, maybe I messed something up.


---

_Comment by @BurntSushi on 2023-03-16 01:20_

Oh wow, I actually had no idea about that. Reading that PR, it does indeed look like this issue might be fixed.

---

_Comment by @FeldrinH on 2023-04-03 16:34_

> Someone else should probably test it also on their machine, maybe I messed something up.

The problem appears to be fixed on my computer as well, at least when installing with `cargo install ripgrep`.

However, the problem is NOT fixed when installing the latest prebuilt release (https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep-13.0.0-x86_64-pc-windows-msvc.zip). Presumably this is because that release was built with an older version of Rust. It might be a good idea to release a new prebuilt version that is built with a newer version of Rust.


---

_Label `rollup` added by @BurntSushi on 2023-07-08 17:28_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---

_Comment by @nod5 on 2023-09-28 10:11_

I'm still experiencing this issue with the latest Windows prebuilt release `ripgrep-13.0.0-x86_64-pc-windows-msvc.zip ` . If a fix by now exists please release a build that includes it.

---

_Comment by @BurntSushi on 2023-09-28 11:25_

@nod5 This fix is not in a release yet. You should be able to compare the dates to arrive at that conclusion. I'll put a new release it when it's convenient for me to do so. See: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release

---

_Locked by @ghost on 2023-09-28 11:26_

---
