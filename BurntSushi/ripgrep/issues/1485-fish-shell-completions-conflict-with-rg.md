```yaml
number: 1485
title: fish shell completions conflict with rg completions provided by fish
type: issue
state: closed
author: ronjouch
labels:
  - bug
assignees: []
created_at: 2020-02-14T20:32:49Z
updated_at: 2020-02-22T16:16:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1485
synced_at: 2026-01-12T16:13:23Z
```

# fish shell completions conflict with rg completions provided by fish

---

_@ronjouch_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 9cb93abd11)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Official amd64 .deb package.

#### What operating system are you using ripgrep on?

Ubuntu 19.10 x64

#### Describe your question, feature request, or bug.

The fish completions conflict with the completions provided by fish's official Ubuntu (.deb) package, https://launchpad.net/~fish-shell/+archive/ubuntu/release-3 , causing installation to fail.

Fish devs suggest at https://github.com/fish-shell/fish-shell/issues/5822#issuecomment-585908454 that 3rd-party completions should be packaged to `/usr/share/fish/vendor_completions.d` instead of `/usr/share/fish/completions`, which is fish's sole kingdom.

---

_Comment by @BurntSushi on 2020-02-14 20:40_

I don't maintain downstream packages. Please file bugs with downstream packages with downstream packagers.

---

_Closed by @BurntSushi on 2020-02-14 20:40_

---

_Comment by @BurntSushi on 2020-02-14 20:44_

Errmmm, well it looks like the debian package that I provide has this wrong too: 

https://github.com/BurntSushi/ripgrep/blob/9cb93abd11906fe1d6f89de712465a7a3aa8c381/Cargo.toml#L104

Is that what you meant by "official" deb package? I took that to mean the deb package in Debian's repositories. But it looks like this should be fixed in my deb package as well. (Which is not official. It's a binary package provided purely as a convenience.)

---

_Reopened by @BurntSushi on 2020-02-14 20:44_

---

_Label `bug` added by @BurntSushi on 2020-02-14 20:44_

---

_Comment by @ronjouch on 2020-02-14 21:33_

> Errmmm, well it looks like the debian package that I provide has this wrong too:
> 
> https://github.com/BurntSushi/ripgrep/blob/9cb93abd11906fe1d6f89de712465a7a3aa8c381/Cargo.toml#L104
> 
> Is that what you meant by "official" deb package? I took that to mean the deb package in Debian's repositories. But it looks like this should be fixed in my deb package as well. (Which is not official. It's a binary package provided purely as a convenience.)

Yup, by "official" I meant the package published in the Releases page. Sorry for the confusion and thanks for re-opening.

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

_Comment by @thernstig on 2020-02-22 13:31_

@BurntSushi Any chance you would push a new release soon? Considering the users of fish shell cannot use ripgrep currently without some workarounds.

---

_Comment by @BurntSushi on 2020-02-22 13:45_

@thernstig https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#release

---

_Comment by @thernstig on 2020-02-22 14:07_

I assume then this does not fall under 

> One exception to this is high impact bugs. 

---

_Comment by @zanchey on 2020-02-22 14:10_

A workaround in the meantime is to run `dpkg-divert --add --divert  /usr/share/fish/completions/rg.fish.0 --rename --package ripgrep /usr/share/fish/completions/rg.fish`

---

_Comment by @BurntSushi on 2020-02-22 14:15_

@thernstig Do you really think it's worth needling me about how I schedule my free time to do a release? I don't. I mean, I wrote the policy. I linked the FAQ entry. Obviously I'm telling you that it applies. Moreover, this isn't some written-in-stone-law that I have to obey. I made the language in the FAQ purposely vague to try to avoid people trying to lawyer me over it. I've now made it even more vague: https://github.com/BurntSushi/ripgrep/commit/ecec6147d117da638c933cf103a3ea14ce87102d

In any case, no, I do not see this as a high impact bug. First of all, this isn't a regression. Nothing changed in the last release that made this a problem. Something else outside of my control appears to have changed. Second of all, my fix _only_ applies to the ripgrep binary Debian package that I distribute. This fix does nothing to resolve any problems ripgrep may or may not have with other packages. If you all of a sudden can't install the binary Debian package, then you can download the generic binary releases until the next release comes out. Or build the binary Debian package from source. Or, if you're using Debian, use whatever release of ripgrep is in the Debian repos. Or, it looks like @zanchey posted a work-around (thank you for that).

---

_Comment by @thernstig on 2020-02-22 16:16_

It was merely a question, not implying it does fall under it. I think you read too much into my answer. Thanks for explaining. All good 😃 

---
