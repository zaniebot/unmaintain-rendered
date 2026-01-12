```yaml
number: 8554
title: Add a ruff docker image at ghcr.io/astral-sh/ruff
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: docker-image
created_at: 2023-11-08T10:45:42Z
updated_at: 2023-12-07T14:15:47Z
url: https://github.com/astral-sh/ruff/pull/8554
synced_at: 2026-01-10T23:40:55Z
```

# Add a ruff docker image at ghcr.io/astral-sh/ruff

---

_Pull request opened by @konstin on 2023-11-08 10:45_

This dockerfile creates a minimal docker container that runs ruff

```console
$ docker run -v .:/io --rm ruff check --select G004 .
scripts/check_ecosystem.py:51:26: G004 Logging statement uses f-string
scripts/check_ecosystem.py:55:22: G004 Logging statement uses f-string
scripts/check_ecosystem.py:84:13: G004 Logging statement uses f-string
scripts/check_ecosystem.py:177:18: G004 Logging statement uses f-string
scripts/check_ecosystem.py:200:18: G004 Logging statement uses f-string
scripts/check_ecosystem.py:354:18: G004 Logging statement uses f-string
scripts/check_ecosystem.py:477:18: G004 Logging statement uses f-string
Found 7 errors.
```

```console
$ docker image ls ruff
 REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
 ruff         latest    505876b0f817   2 minutes ago   16.2MB
```

Test repo: https://github.com/konstin/release-testing2
Successful build: https://github.com/konstin/release-testing2/actions/runs/6862107104/job/18659155108
The package: https://github.com/konstin/release-testing2/pkgs/container/release-testing2

After merging this, i have to manually push the first image and connect it the repo in the github UI or the action will fail due to lack of permissions 

Open questions:
 * Test arm version: Anyone working on an aarch64 linux machine? I don't see this failing or a high-priority deployment (the vast majority of linux users is on x86), but it would be nice to have it tested one.

---

_Comment by @zanieb on 2023-11-08 19:43_

Here's an example of a CI workflow https://github.com/PrefectHQ/prefect/blob/main/.github/workflows/docker-images.yaml — the matrix is of course not necessary for us.

It'd be nice to submit an addition to the DockerHub official image collection, but to start I don't have a strong preference on DockerHub vs ghcr.io.

---

_Comment by @daveisfera on 2023-11-08 22:31_

This is great! I was able to build it for `linux/amd64` and `linux/arm64` with `buildx` so the multi-arch stuff should basically work out of the box. The question would be how many platforms/architecture combinations do you want to provide, because there's a pretty wide list tied to each release currently
I'd recommend to start with just `linux` as the platform with the desired architectures because most images are run that way already
Unless there's a functional difference that is important, I'd recommend to support only GNU or MUSL to keep things simple
And as far as location, I'd vote for an official image on Docker Hub but anywhere that's easily available would be GREAT

---

_Comment by @daveisfera on 2023-11-09 02:59_

Actually, the arm64 build failed with this error:
```
574.8 The following warnings were emitted during compilation:
574.8 
574.8 warning: "`background_threads_runtime_support` not supported for `x86_64-unknown-linux-musl`"
574.8 
574.8 error: failed to run custom build command for `tikv-jemalloc-sys v0.5.4+5.3.0-patched`
574.8 
574.8 Caused by:
574.8   process didn't exit successfully: `/target/release/build/tikv-jemalloc-sys-4976bb44dc369cbb/build-script-build` (exit status: 101)
574.8   --- stdout
574.8   TARGET=x86_64-unknown-linux-musl
574.8   HOST=aarch64-unknown-linux-gnu
574.8   NUM_JOBS=16
574.8   OUT_DIR="/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out"
574.8   BUILD_DIR="/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build"
574.8   SRC_DIR="/usr/local/cargo/registry/src/index.crates.io-6f17d22bba15001f/tikv-jemalloc-sys-0.5.4+5.3.0-patched"
574.8   cargo:rustc-cfg=prefixed
574.8   cargo:rerun-if-env-changed=JEMALLOC_OVERRIDE
574.8   OPT_LEVEL = Some("3")
574.8   TARGET = Some("x86_64-unknown-linux-musl")
574.8   HOST = Some("aarch64-unknown-linux-gnu")
574.8   cargo:rerun-if-env-changed=CC_x86_64-unknown-linux-musl
574.8   CC_x86_64-unknown-linux-musl = None
574.8   cargo:rerun-if-env-changed=CC_x86_64_unknown_linux_musl
574.8   CC_x86_64_unknown_linux_musl = None
574.8   cargo:rerun-if-env-changed=TARGET_CC
574.8   TARGET_CC = None
574.8   cargo:rerun-if-env-changed=CC
574.8   CC = None
574.8   RUSTC_LINKER = None
574.8   cargo:rerun-if-env-changed=CROSS_COMPILE
574.8   CROSS_COMPILE = None
574.8   cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
574.8   CRATE_CC_NO_DEFAULTS = None
574.8   DEBUG = Some("false")
574.8   CARGO_CFG_TARGET_FEATURE = Some("fxsr,sse,sse2")
574.8   cargo:rerun-if-env-changed=CFLAGS_x86_64-unknown-linux-musl
574.8   CFLAGS_x86_64-unknown-linux-musl = None
574.8   cargo:rerun-if-env-changed=CFLAGS_x86_64_unknown_linux_musl
574.8   CFLAGS_x86_64_unknown_linux_musl = None
574.8   cargo:rerun-if-env-changed=TARGET_CFLAGS
574.8   TARGET_CFLAGS = None
574.8   cargo:rerun-if-env-changed=CFLAGS
574.8   CFLAGS = None
574.8   CC="musl-gcc"
574.8   CFLAGS="-O3 -ffunction-sections -fdata-sections -fPIC -m64 -Wall"
574.8   JEMALLOC_REPO_DIR="jemalloc"
574.8   cargo:warning="`background_threads_runtime_support` not supported for `x86_64-unknown-linux-musl`"
574.8   cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_MALLOC_CONF
574.8   cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_PAGE
574.8   cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_HUGEPAGE
574.8   cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_QUANTUM
574.8   cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_VADDR
574.8   --with-jemalloc-prefix=_rjem_
574.8   running: cd "/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build" && CC="musl-gcc" CFLAGS="-O3 -ffunction-sections -fdata-sections -fPIC -m64 -Wall" CPPFLAGS="-O3 -ffunction-sections -fdata-sections -fPIC -m64 -Wall" LDFLAGS="-O3 -ffunction-sections -fdata-sections -fPIC -m64 -Wall" "sh" "/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build/configure" "--disable-cxx" "--enable-doc=no" "--enable-shared=no" "--with-jemalloc-prefix=_rjem_" "--with-private-namespace=_rjem_" "--host=x86_64-unknown-linux-musl" "--build=aarch64-unknown-linux-gnu" "--prefix=/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out"
574.8   checking for xsltproc... false
574.8   checking for x86_64-unknown-linux-musl-gcc... musl-gcc
574.8   checking whether the C compiler works... no
574.8   running: "tail" "-n" "100" "/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build/config.log"
574.8   XSLROOT=''
574.8   XSLTPROC='false'
574.8   a=''
574.8   abi=''
574.8   abs_objroot='/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build/'
574.8   abs_srcroot='/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build/'
574.8   ac_ct_CC=''
574.8   ac_ct_CXX=''
574.8   bindir='${exec_prefix}/bin'
574.8   build='aarch64-unknown-linux-gnu'
574.8   build_alias='aarch64-unknown-linux-gnu'
574.8   build_cpu=''
574.8   build_os=''
574.8   build_vendor=''
574.8   cfghdrs_in=''
574.8   cfghdrs_out=''
574.8   cfgoutputs_in=''
574.8   cfgoutputs_out=''
574.8   datadir='${datarootdir}'
574.8   datarootdir='${prefix}/share'
574.8   docdir='${datarootdir}/doc/${PACKAGE}'
574.8   dvidir='${docdir}'
574.8   enable_autogen=''
574.8   enable_cache_oblivious=''
574.8   enable_cxx='no'
574.8   enable_debug=''
574.8   enable_doc='no'
574.8   enable_experimental_smallocx=''
574.8   enable_fill=''
574.8   enable_initial_exec_tls=''
574.8   enable_lazy_lock=''
574.8   enable_log=''
574.8   enable_opt_safety_checks=''
574.8   enable_opt_size_checks=''
574.8   enable_prof=''
574.8   enable_readlinkat=''
574.8   enable_shared='no'
574.8   enable_static=''
574.8   enable_stats=''
574.8   enable_tls=''
574.8   enable_uaf_detection=''
574.8   enable_utrace=''
574.8   enable_xmalloc=''
574.8   enable_zone_allocator=''
574.8   exe=''
574.8   exec_prefix='/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out'
574.8   host='x86_64-unknown-linux-musl'
574.8   host_alias='x86_64-unknown-linux-musl'
574.8   host_cpu=''
574.8   host_os=''
574.8   host_vendor=''
574.8   htmldir='${docdir}'
574.8   importlib=''
574.8   includedir='${prefix}/include'
574.8   infodir='${datarootdir}/info'
574.8   install_suffix=''
574.8   je_=''
574.8   jemalloc_version=''
574.8   jemalloc_version_bugfix=''
574.8   jemalloc_version_gid=''
574.8   jemalloc_version_major=''
574.8   jemalloc_version_minor=''
574.8   jemalloc_version_nrev=''
574.8   libdir='${exec_prefix}/lib'
574.8   libdl=''
574.8   libexecdir='${exec_prefix}/libexec'
574.8   libprefix=''
574.8   link_whole_archive=''
574.8   localedir='${datarootdir}/locale'
574.8   localstatedir='${prefix}/var'
574.8   mandir='${datarootdir}/man'
574.8   o=''
574.8   objroot=''
574.8   oldincludedir='/usr/include'
574.8   pdfdir='${docdir}'
574.8   prefix='/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out'
574.8   private_namespace=''
574.8   program_transform_name='s,x,x,'
574.8   psdir='${docdir}'
574.8   rev='2'
574.8   sbindir='${exec_prefix}/sbin'
574.8   sharedstatedir='${prefix}/com'
574.8   so=''
574.8   srcroot=''
574.8   sysconfdir='${prefix}/etc'
574.8   target_alias=''
574.8 
574.8   ## ----------- ##
574.8   ## confdefs.h. ##
574.8   ## ----------- ##
574.8 
574.8   /* confdefs.h */
574.8   #define PACKAGE_NAME ""
574.8   #define PACKAGE_TARNAME ""
574.8   #define PACKAGE_VERSION ""
574.8   #define PACKAGE_STRING ""
574.8   #define PACKAGE_BUGREPORT ""
574.8   #define PACKAGE_URL ""
574.8 
574.8   configure: exit 77
574.8 
574.8   --- stderr
574.8   configure: error: in `/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build':
574.8   configure: error: C compiler cannot create executables
574.8   See `config.log' for more details
574.8   thread 'main' panicked at /usr/local/cargo/registry/src/index.crates.io-6f17d22bba15001f/tikv-jemalloc-sys-0.5.4+5.3.0-patched/build.rs:351:9:
574.8   command did not execute successfully: cd "/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build" && CC="musl-gcc" CFLAGS="-O3 -ffunction-sections -fdata-sections -fPIC -m64 -Wall" CPPFLAGS="-O3 -ffunction-sections -fdata-sections -fPIC -m64 -Wall" LDFLAGS="-O3 -ffunction-sections -fdata-sections -fPIC -m64 -Wall" "sh" "/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out/build/configure" "--disable-cxx" "--enable-doc=no" "--enable-shared=no" "--with-jemalloc-prefix=_rjem_" "--with-private-namespace=_rjem_" "--host=x86_64-unknown-linux-musl" "--build=aarch64-unknown-linux-gnu" "--prefix=/target/x86_64-unknown-linux-musl/release/build/tikv-jemalloc-sys-249710264dccf383/out"
574.8   expected success, got: exit status: 77
574.8   note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
574.8 warning: build failed, waiting for other jobs to finish...
```

---

_Comment by @konstin on 2023-11-09 11:22_

Made the multiarch build work for x86_64 and aarch64, do we need other architectures too?

The build works by cross compiling, so we avoid QEMU.

---

_@zanieb reviewed on 2023-11-09 14:37_

---

_Review comment by @zanieb on `.github/workflows/docker.yaml`:50 on 2023-11-09 14:37_

We can drop all the flavor and Python suffixes

```suggestion
            type=pep440,pattern={{version}}
            type=pep440,pattern={{major}}.{{minor}},enable=${{ github.event.release.prerelease == false }}
            type=pep440,pattern={{major}},enable=${{ github.event.release.prerelease == false }}
            type=raw,value=latest${{ matrix.flavor }},enable=github.event.release.prerelease == false }}
```

---

_@zanieb reviewed on 2023-11-09 14:40_

---

_Review comment by @zanieb on `.github/workflows/docker.yaml`:45 on 2023-11-09 14:40_

I'm not sure if we need this part? I think this was for rebuilding old versions?

---

_@zanieb reviewed on 2023-11-09 14:41_

---

_Review comment by @zanieb on `.github/workflows/docker.yaml`:60 on 2023-11-09 14:41_

Looks like you dropped the dev tags?

---

_Review comment by @konstin on `.github/workflows/docker.yaml`:45 on 2023-11-10 11:16_

I've just copied prefect for the initial implementation, this needs a bunch of iterations to work properly with our setup

---

_@konstin reviewed on 2023-11-10 11:16_

---

_Comment by @codspeed-hq[bot] on 2023-11-10 11:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/docker-image)

### Merging #8554 will **not alter performance**

<sub>Comparing <code>docker-image</code> (5be26b8) with <code>main</code> (f7d249a)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Renamed from "Add a ruff docker image" to "Add a ruff docker image at ghcr.io/astral-sh/ruff" by @konstin on 2023-11-14 10:16_

---

_Marked ready for review by @konstin on 2023-11-14 10:22_

---

_Comment by @konstin on 2023-11-14 10:26_

Added a github actions integration, this is ready

---

_Review requested from @zanieb by @charliermarsh on 2023-11-16 01:03_

---

_@zanieb reviewed on 2023-11-17 16:56_

---

_Review comment by @zanieb on `Dockerfile`:5 on 2023-11-17 16:56_

Probably still good to follow best practice to use `apt-get`, clean the cache, not install recommends.

Even if it's not in the production images, reducing the size of the cached layers is important.


---

_@zanieb approved on 2023-11-17 16:56_

---

_Merged by @konstin on 2023-11-17 18:44_

---

_Closed by @konstin on 2023-11-17 18:44_

---

_Branch deleted on 2023-11-17 18:44_

---

_Review comment by @jamesbraza on `Dockerfile`:9 on 2023-11-19 01:34_

Hello @konstin , I was just looking through `ruff` commit history and saw this PR.

One comment here is usually one just uses system Python in a Docker container, mainly cuz Docker containers are short-lived objects already.

I wanted to suggest a nice `Dockerfile` linter [hadolint](https://github.com/hadolint/hadolint) that has a lot of good suggestions:
- Using `pip install --no-cache-dir` to minimize image size
- Using `apt-get` over `apt` and removing the apt cache when done

Here is a sample `pre-commit` integration:

```yaml
    - repo: https://github.com/hadolint/hadolint
      rev: v2.12.1-beta
      hooks:
          - id: hadolint-docker
```

Feel free to disregard this as well, just sharing for reference. Cheers!

---

_@jamesbraza reviewed on 2023-11-19 01:34_

---

_@konstin reviewed on 2023-11-19 07:38_

---

_Review comment by @konstin on `Dockerfile`:9 on 2023-11-19 07:38_

 > One comment here is usually one just uses system Python in a Docker container, mainly cuz Docker containers are short-lived objects already.

I have bad experiences with user packages colliding with system packages, so i won't support that. As `pip install cargo-zigbuild` in the system environment will tell you: "Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv"

> Using pip install --no-cache-dir to minimize image size

This is a multi-stage docker build, the builder does not affect the final image size. As shown in the PR description, the total is 16.2MB, which is the size of the x86_64-unknown-linux-musl release binary.

> Using apt-get over apt and removing the apt cache when done

The `apt` interface is stable nowadays and the same as for the pip cache applies here.

---

_@jamesbraza reviewed on 2023-11-19 18:54_

---

_Review comment by @jamesbraza on `Dockerfile`:9 on 2023-11-19 18:54_

Alright sounds good!  The multi-stage build you set up here is pretty clever, nice work there

---

_Comment by @bschoenmaeckers on 2023-12-07 11:02_

I don't think this image is public accessible, because I can't find the image on the registry of this repo and `git pull ghcr.io/astral-sh/ruff` does not work.

---

_Comment by @konstin on 2023-12-07 14:15_

Sorry, it's public now: https://github.com/astral-sh/ruff/pkgs/container/ruff

---
