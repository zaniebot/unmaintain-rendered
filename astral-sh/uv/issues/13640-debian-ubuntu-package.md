---
number: 13640
title: Debian/Ubuntu Package
type: issue
state: open
author: trymeouteh
labels: []
assignees: []
created_at: 2025-05-24T19:58:11Z
updated_at: 2025-11-29T10:01:06Z
url: https://github.com/astral-sh/uv/issues/13640
synced_at: 2026-01-10T01:25:36Z
---

# Debian/Ubuntu Package

---

_Issue opened by @trymeouteh on 2025-05-24 19:58_

### Summary

Unable to install uv using `apt` and unable to receive updates to uv using `apt`

### Example

Please release uv as an `apt` package to make it available for Debian/Ubuntu users and be able to receive automatic updates to uv with the `apt` package manager.

---

_Label `enhancement` added by @trymeouteh on 2025-05-24 19:58_

---

_Comment by @yonas on 2025-05-24 23:30_

`cargo binstall uv` would also be nice.

---

_Comment by @geofft on 2025-05-26 19:51_

FWIW, there's some work in Debian itself to package a version of uv. It looks like it's making progress, but at the moment Debian is frozen in preparation for their release so this work probably won't pick up until later this summer (meaning it won't be in a Debian release until two years from now, though it might end up available through backports, and it could potentially be in Ubuntu 25.10). See https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1069776

On the other hand, the version in Debian and Ubuntu will likely be quite a bit behind what you can get through the Astral website.

---

_Comment by @konstin on 2025-05-26 20:10_

There are unofficial debian packages (see e.g. https://github.com/astral-sh/uv/issues/13243) and we expect linux distributions to ship uv, but providing a debian package from uv itself is out of scope.

cargo binstall could work with uv, we provide binaries in the GitHub releases page. If cargo binstall extends its parser to read our format (I assume it doesn't recognize the `uv-` prefix), it can be used with uv.

---

_Label `enhancement` removed by @konstin on 2025-05-26 20:11_

---

_Referenced in [cargo-bins/cargo-binstall#2176](../../cargo-bins/cargo-binstall/issues/2176.md) on 2025-06-01 02:50_

---

_Comment by @nth10sd on 2025-06-02 18:21_

I [filed an issue](https://github.com/cargo-bins/cargo-binstall/issues/2176) over at the `cargo-binstall` issue tracker, got some help creating the following `bash` command to install the latest `uv` release via `cargo-binstall`, `python` is needed (tested on Python 3.11+, but may work on earlier versions):

```
dl="$(python3 -u -c 'import hashlib,tempfile,urllib.request;r=urllib.request.urlopen("https://github.com/astral-sh/uv/releases/latest/download/source.tar.gz").read();t=tempfile.NamedTemporaryFile(delete=False);t.write(r);print(t.name);print(hashlib.sha256(r).hexdigest())')" ;  # Download the latest uv release into the system temporary folder, print temporary filename and hash
hsh="$(python3 -u -c 'from urllib.request import urlopen;print(urlopen("https://github.com/astral-sh/uv/releases/latest/download/source.tar.gz.sha256").read().decode("utf-8").split(maxsplit=1)[0])')" ;  # Download the upstream SHA-256 hash
# Only proceed if upstream hash matches downloaded hash
if [[ "$dl" == *"$hsh"* ]] ; then
    tmpy="$(mktemp -d)" ;  # Create a temporary folder
    IFS=' ' read -r -a words <<< $dl ;  # Prepare to split temporary filename and hash of downloaded source archive
    tar -zxf "${words[0]}" -C $tmpy ;  # Extract downloaded .tar.gz source archive
    cargo binstall -y --manifest-path="$tmpy/$(ls $tmpy)" uv ;  # Install using cargo-binstall, pointing --manifest-path to the extracted directory
else exit 1 ;  # Exit code 1 if upstream hash does not match downloaded hash
fi ;
```

Or a Bash one-liner:

```
bash -c 'dl="$(python3 -u -c '\''import hashlib,tempfile,urllib.request;r=urllib.request.urlopen("https://github.com/astral-sh/uv/releases/latest/download/source.tar.gz").read();t=tempfile.NamedTemporaryFile(delete=False);t.write(r);print(t.name);print(hashlib.sha256(r).hexdigest())'\'')" ; hsh="$(python3 -u -c '\''from urllib.request import urlopen;print(urlopen("https://github.com/astral-sh/uv/releases/latest/download/source.tar.gz.sha256").read().decode("utf-8").split(maxsplit=1)[0])'\'')" ; if [[ "$dl" == *"$hsh"* ]] ; then tmpy="$(mktemp -d)" ; IFS='\'' '\'' read -r -a words <<< $dl ; tar -zxf "${words[0]}" -C $tmpy ; cargo binstall -y --manifest-path="$tmpy/$(ls $tmpy)" uv ; else exit 1 ; fi ;'
```

It does basic SHA-256 verification with the hash given upstream.

---

_Comment by @passcod on 2025-06-02 22:12_

Posted upstream but for visibility:

1. The `uv` crate published at the registry seems unrelated to this repo (https://github.com/peterhj/uv/issues/1) and this repo is not published to the registry, this is why `cargo binstall uv` doesn't work.
2. `cargo binstall --git https://github.com/astral-sh/uv uv` works fine.

---

_Comment by @wetneb on 2025-09-30 19:13_

I've been working on preparing the stage for packaging `uv` in debian (so, making an official package). The vast majority of dependencies are packaged.

Among the few missing ones there is `pubgrub`, which according to the current `Cargo.toml` is currently pulled out of git and not crates.io. That's a problem for us, because our packaging pipeline relies on all dependencies being available from crates.io.
It would be fantastic if the changes made to this dependency could be upstreamed to https://github.com/pubgrub-rs/pubgrub and published as https://crates.io/crates/pubgrub. Failing that, publishing the fork on crates.io would also help.

This is true for other dependencies currently being fetched from git repositories instead of crates.io. The more of those you can eliminate, the easier it will be for us to package `uv` in debian.

That being said, it's not needed for the `uv-*` crates themselves to be published on crates.io.

---

_Comment by @zanieb on 2025-09-30 20:32_

Thanks for the update @wetneb!

@konstin and I are maintainers of pubgrub too, we can look into upstreaming the differences — though, given how tightly integrated pubgrub is in uv, it might be not be plausible for us to upstream all of our specific needs so we might need to publish the fork or vendor it as a `uv-pubgrub` crate.

---

_Comment by @geofft on 2025-09-30 23:58_

One other thing we may want to keep in mind is that there's an Ubuntu LTS coming up relatively soon (on the timescales of getting packages into distros) and it will be supported for a long time (on the timescales of wanting to support software). Specifically, judging from the current release and the last LTS, the cutoff for automated imports from Debian _unstable_ to Ubuntu 26.04 is likely to be February 2026, and then that will be the uv version you get out of a supported Ubuntu release for 5+ years. So if we manage to get a package into Debian before February, it would be nice if it's something we're relatively comfortable with people having for a pretty long time. (Of course Ubuntu's schedules have no formal veto over what Debian has in its archive, and it's possible to go to the Ubuntu folks and say "uh that thing you snapshotted from Debian isn't long-term supportable, can I convince you not to ship it / can you ship this version instead," but it's easier for everyone if you don't have to do that.)

On the Debian side, a release just came out in August and they come out every 2 years, so we have until early 2027 to find a version that we're comfortable with Debian users having for a long while (though the normal support cycle is 3 years, not quite as long as an Ubuntu LTS).

---

_Comment by @konstin on 2025-10-01 08:34_

While we generally make upstream PRs for our changes, our remaining Git dependencies are either because we made significant changes to the library (pubgrub, asnyc-zip), or because upstream didn't react to our PRs (reqwest-middleware, tl).

For pubgrub specifically, we upstream most of our changes, but we're using an entirely different internal API we've added over `DependencyProvider`. CC @eh2406 re upstreaming this interface.

---

_Comment by @zanieb on 2025-10-01 13:37_

(I think we should move discussion on upstreaming that out to PubGrub, not here — can you open an issue for that?)

---

_Comment by @konstin on 2025-10-01 13:43_

The tracking issue for that is https://github.com/pubgrub-rs/pubgrub/issues/231

---

_Comment by @konstin on 2025-10-10 15:43_

> I've been working on preparing the stage for packaging `uv` in debian (so, making an official package). The vast majority of dependencies are packaged.
> 
> Among the few missing ones there is `pubgrub`, which according to the current `Cargo.toml` is currently pulled out of git and not crates.io. That's a problem for us, because our packaging pipeline relies on all dependencies being available from crates.io. It would be fantastic if the changes made to this dependency could be upstreamed to [pubgrub-rs/pubgrub](https://github.com/pubgrub-rs/pubgrub?rgh-link-date=2025-09-30T19%3A13%3A37.000Z) and published as [crates.io/crates/pubgrub](https://crates.io/crates/pubgrub). Failing that, publishing the fork on crates.io would also help.
> 
> This is true for other dependencies currently being fetched from git repositories instead of crates.io. The more of those you can eliminate, the easier it will be for us to package `uv` in debian.
> 
> That being said, it's not needed for the `uv-*` crates themselves to be published on crates.io.

Is there something that makes pubgrub special? We can publish pubgrub to crates.io as astral-pubgrub, but we have other Git dependencies too, usually because upstream is unmaintained, sometimes only for one or two releases because we needed to fix something faster than upstream didn't react. I checked `Cargo.toml` and our current list of Git dependencies is:

* async_zip
* pubgrub and version-ranges
* reqwest-middleware and reqwest-retry
* tl

We'd ideally want to avoid re-publishing all of those to crates.io, especially because we don't intend those forks to be used by others.

---

_Comment by @wetneb on 2025-10-11 13:27_

Indeed, all the other Git dependencies will be equally problematic.

I have asked more experienced Debian contributors what my options are to deal with those, and we have identified the following approaches:
* upstreaming the changes of those dependencies to the original crates (I trust you that it's hard or not desired in the cases that you have)
* convincing you to upload them to crates.io (which I understand you are reluctant to)
* convincing you to vendor them as internal crates in your workspace, as `uv-pubgrub` and so on, or as modules in some of your existing internal crates
* me maintaining forks of your forks, that I upload to crates.io myself. When packaging `uv` I would then patch it to point to my crates instead of the git dependencies
* vendoring the dependencies inside uv as part of the Debian packaging, using the `debian/missing-sources` mechanism, or "extra packages" (which I am yet to understand). I am not familiar with this approach, which probably comes at a greater risk that the "ftp masters" reject the upload when reviewing it. Given that such reviews take many months at the moment, I'm reluctant to bet on this (a rejection would likely delay the packaging by nearly a year, because of the times needed for a review of the new uploads of the forks and of uv itself).

---

_Comment by @konstin on 2025-10-13 10:05_

I don't know the specifics for Debian, but I can share how Gentoo and Fedora handle this:

* Gentoo: https://gitweb.gentoo.org/repo/gentoo.git/tree/dev-python/uv/uv-0.9.2.ebuild
* Fedora: https://src.fedoraproject.org/rpms/uv/blob/rawhide/f/uv.spec

Are there any other Rust applications with Git dependencies that we can use as reference?

---

_Comment by @wetneb on 2025-10-13 12:21_

It's difficult to find any Debian package written in Rust whose upstream has git dependencies, because most Rust tools are uploaded to crates.io, and crates.io enforces that all dependencies of the crate are also fetched from crates.io.

It looks like it could be possible for the Debian packager (say, me) to vendor those dependencies as part of the Debian packaging process (say, using `debian/missing-sources`), assuming we have good justifications for why they shouldn't be packaged separately. This could apparently be sufficient to convince the ftp masters to approve the package. For that, I would need a statement from your side on the status of each of those crates, explaining why it wouldn't make sense to package them separately.

---

_Comment by @konstin on 2025-10-13 12:23_

Can you copy the rationale from the fedora sources? The divergence to reqwest-middleware is even larger now (the project doesn't reply to PRs at all), otherwise it still matches.

---

_Comment by @wetneb on 2025-10-13 12:35_

Yes, that could perhaps work. That being said (and apologies for being a bit stubborn with that), I see that there are apparently [interest to make `uv` installable from `crates.io`](https://crates.io/crates/uv). Those git deps will also be a blocker for that, so I still think it would be worth tackling this issue directly. As far as I know, uploading a crate to crates.io doesn't come with a particular commitment to supporting third-party uses compared to just having it as a public GitHub repo. A disclaimer added to the docs of the crate, explaining your intentions, would probably go a long way.

---

_Referenced in [astral-sh/uv#16358](../../astral-sh/uv/issues/16358.md) on 2025-10-19 12:02_

---

_Comment by @sthen on 2025-10-19 12:21_

I understand why it's not preferred but if these could be uploaded to crates.io with a disclaimer as suggested by @wetneb this would simplify things a lot for downstream OS packagers. Having a single source for these seems more sensible than various OS packagers dealing with it themselves in various different ways.

As some other projects are starting to use uv as a build backend (e.g. https://github.com/pyca/cryptography/pull/13137) it's getting more important to have native OS packages. That particular example is easily worked-around for now where needed, but that won't always be the case.

---

_Comment by @zanieb on 2025-10-19 15:58_

I think we'll just publish these to `crates.io` with a `uv-` prefix for now, it seems like the easiest path forward for everyone.

---

_Assigned to @konstin by @zanieb on 2025-10-19 15:58_

---

_Referenced in [astral-sh/uv#16591](../../astral-sh/uv/issues/16591.md) on 2025-11-05 04:29_

---

_Comment by @konstin on 2025-11-14 19:16_

We're re-published all of our forks to crates.io, there are now no git dependencies anymore.

---

_Comment by @zanieb on 2025-11-21 14:49_

@wetneb please let us know if there's anything else blocking an official Debian package.

---

_Unassigned @konstin by @konstin on 2025-11-21 14:57_

---

_Comment by @wetneb on 2025-11-29 10:01_

Thanks, I have looked into it and didn't find any blocker yet. I'm focusing on packaging `uv_build` first, so it can be used as a build backend, without the full `uv` CLI yet (as this will hopefully help with Debian packaging of Python software, and has almost all of its dependencies packaged for Debian already). Progress for that is tracked [here](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1115616).

If anyone wants to get involved, there are about 20 Rust crates that need packaging to be able to package the full CLI. The process for those crates is documented in the [Rust team book](https://rust-team.pages.debian.net/book/).

---
