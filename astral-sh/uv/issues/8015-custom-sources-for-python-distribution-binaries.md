```yaml
number: 8015
title: Custom sources for python distribution binaries
type: issue
state: open
author: brendan-morin
labels:
  - documentation
assignees: []
created_at: 2024-10-08T19:08:34Z
updated_at: 2025-04-10T14:08:43Z
url: https://github.com/astral-sh/uv/issues/8015
synced_at: 2026-01-12T15:59:18Z
```

# Custom sources for python distribution binaries

---

_@brendan-morin_

Many companies have policies around sourcing artifacts from internal sources only. I'm seeing in the docs that uv uses pre-built third-party distributions from the [python-build-standalone](https://github.com/indygreg/python-build-standalone) project. Is it possible to configure uv to point to an alternate private index for python version distributions? 

I imagine as long as there's a well-defined API for how uv queries and returns binaries, the actual source from which these are downloaded should probably not matter much?

Apologies if this is a duplicate, I searched around but didn't spot an existing issue.

---

_Comment by @Crypto-Spartan on 2024-10-08 19:11_

Timing is wild, I was _just_ about to submit an issue with this same request. I would love support for changing the URL that `uv` uses for it's request to the python-build-standalone project as well. (also due to a corporate environment)

---

_Comment by @zanieb on 2024-10-08 19:15_

Yep! `UV_PYTHON_INSTALL_MIRROR`

---

_Comment by @zanieb on 2024-10-08 19:16_

Described at https://docs.astral.sh/uv/configuration/environment/

---

_Assigned to @zanieb by @zanieb on 2024-10-08 19:16_

---

_Label `documentation` added by @zanieb on 2024-10-08 19:16_

---

_Comment by @Crypto-Spartan on 2024-10-08 19:18_

Thank you for the quick response!

Additionally, is there support for providing `uv` a filepath instead of a URL to the `.tar.gz` that's downloaded from the python-build-standalone project (`cpython-3.X.X+<date>-<target>-install_only_stripped.tar.gz`)?

---

_Comment by @zanieb on 2024-10-08 19:21_

You can use `file://` in the install mirror.

---

_Comment by @Crypto-Spartan on 2024-10-08 21:15_

so I'm currently trying to install https://github.com/indygreg/python-build-standalone/releases/download/20241008/cpython-3.12.7+20241008-x86_64_v3-unknown-linux-gnu-pgo+lto-full.tar.zst with `uv`. 

I have it downloaded in `$HOME/downloads/python_standalone/20241008/cpython-3.12.7+20241008-x86_64_v3-unknown-linux-gnu-pgo+lto-full.tar.zst`

I set `UV_PYTHON_INSTALL_MIRROR='file:///home/<user>/downloads/python_standalone'`

When I attempt to run `uv python install 3.12.7`, it fails with
```
Searching for Python versions matching: Python 3.12.7
error: No download found for request: cpython-3.12.7-linux-x86_64-gnu
```

```bash
> echo $UV_PYTHON_INSTALL_MIRROR
file:///home/<user>/downloads/python_standalone
```

Could you provide help/guidance as to how I should proceed? Thanks for your help

I also tried with `UV_PYTHON_INSTALL_MIRROR='file://home/<user>/downloads/python_standalone'` just incase I had too many slashes after the `file:`, still didn't work

I'm on Ubuntu 22.04 LTS (jammy) if that matters

---

_Comment by @zanieb on 2024-10-08 21:19_

Can you share verbose logs? i.e. `RUST_LOG=uv=trace uv python install -v 3.12.7`

Your uv version needs to support that managed download, so you might just be on a uv version that only has metadata for 3.12.6 and not 3.12.7 yet?

---

_Comment by @Crypto-Spartan on 2024-10-08 21:29_

```bash
> RUST_LOG=uv-trace uv python install -v 3.12.7
DEBUG uv 0.4.18
TRACE Checking lock for `/home/<user>/.local/share/uv/python` at `/home/<user>/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/home/<user>/.local/share/uv/python`
Searching for Python versions matching: Python 3.12.7
TRACE ld path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help for more information.\n"
TRACE stdout ouput from `ldd --version`: ld.so (Ubuntu GLIBC 2.35-0ubuntu3.8) stable release version 2.35.\nCopyright ...
TRACE Found manylinux 2.35 in stdout of `ldd --version`
DEBUG Released lock at `/home/<user>/.local/share/uv/python/.lock`
error: No download found for request: cpython-3.12.7-linux-x86_64-gnu
```

`uv 0.4.18` is the latest version of `uv` I'm able to grab off of the pypi mirror in my corporate environment

---

_Comment by @zanieb on 2024-10-08 21:36_

3.12.7 was added in https://github.com/astral-sh/uv/pull/7880 which is in uv 0.4.19

---

_Comment by @Crypto-Spartan on 2024-10-08 21:47_

Tried again with https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-pgo+lto-full.tar.zst instead this time.

```bash
> uv python install 3.12.6
Searching for Python versions matching: Python 3.12.6
cpython-3.12.6-linux-x86_64-gnu
error: Failed to install cpython-3.12.6-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: Invalid gzip header
cpython-3.12.6-linux-x86_64-gnu
```

---

_Comment by @Crypto-Spartan on 2024-10-08 21:50_

does `uv` _**only**_ install the `x86_64-unknown-linux-gnu-install_only_stripped.tar.gz`? 
Why can't I install the `x86_64-unknown-linux-gnu-pgo+lto-full.tar.zst`?

---

_Comment by @zanieb on 2024-10-08 21:51_

Sorry this isn't well documented — yes, we only support the target defined in our [download metadata](https://github.com/astral-sh/uv/blob/f6fd849f2c63e3b319ab03c936beab866dc303ba/crates/uv-python/download-metadata.json).

We could probably support more in the future, it's just more complicated.

---

_Comment by @zanieb on 2024-10-08 21:54_

Basically the reason you can't, is that we generate JSON metadata from GitHub release

https://github.com/astral-sh/uv/blob/f6fd849f2c63e3b319ab03c936beab866dc303ba/crates/uv-python/download-metadata.json#L398-L409

then encode it as static Rust code for performance

https://github.com/astral-sh/uv/blob/f6fd849f2c63e3b319ab03c936beab866dc303ba/crates/uv-python/src/downloads.inc#L471-L484

then when you ask for a custom mirror we transform the static URL

https://github.com/astral-sh/uv/blob/891c91dd3a0141f05adea20674f3ea7f4477c510/crates/uv-python/src/downloads.rs#L577-L585

This is quite a naive implementation, but it's intended to unblock core functionality not be fully feature complete.

---

_Comment by @Crypto-Spartan on 2024-10-08 21:59_

Is there any specific reason why the generated json only includes the `install_only_stripped.tar.gz` and none of the other python standalone builds? I'm willing to work on a PR to support the other builds if you're open to accepting it.

---

_Comment by @zanieb on 2024-10-08 22:05_

I think we just adopted the "choose a single build" behavior from Rye's code. I'd love to support other builds (see #8019), though it's probably non-trivial. If you want to work on it, I'd appreciate it. Otherwise I'll probably work on #8019 in the near future.

---

_Comment by @Crypto-Spartan on 2024-10-08 22:49_

something weird is happening still, it looks like `uv` is attempting to download from github directly still which won't be possible in my environment. 

`cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz` is currently in `/home/<user>/downloads/python_standalone`

```bash
> echo $UV_PYTHON_INSTALL_MIRROR
file:///home/<user>/downloads/python_standalone
```

```bash
> RUST_LOG=uv-trace uv python install -v 3.12.6
DEBUG uv 0.4.18
TRACE Checking lock for `/home/<user>/.local/share/uv/python` at `/home/<user>/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/home/<user>/.local/share/uv/python`
Searching for Python versions matching: Python 3.12.6
TRACE ld path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `/lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout ouput from `ldd --version`: ld.so (Ubuntu GLIBC 2.35-0ubuntu3.8) stable release version 2.35.\nCopyright ...
TRACE Found manylinux 2.35 in stdout of `ldd --version`
DEBUG Using request timeout of 30s
TRACE Handling request for https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Request for https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/<user>/.local/share/uv/python/.cache/.tmpYs2Aii
DEBUG Extracting cpython-3.12.6%2B20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
error: Failed to install cpython-3.12.6-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: Invalid gzip header
DEBUG Released lock at `/home/<user>/.local/share/uv/python/.lock`
```

---

_Comment by @zanieb on 2024-10-08 23:19_

On my machine, I get something like this if I specify an arbitrary directory:

```
❯ UV_PYTHON_INSTALL_MIRROR="file:///Users/zb/test" RUST_LOG=uv=trace uv python install -v 3.12.6
DEBUG uv 0.4.18
TRACE Checking lock for `/Users/zb/.local/share/uv/python` at `/Users/zb/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/Users/zb/.local/share/uv/python`
Searching for Python versions matching: Python 3.12.6
DEBUG Using request timeout of 30s
error: Failed to install cpython-3.12.6-macos-aarch64-none
  Caused by: failed to query metadata of file `/Users/zb/test/20240909/cpython-3.12.6+20240909-aarch64-apple-darwin-install_only_stripped.tar.gz`
  Caused by: No such file or directory (os error 2)
DEBUG Released lock at `/Users/zb/.local/share/uv/python/.lock`
```

---

_Comment by @zanieb on 2024-10-08 23:21_

Similarly:

```
❯ wget https://github.com/indygreg/python-build-standalone/releases/download/20240909/cpython-3.12.6%2B20240909-aarch64-apple-darwin-install_only_stripped.tar.gz
❯ mkdir 20240909
❯ mv cpython-3.12.6+20240909-aarch64-apple-darwin-install_only_stripped.tar.gz 20240909
❯ UV_PYTHON_INSTALL_MIRROR="file:///Users/zb/workspace/uv" RUST_LOG=uv=trace uv python install -v 3.12.6
DEBUG uv 0.4.18
TRACE Checking lock for `/Users/zb/.local/share/uv/python` at `/Users/zb/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/Users/zb/.local/share/uv/python`
Searching for Python versions matching: Python 3.12.6
DEBUG Using request timeout of 30s
DEBUG Downloading file:///Users/zb/workspace/uv/20240909/cpython-3.12.6%2B20240909-aarch64-apple-darwin-install_only_stripped.tar.gz to temporary location: /Users/zb/.local/share/uv/python/.cache/.tmpvg3LcW
DEBUG Extracting cpython-3.12.6%2B20240909-aarch64-apple-darwin-install_only_stripped.tar.gz
DEBUG Moving /Users/zb/.local/share/uv/python/.cache/.tmpvg3LcW/python to /Users/zb/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none
Installed Python 3.12.6 in 380ms
 + cpython-3.12.6-macos-aarch64-none
DEBUG Released lock at `/Users/zb/.local/share/uv/python/.lock`
```

---

_Comment by @Crypto-Spartan on 2024-10-08 23:36_

Figured it out

my filename was urlencoded so was actually `cpython-3.12.6%2B20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz`
instead of `cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz`
(`%2B` vs `+`)

Thanks for all of your help

---

_Comment by @zanieb on 2024-10-08 23:44_

That makes sense. We can add more documentation around this.

---

_Comment by @guillemc23 on 2024-10-09 09:17_

I'm a bit confused. We're currently migrating to `uv` and we need to install python packages from private sources.

Currently, I'm just adding the following to my `pyproject.toml`:
```toml
[tool.uv]
index-url = "https://<user>:<password>@<host>"
```
Which is working properly. 

I have been unable to find a way to configure this authentication in order to avoid just hardcoding the credentials for obvious reasons. Is this somehow related to this issue? Also, is this the way I should be doing this? 

EDIT: I believe what I'm referring to is this PR https://github.com/astral-sh/uv/pull/7741


---

_Comment by @chrisrodrigue on 2024-10-10 21:35_

Just took a look at #7741 and I am wondering why the environment variables can't be specified in the `pyproject.toml` for the index URLs.

---

_Comment by @zanieb on 2024-10-11 02:14_

@guillemc23 sorry this issue is about downloading Python distributions — not index URLs for packages. Could you open a new issue for your question instead?

---

_Comment by @MeitarR on 2024-10-24 22:50_

> Figured it out
> 
> my filename was urlencoded so was actually `cpython-3.12.6%2B20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz` instead of `cpython-3.12.6+20240909-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz` (`%2B` vs `+`)
> 
> Thanks for all of your help

Opened PR with a create mirror script to make it easier https://github.com/astral-sh/uv/pull/8548

---

_Comment by @MeitarR on 2024-12-27 17:19_

maybe relevant #10203

---

_Comment by @leifwalsh on 2025-01-14 20:54_

I'd be interested in something slightly different: rather than just hosting indygreg's tarballs internally, I'd like to use my own builds of python, but I'm content to host those builds with whatever URL pattern `uv` wants, and to provide a config file (say, `https://not-indygregs-trustworthy-snakes.example.com/download-metadata.json`) I'd construct myself, with checksums of my preferred tarballs. (Then, presumably, set `python-install-mirror = "https://not-indygregs-trustworthy-snakes.example.com"`, or maybe more likely set something to the path to `download-metadata.json` in `/etc/uv/uv.toml`)

Would something like that be in scope for this issue, or should I file a separate one?

---

_Comment by @leifwalsh on 2025-01-16 02:15_

@zanieb I can probably work on this or find someone who can, if you all can bounce some design ideas with us. If you'd rather we just pitch something that works too, I just want to make sure you're open to the idea first. 

---

_Comment by @zanieb on 2025-01-16 03:33_

I'm a little hesitant since we don't have a well-defined or stable format for the `python-build-standalone` distributions and we're pretty tightly integrated with them.

We also store all the metadata statically via templated Rust right now so it's a bit of a larger change to be able to read that from a file.

I'm sort of into adding support for reading that metadata dynamically, e.g., it's also useful for updating Python without updating uv — but we'll need to have some sort of versioning scheme and I think if you're rolling your own distributions there's no guarantee they'll work well.

---

_Comment by @leifwalsh on 2025-01-16 19:13_

Yeah, I've been looking at how that gets included statically. I can patch uv with my own json and get it to use our builds, but reading from a file and getting that configured from settings or environment variables is a bit more plumbing. 

What's been changing about the format for distributions? Is that something we could help define/standardize? I also noticed there's no room in the schema for extra version numbers after patch, which we do use (build numbers, in case we rebuild a version), but could probably get by without if that's weird to add. 

---

_Comment by @zanieb on 2025-01-16 19:21_

> What's been changing about the format for distributions? Is that something we could help define/standardize? 

The most recent example I can think of is https://github.com/astral-sh/python-build-standalone/pull/373

It's less that they're changing a lot, and more that I'm hesitant to promise they won't :) the `python-build-standalone` project is pretty young still and we're investing increasing resources into it. I expect things to change more in the future.

Another concern is that we need to apply patches to the distribution at install time. These may or may not be relevant for custom distributions?

I think the best place to start is experimental support for dynamic metadata in uv without stability promises. Then we can iterate on standardization or versioning of the format?

>  I also noticed there's no room in the schema for extra version numbers after patch, which we do use (build numbers, in case we rebuild a version), but could probably get by without if that's weird to add.

Like, you want to be able to have multiple builds of versions side-by-side in your metadata? I'm not particularly opposed, but it may complicate our Python request logic?

---

_Comment by @MeitarR on 2025-01-16 20:08_

@leifwalsh That is exactly what I'm requesting at #10203 

---

_Comment by @zanieb on 2025-01-16 20:15_

I think there's a bit of a difference between a local metadata file and a remote one — then we start having to deal with caching and network overheads. I agree in the long-run we'd want both though.

---

_Comment by @leifwalsh on 2025-01-16 20:35_

> It's less that they're changing a lot, and more that I'm hesitant to promise they won't :) the `python-build-standalone` project is pretty young still and we're investing increasing resources into it. I expect things to change more in the future.

Fair!

> Another concern is that we need to apply patches to the distribution at install time. These may or may not be relevant for custom distributions?

Oh, I didn't realize that. Can you point me toward those patches? The builds we produce work (well enough for our purposes, in a somewhat constrained environment e.g. Linux only) after untarring. 

> I think the best place to start is experimental support for dynamic metadata in uv without stability promises. Then we can iterate on standardization or versioning of the format?

That sounds like a good plan to me. I'd be happy to live without stability promises for a bit. Should we move discussion to #10203 and start planning it there? (@MeitarR thanks! Sorry I couldn't find that as easily as this one.)

> Like, you want to be able to have multiple builds of versions side-by-side in your metadata? I'm not particularly opposed, but it may complicate our Python request logic?

Yeah, I admit this is odd and probably not worth considering as a real requirement, but we produce builds like 3.13.1.40 and 3.13.1.45 - they're just distinct build events for 3.13.1, but as a simple way to not worry about reproducibility, we keep them both and let projects stick with whichever was the latest build last time they updated things. This nearly fits in the Prerelease struct, but not quite because all of those variants (alpha, beta, RC) sort earlier than None. 

> I think there's a bit of a difference between a local metadata file and a remote one — then we start having to deal with caching and network overheads. I agree in the long-run we'd want both though.

Either way you have to await something, right? :)

---

_Comment by @zanieb on 2025-01-16 20:43_

The patches are invoked at https://github.com/astral-sh/uv/blob/11f2882211b4b15a347718ab865590540805090e/crates/uv/src/commands/python/install.rs#L312-L317

---

_Comment by @leifwalsh on 2025-01-16 21:21_

Thanks! Ok yeah, we do some stuff to sysconfig before uploading our builds, and we have some other things that do the externally managed stuff. That makes sense, thanks. 

And, @MeitarR now I see that's literally the issue you linked to before I joined this issue, I think I just misread what you were asking for the first time. Sorry!

---

_Comment by @chrisrodrigue on 2025-03-14 18:07_

Another uv user was asking if one could set the python mirror in `pyproject.toml` (https://github.com/astral-sh/uv/issues/11746#issuecomment-2706614478).

---

_Comment by @chrisrodrigue on 2025-03-14 18:27_

Would it be worth adding another release asset that includes uv for each `python-build-standalone` platform (i.e. `cpython-3.12.9+20250311-x86_64-pc-windows-msvc-install_only_uv.tar.gz`)? Might need to alter uv to look for Python interpreter in the current directory (similar to `uv.toml` discovery).

Just an idea 🙂

---
