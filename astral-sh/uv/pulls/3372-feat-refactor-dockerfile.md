```yaml
number: 3372
title: "feat: Refactor `Dockerfile`"
type: pull_request
state: closed
author: polarathene
labels: []
assignees: []
base: main
head: patch-1
created_at: 2024-05-04T09:43:55Z
updated_at: 2025-04-29T09:41:07Z
url: https://github.com/astral-sh/uv/pull/3372
synced_at: 2026-01-10T11:10:33Z
```

# feat: Refactor `Dockerfile`

---

_Pull request opened by @polarathene on 2024-05-04 09:43_

## Summary

**TL;DR:**
- 4GB disk usage down to 1.7GB (_some extra weight externalized via cache mounts_).
- 68MB binary now builds to smaller 24MB size.
- Layer invalidation concerns reduced, especially with use of cache mounts (_less meaningful to CI, but good for local use_).
- Additional changes detailed below.

---

- The first commit introduces the larger diff as adopting HereDoc feature introduces indents and merging of some `RUN` layers.
  - I've also relocated and grouped some directives where appropriate, minimizing layer invalidation.
  - HereDoc usage brings the added benefit of better formatting for multi-line `RUN` content.
  - There doesn't seem to be a need for the early `--target` until explicitly adding the target triple.
- 2nd commit (will follow shortly) addresses excess disk weight for the builder image layers that's less obvious from the multi-stage build.
  - When ignoring the final stage, the total disk usage for the builder image is about 4GB.
    <details>
    <summary>Expand to see breakdown (4.11GB image)</summary>

    - **80MB** => Ubuntu base image (~116MB after apt update)
    - **+450MB** => apt update + system deps
    - **+13MB** => pip `/root/.venv` creation
    - **+390MB** => pip install cargo-zigbuild
      - 306MB `/root/.venv/lib/python3.12/site-packages/ziglang`
      - 79MB `/root/.cache/pip/http-v2` (_can be skipped with `pip install --no-cache-dir`_)
      - 2.7MB `/root/.venv/bin/cargo-zigbuild`
    - **+1.3GB** => rustup + toolchain + target setup
      - 15MB rustup install
      - 1.3GB `rust toolchain target add`, (_Over 600MB is from toolchain installing docs at `~/.rustup/toolchains/1.78-x86_64-unknown-linux-gnu/share/doc/rust/html`, the `minimal` rustup profile doesn't apply due to `rust-toolchain.toml`_)
    - **+1.8GB** => Build cache for `uv`:
      - 270MB `/root/.cache/zig from cargo-zigbuild`
      - 300MB `/root/.cargo/registry/` (_51MB `cache/`, 21MB `index/`, 225MB `src`_)
      - 1.2GB `target/` dir
        - 360MB `target/release/{build,deps}` (_180MB each_)
        - 835MB `target/<triple>/release/` (_168MB `build/`, 667MB `deps/`_)
      - 68MB `uv` (_copied from target build dir_)

    ---
    </details>

  - The `RUN --mount` usage allows for caching useful data locally that would otherwise be invalidated across builds when you'd rather have it available. It also minimizes the impact of unused layers from past builds piling up until a prune is run. This is mostly a benefit for local usage rather than CI runners.
  - With the fix for the rustup profile and proper handling of cacheable content, the image size falls below 1.6GB and iterative builds of the image are much faster.
- The 3rd / 4th commits simplify the cross-platform build. There isn't a need for the conditional block to install a target.
  - Since the image is to be built from the same build host (`FROM --platform=${BUILDPLATFORM}`), you can just install both targets in advance? Then use `TARGETPLATFORM` ARG to implicitly get the preferred platform  binary build.
  - Now the `builder` stage shares all the same common layers, instead of diverging at the `TARGETPLATFORM` ARG. This is viable due to `cargo-zigbuild` supporting cross-platform builds easily.
- The 5th commit tackles the inline comment about optimizing the build size of the binary, reducing it from the current 68MB to 24MB, better than the current `uv` binary distributed via other install methods. You can use UPX to reduce this furth from 23.6MB to 6.7MB (_<10% current published size for the image_), at the expense of 300ms startup latency and potentially false positive to AV scanners.



## Test Plan

I've built the image locally, you still get the binary output successfully. Let me know if you'd like me to test something specific?

```console
# Using Docker v25 on Windows 11 via WSL2, with docker-container driver builder:
$ docker buildx create --name=container --driver=docker-container --use --bootstrap

# Excess platforms omitted:
$ docker buildx ls
NAME/NODE     DRIVER/ENDPOINT             STATUS  BUILDKIT PLATFORMS
container *   docker-container
  container0  unix:///var/run/docker.sock running v0.13.2  linux/amd64, linux/arm64

# Building with both platforms and outputting the image contents from both platforms:
$ docker buildx build --builder=container --platform=linux/arm64,linux/amd64 --tag local/uv --output type=local,dest=/tmp/uv .
# ... Build output ...
 => exporting to client directory
 => => copying files linux/arm64 20.98MB
 => => copying files linux/amd64 23.67MB

# Exported contents:
$ ls /tmp/uv/*
/tmp/uv/linux_amd64:
io  uv
/tmp/uv/linux_arm64:
io  uv

# AMD64:
$ du -h /tmp/uv/linux_amd64/uv
23M     /tmp/uv/linux_amd64/uv
$ file /tmp/uv/linux_amd64/uv
/tmp/uv/linux_amd64/uv: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, stripped

# ARM64:
$ du -h /tmp/uv/linux_arm64/uv
20M     /tmp/uv/linux_arm64/uv
$ file /tmp/uv/linux_arm64/uv
/tmp/uv/linux_arm64/uv: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, stripped
```

```console
# Equivalent output to running `docker run --rm -it local/uv`
$ /tmp/uv/linux_arm64/uv
Usage: uv [OPTIONS] <COMMAND>

Commands:
  pip      Resolve and install Python packages
  venv     Create a virtual environment
  cache    Manage the cache
  version  Display uv's version
  help     Print this message or the help of the given subcommand(s)

Options:
  -q, --quiet                  Do not print any output
  -v, --verbose...             Use verbose output
      --color <COLOR_CHOICE>   Control colors in output [default: auto] [possible values: auto, always, never]
      --native-tls             Whether to load TLS certificates from the platform's native certificate store [env: UV_NATIVE_TLS=]
  -n, --no-cache               Avoid reading from or writing to the cache [env: UV_NO_CACHE=]
      --cache-dir <CACHE_DIR>  Path to the cache directory [env: UV_CACHE_DIR=]
  -h, --help                   Print help (see more with '--help')
  -V, --version                Print version
```


---

_Marked ready for review by @polarathene on 2024-05-04 14:53_

---

_Review requested from @konstin by @charliermarsh on 2024-05-04 21:51_

---

_Comment by @charliermarsh on 2024-05-04 21:51_

Tagging @konstin for this one.

---

_Review comment by @polarathene on `Dockerfile`:63 on 2024-05-05 00:58_

I've used the `RUSTFLAGS` ENV here, although you can configure these in `Cargo.toml` + `.cargo/config.toml` (_`relocation-model` and `target-feature` would still be rustflags IIRC_). Presumably this is only relevant to the CI / Docker builds, so they're probably better managed here?

- I'm not sure if `opt-level` with `z` (best size) is ideal? It should probably be compared/benched against a build for performance, since that is a focus of `uv` I doubt a little size savings is worth it if the performance regresses quite a bit. I'm not familiar with this project to know how you'd want to verify the impact.
- `+crt-static` isn't necessary at the moment for the `musl` targets being built, but there has been talk about future changes for these targets to default to dynamic linking, so I've included it as more of a precaution, it also better communicates the desire for a static linked build.
- `relocation-model=static` tends to help save on binary size, I think this is ok. The default AFAIK is more of a security feature for memory layout, but I'm not sure if that's a concern for `uv` as an attack vector (_some attacker with access to read memory_). It's related to PIE if you're familiar with that.

---

_Review comment by @polarathene on `Dockerfile`:76 on 2024-05-05 01:24_

The cache mounting here is a bit excessive, I've added some context with comments. You're not really going to need to maintain much here AFAIK, so it's mostly out of sight/mind in practice.

If you're not familiar with this feature, the default `sharing` type is `shared`, which allows other concurrent builds to share the same data for each mount by matching `id`, if no `id` is configured it implicitly uses the `target` path.

We have 3 caches here, `zig`, `cargo`, and the project specific `target/` directory.
- `zig` I'm not familiar with it's cache system. I assume it's a global cache from the path and that it's fine for concurrent writers (_aka `sharing=shared`, the implicit default_).
- `cargo` mounts the entire cargo home location as it doesn't yet have an isolated cache location.
  - This does mean if any other `Dockerfile` used the `id` of `cargo-cache` they'd both be sharing the same contents, including whatever is installed in `bin/`.
  - It'd normally not be safe for concurrent writers AFAIK, but due to the lock files cargo manages here, it's a non-issue.
- The `target/` location has an `id` assigned per `TARGETPLATFORM`, and is also isolated by `APP_NAME` (`uv`) as this is not useful to share with other `Dockerfile` unrelated to `uv` project.
  - While there shouldn't be any concurrent writers, if someone was to to do several parallel builds with `RUSTFLAGS` arg being changed, the `sharing=locked` will block them as they'd all want to compile the dependencies again (_making the cache a bit less useful, but not as wasteful as individual caches via `sharing=private` which aren't deterministic for repeat builds_).
  - A previous commit did take a different approach, where the build process was part of the original builder stage and built both targets together, instead of relying on `TARGETPLATFORM`. If you don't mind always building for both targets, they could share a bit of a common `target/release/` cache contents AFAIK. The logic for the two commands below was complicated a bit more with a bash loop (_since static builds require and explicit `--target` AFAIK, even when multiple targets are configured via `rust-toolchain.toml` / `.cargo/config.toml`?_), so I opted for keeping the logic below simple to grok by using separate `TARGETPLATFORM` caches (_these can still build concurrently as I demo'd in the PR description_).

The last two `tmpfs` mounts are rather self-explanatory. There is no value in caching these to disk.

If you do want something that looks simpler, Earthly has tooling to simplify rust builds and cache management. However `Dockerfile` is more common knowledge to have and troubleshoot/maintain, adding another tool/abstraction didn't seem worthwhile to contribute.

---

_Review comment by @polarathene on `Dockerfile`:80 on 2024-05-05 01:26_

This is using `--release`, but I noticed in your `Cargo.toml` you have a custom release profile that uses `lto = "thin"`, it could be changed to use that profile instead, otherwise LTO should implicitly default to thin-local.

---

_Review comment by @polarathene on `Dockerfile`:55 on 2024-05-05 01:36_

This is effectively what you were doing earlier with the bash conditional statement on `TARGETPLATFORM` arg. But instead of writing to a file (`rust_target.txt`), a new intermediary stage is introduced and sets the expected target as an appropriate ENV.

This actually removes the need for the `--target` option when building, but I kept it for clarity.

The next stage (`builder-app`) refers to the `TARGETARCH` implicit arg from BuildKit. Thus when building the `Dockerfile` the stage selection here is determined from the final stage being built.

It'll remain as being built from the same `BUILDPLATFORM` above, but diverge at this point. Due to the cache mounts used in `builder-app`, this isn't a big concern, you really only have a few MB repeated from the `COPY` instruction, followed by the binary `cp` (_since the `target/` cache mount excludes the original binary from the image_).

A prior commit in this PR history had an alternative approach where both targets were built, and these separate stages were located at the end of the `Dockerfile`, where they instead used `COPY --from` with the location to their respective target binary.

---

_Review comment by @polarathene on `Dockerfile`:48 on 2024-05-05 01:46_

When installing `rustup` in the first line, as the commit history notes (and the PR description), setting `--target` was redundant, it emits a warning that it will ignore it.

Likewise there was a mishap with the `--profile minimal` being ignored when `rust-toolchain.toml` exists and doesn't define it's own `profile` key. If you'd like that file could add this line instead? I've raised an issue upstream to get the regression in `rustup` fixed.

The next line added to `rust-toolchain.toml` is probably not something you'd want to add to the actual file, since both targets aren't mandatory 🤷‍♂️ 
- I could instead add them individually with `rustup target add ...` in their respective stages that come afterwards
- Due to the `--default-toolchain none` this ~~still requires the `rustup show` workaround~~ (_**UPDATE:** Since Aug 2024, you can use [`rustup toolchain install`](https://github.com/rust-lang/rustup/pull/3983#issuecomment-2275736111) instead of `rustup show`_), otherwise you'd install the toolchain twice. An alternative is to use an ARG directive for passing in the toolchain version to use for `--default-toolchain`, that way you could just build with the current stable release, but if you really need to install a toolchain that matches `rust-toolchain.toml`, provide that via the ARG instead?

---

_Review comment by @polarathene on `Dockerfile`:22 on 2024-05-05 01:48_

The cache mount here (_and workaround for `docker-clean` in this base image used_), doesn't bring any savings to disk usage of this layer. It does however keep the cache external instead of the automatic clean, which is useful when the layer is invalidated, as it speeds up the build time.

---

_@polarathene reviewed on 2024-05-05 01:51_

Some contextual feedback, apologies for the verbosity. It's roughly what I've already covered in the PR description, but targeted, along with some additional insights if helpful.

Might help ease reviewing the diff directly 😎 

---

_Review comment by @samypr100 on `Dockerfile`:66 on 2024-05-06 01:10_

```suggestion
  --mount=type=cache,target="$HOME/.cache/zig",id="zig-cache" \
```

Worth using `$HOME` to be explicit

---

_@samypr100 reviewed on 2024-05-06 01:23_

---

_Comment by @konstin on 2024-05-06 10:42_

Hi, and thanks for looking into our dockerfile! I have two main concerns with the changes:

Docker is a secondary target for us (most users are using the curl, powershell or pip installer), so we'd like to keep it simple to maintain. The main audience are projects with a fully dockerized CI pipeline who want to run ruff or uv as a docker step; to people creating docker images we recommend the pip or curl installers inside the dockerfile. We don't intend for users to build the uv container themselves, so we haven't optimized for builder size. There is one big exception though: Building the image for the release job is the bottleneck in our release process, if that could be improved that would be amazing!

Performance is crucial for us, and we optimize performance over binary size.

---

_Assigned to @konstin by @konstin on 2024-05-06 10:42_

---

_Comment by @polarathene on 2024-05-10 07:57_

**TL;DR:** I'm happy to adjust the `Dockerfile` if you'd still like to keep the build process in there.

I can look into optimizing the workflow to bring down that bottleneck time with compilation, but I suspect the pragmatic approach might just be to take an existing `uv` build from CI and publish an image with that?

---

> so we'd like to keep it simple to maintain.

Sure! I can make an adjustment that prioritizes that over the other optimizations 👍 

> There is one big exception though: Building the image for the release job is the bottleneck in our release process, if that could be improved that would be amazing!

~~Could you please elaborate a bit more here? Is it with Github Actions and the time it takes to build the image?~~

I looked at the recent workflow runs and see the bulk of the time is spent compiling `amd64` + `arm64` stages concurrently, taking about 20+ minutes to complete.

As long as future runs wouldn't trigger compiling everything all over again if you had the `target/` dir for each platform cached, that'd speed up that step quite a bit.

However, you're already building `uv` with these binaries outside of Docker in CI? Why not just use `FROM scratch` + `COPY` like the final stage of the current `Dockerfile`, skipping the bottleneck concern?

I prefer having a `Dockerfile` as an easy way to build a project and not worry about dependencies or other setup, but since you don't seem to have an audience for that and the build step is purely for your CI... might as well skip all this since you want to prioritize keeping maintenance simple? (_unless the build stage is useful to maintainers or users outside of CI?_)


---

_Comment by @konstin on 2024-05-10 17:15_

> However, you're already building uv with these binaries outside of Docker in CI? Why not just use FROM scratch + COPY like the final stage of the current Dockerfile, skipping the bottleneck concern?

We would like to retain the isolation of the Dockerfile for building the image. The build stage isn't used by us otherwise, we just want to publish the final image, everything else goes through `cargo build --release` or `maturin build --release`. Anything that makes the build faster (incl. caching) would be great. Note that we update dependencies often, so `Cargo.lock` doesn't work as a cache key unfortunately

---

_Comment by @polarathene on 2024-05-11 01:15_

I've tried to summarize the considerations in the TLDR, let me know which direction you'd like to take.

**TL;DR:**
- The build stage in the `Dockerfile` could be kept for potential use elsewhere, but an alternative stage that is easy to maintain could be used purely by the release CI to provide existing release binaries already built, completely removing the bottleneck.
- Otherwise, to optimize the cache you need to only cache what is relevant but presently you cache image layers that aren't helping that much and cannot cache the data of interest due to layer invalidation. You will need the cache mount syntax or similar approach to restore `target/` from cache.
- `sccache` is probably needed for the above cache optimization until `cargo` moves away from `mtime` based invalidation (_which makes `target/` cache likely ignored_).
- Splitting the image builds by target to separate runners and caching the final output is an alternative approach. It'll shift the maintenance complexity to the GH workflow where it needs to merge the two final images into a single multi-platform image manifest (_which is currently done implicitly_).

---

> We would like to retain the isolation of the Dockerfile for building the image.
> Anything that makes the build faster (incl. caching) would be great.

Just to confirm that isolation isn't serving any real purpose though? You could use the CI binaries you already built for AMD64/ARM64 if you wanted to right?

If you just want the build stage to remain in the `Dockerfile` as a useful isolated build environment for reproductions / documentation or something like that, that's fine 👍 

- You could still use that stage for testing PRs or anything else in CI if you find a use for it. However for the release workflow, where it's not really needed, you could have an alternative stage that does take an external binary as input.
- As you stated you don't really have any actual need or value with the build stage for the release as you're only publishing an image with the binary, you might as well use the same one that you distribute elsewhere?

At least that is just the more pragmatic / simple approach to take and doesn't complicate maintenance. To optimize the build stage for CI will require cache optimizations such as with the cache mounts I showed earlier, otherwise the layer that creates `target/` during build is invalidated and this data is discarded... which has no benefit to cache import/export like you presently do.

---

> Anything that makes the build faster (incl. caching) would be great. Note that we update dependencies often, so Cargo.lock doesn't work as a cache key unfortunately

`Cargo.lock` doesn't matter much here, it's the `target/` dir you want to cache, not the crates source. You can optionally create a cache by any content/input you'd like to, and when that has no cache hit GH will use the latest cache item instead IIRC.

You're looking at 1GB roughly after compilation. That can be compressed fairly quickly with zstd down to 300MB, there isn't much value in higher compression levels (_the tradeoff is barely worth it for 50MB-ish less_). So 600MB for both targets to build with. There is little value in caching anything else in the image itself, the toolchain and crates all install fairly fast that you're just wasting the 10GB cache storage available by doing that. I would only cache what is useful and would assist with the speedup.

I haven't tried `sccache`, but this might be worthwhile to look into. A key problem with builds in CI is that `git clone` doesn't preserve `mtime` attribute on files, yet `cargo` relies on that for it's own "cache key", thus even with the `target/` restored, since the source files have different mtime I think it'll still perform quite a bit of unnecessary compilation. By leveraging `sccache` instead, I think we can avoid the overhead that introduces.

Finally one other trick you can use is to split it across two runners, one for each target. In your case this works quite well due to the minimal image you actually care about. If the cache upload is only your final scratch image with just the binary, then you can quite easily leverage cache to retrieve both platforms (using the release tag as the cache key for example) and have these two separate images combined into a multi-platform manifest (_which is what's already happening implicitly_). The added complexity here splits the bottleneck of 20+ min build time in half, so that it more closely aligns with the other jobs running. You may not need this optimization though if that `sccache` approach works well enough.



---

_Comment by @konstin on 2024-05-14 15:33_

> Splitting the image builds by target to separate runners and caching the final output is an alternative approach. It'll shift the maintenance complexity to the GH workflow where it needs to merge the two final images into a single multi-platform image manifest (which is currently done implicitly).

Splitting into two separate jobs would be a good idea.

I don't have experience with caching docker builds beyond layer caching, if you know a way i'm happy to use that.

> You could still use that stage for testing PRs or anything else in CI if you find a use for it. However for the release workflow, where it's not really needed, you could have an alternative stage that does take an external binary as input.

We only build the docker container for release, we don't use those images ourselves otherwise.

> You're looking at 1GB roughly after compilation. That can be compressed fairly quickly with zstd down to 300MB, there isn't much value in higher compression levels (the tradeoff is barely worth it for 50MB-ish less). So 600MB for both targets to build with. There is little value in caching anything else in the image itself, the toolchain and crates all install fairly fast that you're just wasting the 10GB cache storage available by doing that. I would only cache what is useful and would assist with the speedup.

The github docs [says]:

> GitHub will remove any cache entries that have not been accessed in over 7 days. There is no limit on the number of caches you can store, but the total size of all caches in a repository is limited to 10 GB. Once a repository has reached its maximum cache storage, the cache eviction policy will create space by deleting the oldest caches in the repository.

If we get into a release cadence of once a week the cache will already be evicted by then, i'm afraid this wouldn't work for us.

---

_Comment by @polarathene on 2024-05-16 00:04_

**TL;DR:**

- You can build and cache on a schedule (daily) or when `main` branch is pushed to. Since it's only intended for releases, daily builds might work well? (_ideally minimizing cache storage use to not impact other workflows usage_)
- Your GH Actions cache is somehow at **50GB from the past 2 days**, despite the 10GB limit it's meant to impose 🤷‍♂️ 
- Splitting the build to separate runners should be simple enough I think.
- Cache mounts to speed up the build may be viable, I'll look into trying it with a test repo this weekend.
- You could alternatively consider [`depot.dev`](https://depot.dev) as they seem to have an open-source tier which may like other services equate to free use. They're apparently quite simple to switch to and claim to speed up image builds considerably (_especially ARM64 due to native build nodes_).

---

> If we get into a release cadence of once a week the cache will already be evicted by then, i'm afraid this wouldn't work for us.

Technically you could probably have a scheduled build job that just builds and caches the image, or triggers when the main branch is updated (_something we have at `docker-mailserver`, except we publish a preview/beta image_). You don't have to publish that way, but it should keep the cache available for an actual release.

That said, I don't know if there's any nice ways to partition/manage the CI cache... and wow, I just looked at [your current cache use](https://github.com/astral-sh/uv/actions/caches):

![image](https://github.com/astral-sh/uv/assets/5098581/9af19e63-4e8e-4e6f-9c60-0d949d7f170a)

![image](https://github.com/astral-sh/uv/assets/5098581/08efb3da-ef5b-4488-96bc-ba26787d4b64)

Your largest cache items are about 1GB of which multiple were uploaded within the same hours. So not quite sure what's happening there, perhaps a bit of a bug on GH side. You have one item (2MB) from almost a week ago, while everything else is within the past 2 days, so it's not like it's evicting the old items regularly when exceeding the 10GB capacity limit 🤔 

---

> Splitting into two separate jobs would be a good idea.

The way this is handled for `docker-mailserver` I explained [here](https://github.com/squidfunk/mkdocs-material/pull/6191#issuecomment-2089962754) with some rough examples demonstrated. We have it modularized into separate build and publish workflows, which a much smaller and simpler workflow(s) can then call (_GH Actions calls these "re-usable" workflows_).

That way you can separately build and cache the two images on a schedule or when commits are pushed to main branch like described above, with the separate publishing workflow triggered only on tagging new releases?

You wanted to keep the `Dockerfile` simple to maintain, so I'm not sure how you feel about this kind of change for the CI?

---

> I don't have experience with caching docker builds beyond layer caching, if you know a way i'm happy to use that.

I don't have any workflow examples to cite for this as I haven't done it yet myself. There is a GHA cache exporter for BuildKit that can be used.. oh you're already using it here:

https://github.com/astral-sh/uv/blob/0a055b79420fdf1342c278e97d0c78cc487df89b/.github/workflows/build-docker.yml#L64-L65

It was experimental when I implemented the workflows for `docker-mailserver`. There's a separate action that allows leveraging that cache method when building a `Dockerfile` that uses cache mounts. Seems [like there was a related effort for this approach 4 months ago](https://github.com/astral-sh/uv/pull/995#issuecomment-1899617504). [This is the action](https://github.com/reproducible-containers/buildkit-cache-dance) I had in mind for using to support cache mounts.

---

Alternatively, you could apply to [`depot.dev`](https://depot.dev) for an open-source plan to be granted for UV? They claim to be considerably faster at builds, automate the cache for you and offer native ARM64 hosts instead of slower QEMU emulation we presently are limited with on GH.

So from a maintenance point of view, `depot.dev` may be a simpler option available to you (_assuming like other services like Netlify and DockerHub that offer similar plans that haven't cost `docker-mailserver` anything to adopt_):

![image](https://github.com/astral-sh/uv/assets/5098581/4a33ae71-2373-4cab-92b9-ff9f8fe05170)


---

_Comment by @zanieb on 2024-05-17 01:41_

The cache mounts seem nice to have but yeah per my earlier attempt in #995 probably not worth it in GitHub Actions. I did consider https://github.com/reproducible-containers/buildkit-cache-dance but it didn't seem worth the dependency and complexity for us. It seems like the ecosystem is lagging a bit here and I've just been waiting for those upstream issues to be fixed. We could consider `depot.dev`, I'm not sure if we'd get a discount since we're VC funded but the project is open source 🤷‍♀️ I'm hesitant to administer an external service for something that's not causing us problems right now, but perhaps that's the simplest path forward.

Sorry about the back and forth and hesitant here. This is a tough pull request to review, we appreciate that you're investing time into it but the release artifacts are a critical part of our software. And, since we're a small team, we're careful about taking on more maintenance and services to manage.

---

_Comment by @polarathene on 2024-05-22 12:13_

This was written last week to respond on the same day but I didn't seem to send it (_just came across the tab_), I've still got this on my backlog and hopefully can tackle it this weekend instead 👍 

---

# Original response

> Sorry about the back and forth and hesitant here. This is a tough pull request to review, we appreciate that you're investing time into it but the release artifacts are a critical part of our software. And, since we're a small team, we're careful about taking on more maintenance and services to manage.

No worries! If you'd prefer to leave the `Dockerfile` as it is, we can close the PR - or I can improve only the `Dockerfile` and not bother with the caching improvement for the CI in this PR👍 

When I revisit this in the weekend I'll take this approach:
1. Build the image for each platform (AMD64 + ARM64) on separate runners.
2. Upload the final image output only as cache (_subsequent builds won't be sped up, but only relevant if you release again within 7 days_)
3. Next job depends on the prior two completing, brings in their output and publishes the multi-platform image.

---

Alternatively (_reduced complexity, minimal time to publish_):
1. The CI could verify the build stage is successful, without depending upon it for the binary to publish? This avoids any bottleneck on publish time for the actual image since publishing an image for release doesn't need to block on verifying a build via the `Dockerfile`?
2. A separate `release.Dockerfile` ([naming convention](https://docs.docker.com/build/building/packaging/#filename)) could be equivalent to the final `scratch` stage but with `COPY` adding CI build artifact for their respective image and publish that for a quick release?
3. This wouldn't change the value of the `Dockerfile` which still builds self-contained by default. Just a difference in how the CI uses it. Although I could understand wanting the officially published images to actually match the `Dockerfile` in the repo.

---

## Cache Mounts + CI Action

> I did consider https://github.com/reproducible-containers/buildkit-cache-dance but it didn't seem worth the dependency and complexity for us.

Anything in particular about the complexity? It adds one additional step into the mix (_technically two since you don't have `actions/cache` in this workflow_):

```yaml
      - name: inject cache into docker
        uses: reproducible-containers/buildkit-cache-dance@v3.1.0
        with:
          cache-map: |
            {
              "var-cache-apt": "/var/cache/apt",
              "var-lib-apt": "/var/lib/apt"
            }
          skip-extraction: ${{ steps.cache.outputs.cache-hit }}
```

All that's being configured is 1 or more locations in a container to cache, with a cache key to skip if an exact match (_`actions/cache` will download the latest relevant cache found if not an exact match_).

On the `Dockerfile` side, yes their example is more complicated:

```Dockerfile
FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive
RUN \
  --mount=type=cache,target=/var/cache/apt,sharing=locked \
  --mount=type=cache,target=/var/lib/apt,sharing=locked \
  rm -f /etc/apt/apt.conf.d/docker-clean && \
  echo 'Binary::apt::APT::Keep-Downloaded-Packages "true";' >/etc/apt/apt.conf.d/keep-cache && \
  apt-get update && \
  apt-get install -y gcc
```

This is because they're:
- Using two separate cache mounts.
- Ubuntu images have a post-hook for `apt-install` to clean up that cache, thus you need to disable that and adjust configuration.

With ubuntu images sometimes the `apt update` command can be slow, and for `apt install` it varies. Easily remedied by using an alternative base image like Fedora, but in your CI this isn't where the bottleneck is.

---

I can look into this as a follow-up, but I think just caching the `sccache` directory may help a fair amount? So the `Dockerfile` adjustment would be in the build stage and look something like this:

```Dockerfile
RUN \
  --mount=type=cache,target="/path/to/cache",id="cache-dirs" \
  <<HEREDOC
    cargo zigbuild --release --bin "${APP_NAME}" --target "${CARGO_BUILD_TARGET}"
    cp "target/${CARGO_BUILD_TARGET}/release/${APP_NAME}" "/${APP_NAME}"
HEREDOC
```

or as two separate `RUN` (_if that's easier to grok without the HereDoc_):

```Dockerfile
RUN --mount=type=cache,target="/path/to/cache",id="cache-dirs" \
    cargo zigbuild --release --bin "${APP_NAME}" --target "${CARGO_BUILD_TARGET}"

RUN cp "target/${CARGO_BUILD_TARGET}/release/${APP_NAME}" "/${APP_NAME}"
```


---

_Comment by @konstin on 2025-04-28 12:25_

I'm closing this since we're currently unlikely to make that big changes to our Dockerfile currently, see also #11106 for a smaller approach that adds caching to the docker builds.

---

_Closed by @konstin on 2025-04-28 12:25_

---

_Comment by @polarathene on 2025-04-29 09:39_

> for a smaller approach that adds caching to the docker builds.

I've gone over that PR and left a review. It seems to have the bulk of worthwhile changes I proposed + better `RUN --mount` setup 👍

---

> unlikely to make that big changes to our Dockerfile

My PR here does have a few extra changes and some more granular cargo cache mounts but other than that the PR diff itself looks much larger in changeset than it is (_since I used HereDoc + shuffled some parts around as an optimization_).

The other PR builds a 40MB executable. I haven't checked how my PR compares today, but I'm aware that `opt-level=z` was discussed as not desirable. This PR at the time was producing binaries that were only 24MB though, I don't have any comments saying that I built with LTO beyond default implicit thin-local LTO, presumably with the now current release profile `lto="fat"` it would build for even less 😅 

---
