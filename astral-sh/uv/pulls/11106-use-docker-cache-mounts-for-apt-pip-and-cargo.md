```yaml
number: 11106
title: Use docker cache mounts for apt, pip and cargo
type: pull_request
state: open
author: mjpieters
labels: []
assignees: []
base: main
head: docker-caching
created_at: 2025-01-30T18:02:38Z
updated_at: 2025-04-29T08:21:54Z
url: https://github.com/astral-sh/uv/pull/11106
synced_at: 2026-01-12T16:09:41Z
```

# Use docker cache mounts for apt, pip and cargo

---

_@mjpieters_

The cache mounts are cached using standard github actions cache when building in the CI pipeline.

Note that the build stage no longer contains the whole source tree, these are instead mounted into the build container when building to avoid invalidating cached build container layers.



---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:43 on 2025-01-30 18:06_

Same as for #8685, inserted to unblock running this job in my PR.

---

_@mjpieters reviewed on 2025-01-30 18:06_

---

_Marked ready for review by @mjpieters on 2025-01-30 18:07_

---

_Comment by @zanieb on 2025-01-30 18:38_

Is this just for Docker image build performance?

---

_Comment by @zanieb on 2025-01-30 18:39_

See also, previous discussion at https://github.com/astral-sh/uv/pull/3372

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:103 on 2025-01-30 18:43_

You _could_ drop the crates directory from the hash key and so reuse the cargo cache even when source code files change. This would give you incremental builds. 

---

_@mjpieters reviewed on 2025-01-30 18:43_

---

_@zanieb reviewed on 2025-01-30 18:52_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:103 on 2025-01-30 18:52_

I'm not sure we _want_ incremental builds in the release pipeline. A clean build seems like a good property, right?

---

_Comment by @mjpieters on 2025-01-30 18:57_

> Is this just for Docker image build performance?

Yes, both locally and CI workflows. I had completely missed the other PR 🤦 so thanks for the reference! I'll review the discussion and see if this needs closing or updating or if I change the merge target to that PR.

---

_Comment by @zanieb on 2025-01-30 18:59_

I think that one might have been a little more aggressive about refactoring. I'm more liable to just accept cache mounts. The big caveat there is probably captured by https://github.com/astral-sh/uv/pull/3372#issuecomment-2116475873 with the notable update that we're now using Depot runners in our org! So... we could set up Depot for Docker builds.

---

_Comment by @mjpieters on 2025-01-30 19:22_

Yeah, the author of that pr clearly has a lot of rust-in-docker build knowledge, more than I have.

Bottom line is that this PR makes an incremental change with the aim of improving build time here in the GH pipeline. If you are moving the build to a different platform however then there is not much value in merging this one.

---

_@mjpieters reviewed on 2025-01-30 19:35_

---

_Review comment by @mjpieters on `.github/workflows/build-docker.yml`:103 on 2025-01-30 19:35_

That's why I picked this config, but I just wanted to point out the option.

---

_@samypr100 reviewed on 2025-01-31 03:18_

---

_Review comment by @samypr100 on `Dockerfile`:1 on 2025-01-31 03:18_

~~Note, I would see https://github.com/astral-sh/uv/pull/3372 for context as to why some of these changes haven't been done.~~

(edit: woops, hadn't seen this was already mentioned https://github.com/astral-sh/uv/pull/11106#issuecomment-2625288136)

---

_Comment by @mjpieters on 2025-01-31 11:28_

Heads-up: something is wrong with the way the key for the cargo cache is being calculated; when I update the head sha of my PR (`git commit --amend --no-edit && git push --force`) to trigger a rebuild, the cache key has changed from the previous build, which means that `hashFiles('Dockerfile', 'crates/**', 'Cargo.toml', 'Cargo.lock')` picks up run-unique files that should not be there.

I'll have to debug this and so there will be a few temporary changes to figure out what is going on.

---

_Comment by @mjpieters on 2025-01-31 12:46_

> Heads-up: something is wrong with the way the key for the cargo cache is being calculated; when I update the head sha of my PR (`git commit --amend --no-edit && git push --force`) to trigger a rebuild, the cache key has changed from the previous build, which means that `hashFiles('Dockerfile', 'crates/**', 'Cargo.toml', 'Cargo.lock')` picks up run-unique files that should not be there.
> 
> I'll have to debug this and so there will be a few temporary changes to figure out what is going on.

Never mind, it was just me jumping to conclusions. Every time I've triggered a new build there have been new commits on main and the since PR workflows run against a merge commit `crates/**` really has changed since the last run. 🤦

---

_Comment by @mjpieters on 2025-01-31 15:52_

I've now refactored the cache mounts to be simpler and (hopefully) more effective in the Github pipeline

All in-docker build tool caches now live in a single `/buildkit-cache` directory. This includes the global caches for cargo, rustup, pip, zig and cargo-zigbuild. This is keyed _just_ on the Dockerfile hash, because these can trivially be re-used even if the Cargo.lock file changes.

The `/root/target` cache mount is no longer cached or restored in the GitHub pipeline, but it is still hugely helpful when building the docker container locally as it massively speeds up incremental builds in that case. Propagating the cache to the Github action cache was not effective however because when you have the exact same Cargo.lock, Cargo.toml and `crates` file tree then Github has also cached that specific Docker layer already, meaning that the whole build step is cached.

---

_Comment by @mjpieters on 2025-01-31 22:16_

I forced a push and this does make the build faster, by about 90-120 seconds. Compare the times for the latest release:

- release:
  - [amd64 build](https://github.com/astral-sh/uv/actions/runs/13061715804/job/36446006635): 12m 38s
  - [arm64 build](https://github.com/astral-sh/uv/actions/runs/13061715804/job/36446007253): 13m 57s

- this PR:
  - [amd64 build](https://github.com/astral-sh/uv/actions/runs/13081435856/job/36505668471): 11m 4s (Δ 1m 34s faster)
  - [arm64 build](https://github.com/astral-sh/uv/actions/runs/13081435856/job/36505668726): 11m 50s (Δ 2m 7s faster)

I've since pushed another small update to add a `rustup self update` to use the cached version of rustup when available, instead of re-downloading and having to silence rustup from complaining about the cached cargo and rust toolchain.

---

_Review comment by @polarathene on `Dockerfile`:50 on 2025-04-29 00:01_

Since no toolchain is installed at this point, the `--target` option seems redundant?

```
bash: rustup: command not found
info: downloading installer
info: profile set to 'minimal'
info: default host triple is x86_64-unknown-linux-gnu
info: skipping toolchain installation
warn: ignoring requested target: x86_64-unknown-linux-musl
```

Also, due to the copied `rust-toolchain.toml`, `--profile minimal` is ignored too. You could patch it [like I did in my PR](https://github.com/astral-sh/uv/pull/3372/files#r1590191580) since we don't need the extra components that'd otherwise be brought in (_`share/doc/rust/html` is 600MB for example_). Granted this is going into a cache mount for you, it's less noticeable but contributes towards CI cache storage?

---

Given that you're using the same `tool-caches` and the image uses a `FROM` with platform constraint tied to the native build host arch rather than the target, you might as well keep the shared layers between `TARGETPLATFORM` images the same here? (_**EDIT:** I just noticed compared to my PR your rust toolchain is stored in a cache mount, thus layer sharing won't improve much here_)

To do that, shift the earlier `ARG TARGETPLATFORM` block below this rustup one, and explicitly install both musl AMD64 + ARM64 targets. In fact, since the only usage for `TARGETPLATFORM` will be in that final `RUN`, you can completely avoid `rust_target.txt`.


---

_Review comment by @polarathene on `Dockerfile`:64 on 2025-04-29 00:17_

Adjusted `RUN` content that makes the earlier `ARG TARGETPLATFORM` block redundant (_so ARM64 + AMD64 builds only diverge common image layers at this point of the build instead_).

```Dockerfile
ARG TARGETPLATFORM
RUN \
  # Use bind mounts to access Cargo config, lock, and sources; without needing to
  # copy them into a build layer (avoids bloating the docker build layer cache):
  --mount=type=bind,source=crates,target=crates \
  --mount=type=bind,source=Cargo.toml,target=Cargo.toml \
  --mount=type=bind,source=Cargo.lock,target=Cargo.lock \
  # Add cache mounts to speed up builds:
  --mount=type=cache,target=${HOME}/target/ \
  --mount=type=cache,target=/buildkit-cache,id="tool-caches" \
  <<HEREDOC
  # Handle platform differences like mapping target arch to naming convention used by cargo targets:
  # https://en.wikipedia.org/wiki/X86-64#Industry_naming_conventions
  case "${TARGETPLATFORM}" in
    ( 'linux/amd64' )
      export CARGO_BUILD_TARGET='x86_64-unknown-linux-musl'
      ;;
    ( 'linux/arm64' )
      export CARGO_BUILD_TARGET='aarch64-unknown-linux-musl'
      export JEMALLOC_SYS_WITH_LG_PAGE=16
      ;;
    ( * )
      echo "ERROR: Unsupported target platform: '${TARGETPLATFORM}'"
      return 1
      ;;
  esac

  cargo zigbuild --release --bin uv --bin uvx --target "${CARGO_BUILD_TARGET}"
  cp "target/${CARGO_BUILD_TARGET}/release/uv" /uv
  cp "target/${CARGO_BUILD_TARGET}/release/uvx" /uvx
HEREDOC
```


---

_Review comment by @polarathene on `Dockerfile`:44 on 2025-04-29 04:43_

This `RUN` does not play well with concurrent writers when that `tool-caches` cache mount is used. Causing builds to fail:

```
1.499 info: downloading component 'cargo'
1.790 error: component download failed for cargo-x86_64-unknown-linux-gnu: could not rename downloaded file from '/buildkit-cache/rustup/downloads/c5c1590f7e9246ad9f4f97cfe26ffa92707b52a769726596a9ef81565ebd908b.partial' to '/buildkit-cache/rustup/downloads/c5c1590f7e9246ad9f4f97cfe26ffa92707b52a769726596a9ef81565ebd908b': No such file or directory (os error 2)
```

While cargo might manage lock files to avoid this type of scenario, you need to be mindful of cache mount usage when it's not compatible with the default `sharing=shared` mount option.

```bash
# When using a Buildx container driver:
docker buildx create --name=container --driver=docker-container --use --bootstrap

# You can now build for multiple platforms concurrently:
docker buildx build --builder=container --platform=linux/arm64,linux/amd64 --tag localhost/uv .
```

To prevent this problem use `sharing=locked` to block another build from writing to the same cache mount id. That or running two separate build commands to build one platform at a time.

---

While on the topic of cache mounts. It's a non-issue for CI of a project where you only build a single `Dockerfile` your project maintains.

However on user systems, AFAIK if that `id` is used in another project `Dockerfile`, it also shares that cache. Sometimes that's a non-issue, but be mindful of accidentally mixing/sharing with other projects that shouldn't share a cache mount due to concerns like invalidating each others storage, or like seen here conflicting write access, or with `sharing=locked` blocking a build of another project.

---

_Review comment by @polarathene on `Dockerfile`:44 on 2025-04-29 05:57_

```suggestion
RUN \
  --mount=type=cache,target=/buildkit-cache,id="tool-caches",sharing=locked \
```

**EDIT:** As per feedback in the next comment, I'm really not sure about the toolchain being stored in a cache mount as a good idea? Rather then apply this fix it may be better to just avoid the cache mount entirely (_you'd then have the ability to build the `build` stage and shell into it to troubleshoot building if need be too, actually maybe not due to `CARGO_HOME` if you need zigbuild_)

---

_Review comment by @polarathene on `Dockerfile`:50 on 2025-04-29 05:58_

```suggestion
  <<HEREDOC
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --profile minimal -- default-toolchain none

  echo 'targets = [ "aarch64-unknown-linux-musl", "x86_64-unknown-linux-musl" ]' >> rust-toolchain.toml
  rustup toolchain install
HEREDOC
```

---

**NOTE:** In my older PR I also set `profile` to `minimal` as well:
```
echo 'profile = "minimal"' >> rust-toolchain.toml
```

This is required if you run another `rustup` command like `rustup target add`, but with the newer `rustup toolchain install` command, it actually respects the `--profile minimal` originally set as a fallback.

`rustup toolchain install` is [intended to be the proper approach](https://github.com/rust-lang/rustup/pull/3983#issuecomment-2275736111) (_[requires Rustup 1.28.0+](https://blog.rust-lang.org/2025/03/02/Rustup-1.28.0/), released in March 2025_) to installing the toolchain from `rust-toolchain.toml`, rather than implicitly installing when using other rustup commands. So this should help justify preferring the switch 👍 

---

_Review comment by @polarathene on `Dockerfile`:44 on 2025-04-29 06:42_

I am not sure about why the rust toolchain is stored in a cache mount, while Zig and other toolchains are left in the image layers? To pair an update of `rust-toolchain.toml` bumping the toolchain to trigger `rustup self update`?

The `COPY` for `rust-toolchain.toml` would invalidate the `RUN` layer, so it would be updated just the same no?
- I could understand if you were sharing this cache mount with other `Dockerfile` without common base layer sharing, but if those projects were configured with different toolchains they likewise accumulate in cache storage? (_which is more prone to GC than an actively used layer_) Cleaning up unused layers is probably preferable, cache should really be used for actual cache (_I think it's possible for a cache mount to clear between `RUN`, not ideal for a toolchain_).
- The other possibility being for CI image caching and wanting to minimize storage.
  - The bulk of your build time with this `Dockerfile` is with the actual cargo build later on, so pulling from a CI cache blob or from the remote source (rustup, package manager, etc) are not likely to be that much faster. Regardless you're configuring persistence in CI via cache mounts, is that beneficial vs standard caching of image layers?
  - If you lose CI time to the large cache import/export delays (eg: due to de/compression), it may be faster to just not cache that portion of the image at all and do a clean build of it. Cache only what's helpful.

You will however benefit from the cache mount when building multiple targets separately (_rather than multiple `cargo build` in the same `RUN`_):
- This is only because of the earlier `ARG TARGETPLATFORM` introducing a divergence in layer cache (_1.3GB + 1.4GB to support without cache mount but actual diff is approx 200MB only_).
- Since both targets build from the same build host arch, there's no concern about conflict there with the cache mount either 👍
- It should be rare for earlier layer cache invalidation to really matter, but that'd be a win for cache mounts. Personally I prefer the immutable/predictable layer content vs accumulating cache mount that if I'm not mistaken can be cleared during build between layers (_as cache is intended to be disposable_).

That concern is easily fixed as per my suggestion for avoiding divergence at this point. Both targets added are 354MB combined. Total layer weight with minimal profile is 930MB (_instead of 1.6GB_), be that layer cache or a cache mount.

---

**Breakdown:**

```bash
# Build (without `tool-caches` cache mount):
docker buildx build --builder=container --platform=linux/amd64 --tag localhost/uv --load .

# Inspect:
docker run --rm -it -v /var/run/docker.sock:/var/run/docker.sock ghcr.io/wagoodman/dive:latest localhost/uv
```

**Sizes (bolded is within a cache mount):**
- **1.6GB** (_930MB minimal profile_) => Rustup toolchain `/buildkit-cache/rustup` (_also adds 19MB to sibling dir `cargo/`_):
  - `lib/rustlib/aarch64-unknown-linux-musl/lib` (135MB) / `lib/rustlib/x86_64-unknown-linux-musl/lib` (219MB)
  - `lib/rustlib/x86_64-unknown-linux-gnu/bin` (18MB) + `lib/rustlib/x86_64-unknown-linux-gnu/lib` (158MB)
  - `lib/libLLVM.so.19.1-rust-1.86.0-stable` (174MB) + `lib/librustc_driver-ea2439778c0a32ac.so` (141MB)
- **85MB** => Pip cache `/buildkit-cache/pip/http-v2`
- **258MB** => Apt cache `/var/cache/apt` (220MB) + `/var/lib/apt` (48MB)
- 310MB => Zig toolchain at `/root/.venv/lib/python3.12/site-packages/ziglang`
- 527MB => Base image (78MB) + 13MB (python venv setup) + `/usr` (_base package layer adds 436MB_)

**Image build time:**

On a budget VPS (_Fedora 42 at Vultr, 1vCPU + 2GB RAM with 3GB more via zram swap_):
- `apt` layer built within 37s
- `cargo-zigbuild` install 12s
- `rustup` setup 32s
- `cargo` release build (x86_64), **2 hours 25 minutes**.

The build took excessively long presumably due to single CPU and quite possibly RAM, I didn't investigate that too extensively. Changing from `lto="fat"` to `lto="thin"` brought that build time down to **43 minutes**, at the expense of being 25% larger (40MB => 50MB).

You're getting much better results reported for the build, but the bulk of the time is down to the actual build. I'd avoid wasting CI cache store (_causing evictions sooner than necessary for cache items that are actually helpful_) on the rust toolchain, saving a minute at best is not worth better using the cache to optimize the build time (_requires [`sccache`](https://doc.rust-lang.org/cargo/reference/build-cache.html#shared-cache) IIRC to be decent but is not without quirks_).

That said you can use the cache mounts in CI and not upload/restore them for minimizing the image layers cache, but presently there is very little benefit in caching image layers at all? You could instead just focus on the cache mount(s) for the `cargo` build itself.

The `cargo` target cache is 1GB alone when building this project, but [as mentioned](https://github.com/astral-sh/uv/pull/11106#issuecomment-2837804391) it's a bit of a hassle to actually leverage for the CI.

---

### After a build

For reference, the cargo and zig caches are decent in size, but a good portion of the cargo one isn't relevant, nor is the zigbuild cache mount worthwhile?

```console
# Cargo:
$ du -shx /buildkit-cache/cargo
298M    /buildkit-cache/cargo
# Bulk is from registry dir:
$ /buildkit-cache/cargo/registry/
217M    /buildkit-cache/cargo/registry/src
33M     /buildkit-cache/cargo/registry/cache
26M     /buildkit-cache/cargo/registry/index
275M    /buildkit-cache/cargo/registry/

# Zig:
du -shx /buildkit-cache/zig
164M    /buildkit-cache/zig

# Zigbuild:
# Nothing worthwhile to cache? (plus it created another folder for itself):
du -shx /buildkit-cache/cargo-zigbuild/cargo-zigbuild/0.20.0
24K     /buildkit-cache/cargo-zigbuild/cargo-zigbuild/0.20.0

# Rustup for reference (before optimization):
$ du -hx --max-depth=1 /buildkit-cache/rustup
4.0K    /buildkit-cache/rustup/tmp
1.5G    /buildkit-cache/rustup/toolchains
8.0K    /buildkit-cache/rustup/update-hashes
4.0K    /buildkit-cache/rustup/downloads
1.5G    /buildkit-cache/rustup
# This was built without minimal profile applied + only x86_64 musl target:
$ du -hx --max-depth=1 /buildkit-cache/rustup/toolchains/1.86-x86_64-unknown-linux-gnu
20K     /buildkit-cache/rustup/toolchains/1.86-x86_64-unknown-linux-gnu/etc
1.4M    /buildkit-cache/rustup/toolchains/1.86-x86_64-unknown-linux-gnu/libexec
728M    /buildkit-cache/rustup/toolchains/1.86-x86_64-unknown-linux-gnu/share
73M     /buildkit-cache/rustup/toolchains/1.86-x86_64-unknown-linux-gnu/bin
679M    /buildkit-cache/rustup/toolchains/1.86-x86_64-unknown-linux-gnu/lib
1.5G    /buildkit-cache/rustup/toolchains/1.86-x86_64-unknown-linux-gnu
```

As per [my PR attempt](https://github.com/astral-sh/uv/pull/3372/files#r1590189557), the bulk of the cargo cache mount there is from data that is quick to generate/compute at build time, thus not worth persisting. I used two separate tmpfs cache mounts to filter those out:

```Dockerfile
  # These are redundant as they're easily reconstructed from cache above. Use TMPFS mounts to exclude from cache mounts:
  # TMPFS mount is a better choice than `rm -rf` command (which is risky on a cache mount that is shared across concurrent builds).
  --mount=type=tmpfs,target="${CARGO_HOME}/registry/src" \
  --mount=type=tmpfs,target="${CARGO_HOME}/git/checkouts" \
```

Only relevant if storage of the cache mount is a concern, which it may be for CI limits to keep tame, otherwise is overkill :)

---

_Review comment by @polarathene on `Dockerfile`:50 on 2025-04-29 07:06_

Minor improvement from [my rejected PR](https://github.com/astral-sh/uv/pull/3372) was to fail early, such as with pipelines with `curl ... | sh ...`.

You'd add this `SHELL` instruction at the top of the file:

```Dockerfile
FROM --platform=$BUILDPLATFORM ubuntu AS build
# Configure the shell to exit early if any command fails, or when referencing unset variables.
# Additionally `-x` outputs each command run, this is helpful for troubleshooting failures.
SHELL ["/bin/bash", "-eux", "-o", "pipefail", "-c"]
```

I had some build failures when building the image locally, for `RUN` with multiple chains of commands, `+x` would have been a bit useful. Took a while for me to realize the issue with `rustup` I encountered was only reproducible with a cache mount being accessed concurrently 😓

---

_Comment by @polarathene on 2025-04-29 07:36_

Hello 👋 I only just got notified about this alternative PR to mine when mine was rejected 😓

> The `/root/target` cache mount is no longer cached or restored in the GitHub pipeline, but it is still hugely helpful when building the docker container locally as it massively speeds up incremental builds in that case. Propagating the cache to the Github action cache was not effective

The target cache last I recall does not work well with CI systems due to cargo relying on mtime for cache invalidation IIRC. Might have been related to source files mtime, so git checkouts (_which don't include mtime in commits_) would be a mismatch to the target cache each time, preventing cache re-use.

There was talk about that upstream to be handled differently but last I heard this had not changed 🤔 (if it has awesome).

So it may only be beneficial to local builds, not for the GHA runners. Any build layer skipping when there is no changes is different, that's due to Docker layer cache, if the inputs haven't changed and there is a cache layer mapping for that it'll be used, otherwise invalidated.

So your cache mount just never got used in that scenario in the first place, and with/without that cache mount you'd find that on Github due to the caveat with cargo that target cache won't be used, hence why it seems ineffective always vs locally 😅

You could look into `sscache` to workaround the issue in the meantime, but IIRC there are some caveats there to be aware of where cache may accidentally be used (_unless something has changed since I last read about that issue_).

---

_@polarathene reviewed on 2025-04-29 08:21_

My full review feedback is a bit verbose, but unless you need more details, here's a TLDR and summary of each concern in the feedback comments :)

- [ ] Rustup cache mount is not compatible for concurrent builds unless you **lock it's access**.
  - Alternatively (preferably) **don't store the toolchain in a cache mount** (_where it can be evicted, breaking future builds_), but without a minor refactor this will waste 1.4GB of disk when building both targets.
- [ ] Use `rustup toolchain install` and install both targets (_+135 MB weight for an AMD64 image to support ARM64 target_).
  - This will also respect `--profile minimal` saving 600MB+, so it'll still use less disk overall.
- [ ] Optionally improve troubleshooting build failures via setting `SHELL`.

Great to see your PR adopt the action support to persist cache mounts too btw 😎 

---

The below snippets use HereDoc syntax as it's arguably far better to grok/maintain, feel free to keep in the existing format instead for a better diff to assist review (_HereDoc syntax could always be added as a follow-up PR_).

## Installing toolchain via `rustup` with concurrent builds

I don't think you should use a cache mount for `rustup`, the toolchain + target(s) it installs at least should not be part of the cache mount. Keep them in the image like you do with `zig` and `gcc`.

If you do keep it with a cache mount, there is a not so obvious failure when doing concurrent platform builds that both want to write to the same location at once as they install their own copy of the toolchain. To prevent that you'd need `,sharing=locked` on the cache mount options.

I'd personally just cache it in a layer and let updates to `rust-toolchain.toml` invalidate it:

```Dockerfile
RUN \
  <<HEREDOC
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --profile minimal -- default-toolchain none

  echo 'targets = [ "aarch64-unknown-linux-musl", "x86_64-unknown-linux-musl" ]' >> rust-toolchain.toml
  rustup toolchain install
HEREDOC
```

If you're concerned about prior layers invalidating that, you could use a separate stage to minimize that concern and `COPY --link` the `RUSTUP_HOME` and any other necessary changes.

---

## Removing `RUSTUP_HOME` from cache mount can bloat disk usage by 1.4GB

If you remove the cache mount for rustup, you will encounter another concern where disk usage for both AMD64 + ARM64 platforms diverges from the earlier `ARG TARGETPLATFORM` + `RUN`.

Those two instructions can be removed so that the image only diverges by platform for the `RUN` that actually builds `uv`, all that changes is that instead of `rust_target.txt` an ENV is used (_`CARGO_BUILD_TARGET`, an actual cargo ENV_) to store the build target. That's handled by updating the already existing switch-case statement.

Here's what that looks like:

```Dockerfile
ARG TARGETPLATFORM
RUN \
  # Use bind mounts to access Cargo config, lock, and sources; without needing to
  # copy them into a build layer (avoids bloating the docker build layer cache):
  --mount=type=bind,source=crates,target=crates \
  --mount=type=bind,source=Cargo.toml,target=Cargo.toml \
  --mount=type=bind,source=Cargo.lock,target=Cargo.lock \
  # Add cache mounts to speed up builds:
  --mount=type=cache,target=${HOME}/target/ \
  --mount=type=cache,target=/buildkit-cache,id="tool-caches" \
  <<HEREDOC
  # Handle platform differences like mapping target arch to naming convention used by cargo targets:
  # https://en.wikipedia.org/wiki/X86-64#Industry_naming_conventions
  case "${TARGETPLATFORM}" in
    ( 'linux/amd64' )
      export CARGO_BUILD_TARGET='x86_64-unknown-linux-musl'
      ;;
    ( 'linux/arm64' )
      export CARGO_BUILD_TARGET='aarch64-unknown-linux-musl'
      export JEMALLOC_SYS_WITH_LG_PAGE=16
      ;;
    ( * )
      echo "ERROR: Unsupported target platform: '${TARGETPLATFORM}'"
      return 1
      ;;
  esac

  cargo zigbuild --release --bin uv --bin uvx --target "${CARGO_BUILD_TARGET}"
  cp "target/${CARGO_BUILD_TARGET}/release/uv" /uv
  cp "target/${CARGO_BUILD_TARGET}/release/uvx" /uvx
HEREDOC
```

---

## Better troubleshooting with `SHELL`

Final note, an optional improvement that improves troubleshooting when stuff breaks, is to add this `SHELL` instruction to the top of the `Dockerfile`:

```Dockerfile
FROM --platform=$BUILDPLATFORM ubuntu AS build
# Configure the shell to exit early if any command fails, or when referencing unset variables.
# Additionally `-x` outputs each command run, this is helpful for troubleshooting failures.
SHELL ["/bin/bash", "-eux", "-o", "pipefail", "-c"]
```

---
