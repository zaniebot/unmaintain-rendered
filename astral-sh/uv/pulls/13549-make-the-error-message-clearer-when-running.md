```yaml
number: 13549
title: make the error message clearer when running distroless containers
type: pull_request
state: merged
author: oconnor663
labels:
  - error messages
assignees: []
merged: true
base: main
head: jack/find_ld_add_warnings
created_at: 2025-05-20T07:12:03Z
updated_at: 2025-05-29T17:29:03Z
url: https://github.com/astral-sh/uv/pull/13549
synced_at: 2026-01-10T11:10:41Z
```

# make the error message clearer when running distroless containers

---

_Pull request opened by @oconnor663 on 2025-05-20 07:12_

Previously you could generate a confusing warning like this:

```
$ dr ghcr.io/astral-sh/uv run python
error: Could not read ELF interpreter from any of the following paths: /bin/sh, /usr/bin/env, /bin/dash, /bin/ls
```

Now the error message is better (updated):

```
error: Failed to discover managed Python installations
  Caused by: Failed to determine the libc used on the current platform
  Caused by: Failed to find any common binaries to determine libc from: /bin/sh, /usr/bin/env, /bin/dash, /bin/ls
```

See https://github.com/astral-sh/uv/issues/8635.

---

_Review requested from @zanieb by @oconnor663 on 2025-05-20 07:12_

---

_Comment by @oconnor663 on 2025-05-20 07:27_

The direct way to test this is to rebuild our docker image and run a command in it, though that takes a long time:

```
$ cd ~/astral/uv
$ docker build . -t test
...
$ docker run test run python
error: Failed to determine libc version from common binaries (/bin/sh, /usr/bin/env, /bin/dash, /bin/ls). Are you running uv from a distroless Docker image or similar? That's not currently supported.
```

An more direct way to create the same condition is to use an empty `chroot`, though the build needs to be statically linked:

```
$ cd ~/astral/uv
$ cargo build --target x86_64-unknown-linux-musl
...
$ mkdir /tmp/empty
$ cp target/x86_64-unknown-linux-musl/debug/uv /tmp/empty/
$ sudo chroot /tmp/empty /uv run python
error: Failed to determine libc version from common binaries (/bin/sh, /usr/bin/env, /bin/dash, /bin/ls). Are you running uv from a distroless Docker image or similar? That's not currently supported.
```

I don't _love_ this run-on error message. My first instinct was to use two or three separate `error!` lines, but I saw that those don't show up at all unless the caller uses `-v`, so it looks like we have to use the error message? Also I imagine this isn't really in the style of our other error messages? Happy to let someone else suggest a better phrasing.

Pie-in-the-sky-new-guy thought: Would it be nice/useful if our errors supported some sort of `.with_context()` method similar to what `anyhow` does? It seems tough to figure out how much context to pack into this error message at the point in the code where we need to define it. Like the user probably doesn't know or care why we might want to "determine the libc version". That's just what the proximate functions are doing when the error happens. The `DEBUG` lines in the original bug report are more informative, and if we're about to crash with an obscure error we might wish we could retroactively turn on verbose mode to log how we got there?

What do you think about adding a test for this? Do we have any integration tests for the Docker image? My quick read of `.github/workflows/build-docker.yml` is that it's a publishing flow and not a testing flow.

---

_Comment by @konstin on 2025-05-20 11:18_

My intuition is to split the error into two errors: A source error, the current one, and a nicer one that wraps it that says something like `Failed to determine libc version, are you in a distroless Docker image?`.

We can also add an outer error that says something like "Failed to install a managed Python interpreter" to make it more clear what component failed and how to work around this (provide a system (or nix?) interpreter or install a Python yourself). In this case it's not too helpful since the problem is usually you're in the wrong docker container, but I generally try contextualize errors by component, it also helps when users are sharing error traces in bug reports.

A trick that we sometimes use is to make a line break and add a `hint:`; this only renders nicely though if you're the first or the last error in the chain (which may not be applicable here; I'd love to properly propagate hints upwards but trait restrictions don't allow it). You can find the existing hints by grepping for `"hint".bold().cyan()`.

Can we add guidance in the error message what the user should do, e.g. if they are on nix?

> What do you think about adding a test for this? Do we have any integration tests for the Docker image? My quick read of `.github/workflows/build-docker.yml` is that it's a publishing flow and not a testing flow.

One option is to use the musl build we already have in CI and mount it into a distroless docker and capture the output. This is complicated though, and for this error path may not be worth the complexity, manual testing might be sufficient.

---

_Label `error messages` added by @konstin on 2025-05-20 11:18_

---

_Review requested from @geofft by @konstin on 2025-05-20 11:18_

---

_Comment by @zanieb on 2025-05-20 13:17_

> We can also add an outer error that says something like "Failed to install a managed Python interpreter" to make it more clear what component failed and how to work around this (provide a system (or nix?) interpreter or install a Python yourself). In this case it's not too helpful since the problem is usually you're in the wrong docker container, but I generally try contextualize errors by component, it also helps when users are sharing error traces in bug reports.

Yeah I agree this is another thing I'd expect to see here. We should add one or more layers that elucidate _why_ we're trying to read this thing as well.

---

_Comment by @zanieb on 2025-05-20 13:19_

I don't think we need to add test coverage for this, a manual check is sufficient for now. We're not really set up to snapshot the output in a meaningful way. It might be a nice thing to have a harness for integration tests in the future so we can capture things like this (maybe open an issue to track that?)

---

_Comment by @geofft on 2025-05-20 14:11_

1. I think we should change the existing "Could not read ELF interpreter from any of the following paths" message, perhaps in addition to the new error you're introducing here. It's not at all obvious (even to me!) why uv is trying to read an ELF interpreter, why it's doing it from those particular paths, whether I can get it to read from other paths, what it's trying to do with that information etc. We should start off by reporting the high-level thing that failed (we couldn't figure out what type of OS you are on to find a compatible Python distribution) and what to do about it, and maybe keep information about how we failed to figure that out in verbose logs or something. We should probably update "Failed to determine libc" too.
2. I would not use the word "distroless" here; while that is the terminology we currently use on [our Docker page](https://docs.astral.sh/uv/guides/integration/docker/), there is a project called [distroless](https://github.com/GoogleContainerTools/distroless), and many of its containers have a libc installed, and running uv + python-build-standalone on those is fully supported. What's not supported is _our_ ghcr.io/astral-sh/uv:latest container, which is built `FROM scratch` with only the uv binaries, and is kind of intended just for copying the binaries into some other container you're building (see the examples in our docs), not for running directly. If we can reliably detect that we're running in ghcr.io/astral-sh/uv:latest, it would be neat to provide some advice on what to run instead. (This may require reworking our docs first, or changing the ENTRYPOINT in uv:latest to print an error so that you don't even get to this part of the uv code, or something.)

---

_Comment by @geofft on 2025-05-20 14:32_

One way to test this logic on an existing binary, without rebuilding anything, is to pick a different path for the ELF interpreter inside the chroot and use `patchelf` to change the path to the ELF interpreter in the built copy of uv. Here's a fully worked example on Ubuntu:

```sh-session
$ mkdir chroot
$ patchelf ~/.local/bin/uv --set-interpreter /ld.so --output chroot/uv 
$ cp "$(patchelf --print-interpreter ~/.local/bin/uv)" chroot/ld.so
$ ldd ~/.local/bin/uv | sed -n 's/.*=> \(.*\) .*/\1/p' | xargs -I_ rsync -aLR _ chroot
$ busybox unshare -Urm chroot chroot /uv run python
error: Could not read ELF interpreter from any of the following paths: /bin/sh, /usr/bin/env, /bin/dash, /bin/ls
```

`unshare -U` creates a user namespace, which is an unprivileged way to do chroots. Because Ubuntu is paranoid they kind of sort of turn this off by default for the normal `unshare` command but [not for `busybox unshare`](https://x.com/roddux/status/1903028631514837107); see also [Qualys' writeup](https://www.qualys.com/2025/three-bypasses-of-Ubuntu-unprivileged-user-namespace-restrictions.txt) and [Ubuntu's response](https://discourse.ubuntu.com/t/understanding-apparmor-user-namespace-restriction/58007) about how this is actually a feature. On non-Ubuntu systems, regular `unshare` works. It might also work on Ubuntu GHA runners, I haven't checked. In any case, the point of this is that you can chroot without actually needing root/sudo; ultimately it should be possible to do this in a test case in Rust or whatever other language instead of as its own full-scale integration test, though it will be a little more than your average unit test because it will need to run the `uv` binary as a separate process.

The `ldd | xargs rsync` adventure copies all needed normal libraries to the chroot, since you need those too for a non-statically-linked uv.

Also, because we aren't currently looking for the interpreter itself, just these files, you can reproduce the current behavior without even calling patchelf, just copy the ELF interpreter under its original name into the chroot:

```sh-session
$ rm -rf chroot
$ mkdir chroot
$ cp ~/.local/bin/uv chroot/uv
$ rsync -aLR "$(patchelf --print-interpreter ~/.local/bin/uv)" chroot
$ ldd ~/.local/bin/uv | sed -n 's/.*=> \(.*\) .*/\1/p' | xargs -I_ rsync -aLR _ chroot
$ busybox unshare -Urm chroot chroot /uv run python
error: Could not read ELF interpreter from any of the following paths: /bin/sh, /usr/bin/env, /bin/dash, /bin/ls
```

(This is a bug - this chroot has a working ELF interpreter and python-build-standalone will work - but we can tackle that separately from improving the error messages of the current logic.)

---

_Comment by @konstin on 2025-05-20 15:28_

Fwiw you can build a statically linked uv through musl and put that into a docker container (Using `ghcr.io/astral-sh/uv` in place of scratch as docker doesn't want to let me use scratch):

```
$ cargo build --target x86_64-unknown-linux-musl
$ docker run --rm -it -w /io -v $(pwd)/target:/io --entrypoint /io/x86_64-unknown-linux-musl/debug/uv ghcr.io/astral-sh/uv venv
  x Could not read ELF interpreter from any of the following paths: /bin/sh, /usr/bin/env, /bin/dash, /bin/ls
```

---

_Comment by @oconnor663 on 2025-05-21 23:52_

Brief check-in: I wound up spending the last couple days on https://github.com/astral-sh/uv/pull/13583. I'm going to put out a second iteration on that PR and get back to this one tomorrow.

Edit: This is on top of my plate for tomorrow (Friday) morning.

---

_Comment by @oconnor663 on 2025-05-24 00:15_

Ok this is ready for another look. Here's what the error looks like now:

```
$ cargo build --target x86_64-unknown-linux-musl
...
$ cp target/x86_64-unknown-linux-musl/fast-build/uv /tmp/chroot/uv
$ sudo chroot /tmp/chroot /uv run python
error: Failed to find a managed Python installation matching the current platform
  Caused by: Failed to determine the current platform
  Caused by: Failed to find any common binaries (/bin/sh, /usr/bin/env, /bin/dash, /bin/ls)
```

This is my first time working with `thiserror`, so I wouldn't be at all surprised if there's some more idiomatic way to do these things. Thanks @konstin for talking me through a lot of this stuff.

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:227 on 2025-05-26 10:31_

nit:
```suggestion
    #[error("Failed to find a managed Python installation")]
```

---

_@konstin approved on 2025-05-26 10:35_

Looks good!

---

_Comment by @konstin on 2025-05-26 10:37_

There's a second `#[error(transparent)] LibcDetection(#[from] LibcDetectionError)`, should we annotate that one too? I'm not sure if the code path is accessible right now, maybe with a `uv python install`.

---

_@oconnor663 reviewed on 2025-05-26 16:00_

---

_Review comment by @oconnor663 on `crates/uv-python/src/downloads.rs`:93 on 2025-05-26 16:00_

Copy/pasting this in two places makes me wonder if there's an easy way to make the `LibcDetectionError` apply this annotation itself. I assume not without changing it into a struct with an internal "error kind"?

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:93 on 2025-05-26 16:03_

thiserror at least doesn't support it, I think that's a limitation because every `.source()` needs a type and we'd need two type for the `LibcDetectionError` error message and the variant error message, but we only have one type.

---

_@konstin reviewed on 2025-05-26 16:03_

---

_@oconnor663 reviewed on 2025-05-26 16:08_

---

_Review comment by @oconnor663 on `crates/uv-python/src/downloads.rs`:93 on 2025-05-26 16:08_

Ah, I wasn't even thinking about the `.source()` representation, just prefixing the string representation of all variants of the enum with `"Failed to find the current platform: (...)"` or something like that.

---

_Review comment by @geofft on `crates/uv-python/src/discovery.rs`:227 on 2025-05-27 19:07_

I probably need to see the actual errors as they would be output to understand what's going on here, so feel free to disregard this, but - "Failed to find a managed Python installation" reads to me as that we were expecting something to already be installed, and it wasn't installed. I think this error is saying that it failed to find a _distribution_ (though I hate that word, it's overloaded) that is _available_ to be installed, that matches the current platform, right?

"Failed to find a compatible managed Python version" maybe?

---

_@geofft approved on 2025-05-27 19:08_

I'm going to step back from this review so that you're not blocked on me being around to review, but in general it looks like what you have here is a definite improvement on the status quo, and getting much farther would require reworking the actual detection logic.

---

_@zanieb reviewed on 2025-05-27 19:11_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:227 on 2025-05-27 19:11_

This is in the `Discovery` enum so it should be correct as-written?

I'm more confused about why this isn't a `ManagedPython` enum member if it's `ManagedPython`-specific?

---

_@zanieb reviewed on 2025-05-27 19:13_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:93 on 2025-05-27 19:13_

Should this be "Failed to determine the libc used on the current platform"? This message seems too generic for a libc-detection specific problem.

---

_@zanieb reviewed on 2025-05-27 19:13_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:90 on 2025-05-27 19:13_

Same as https://github.com/astral-sh/uv/pull/13549/files#r2109990567

---

_@zanieb reviewed on 2025-05-27 19:15_

---

_Review comment by @zanieb on `crates/uv-python/src/libc.rs`:40 on 2025-05-27 19:15_

Should we say what we're doing with these binaries?

```suggestion
    #[error("Failed to find any common binaries to determine libc from: {0}")]
```

Separately, we generally, stylize as `: <values>` at the end of a message

---

_@zanieb reviewed on 2025-05-27 19:17_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:227 on 2025-05-27 19:17_

I think this `from / source` split for managed Python errors is pretty awkward here.

---

_@konstin reviewed on 2025-05-27 19:29_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:93 on 2025-05-27 19:29_

I'd push the libc part down to the most detailed error, I don't expect Python devs to know about libc flavors.

---

_@zanieb reviewed on 2025-05-27 19:36_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:93 on 2025-05-27 19:36_

As structured, it needs to be present here or it's too broad / inaccurate. I also don't expect devs to know about libc flavors, but there should be an error above this explaining why we're looking for it. I'm also fine with it being structured differently.

---

_@oconnor663 reviewed on 2025-05-28 00:07_

---

_Review comment by @oconnor663 on `crates/uv-python/src/discovery.rs`:227 on 2025-05-28 00:07_

@geofft I don't know if we're necessarily _expecting_ something to be installed, but this error occurs as we're looking at the versions that are already installed and filtering for the ones that are compatible with the current platform. (Presumably if we didn't find anything we would go on to try to download something, but we don't make it that far.) There's a piece of machinery called [`Error::is_critical`](https://github.com/astral-sh/uv/blob/20cfc93c58fed2c30f778522d64bc86aa5cab60f/crates/uv-python/src/discovery.rs#L795) involved here, where certain expected errors like "we failed to invoke that interpreter" are considered non-critical (because it's presumably just not compatible and we should try others), but this libc error isn't one of those expected cases.

---

_Assigned to @zanieb by @zanieb on 2025-05-28 15:13_

---

_Comment by @oconnor663 on 2025-05-29 04:06_

@zanieb I just pushed the `discovery::Error` change we talked about earlier today.

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:227 on 2025-05-29 12:18_

I think this is too specific? This can be raised for any managed Python error.

```suggestion
    #[error("Failed to discover managed Python installations")]
```

It seems misleading to suggest it's specifically related to "matching the current platform".

---

_@zanieb reviewed on 2025-05-29 12:18_

---

_Comment by @oconnor663 on 2025-05-29 14:50_

Committed @zanieb's wording change and updated the PR description.

---

_@zanieb approved on 2025-05-29 15:33_

---

_Merged by @oconnor663 on 2025-05-29 17:29_

---

_Closed by @oconnor663 on 2025-05-29 17:29_

---

_Branch deleted on 2025-05-29 17:29_

---
