```yaml
number: 3791
title: ruff can not run on aarch64 machine
type: issue
state: closed
author: v1c77
labels:
  - bug
assignees: []
created_at: 2023-03-29T06:52:41Z
updated_at: 2023-05-08T06:10:05Z
url: https://github.com/astral-sh/ruff/issues/3791
synced_at: 2026-01-12T15:54:44Z
```

# ruff can not run on aarch64 machine

---

_@v1c77_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

### reproducible steps with docker container (same as bare metal)

1. prepare an aarch64 linux env with docker installed.
```
# uname -m
aarch64
```
2. set up a python:3.10 container and install ruff.
```
# docker run -it -v `pwd`:/app -w /app  python:3.10.10 bash
root@a57c3904b899:/app# pip install ruff
Collecting ruff
  Downloading ruff-0.0.259-py3-none-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (4.4 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.4/4.4 MB 8.1 MB/s eta 0:00:00
Installing collected packages: ruff
Successfully installed ruff-0.0.259
```

3. run ruff but got  jemalloc error

```
root@a57c3904b899:/app# ruff ./
<jemalloc>: Unsupported system page size
<jemalloc>: Unsupported system page size
memory allocation of 5 bytes failed
Aborted (core dumped)
```





---

_Label `bug` added by @MichaReiser on 2023-03-29 07:42_

---

_Comment by @MichaReiser on 2023-03-29 07:45_

Thanks for reporting this bug. What's the linux distribution/ CPU that you use?

The issue seems to be that jemalloc is compiled with fixed page sizes and using a larger page size than what it is compiled for segfaults. But jemalloc supports running on systems with smaller page sizes then configured at compile time. 

Sources: [[1](https://github.com/jemalloc/jemalloc/issues/467), [2](https://gitlab.com/gitlab-org/omnibus-gitlab/-/merge_requests/4800)]

---

_Comment by @messense on 2023-03-30 05:29_

Looks like the fix is easy, just set `JEMALLOC_SYS_WITH_LG_PAGE=16` when building for aarch64.

See https://github.com/meilisearch/meilisearch/issues/1410#issuecomment-872410768

---

_Comment by @MichaReiser on 2023-03-30 06:26_

Would someone with an aarch64 architecture be willing to try this (first compile with `...LG_PAGE=1` to verify that it fails, then set the env variable to test if the fix works)?

---

_Comment by @benjaminbauer on 2023-04-03 15:22_

I ran ruff `0.0.260` installed via pip in a debian container on an M2 Mac. I cannot reproduce the described error

---

_Comment by @v1c77 on 2023-04-11 04:22_

I tried with  `JEMALLOC_SYS_WITH_LG_PAGE=`  1,4,5,8,16,32,64,128..., on a kylinv10 Linux for aarch64(a centos-based dist). Only JEMALLOC_SYS_WITH_LG_PAGE=16 is compatible. Otherwise, some compile error logs just throw out.

I wonder what is the recommended linux distribution version for aarch64. I may take some time to test this issue now.

```
RUST_BACKTRACE=full JEMALLOC_SYS_WITH_LG_PAGE=4 cargo run -p ruff_cli -- check ../app/
   Compiling tikv-jemalloc-sys v0.5.3+5.3.0-patched
error: failed to run custom build command for `tikv-jemalloc-sys v0.5.3+5.3.0-patched`

Caused by:
  process didn't exit successfully: `/root/proj/ruff/target/debug/build/tikv-jemalloc-sys-3e856d7e10f51b2d/build-script-build` (exit status: 101)
  --- stdout
  TARGET=aarch64-unknown-linux-gnu
  HOST=aarch64-unknown-linux-gnu
  NUM_JOBS=4
  OUT_DIR="/root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out"
  BUILD_DIR="/root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/build"
  SRC_DIR="/root/.cargo/registry/src/github.com-1ecc6299db9ec823/tikv-jemalloc-sys-0.5.3+5.3.0-patched"
  cargo:rustc-cfg=prefixed
  cargo:rerun-if-env-changed=JEMALLOC_OVERRIDE
  OPT_LEVEL = Some("0")
  TARGET = Some("aarch64-unknown-linux-gnu")
  HOST = Some("aarch64-unknown-linux-gnu")
  cargo:rerun-if-env-changed=CC_aarch64-unknown-linux-gnu
  CC_aarch64-unknown-linux-gnu = None
  cargo:rerun-if-env-changed=CC_aarch64_unknown_linux_gnu
  CC_aarch64_unknown_linux_gnu = None
  cargo:rerun-if-env-changed=HOST_CC
  HOST_CC = None
  cargo:rerun-if-env-changed=CC
  CC = None
  cargo:rerun-if-env-changed=CFLAGS_aarch64-unknown-linux-gnu
  CFLAGS_aarch64-unknown-linux-gnu = None
  cargo:rerun-if-env-changed=CFLAGS_aarch64_unknown_linux_gnu
  CFLAGS_aarch64_unknown_linux_gnu = None
  cargo:rerun-if-env-changed=HOST_CFLAGS
  HOST_CFLAGS = None
  cargo:rerun-if-env-changed=CFLAGS
  CFLAGS = None
  cargo:rerun-if-env-changed=CRATE_CC_NO_DEFAULTS
  CRATE_CC_NO_DEFAULTS = None
  DEBUG = Some("true")
  CARGO_CFG_TARGET_FEATURE = Some("neon")
  CC="cc"
  CFLAGS="-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall"
  JEMALLOC_REPO_DIR="jemalloc"
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_MALLOC_CONF
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_PAGE
  --with-lg-page=4
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_HUGEPAGE
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_QUANTUM
  cargo:rerun-if-env-changed=JEMALLOC_SYS_WITH_LG_VADDR
  --with-jemalloc-prefix=_rjem_
  running: "sh" "/root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/build/configure" "--disable-cxx" "--enable-doc=no" "--enable-shared=no" "--with-lg-page=4" "--with-jemalloc-prefix=_rjem_" "--with-private-namespace=_rjem_" "--host=aarch64-unknown-linux-gnu" "--build=aarch64-unknown-linux-gnu" "--prefix=/root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out"
  checking for xsltproc... /usr/bin/xsltproc
  checking for aarch64-unknown-linux-gnu-gcc... cc
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
  checking build system type... aarch64-unknown-linux-gnu
  checking host system type... aarch64-unknown-linux-gnu
  checking whether isb instruction is compilable... yes
  checking number of significant virtual address bits... 48
  checking for aarch64-unknown-linux-gnu-ar... no
  checking for ar... ar
  checking for aarch64-unknown-linux-gnu-nm... no
  checking for nm... nm
  checking for gawk... gawk
  checking malloc.h usability... yes
  checking malloc.h presence... yes
  checking for malloc.h... yes
  checking whether malloc_usable_size definition can use const argument... no
  checking for library containing log... -lm
  checking whether __attribute__ syntax is compilable... yes
  checking whether compiler supports -fvisibility=hidden... yes
  checking whether compiler supports -fvisibility=hidden... no
  checking whether compiler supports -Werror... yes
  checking whether compiler supports -herror_on_warning... yes
  checking whether tls_model attribute is compilable... yes
  checking whether compiler supports -Werror... yes
  checking whether compiler supports -herror_on_warning... yes
  checking whether alloc_size attribute is compilable... yes
  checking whether compiler supports -Werror... yes
  checking whether compiler supports -herror_on_warning... yes
  checking whether format(gnu_printf, ...) attribute is compilable... yes
  checking whether compiler supports -Werror... yes
  checking whether compiler supports -herror_on_warning... yes
  checking whether format(printf, ...) attribute is compilable... yes
  checking whether compiler supports -Werror... yes
  checking whether compiler supports -herror_on_warning... yes
  checking whether format(printf, ...) attribute is compilable... yes
  checking whether compiler supports -Wimplicit-fallthrough... yes
  checking whether fallthrough attribute is compilable... yes
  checking whether compiler supports -Wimplicit-fallthrough... yes
  checking whether compiler supports -Wimplicit-fallthrough... no
  checking whether compiler supports -Werror... yes
  checking whether compiler supports -herror_on_warning... yes
  checking whether cold attribute is compilable... yes
  checking whether vm_make_tag is compilable... no
  checking for a BSD-compatible install... /usr/bin/install -c
  checking for aarch64-unknown-linux-gnu-ranlib... no
  checking for ranlib... ranlib
  checking for ld... /usr/bin/ld
  checking for autoconf... /usr/bin/autoconf
  checking for memalign... yes
  checking for valloc... yes
  checking for malloc_size... no
  checking whether compiler supports -O3... yes
  checking whether compiler supports -O3... no
  checking whether compiler supports -funroll-loops... yes
  checking configured backtracing method... N/A
  checking for sbrk... yes
  checking whether utrace(2) is compilable... no
  checking whether utrace(2) with label is compilable... no
  checking whether a program using __builtin_unreachable is compilable... yes
  checking whether a program using __builtin_ffsl is compilable... yes
  checking whether a program using __builtin_popcountl is compilable... yes
  checking pthread.h usability... yes
  checking pthread.h presence... yes
  checking for pthread.h... yes
  checking for pthread_create in -lpthread... yes
  checking dlfcn.h usability... yes
  checking dlfcn.h presence... yes
  checking for dlfcn.h... yes
  checking for dlsym... no
  checking for dlsym in -ldl... yes
  checking whether pthread_atfork(3) is compilable... yes
  checking whether pthread_setname_np(3) is compilable... yes
  checking whether pthread_getname_np(3) is compilable... yes
  checking whether pthread_get_name_np(3) is compilable... no
  checking for library containing clock_gettime... none required
  checking whether clock_gettime(CLOCK_MONOTONIC_COARSE, ...) is compilable... yes
  checking whether clock_gettime(CLOCK_MONOTONIC, ...) is compilable... yes
  checking whether mach_absolute_time() is compilable... no
  checking whether clock_gettime(CLOCK_REALTIME, ...) is compilable... yes
  checking whether compiler supports -Werror... yes
  checking whether syscall(2) is compilable... yes
  checking for secure_getenv... yes
  checking for sched_getcpu... yes
  checking for sched_setaffinity... yes
  checking for issetugid... no
  checking for _malloc_thread_cleanup... no
  checking for _pthread_mutex_init_calloc_cb... no
  checking for memcntl... no
  checking for TLS... yes
  checking whether C11 atomics is compilable... yes
  checking whether GCC __atomic atomics is compilable... yes
  checking whether GCC 8-bit __atomic atomics is compilable... yes
  checking whether GCC __sync atomics is compilable... yes
  checking whether GCC 8-bit __sync atomics is compilable... yes
  checking whether Darwin OSAtomic*() is compilable... no
  checking whether madvise(2) is compilable... yes
  checking whether madvise(..., MADV_FREE) is compilable... yes
  checking whether madvise(..., MADV_DONTNEED) is compilable... yes
  checking whether madvise(..., MADV_DO[NT]DUMP) is compilable... yes
  checking whether madvise(..., MADV_[NO]HUGEPAGE) is compilable... yes
  checking whether madvise(..., MADV_[NO]CORE) is compilable... no
  checking whether mprotect(2) is compilable... yes
  checking for __builtin_clz... yes
  checking whether Darwin os_unfair_lock_*() is compilable... no
  checking whether glibc malloc hook is compilable... yes
  checking whether glibc memalign hook is compilable... yes
  checking whether pthreads adaptive mutexes is compilable... yes
  checking whether compiler supports -D_GNU_SOURCE... yes
  checking whether compiler supports -Werror... yes
  checking whether compiler supports -herror_on_warning... yes
  checking whether strerror_r returns char with gnu source is compilable... yes
  checking for stdbool.h that conforms to C99... yes
  checking for _Bool... yes
  configure: creating ./config.status
  config.status: creating Makefile
  config.status: creating jemalloc.pc
  config.status: creating doc/html.xsl
  config.status: creating doc/manpages.xsl
  config.status: creating doc/jemalloc.xml
  config.status: creating include/jemalloc/jemalloc_macros.h
  config.status: creating include/jemalloc/jemalloc_protos.h
  config.status: creating include/jemalloc/jemalloc_typedefs.h
  config.status: creating include/jemalloc/internal/jemalloc_preamble.h
  config.status: creating test/test.sh
  config.status: creating test/include/test/jemalloc_test.h
  config.status: creating config.stamp
  config.status: creating bin/jemalloc-config
  config.status: creating bin/jemalloc.sh
  config.status: creating bin/jeprof
  config.status: creating include/jemalloc/jemalloc_defs.h
  config.status: creating include/jemalloc/internal/jemalloc_internal_defs.h
  config.status: creating test/include/test/jemalloc_test_defs.h
  config.status: executing include/jemalloc/internal/public_symbols.txt commands
  config.status: executing include/jemalloc/internal/private_symbols.awk commands
  config.status: executing include/jemalloc/internal/private_symbols_jet.awk commands
  config.status: executing include/jemalloc/internal/public_namespace.h commands
  config.status: executing include/jemalloc/internal/public_unnamespace.h commands
  config.status: executing include/jemalloc/jemalloc_protos_jet.h commands
  config.status: executing include/jemalloc/jemalloc_rename.h commands
  config.status: executing include/jemalloc/jemalloc_mangle.h commands
  config.status: executing include/jemalloc/jemalloc_mangle_jet.h commands
  config.status: executing include/jemalloc/jemalloc.h commands
  ===============================================================================
  jemalloc version   : 5.3.0-0-g54eaed1d8b56b1aa528be3bdd1877e59c56fa90c
  library revision   : 2

  CONFIG             : --disable-cxx --enable-doc=no --enable-shared=no --with-lg-page=4 --with-jemalloc-prefix=_rjem_ --with-private-namespace=_rjem_ --host=aarch64-unknown-linux-gnu --build=aarch64-unknown-linux-gnu --prefix=/root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out build_alias=aarch64-unknown-linux-gnu host_alias=aarch64-unknown-linux-gnu CC=cc 'CFLAGS=-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall' 'LDFLAGS=-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall' 'CPPFLAGS=-O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall'
  CC                 : cc
  CONFIGURE_CFLAGS   : -std=gnu11 -Wall -Wextra -Wsign-compare -Wundef -Wno-format-zero-length -Wpointer-arith -Wno-missing-braces -Wno-missing-field-initializers -Wno-missing-attributes -pipe -g3 -fvisibility=hidden -Wimplicit-fallthrough -O3 -funroll-loops
  SPECIFIED_CFLAGS   : -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall
  EXTRA_CFLAGS       :
  CPPFLAGS           : -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -D_GNU_SOURCE -D_REENTRANT
  CXX                :
  CONFIGURE_CXXFLAGS :
  SPECIFIED_CXXFLAGS :
  EXTRA_CXXFLAGS     :
  LDFLAGS            : -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall
  EXTRA_LDFLAGS      :
  DSO_LDFLAGS        : -shared -Wl,-soname,$(@F)
  LIBS               : -lm  -pthread -ldl
  RPATH_EXTRA        :

  XSLTPROC           : /usr/bin/xsltproc
  XSLROOT            : /usr/share/sgml/docbook/xsl-stylesheets

  PREFIX             : /root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out
  BINDIR             : /root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/bin
  DATADIR            : /root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/share
  INCLUDEDIR         : /root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/include
  LIBDIR             : /root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/lib
  MANDIR             : /root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/share/man

  srcroot            :
  abs_srcroot        : /root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/build/
  objroot            :
  abs_objroot        : /root/proj/ruff/target/debug/build/tikv-jemalloc-sys-d5c93359b7f0c180/out/build/

  JEMALLOC_PREFIX    : _rjem_
  JEMALLOC_PRIVATE_NAMESPACE
                     : _rjem_je_
  install_suffix     :
  malloc_conf        :
  documentation      : 0
  shared libs        : 0
  static libs        : 1
  autogen            : 0
  debug              : 0
  stats              : 1
  experimental_smallocx : 0
  prof               : 0
  prof-libunwind     : 0
  prof-libgcc        : 0
  prof-gcc           : 0
  fill               : 1
  utrace             : 0
  xmalloc            : 0
  log                : 0
  lazy_lock          : 0
  cache-oblivious    : 1
  cxx                : 0
  ===============================================================================
  running: "make" "-j" "4"
  cc -std=gnu11 -Wall -Wextra -Wsign-compare -Wundef -Wno-format-zero-length -Wpointer-arith -Wno-missing-braces -Wno-missing-field-initializers -Wno-missing-attributes -pipe -g3 -fvisibility=hidden -Wimplicit-fallthrough -O3 -funroll-loops -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -c -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -DJEMALLOC_NO_PRIVATE_NAMESPACE -o src/jemalloc.sym.o src/jemalloc.c
  cc -std=gnu11 -Wall -Wextra -Wsign-compare -Wundef -Wno-format-zero-length -Wpointer-arith -Wno-missing-braces -Wno-missing-field-initializers -Wno-missing-attributes -pipe -g3 -fvisibility=hidden -Wimplicit-fallthrough -O3 -funroll-loops -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -c -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -DJEMALLOC_NO_PRIVATE_NAMESPACE -o src/arena.sym.o src/arena.c
  cc -std=gnu11 -Wall -Wextra -Wsign-compare -Wundef -Wno-format-zero-length -Wpointer-arith -Wno-missing-braces -Wno-missing-field-initializers -Wno-missing-attributes -pipe -g3 -fvisibility=hidden -Wimplicit-fallthrough -O3 -funroll-loops -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -c -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -DJEMALLOC_NO_PRIVATE_NAMESPACE -o src/background_thread.sym.o src/background_thread.c
  cc -std=gnu11 -Wall -Wextra -Wsign-compare -Wundef -Wno-format-zero-length -Wpointer-arith -Wno-missing-braces -Wno-missing-field-initializers -Wno-missing-attributes -pipe -g3 -fvisibility=hidden -Wimplicit-fallthrough -O3 -funroll-loops -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -c -O0 -ffunction-sections -fdata-sections -fPIC -gdwarf-4 -fno-omit-frame-pointer -Wall -D_GNU_SOURCE -D_REENTRANT -Iinclude -Iinclude -DJEMALLOC_NO_PRIVATE_NAMESPACE -o src/base.sym.o src/base.c

  --- stderr
  In file included from include/jemalloc/internal/arena_types.h:4:0,
                   from include/jemalloc/internal/jemalloc_internal_includes.h:43,
                   from src/jemalloc.c:3:
  include/jemalloc/internal/sc.h:263:4: error: #error "Lookup table sizes must be small"
   #  error "Lookup table sizes must be small"
      ^~~~~
  In file included from include/jemalloc/internal/arena_types.h:4:0,
                   from include/jemalloc/internal/jemalloc_internal_includes.h:43,
                   from src/background_thread.c:2:
  include/jemalloc/internal/sc.h:263:4: error: #error "Lookup table sizes must be small"
   #  error "Lookup table sizes must be small"
      ^~~~~
  In file included from include/jemalloc/internal/arena_types.h:4:0,
                   from include/jemalloc/internal/jemalloc_internal_includes.h:43,
                   from src/arena.c:2:
  include/jemalloc/internal/sc.h:263:4: error: #error "Lookup table sizes must be small"
   #  error "Lookup table sizes must be small"
      ^~~~~
  In file included from include/jemalloc/internal/arena_types.h:4:0,
                   from include/jemalloc/internal/jemalloc_internal_includes.h:43,
                   from src/base.c:2:
  include/jemalloc/internal/sc.h:263:4: error: #error "Lookup table sizes must be small"
   #  error "Lookup table sizes must be small"
      ^~~~~
  cc1: warning: unrecognized command line option ‘-Wno-missing-attributes’
  make: *** [Makefile:479: src/jemalloc.sym.o] Error 1
  make: *** Waiting for unfinished jobs....
  cc1: warning: unrecognized command line option ‘-Wno-missing-attributes’
  make: *** [Makefile:479: src/background_thread.sym.o] Error 1
  cc1: warning: unrecognized command line option ‘-Wno-missing-attributes’
  make: *** [Makefile:479: src/arena.sym.o] Error 1
  cc1: warning: unrecognized command line option ‘-Wno-missing-attributes’
  make: *** [Makefile:479: src/base.sym.o] Error 1
  thread 'main' panicked at 'command did not execute successfully: "make" "-j" "4"
  expected success, got: exit status: 2', /root/.cargo/registry/src/github.com-1ecc6299db9ec823/tikv-jemalloc-sys-0.5.3+5.3.0-patched/build.rs:346:9
  stack backtrace:
     0:     0xaaadecdc6568 - std::backtrace_rs::backtrace::libunwind::trace::h4094424764928517
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/../../backtrace/src/backtrace/libunwind.rs:93:5
     1:     0xaaadecdc6568 - std::backtrace_rs::backtrace::trace_unsynchronized::h66a4d558a047d429
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/../../backtrace/src/backtrace/mod.rs:66:5
     2:     0xaaadecdc6568 - std::sys_common::backtrace::_print_fmt::he501a9dfcbc65602
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:65:5
     3:     0xaaadecdc6568 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h500c865c023970a8
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:44:22
     4:     0xaaadecde2af0 - core::fmt::write::h746b6837467f52c4
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/fmt/mod.rs:1208:17
     5:     0xaaadecdc3380 - std::io::Write::write_fmt::h9c3773c135795f84
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/io/mod.rs:1682:15
     6:     0xaaadecdc6374 - std::sys_common::backtrace::_print::haf80eda742d20a82
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:47:5
     7:     0xaaadecdc6374 - std::sys_common::backtrace::print::h1af712ffba8f7735
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:34:9
     8:     0xaaadecdc7bfc - std::panicking::default_hook::{{closure}}::hd82c56e3724f8bd8
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:267:22
     9:     0xaaadecdc7940 - std::panicking::default_hook::h1f2a94f3a1c1c1aa
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:286:9
    10:     0xaaadecdc827c - std::panicking::rust_panic_with_hook::h01eb01a06bf17753
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:688:13
    11:     0xaaadecdc805c - std::panicking::begin_panic_handler::{{closure}}::h657c9d34bb6d0855
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:579:13
    12:     0xaaadecdc69f8 - std::sys_common::backtrace::__rust_end_short_backtrace::h0b2d3ba55b6e5445
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:137:18
    13:     0xaaadecdc7da8 - rust_begin_unwind
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:575:5
    14:     0xaaadecd5bc10 - core::panicking::panic_fmt::h29641d0c529764f0
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/panicking.rs:64:14
    15:     0xaaadecd614d8 - build_script_build::execute::h663fa93d1a83fe8e
                                 at /root/.cargo/registry/src/github.com-1ecc6299db9ec823/tikv-jemalloc-sys-0.5.3+5.3.0-patched/build.rs:346:9
    16:     0xaaadecd612c0 - build_script_build::run::hb342b14d880c1b34
                                 at /root/.cargo/registry/src/github.com-1ecc6299db9ec823/tikv-jemalloc-sys-0.5.3+5.3.0-patched/build.rs:335:5
    17:     0xaaadecd60438 - build_script_build::main::hb03168a57fdfbe1a
                                 at /root/.cargo/registry/src/github.com-1ecc6299db9ec823/tikv-jemalloc-sys-0.5.3+5.3.0-patched/build.rs:273:5
    18:     0xaaadecd65920 - core::ops::function::FnOnce::call_once::ha895ca2c277b63b6
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/ops/function.rs:507:5
    19:     0xaaadecd632c4 - std::sys_common::backtrace::__rust_begin_short_backtrace::hda7f1aa9a8320c28
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/sys_common/backtrace.rs:121:18
    20:     0xaaadecd669cc - std::rt::lang_start::{{closure}}::h13199b19da459dfb
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/rt.rs:166:18
    21:     0xaaadecdbf9a0 - core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once::h529a25abf285dbab
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/core/src/ops/function.rs:606:13
    22:     0xaaadecdbf9a0 - std::panicking::try::do_call::h660335bd09680bb9
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:483:40
    23:     0xaaadecdbf9a0 - std::panicking::try::h94641ffd91114cda
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:447:19
    24:     0xaaadecdbf9a0 - std::panic::catch_unwind::hb02f598a6506eb1a
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panic.rs:137:14
    25:     0xaaadecdbf9a0 - std::rt::lang_start_internal::{{closure}}::h2bafc04d50d16864
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/rt.rs:148:48
    26:     0xaaadecdbf9a0 - std::panicking::try::do_call::he27a0671832dd909
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:483:40
    27:     0xaaadecdbf9a0 - std::panicking::try::h517b21015b9f6abd
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panicking.rs:447:19
    28:     0xaaadecdbf9a0 - std::panic::catch_unwind::hc734f97b8d8dbcc6
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/panic.rs:137:14
    29:     0xaaadecdbf9a0 - std::rt::lang_start_internal::h3fd169391cc4081c
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/rt.rs:148:20
    30:     0xaaadecd6699c - std::rt::lang_start::h00dd1d9d587cbb7b
                                 at /rustc/d5a82bbd26e1ad8b7401f6a718a9c57c96905483/library/std/src/rt.rs:165:17
    31:     0xaaadecd61c2c - main
    32:     0xfffbdcf33fe0 - __libc_start_main
    33:     0xaaadecd5bfec - <unknown>
```




---

_Comment by @charliermarsh on 2023-04-11 04:24_

(Thank you for doing that!)

---

_Comment by @MichaReiser on 2023-04-11 07:55_

@konstin would you recommend setting the environment variable using wheels or in the GitHub action that performs the cross-compilation? 

https://github.com/charliermarsh/ruff/blob/5e03d442e6033d99877b720f402d8c0648166f2a/.github/workflows/ruff.yaml#L183-L210

---

_Comment by @konstin on 2023-04-11 12:25_

I'd set it in the github action. The only case that then still fails is if someone cross-compiles for aarch64 themselves.

I've asked in https://github.com/gnzlbg/jemallocator/issues/170#issuecomment-1503228963 if that could be upstreamed, but it seems that repository is abandoned and only https://github.com/tikv/jemallocator is still somewhat active, so it might make sense to upstream it to the latter.

---

_Closed by @MichaReiser on 2023-05-08 06:10_

---
