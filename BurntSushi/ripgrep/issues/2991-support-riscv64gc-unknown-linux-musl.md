```yaml
number: 2991
title: Support riscv64gc-unknown-linux-musl
type: issue
state: closed
author: bwbuhse
labels: []
assignees: []
created_at: 2025-02-16T19:03:51Z
updated_at: 2025-08-17T18:28:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2991
synced_at: 2026-01-12T16:13:25Z
```

# Support riscv64gc-unknown-linux-musl

---

_@bwbuhse_

Right now, building ripgrep fails because of `Invalid configuration `riscv64gc-unknown-linux-musl': machine `riscv64gc-unknown' not recognized`.

Upstream tikv/jemalloc supports this target: https://github.com/tikv/jemallocator/blob/fa4486d23f7402f3999b8c857333c60090001314/jemalloc-sys/build.rs#L393

```
$ cargo build
   Compiling jemalloc-sys v0.5.4+5.3.0-patched
    Building [=======================>   ] 47/51: jemalloc-sys(build)


warning: jemalloc-sys@0.5.4+5.3.0-patched: "`background_threads_runtime_support` not supported for `riscv64gc-unknown-linux-musl`"
error: failed to run custom build command for `jemalloc-sys v0.5.4+5.3.0-patched`

Caused by:
  process didn't exit successfully: `/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-e3436d56a82ecf43/build-script-build` (exi
t status: 101)
  --- stdout
  TARGET=riscv64gc-unknown-linux-musl
  HOST=riscv64gc-unknown-linux-musl
  NUM_JOBS=8
  OUT_DIR="/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out"
  BUILD_DIR="/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out/build"
  SRC_DIR="/home/ben/.cargo/registry/src/index.crates.io-6f17d22bba15001f/jemalloc-sys-0.5.4+5.3.0-patched"
  cargo:rustc-cfg=prefixed
  cargo:rerun-if-env-changed=JEMALLOC_OVERRIDE
  OPT_LEVEL = Some(0)
  TARGET = Some(riscv64gc-unknown-linux-musl)
  OUT_DIR = Some(/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out)
  HOST = Some(riscv64gc-unknown-linux-musl)
  cargo:rerun-if-env-changed=CC_riscv64gc-unknown-linux-musl
  CC_riscv64gc-unknown-linux-musl = None
  cargo:rerun-if-env-changed=CC_riscv64gc_unknown_linux_musl
  CC_riscv64gc_unknown_linux_musl = None
  cargo:rerun-if-env-changed=HOST_CC
  HOST_CC = None
  cargo:rerun-if-env-changed=CC
  CC = None
  cargo:rerun-if-env-changed=CC_ENABLE_DEBUG_OUTPUT
  RUSTC_WRAPPER = None
  cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
  CRATE_CC_NO_DEFAULTS = None
  DEBUG = Some(true)
  CARGO_CFG_TARGET_FEATURE = Some(a,c,m)
  cargo:rerun-if-env-changed=CFLAGS_riscv64gc-unknown-linux-musl
  CFLAGS_riscv64gc-unknown-linux-musl = None
  cargo:rerun-if-env-changed=CFLAGS_riscv64gc_unknown_linux_musl
  CFLAGS_riscv64gc_unknown_linux_musl = None
  cargo:rerun-if-env-changed=HOST_CFLAGS
  HOST_CFLAGS = None
  cargo:rerun-if-env-changed=CFLAGS
  CFLAGS = None
  CC="cc"
  CFLAGS="-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -march=rv64gc -mabi=lp64d -Wall"
  JEMALLOC_REPO_DIR="jemalloc"
  cargo:warning="`background_threads_runtime_support` not supported for `riscv64gc-unknown-linux-musl`"
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_MALLOC_CONF
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_PAGE
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_HUGEPAGE
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_QUANTUM
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_VADDR
  --with-jemalloc-prefix=_rjem_
  running: cd "/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out/build" && CC="cc" CFLAGS="-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -march=rv64gc -mabi=lp64d -Wall" CPPFLAGS="-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -march=rv64gc -mabi=lp64d -Wall" LDFLAGS="-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -march=rv64gc -mabi=lp64d -Wall" "sh" "/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out/build/configure" "--disable-cxx" "--enable-doc=no" "--enable-shared=no" "--with-jemalloc-prefix=_rjem_" "--with-private-namespace=_rjem_" "--host=riscv64gc-unknown-linux-musl" "--build=riscv64gc-unknown-linux-musl" "--prefix=/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out"
  checking for xsltproc... /usr/bin/xsltproc
  checking for riscv64gc-unknown-linux-musl-gcc... cc
  checking whether the C compiler works... yes
  checking for C compiler default output file name... a.out
  checking for suffix of executables...
  checking whether we are cross compiling... no
  checking for suffix of object files... o
  checking whether we are using the GNU C compiler... yes
  checking whether cc accepts -g... yes
  checking for cc option to accept ISO C89... none needed
  checking whether compiler is cray... no
  checking whether compiler supports -std=gnu11... yes
  checking whether compiler supports -Werror=unknown-warning-option... no
  checking whether compiler supports -Wall... yes
  checking whether compiler supports -Wextra... yes
  checking whether compiler supports -Wshorten-64-to-32... no
  checking whether compiler supports -Wsign-compare... yes
  checking whether compiler supports -Wundef... yes
  checking whether compiler supports -Wno-format-zero-length... yes
  checking whether compiler supports -Wpointer-arith... yes
  checking whether compiler supports -Wno-missing-braces... yes
  checking whether compiler supports -Wno-missing-field-initializers... yes
  checking whether compiler supports -Wno-missing-attributes... yes
  checking whether compiler supports -pipe... yes
  checking whether compiler supports -g3... yes
  checking how to run the C preprocessor... cc -E
  checking for grep that handles long lines and -e... /usr/bin/grep
  checking for egrep... /usr/bin/grep -E
  checking for ANSI C header files... yes
  checking for sys/types.h... yes
  checking for sys/stat.h... yes
  checking for stdlib.h... yes
  checking for string.h... yes
  checking for memory.h... yes
  checking for strings.h... yes
  checking for inttypes.h... yes
  checking for stdint.h... yes
  checking for unistd.h... yes
  checking whether byte ordering is bigendian... no
  checking size of void *... 8
  checking size of int... 4
  checking size of long... 8
  checking size of long long... 8
  checking size of intmax_t... 8
  checking build system type... running: "tail" "-n" "100" "/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out/build/config.log"
  dvidir='${docdir}'
  enable_autogen=''
  enable_cache_oblivious=''
  enable_cxx='0'
  enable_debug=''
  enable_doc='no'
  enable_experimental_smallocx=''
  enable_fill=''
  enable_initial_exec_tls=''
  enable_lazy_lock=''
  enable_log=''
  enable_opt_safety_checks=''
  enable_opt_size_checks=''
  enable_prof=''
  enable_readlinkat=''
  enable_shared='no'
  enable_static=''
  enable_stats=''
  enable_tls=''
  enable_uaf_detection=''
  enable_utrace=''
  enable_xmalloc=''
  enable_zone_allocator=''
  exe=''
  exec_prefix='/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out'
  host='riscv64gc-unknown-linux-musl'
  host_alias='riscv64gc-unknown-linux-musl'
  host_cpu=''
  host_os=''
  host_vendor=''
  htmldir='${docdir}'
  importlib=''
  includedir='${prefix}/include'
  infodir='${datarootdir}/info'
  install_suffix=''
  je_=''
  jemalloc_version=''
  jemalloc_version_bugfix=''
  jemalloc_version_gid=''
  jemalloc_version_major=''
  jemalloc_version_minor=''
  jemalloc_version_nrev=''
  libdir='${exec_prefix}/lib'
  libdl=''
  libexecdir='${exec_prefix}/libexec'
  libprefix=''
  link_whole_archive=''
  localedir='${datarootdir}/locale'
  localstatedir='${prefix}/var'
  mandir='${datarootdir}/man'
  o=''
  objroot=''
  oldincludedir='/usr/include'
  pdfdir='${docdir}'
  prefix='/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out'
  private_namespace=''
  program_transform_name='s,x,x,'
  psdir='${docdir}'
  rev='2'
  sbindir='${exec_prefix}/sbin'
  sharedstatedir='${prefix}/com'
  so=''
  srcroot=''
  sysconfdir='${prefix}/etc'
  target_alias=''

  ## ----------- ##
  ## confdefs.h. ##
  ## ----------- ##

  /* confdefs.h */
  #define PACKAGE_NAME ""
  #define PACKAGE_TARNAME ""
  #define PACKAGE_VERSION ""
  #define PACKAGE_STRING ""
  #define PACKAGE_BUGREPORT ""
  #define PACKAGE_URL ""
  #define JEMALLOC_HAS_RESTRICT
  #define STDC_HEADERS 1
  #define HAVE_SYS_TYPES_H 1
  #define HAVE_SYS_STAT_H 1
  #define HAVE_STDLIB_H 1
  #define HAVE_STRING_H 1
  #define HAVE_MEMORY_H 1
  #define HAVE_STRINGS_H 1
  #define HAVE_INTTYPES_H 1
  #define HAVE_STDINT_H 1
  #define HAVE_UNISTD_H 1
  #define SIZEOF_VOID_P 8
  #define LG_SIZEOF_PTR 3
  #define SIZEOF_INT 4
  #define LG_SIZEOF_INT 2
  #define SIZEOF_LONG 8
  #define LG_SIZEOF_LONG 3
  #define SIZEOF_LONG_LONG 8
  #define LG_SIZEOF_LONG_LONG 3
  #define SIZEOF_INTMAX_T 8
  #define LG_SIZEOF_INTMAX_T 3

  configure: exit 1

  --- stderr
  Invalid configuration `riscv64gc-unknown-linux-musl': machine `riscv64gc-unknown' not recognized
  configure: error: /bin/sh build-aux/config.sub riscv64gc-unknown-linux-musl failed
  thread 'main' panicked at /home/ben/.cargo/registry/src/index.crates.io-6f17d22bba15001f/jemalloc-sys-0.5.4+5.3.0-patched/build.rs:351:9:
  command did not execute successfully: cd "/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out/build" && CC="cc" CFLAGS="-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -march=rv64gc -mabi=lp64d -Wall" CPPFLAGS="-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -march=rv64gc -mabi=lp64d -Wall" LDFLAGS="-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -march=rv64gc -mabi=lp64d -Wall" "sh" "/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out/build/configure" "--disable-cxx" "--enable-doc=no" "--enable-shared=no" "--with-jemalloc-prefix=_rjem_" "--with-private-namespace=_rjem_" "--host=riscv64gc-unknown-linux-musl" "--build=riscv64gc-unknown-linux-musl" "--prefix=/home/ben/projects/ripgrep/target/debug/build/jemalloc-sys-c6526b244f290581/out"
  expected success, got: exit status: 1
  note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

I'd like to be able to run ripgrep on my RISC-V board that's using musl libc

---

_Comment by @bwbuhse on 2025-02-16 19:06_

Specifically, I guess, the jemallocator crate just fixed the riscv64gc-unknown-linux-musl build in January 2024, but the current release is actually from mid-2023 :( 

---

_Comment by @BurntSushi on 2025-02-16 19:14_

The simplest thing here would be to just not use `jemallocator` on this target. It's already a target specific dependency. Patches are welcome.

(The purpose of `jemallocator` is to replace the musl allocator, which is somewhat slow. But this is in the pursuit of squeezing every ounce of perf we can and probably doesn't make a huge difference in many workloads. So you might be perfectly fine with musl's allocator.)

---

_Comment by @bwbuhse on 2025-02-17 00:26_

I just opened #2992 which disables `jemallocator` for RISC-V musl targets and builds successfully, though hopefully it can be re-enabled once `jemallocator` gets a new release and the crates.io version is able to support RISC-V + musl. 

---

_Comment by @krant on 2025-02-28 07:38_

>  jemallocator crate just fixed the riscv64gc-unknown-linux-musl build in January 2024, but the current release is actually from mid-2023

Yeah, that's why https://github.com/BurntSushi/ripgrep/pull/2889 was suggested

---

_Comment by @BurntSushi on 2025-08-17 18:28_

I'm switching to `tikv-jemallocator`, which [purports to support riscv64](https://github.com/tikv/jemallocator/blob/5c5a9f79b3be3c19e253527f16a091132a455c2e/CHANGELOG.md?plain=1#L44). So I think this will be resolved by #2889. If riscv64 still doesn't work after the next ripgrep release, please feel free to re-open this issue.

---

_Closed by @BurntSushi on 2025-08-17 18:28_

---
