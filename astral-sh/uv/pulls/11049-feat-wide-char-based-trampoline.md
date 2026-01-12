```yaml
number: 11049
title: "feat: wide char based trampoline"
type: pull_request
state: closed
author: samypr100
labels:
  - windows
assignees: []
base: main
head: wide-char-trampoline
created_at: 2025-01-29T02:46:18Z
updated_at: 2025-11-13T06:30:55Z
url: https://github.com/astral-sh/uv/pull/11049
synced_at: 2026-01-12T16:09:39Z
```

# feat: wide char based trampoline

---

_@samypr100_

## Summary

I've had this branch gathering dust from [a while ago](https://github.com/astral-sh/uv/pull/5751) where I had switched the manual CLI parsing into using std::process for the windows trampoline for more consistent argument handling.

I had come across a few weeks ago with the following blog https://blog.orange.tw/posts/2025-01-worstfit-unveiling-hidden-transformers-in-windows-ansi/ which basically calls out a security flaw that can occur when not using wide-char implementations of certain windows API's such as GetCommandLineA (which we do here).

I thought this would be an opportunity to revisit some of that previous work in leveraging std::process and also remove any `A` API usage in favor of `W` variants. I didn't change mainCRTStartup to wmainCRTStartup since the way we use it doesn't seem like can cause a problems, although I've not investigated further to see the if the ANSI API is loaded regardless.

Note, this does have the side-effect of bringing along more of core::fmt and doubling the size of the trampoline to about 80kb uncompressed so it's something to consider carefully. I also thought about not using std::process and porting more to use wide-char instead to reduce the size but the implementation started to become a tad unsighly and unsafe for what I'd like it to be and seemed it would only save about 15kb.

## Test Plan

~~I did some initial basic local tests and things seemed functional, although I ideally would like to test a bit more cases with varying code pages before and after hence why I'm marking this as draft for the time being.~~

~~I've not attempted to do a POC on exploiting the Worst Fit vulnerability via uv with varying code pages, but it may be something worth exploring since command line parsing in the way uv does was a primary category for Worst Fit exploitation.~~

~~PS. Its possible this vulnerability may also be completely irrelevant and I'm just being paranoid 😆 thoughts welcome.~~

I did some local tests with varying code pages and things seemed fully functional.

(edit: POC exploit can be carried out successfully by modifying the manifest in the binary and removing the `activeCodePage` entry)




---

_Review requested from @konstin by @samypr100 on 2025-01-29 02:58_

---

_Label `windows` added by @samypr100 on 2025-01-29 03:00_

---

_Review requested from @Gankra by @konstin on 2025-01-29 08:49_

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:338 on 2025-01-29 08:51_

Can we skip collecting here? `args` takes an `IntoIterator`

---

_@konstin approved on 2025-01-29 08:54_

---

_@samypr100 reviewed on 2025-01-29 13:13_

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:338 on 2025-01-29 13:13_

Ah, good point. https://github.com/astral-sh/uv/pull/11049/commits/dded30bf279ea4553a86dfa39c87f280352b42cd

---

_Comment by @zanieb on 2025-01-29 14:16_

We got a vulnerability report for this and @Gankra verified we were not affected. I'm not sure about the general benefit of the changes :)

---

_Comment by @Gankra on 2025-01-29 19:48_

For reference the crux of the situation is this manifest we add to our binaries via build.rs that turns all the `A` APIs into UTF-8, making things work even better than if we used the `W` APIs, and completely avoiding the issue (it continues to be surprising that the worstfit writeups don't mention this solution).

https://github.com/astral-sh/uv/blob/503f9a97af39abbe05ce47ac8d57397ed8bbf274/crates/uv-trampoline/build.rs#L1-L5

---

_Comment by @samypr100 on 2025-01-30 00:23_

> For reference the crux of the situation is this manifest we add to our binaries via build.rs that turns all the `A` APIs into UTF-8, making things work even better than if we used the `W` APIs, and completely avoiding the issue (it continues to be surprising that the worstfit writeups don't mention this solution).
> 
> https://github.com/astral-sh/uv/blob/503f9a97af39abbe05ce47ac8d57397ed8bbf274/crates/uv-trampoline/build.rs#L1-L5

Indeed, I do remember this manifest setup. It sets `<activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>` on the executable manifest. The caveat to consider is that this only works starting with Windows 10 1903, but not older versions or windows gdi-based gui applications regardless of version and falls back to legacy behavior so it depends how far back we'd want to support windows.

On the other hand, I'm not sure that relying on the manifest guarantees complete protection as a result. The manifest can be very changed easily by exernal manipulation so I can see the risks there and maybe that's why they don't bring that up as a workaround unlike the `Use Unicode UTF-8 for worldwide language support` setting which will require a full system reboot and its permanent at system runtime after that.


---

_Comment by @Gankra on 2025-01-30 13:46_

> The caveat to consider is that this only works starting with Windows 10 1903

Windows 10 1903 [has been EOL for about 4 years now](https://learn.microsoft.com/en-us/lifecycle/announcements/windows-10-1903-end-of-servicing) and Rust [has only supported Windows 10 for over a year now](https://blog.rust-lang.org/2024/02/26/Windows-7.html) and [Microsoft plans to EOL Windows 10 by the end of the year](https://arstechnica.com/gadgets/2025/01/as-it-buries-windows-10-microsoft-declares-2025-year-of-the-windows-11-pc-refresh/).

> The manifest can be very changed easily by external manipulation

Could you elaborate? Keep in mind the matter in question is breaking out of command line escaping, so if you can mess with the manifest of a binary you've probably already got way more power than that.

---

_Comment by @samypr100 on 2025-01-31 03:07_

> Windows 10 1903 [has been EOL for about 4 years now](https://learn.microsoft.com/en-us/lifecycle/announcements/windows-10-1903-end-of-servicing) and Rust [has only supported Windows 10 for over a year now](https://blog.rust-lang.org/2024/02/26/Windows-7.html) and [Microsoft plans to EOL Windows 10 by the end of the year](https://arstechnica.com/gadgets/2025/01/as-it-buries-windows-10-microsoft-declares-2025-year-of-the-windows-11-pc-refresh/).

Understood, its roughly around the same timeframe Python 3.8 came out. This also applies to windows server 2016 to some regards. Statistics wise, see the following big query for uv usage on windows for just a single day (sorry free tier).

<img width="373" alt="uv-usage-windows" src="https://github.com/user-attachments/assets/71b2871a-610c-4a4d-b1c9-7019d2ea0698" />

> Could you elaborate? Keep in mind the matter in question is breaking out of command line escaping, so if you can mess with the manifest of a binary you've probably already got way more power than that.

Sorry, this part I meant more-so as a general statement as to why I think they don't consider the manifest approach as a solution to the problem and its more-so of a bandaid. I've used tools like Mage in the past to easily modify manifests, although binedit tools can be used too. Yes, if someone can maliciously mess with the manifest of a "trusted executable" (in this case uv trampolines) you have other problems at hand, but it all boils down to the hardening of the system at that point. From my perspective, I'd like for organizations/individuals to be able to trust uv to be free of possible security vulnerabilities under any circumstances but that's just my opinion 😆 

---

_Comment by @Gankra on 2025-02-06 15:20_

@samypr100 are you still interested in landing the std::process stuff without landing the A->W changes? (Or does the std::process stuff not make sense without it because OsString stuff is happening?). I like how much spooky code gets removed, although idk how Bad it is that the compiled size is doubling... (80kb is still real small...)

If moving to W stuff is the crux here, I'd prefer to close this. I appreciate the paranoia but like, binaries have to be able to rely on their manifests for correctness, otherwise the manifest doesn't really make sense at all.

---

_Comment by @samypr100 on 2025-02-08 17:34_

> @samypr100 are you still interested in landing the std::process stuff without landing the A->W changes? (Or does the std::process stuff not make sense without it because OsString stuff is happening?). I like how much spooky code gets removed, although idk how Bad it is that the compiled size is doubling... (80kb is still real small...)
> 
> If moving to W stuff is the crux here, I'd prefer to close this. I appreciate the paranoia but like, binaries have to be able to rely on their manifests for correctness, otherwise the manifest doesn't really make sense at all.

Hey, appreciate the follow up. I can go either way, let me know. The size ended up being actually much smaller in the compressed uv binary (just 15kb~ increase). Mostly curious, is the concern about this using W APIs or the size increase? I did like the fact we were able to move away from CString and unsafe removals related to CString, although I think that is still possible with W->A.

The crux here is both std:process move and A->W API move to make code more clean, consistent and removal of CString, unsafe and spooky code 😆 . Note, switching the W->A won't do much help related to size changes (savings of ~0.5kb)

---

_Marked ready for review by @samypr100 on 2025-02-08 23:24_

---

_Closed by @samypr100 on 2025-09-22 22:22_

---

_Branch deleted on 2025-11-13 06:30_

---
