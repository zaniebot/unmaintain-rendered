---
number: 8429
title: "Improve `sysconfig` metadata when installing managed Python distributions"
type: issue
state: closed
author: zanieb
labels:
  - tracking
assignees: []
created_at: 2024-10-21T22:11:31Z
updated_at: 2025-04-30T15:27:33Z
url: https://github.com/astral-sh/uv/issues/8429
synced_at: 2026-01-07T13:12:17-06:00
---

# Improve `sysconfig` metadata when installing managed Python distributions

---

_Issue opened by @zanieb on 2024-10-21 22:11_

This is a tracking issue for various problems that come from some of the `sysconfig` variables in the `python-build-standalone` distribution being incorrect.

As described in the [upstream documentation](https://github.com/indygreg/python-build-standalone/blob/main/docs/quirks.rst#references-to-build-time-paths) 

Some examples of where this goes wrong:

- #7525
- https://github.com/indygreg/python-build-standalone/issues/374
- #7369
- #8036
- #6488
- https://github.com/astral-sh/uv/issues/9347

---

_Label `tracking` added by @zanieb on 2024-10-21 22:11_

---

_Referenced in [astral-sh/uv#7525](../../astral-sh/uv/issues/7525.md) on 2024-10-21 22:11_

---

_Referenced in [astral-sh/python-build-standalone#374](../../astral-sh/python-build-standalone/issues/374.md) on 2024-10-21 22:16_

---

_Referenced in [astral-sh/uv#7369](../../astral-sh/uv/issues/7369.md) on 2024-10-21 22:16_

---

_Referenced in [astral-sh/uv#8036](../../astral-sh/uv/issues/8036.md) on 2024-10-21 22:16_

---

_Comment by @ringsaturn on 2024-10-22 00:39_

Should #6488 be included?

---

_Referenced in [astral-sh/uv#6488](../../astral-sh/uv/issues/6488.md) on 2024-10-22 01:34_

---

_Comment by @zanieb on 2024-10-22 01:35_

Yes thanks!

---

_Referenced in [astral-sh/uv#8459](../../astral-sh/uv/issues/8459.md) on 2024-10-22 16:58_

---

_Referenced in [astral-sh/python-build-standalone#380](../../astral-sh/python-build-standalone/issues/380.md) on 2024-10-23 13:20_

---

_Referenced in [astral-sh/uv#8481](../../astral-sh/uv/pulls/8481.md) on 2024-10-24 13:46_

---

_Referenced in [astral-sh/uv#8644](../../astral-sh/uv/issues/8644.md) on 2024-10-28 19:33_

---

_Referenced in [astral-sh/uv#8879](../../astral-sh/uv/issues/8879.md) on 2024-11-07 13:51_

---

_Referenced in [astral-sh/uv#8821](../../astral-sh/uv/issues/8821.md) on 2024-11-07 13:53_

---

_Referenced in [astral-sh/python-build-standalone#307](../../astral-sh/python-build-standalone/issues/307.md) on 2024-11-26 14:33_

---

_Referenced in [Jij-Inc/ommx#165](../../Jij-Inc/ommx/issues/165.md) on 2024-11-27 05:39_

---

_Referenced in [PyPSA/pypsa-usa#483](../../PyPSA/pypsa-usa/issues/483.md) on 2024-11-27 18:09_

---

_Referenced in [astral-sh/uv#9501](../../astral-sh/uv/issues/9501.md) on 2024-11-29 17:25_

---

_Referenced in [astral-sh/uv#9347](../../astral-sh/uv/issues/9347.md) on 2024-12-11 19:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-17 13:37_

---

_Closed by @charliermarsh on 2024-12-17 20:09_

---

_Comment by @davidszotten on 2024-12-18 21:42_

should this being closed mean that e.g. [#6488](https://github.com/astral-sh/uv/issues/6488) should now be fixed?

```
$ uv --version
uv 0.5.10 (37b11ddb2 2024-12-17)

$ uv python install 3.13 --reinstall
Installed Python 3.13.1 in 2.57s
 ~ cpython-3.13.1-macos-aarch64-none

$ uvx uwsgi
dyld[94596]: Library not loaded: /install/lib/libpython3.13.dylib
  Referenced from: <9E38007F-64A0-3C50-B7CA-7AA5091E8932> /Users/david/Library/Caches/uv/archive-v0/Hptz7gkIv7_5PLk5p4xJ8/bin/uwsgi
  Reason: tried: '/install/lib/libpython3.13.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/install/lib/libpython3.13.dylib' (no such file), '/install/lib/libpython3.13.dylib' (no such file)
```



---

_Comment by @zanieb on 2024-12-18 21:56_

I can reproduce that failure

```
❯ RUST_LOG=uv=trace uv python install 3.13 --reinstall -v
DEBUG uv 0.5.10 (37b11ddb2 2024-12-17)
TRACE Checking lock for `/Users/zb/.local/share/uv/python` at `/Users/zb/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/Users/zb/.local/share/uv/python`
TRACE Found existing installation cpython-3.13.1-macos-aarch64-none
TRACE Found existing installation cpython-3.13.0-macos-aarch64-none
TRACE Found existing installation cpython-3.12.6-macos-aarch64-none
TRACE Found existing installation cpython-3.11.10-macos-aarch64-none
TRACE Found existing installation cpython-3.10.15-macos-aarch64-none
TRACE Found existing installation cpython-3.9.20-macos-aarch64-none
TRACE Found existing installation cpython-3.8.18-macos-aarch64-none
TRACE Found existing installation cpython-3.8.12-macos-aarch64-none
DEBUG Ignoring match `cpython-3.13.1-macos-aarch64-none` for request `Python 3.13` due to `--reinstall` flag
DEBUG Found download `cpython-3.13.1-macos-aarch64-none` for request `Python 3.13`
DEBUG Using request timeout of 30s
TRACE Handling request for https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.13.1%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz
TRACE Request for https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.13.1%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.13.1%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.13.1%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20241206/cpython-3.13.1%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz to temporary location: /Users/zb/.local/share/uv/python/.temp/.tmpeZkDfU
DEBUG Extracting cpython-3.13.1%2B20241206-aarch64-apple-darwin-install_only_stripped.tar.gz
DEBUG Removing existing directory: /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none
DEBUG Moving /Users/zb/.local/share/uv/python/.temp/.tmpeZkDfU/python to /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none
TRACE Discovered `sysconfig` data at: /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13/_sysconfigdata__darwin_darwin.py
TRACE Updated `AR` from `/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/tools/llvm/bin/llvm-ar` to `ar`
TRACE Updated `BINDIR` from `/install/bin` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin`
TRACE Updated `BINLIBDEST` from `/install/lib/python3.13` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13`
TRACE Updated `BLDSHARED` from `clang -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl` to `cc -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl`
TRACE Updated `CC` from `clang` to `cc`
TRACE Updated `CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `CONFIGURE_CFLAGS` from `-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `CONFIGURE_CPPFLAGS` from `-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `CONFIGURE_LDFLAGS` from `-arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl` to `-arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl`
TRACE Updated `CONFIG_ARGS` from `'--build=aarch64-apple-darwin' '--host=aarch64-apple-darwin' '--prefix=/install' '--with-openssl=/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/tools/deps' '--with-system-expat' '--with-system-libmpdec' '--without-ensurepip' '--enable-shared' '--enable-optimizations' '--with-lto' '--with-build-python=/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/tools/host/bin/python3.13' 'ac_cv_lib_intl_textdomain=no' 'ac_cv_func_ptsname_r=no' '--with-dbmliborder=ndbm' 'build_alias=aarch64-apple-darwin' 'host_alias=aarch64-apple-darwin' 'CC=clang' 'CFLAGS=-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new' 'LDFLAGS=-arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl' 'CPPFLAGS=-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new'` to `'--build=aarch64-apple-darwin' '--host=aarch64-apple-darwin' '--prefix=/install' '--with-openssl=/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/tools/deps' '--with-system-expat' '--with-system-libmpdec' '--without-ensurepip' '--enable-shared' '--enable-optimizations' '--with-lto' '--with-build-python=/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/tools/host/bin/python3.13' 'ac_cv_lib_intl_textdomain=no' 'ac_cv_func_ptsname_r=no' '--with-dbmliborder=ndbm' 'build_alias=aarch64-apple-darwin' 'host_alias=aarch64-apple-darwin' 'CC=clang' 'CFLAGS=-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new' 'LDFLAGS=-arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl' 'CPPFLAGS=-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new'`
TRACE Updated `CONFINCLUDEDIR` from `/install/include` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include`
TRACE Updated `CONFINCLUDEPY` from `/install/include/python3.13` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13`
TRACE Updated `CPPFLAGS` from `-I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `CXX` from `clang++` to `c++`
TRACE Updated `DESTDIRS` from `/install /install/lib /install/lib/python3.13 /install/lib/python3.13/lib-dynload` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13 /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13/lib-dynload`
TRACE Updated `DESTLIB` from `/install/lib/python3.13` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13`
TRACE Updated `DESTSHARED` from `/install/lib/python3.13/lib-dynload` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13/lib-dynload`
TRACE Updated `EXENAME` from `/install/bin/python3.13` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13`
TRACE Updated `INCLDIRSTOMAKE` from `/install/include /install/include /install/include/python3.13 /install/include/python3.13` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13`
TRACE Updated `INCLUDEDIR` from `/install/include` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include`
TRACE Updated `INCLUDEPY` from `/install/include/python3.13` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13`
TRACE Updated `LDCXXSHARED` from `clang++ -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl` to `c++ -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl`
TRACE Updated `LDFLAGS` from `-arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl` to `-arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl`
TRACE Updated `LDSHARED` from `clang -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl` to `cc -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl`
TRACE Updated `LIBDEST` from `/install/lib/python3.13` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13`
TRACE Updated `LIBDIR` from `/install/lib` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib`
TRACE Updated `LIBEXPAT_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `LIBHACL_CFLAGS` from `-I./Modules/_hacl/include -D_BSD_SOURCE -D_DEFAULT_SOURCE -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-I./Modules/_hacl/include -D_BSD_SOURCE -D_DEFAULT_SOURCE -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `LIBMPDEC_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `LIBPC` from `/install/lib/pkgconfig` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/pkgconfig`
TRACE Updated `LIBPL` from `/install/lib/python3.13/config-3.13-darwin` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13/config-3.13-darwin`
TRACE Updated `LIBS` from `-ldl  -framework CoreFoundation` to `-ldl -framework CoreFoundation`
TRACE Updated `LINKCC` from `clang` to `cc`
TRACE Updated `LINKFORSHARED` from `-Wl,-stack_size,1000000  -framework CoreFoundation` to `-Wl,-stack_size,1000000 -framework CoreFoundation`
TRACE Updated `LOCALMODLIBS` from `-Xlinker -hidden-lbz2          -Xlinker -hidden-lffi -Xlinker -hidden-ldl  -Xlinker -hidden-lm  -Xlinker -hidden-lncurses  -Xlinker -hidden-lpanel -Xlinker -hidden-lncurses    -Xlinker -hidden-lmpdec  -Xlinker -hidden-lexpat  -Xlinker -hidden-lcrypto        -Xlinker -hidden-llzma           -framework CoreFoundation -framework SystemConfiguration      -Xlinker -hidden-lsqlite3  -Xlinker -hidden-lssl -Xlinker -hidden-lcrypto            -Xlinker -hidden-ltcl8.6 -Xlinker -hidden-ltk8.6 -framework AppKit -framework ApplicationServices -framework Carbon -framework Cocoa -framework CoreFoundation -framework CoreServices -framework CoreGraphics -framework IOKit -framework QuartzCore -Xlinker -ObjC   -Xlinker -hidden-luuid      -Xlinker -hidden-lm    -Xlinker -hidden-lm   -Xlinker -hidden-lexpat  -Xlinker -hidden-ledit -Xlinker -hidden-lncurses        -Xlinker -hidden-lz` to `-Xlinker -hidden-lbz2 -Xlinker -hidden-lffi -Xlinker -hidden-ldl -Xlinker -hidden-lm -Xlinker -hidden-lncurses -Xlinker -hidden-lpanel -Xlinker -hidden-lncurses -Xlinker -hidden-lmpdec -Xlinker -hidden-lexpat -Xlinker -hidden-lcrypto -Xlinker -hidden-llzma -framework CoreFoundation -framework SystemConfiguration -Xlinker -hidden-lsqlite3 -Xlinker -hidden-lssl -Xlinker -hidden-lcrypto -Xlinker -hidden-ltcl8.6 -Xlinker -hidden-ltk8.6 -framework AppKit -framework ApplicationServices -framework Carbon -framework Cocoa -framework CoreFoundation -framework CoreServices -framework CoreGraphics -framework IOKit -framework QuartzCore -Xlinker -ObjC -Xlinker -hidden-luuid -Xlinker -hidden-lm -Xlinker -hidden-lm -Xlinker -hidden-lexpat -Xlinker -hidden-ledit -Xlinker -hidden-lncurses -Xlinker -hidden-lz`
TRACE Updated `MACHDESTLIB` from `/install/lib/python3.13` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib/python3.13`
TRACE Updated `MANDIR` from `/install/share/man` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/share/man`
TRACE Updated `MODBUILT_NAMES` from `_asyncio  _bisect  _blake2  _bz2  _codecs_cn  _codecs_hk  _codecs_iso2022  _codecs_jp  _codecs_kr  _codecs_tw  _contextvars  _csv  _ctypes  _ctypes_test  _curses  _curses_panel  _datetime  _dbm  _decimal  _elementtree  _hashlib  _heapq  _interpchannels  _interpqueues  _interpreters  _json  _lsprof  _lzma  _md5  _multibytecodec  _multiprocessing  _opcode  _pickle  _posixshmem  _posixsubprocess  _queue  _random  _scproxy  _sha1  _sha2  _sha3  _socket  _sqlite3  _ssl  _statistics  _struct  _suggestions  _sysconfig  _testbuffer  _testexternalinspection  _testimportmultiple  _testinternalcapi  _testmultiphase  _testsinglephase  _tkinter  _typing  _uuid  _xxtestfuzz  _zoneinfo  array  binascii  cmath  fcntl  grp  math  mmap  pyexpat  readline  resource  select  syslog  termios  unicodedata  xxsubtype  zlib  atexit  faulthandler  posix  _signal  _tracemalloc  _codecs  _collections  errno  _io  itertools  _sre  _thread  time  _weakref  _abc  _functools  _locale  _operator  _stat  _symtable  pwd` to `_asyncio _bisect _blake2 _bz2 _codecs_cn _codecs_hk _codecs_iso2022 _codecs_jp _codecs_kr _codecs_tw _contextvars _csv _ctypes _ctypes_test _curses _curses_panel _datetime _dbm _decimal _elementtree _hashlib _heapq _interpchannels _interpqueues _interpreters _json _lsprof _lzma _md5 _multibytecodec _multiprocessing _opcode _pickle _posixshmem _posixsubprocess _queue _random _scproxy _sha1 _sha2 _sha3 _socket _sqlite3 _ssl _statistics _struct _suggestions _sysconfig _testbuffer _testexternalinspection _testimportmultiple _testinternalcapi _testmultiphase _testsinglephase _tkinter _typing _uuid _xxtestfuzz _zoneinfo array binascii cmath fcntl grp math mmap pyexpat readline resource select syslog termios unicodedata xxsubtype zlib atexit faulthandler posix _signal _tracemalloc _codecs _collections errno _io itertools _sre _thread time _weakref _abc _functools _locale _operator _stat _symtable pwd`
TRACE Updated `MODDISABLED_NAMES` from `_gdbm  _testcapi  xx  xxlimited  xxlimited_35` to `_gdbm _testcapi xx xxlimited xxlimited_35`
TRACE Updated `MODLIBS` from `-Xlinker -hidden-lbz2          -Xlinker -hidden-lffi -Xlinker -hidden-ldl  -Xlinker -hidden-lm  -Xlinker -hidden-lncurses  -Xlinker -hidden-lpanel -Xlinker -hidden-lncurses    -Xlinker -hidden-lmpdec  -Xlinker -hidden-lexpat  -Xlinker -hidden-lcrypto        -Xlinker -hidden-llzma           -framework CoreFoundation -framework SystemConfiguration      -Xlinker -hidden-lsqlite3  -Xlinker -hidden-lssl -Xlinker -hidden-lcrypto            -Xlinker -hidden-ltcl8.6 -Xlinker -hidden-ltk8.6 -framework AppKit -framework ApplicationServices -framework Carbon -framework Cocoa -framework CoreFoundation -framework CoreServices -framework CoreGraphics -framework IOKit -framework QuartzCore -Xlinker -ObjC   -Xlinker -hidden-luuid      -Xlinker -hidden-lm    -Xlinker -hidden-lm   -Xlinker -hidden-lexpat  -Xlinker -hidden-ledit -Xlinker -hidden-lncurses        -Xlinker -hidden-lz` to `-Xlinker -hidden-lbz2 -Xlinker -hidden-lffi -Xlinker -hidden-ldl -Xlinker -hidden-lm -Xlinker -hidden-lncurses -Xlinker -hidden-lpanel -Xlinker -hidden-lncurses -Xlinker -hidden-lmpdec -Xlinker -hidden-lexpat -Xlinker -hidden-lcrypto -Xlinker -hidden-llzma -framework CoreFoundation -framework SystemConfiguration -Xlinker -hidden-lsqlite3 -Xlinker -hidden-lssl -Xlinker -hidden-lcrypto -Xlinker -hidden-ltcl8.6 -Xlinker -hidden-ltk8.6 -framework AppKit -framework ApplicationServices -framework Carbon -framework Cocoa -framework CoreFoundation -framework CoreServices -framework CoreGraphics -framework IOKit -framework QuartzCore -Xlinker -ObjC -Xlinker -hidden-luuid -Xlinker -hidden-lm -Xlinker -hidden-lm -Xlinker -hidden-lexpat -Xlinker -hidden-ledit -Xlinker -hidden-lncurses -Xlinker -hidden-lz`
TRACE Updated `MODOBJS` from `Modules/_asynciomodule.o  Modules/_bisectmodule.o  Modules/_blake2/blake2module.o Modules/_blake2/blake2b_impl.o Modules/_blake2/blake2s_impl.o  Modules/_bz2module.o  Modules/cjkcodecs/_codecs_cn.o  Modules/cjkcodecs/_codecs_hk.o  Modules/cjkcodecs/_codecs_iso2022.o  Modules/cjkcodecs/_codecs_jp.o  Modules/cjkcodecs/_codecs_kr.o  Modules/cjkcodecs/_codecs_tw.o  Modules/_contextvarsmodule.o  Modules/_csv.o  Modules/_ctypes/_ctypes.o Modules/_ctypes/callbacks.o Modules/_ctypes/callproc.o Modules/_ctypes/stgdict.o Modules/_ctypes/cfield.o Modules/_ctypes/malloc_closure.o  Modules/_ctypes/_ctypes_test.o  Modules/_cursesmodule.o  Modules/_curses_panel.o  Modules/_datetimemodule.o  Modules/_dbmmodule.o  Modules/_decimal/_decimal.o  Modules/_elementtree.o  Modules/_hashopenssl.o  Modules/_heapqmodule.o  Modules/_interpchannelsmodule.o  Modules/_interpqueuesmodule.o  Modules/_interpretersmodule.o  Modules/_json.o  Modules/_lsprof.o Modules/rotatingtree.o  Modules/_lzmamodule.o  Modules/md5module.o Modules/_hacl/Hacl_Hash_MD5.o  Modules/cjkcodecs/multibytecodec.o  Modules/_multiprocessing/multiprocessing.o Modules/_multiprocessing/semaphore.o  Modules/_opcode.o  Modules/_pickle.o  Modules/_multiprocessing/posixshmem.o  Modules/_posixsubprocess.o  Modules/_queuemodule.o  Modules/_randommodule.o  Modules/_scproxy.o  Modules/sha1module.o Modules/_hacl/Hacl_Hash_SHA1.o  Modules/sha2module.o Modules/_hacl/Hacl_Hash_SHA2.o  Modules/sha3module.o Modules/_hacl/Hacl_Hash_SHA3.o  Modules/socketmodule.o  Modules/_sqlite/connection.o Modules/_sqlite/cursor.o Modules/_sqlite/microprotocols.o Modules/_sqlite/module.o Modules/_sqlite/prepare_protocol.o Modules/_sqlite/row.o Modules/_sqlite/statement.o Modules/_sqlite/util.o Modules/_sqlite/blob.o  Modules/_ssl.o  Modules/_statisticsmodule.o  Modules/_struct.o  Modules/_suggestions.o  Modules/_sysconfig.o  Modules/_testbuffer.o  Modules/_testexternalinspection.o  Modules/_testimportmultiple.o  Modules/_testinternalcapi.o Modules/_testinternalcapi/pytime.o Modules/_testinternalcapi/set.o Modules/_testinternalcapi/test_critical_sections.o Modules/_testinternalcapi/test_lock.o  Modules/_testmultiphase.o  Modules/_testsinglephase.o  Modules/_tkinter.o Modules/tkappinit.o  Modules/_typingmodule.o  Modules/_uuidmodule.o  Modules/_xxtestfuzz/_xxtestfuzz.o Modules/_xxtestfuzz/fuzzer.o  Modules/_zoneinfo.o  Modules/arraymodule.o  Modules/binascii.o  Modules/cmathmodule.o  Modules/fcntlmodule.o  Modules/grpmodule.o  Modules/mathmodule.o  Modules/mmapmodule.o  Modules/pyexpat.o  Modules/readline.o  Modules/resource.o  Modules/selectmodule.o  Modules/syslogmodule.o  Modules/termios.o  Modules/unicodedata.o  Modules/xxsubtype.o  Modules/zlibmodule.o  Modules/atexitmodule.o  Modules/faulthandler.o  Modules/posixmodule.o  Modules/signalmodule.o  Modules/_tracemalloc.o  Modules/_codecsmodule.o  Modules/_collectionsmodule.o  Modules/errnomodule.o  Modules/_io/_iomodule.o Modules/_io/iobase.o Modules/_io/fileio.o Modules/_io/bytesio.o Modules/_io/bufferedio.o Modules/_io/textio.o Modules/_io/stringio.o  Modules/itertoolsmodule.o  Modules/_sre/sre.o  Modules/_threadmodule.o  Modules/timemodule.o  Modules/_weakref.o  Modules/_abc.o  Modules/_functoolsmodule.o  Modules/_localemodule.o  Modules/_operator.o  Modules/_stat.o  Modules/symtablemodule.o  Modules/pwdmodule.o` to `Modules/_asynciomodule.o Modules/_bisectmodule.o Modules/_blake2/blake2module.o Modules/_blake2/blake2b_impl.o Modules/_blake2/blake2s_impl.o Modules/_bz2module.o Modules/cjkcodecs/_codecs_cn.o Modules/cjkcodecs/_codecs_hk.o Modules/cjkcodecs/_codecs_iso2022.o Modules/cjkcodecs/_codecs_jp.o Modules/cjkcodecs/_codecs_kr.o Modules/cjkcodecs/_codecs_tw.o Modules/_contextvarsmodule.o Modules/_csv.o Modules/_ctypes/_ctypes.o Modules/_ctypes/callbacks.o Modules/_ctypes/callproc.o Modules/_ctypes/stgdict.o Modules/_ctypes/cfield.o Modules/_ctypes/malloc_closure.o Modules/_ctypes/_ctypes_test.o Modules/_cursesmodule.o Modules/_curses_panel.o Modules/_datetimemodule.o Modules/_dbmmodule.o Modules/_decimal/_decimal.o Modules/_elementtree.o Modules/_hashopenssl.o Modules/_heapqmodule.o Modules/_interpchannelsmodule.o Modules/_interpqueuesmodule.o Modules/_interpretersmodule.o Modules/_json.o Modules/_lsprof.o Modules/rotatingtree.o Modules/_lzmamodule.o Modules/md5module.o Modules/_hacl/Hacl_Hash_MD5.o Modules/cjkcodecs/multibytecodec.o Modules/_multiprocessing/multiprocessing.o Modules/_multiprocessing/semaphore.o Modules/_opcode.o Modules/_pickle.o Modules/_multiprocessing/posixshmem.o Modules/_posixsubprocess.o Modules/_queuemodule.o Modules/_randommodule.o Modules/_scproxy.o Modules/sha1module.o Modules/_hacl/Hacl_Hash_SHA1.o Modules/sha2module.o Modules/_hacl/Hacl_Hash_SHA2.o Modules/sha3module.o Modules/_hacl/Hacl_Hash_SHA3.o Modules/socketmodule.o Modules/_sqlite/connection.o Modules/_sqlite/cursor.o Modules/_sqlite/microprotocols.o Modules/_sqlite/module.o Modules/_sqlite/prepare_protocol.o Modules/_sqlite/row.o Modules/_sqlite/statement.o Modules/_sqlite/util.o Modules/_sqlite/blob.o Modules/_ssl.o Modules/_statisticsmodule.o Modules/_struct.o Modules/_suggestions.o Modules/_sysconfig.o Modules/_testbuffer.o Modules/_testexternalinspection.o Modules/_testimportmultiple.o Modules/_testinternalcapi.o Modules/_testinternalcapi/pytime.o Modules/_testinternalcapi/set.o Modules/_testinternalcapi/test_critical_sections.o Modules/_testinternalcapi/test_lock.o Modules/_testmultiphase.o Modules/_testsinglephase.o Modules/_tkinter.o Modules/tkappinit.o Modules/_typingmodule.o Modules/_uuidmodule.o Modules/_xxtestfuzz/_xxtestfuzz.o Modules/_xxtestfuzz/fuzzer.o Modules/_zoneinfo.o Modules/arraymodule.o Modules/binascii.o Modules/cmathmodule.o Modules/fcntlmodule.o Modules/grpmodule.o Modules/mathmodule.o Modules/mmapmodule.o Modules/pyexpat.o Modules/readline.o Modules/resource.o Modules/selectmodule.o Modules/syslogmodule.o Modules/termios.o Modules/unicodedata.o Modules/xxsubtype.o Modules/zlibmodule.o Modules/atexitmodule.o Modules/faulthandler.o Modules/posixmodule.o Modules/signalmodule.o Modules/_tracemalloc.o Modules/_codecsmodule.o Modules/_collectionsmodule.o Modules/errnomodule.o Modules/_io/_iomodule.o Modules/_io/iobase.o Modules/_io/fileio.o Modules/_io/bytesio.o Modules/_io/bufferedio.o Modules/_io/textio.o Modules/_io/stringio.o Modules/itertoolsmodule.o Modules/_sre/sre.o Modules/_threadmodule.o Modules/timemodule.o Modules/_weakref.o Modules/_abc.o Modules/_functoolsmodule.o Modules/_localemodule.o Modules/_operator.o Modules/_stat.o Modules/symtablemodule.o Modules/pwdmodule.o`
TRACE Updated `MODULE__CODECS_HK_DEPS` from `./Modules/cjkcodecs/mappings_hk.h  ./Modules/cjkcodecs/multibytecodec.h ./Modules/cjkcodecs/cjkcodecs.h` to `./Modules/cjkcodecs/mappings_hk.h ./Modules/cjkcodecs/multibytecodec.h ./Modules/cjkcodecs/cjkcodecs.h`
TRACE Updated `PY_BUILTIN_MODULE_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new -DPy_BUILD_CORE_BUILTIN` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -DPy_BUILD_CORE_BUILTIN`
TRACE Updated `PY_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `PY_CORE_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new -DPy_BUILD_CORE` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -DPy_BUILD_CORE`
TRACE Updated `PY_CORE_LDFLAGS` from `-arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl -flto=thin -Wl,-export_dynamic -Wl,-object_path_lto,"$@".lto -g` to `-arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl -flto=thin -Wl,-export_dynamic -Wl,-object_path_lto,"$@".lto -g`
TRACE Updated `PY_CPPFLAGS` from `-I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `PY_LDFLAGS` from `-arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl` to `-arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl`
TRACE Updated `PY_LDFLAGS_NOLTO` from `-arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk  -LModules/_hacl -flto=thin` to `-arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl -flto=thin`
TRACE Updated `PY_STDMODULE_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix  -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC    -Werror=unguarded-availability-new` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -flto=thin -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/private/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/Python-3.13.1/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new`
TRACE Updated `SCRIPTDIR` from `/install/lib` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/lib`
TRACE Updated `SHLIBS` from `-ldl  -framework CoreFoundation` to `-ldl -framework CoreFoundation`
TRACE Updated `SRCDIRS` from `Modules   Modules/_blake2   Modules/_ctypes   Modules/_decimal   Modules/_decimal/libmpdec   Modules/_hacl   Modules/_io   Modules/_multiprocessing   Modules/_sqlite   Modules/_sre   Modules/_testcapi   Modules/_testinternalcapi   Modules/_testlimitedcapi   Modules/_xxtestfuzz   Modules/cjkcodecs   Modules/expat   Objects   Objects/mimalloc   Objects/mimalloc/prim   Parser   Parser/tokenizer   Parser/lexer   Programs   Python   Python/frozen_modules` to `Modules Modules/_blake2 Modules/_ctypes Modules/_decimal Modules/_decimal/libmpdec Modules/_hacl Modules/_io Modules/_multiprocessing Modules/_sqlite Modules/_sre Modules/_testcapi Modules/_testinternalcapi Modules/_testlimitedcapi Modules/_xxtestfuzz Modules/cjkcodecs Modules/expat Objects Objects/mimalloc Objects/mimalloc/prim Parser Parser/tokenizer Parser/lexer Programs Python Python/frozen_modules`
TRACE Updated `datarootdir` from `/install/share` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/share`
TRACE Updated `exec_prefix` from `/install` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none`
TRACE Updated `prefix` from `/install` to `/Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none`
TRACE Updated 56 values
DEBUG Skipping installation of Python executables, use `--preview` to enable.
Installed Python 3.13.1 in 940ms
 ~ cpython-3.13.1-macos-aarch64-none
DEBUG Released lock at `/Users/zb/.local/share/uv/python/.lock`

❯ uvx uwsgi --python /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python
dyld[14111]: Library not loaded: /install/lib/libpython3.13.dylib
  Referenced from: <3909DC74-45DD-36D0-A1CA-68225017613B> /Users/zb/.cache/uv/archive-v0/OpyXzZ7V4vIBs8_rJ33gg/bin/uwsgi
  Reason: tried: '/install/lib/libpython3.13.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/install/lib/libpython3.13.dylib' (no such file), '/install/lib/libpython3.13.dylib' (no such file)
```

I'm surprised.

---

_Comment by @zanieb on 2024-12-18 21:58_

No clue where they're reading `/install/lib/libpython3.13.dylib` from?

---

_Comment by @charliermarsh on 2024-12-18 21:59_

(Don't you need to put the `--python` before `uvx wsgi`? )

---

_Comment by @zanieb on 2024-12-18 22:01_

Yeah sorry — that doesn't change anything though

```
❯ uvx --python /Users/zb/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python uwsgi
Installed 1 package in 2ms
dyld[14405]: Library not loaded: /install/lib/libpython3.13.dylib
  Referenced from: <3909DC74-45DD-36D0-A1CA-68225017613B> /Users/zb/.cache/uv/archive-v0/9vPn9XpgxPKqROT71vc2W/bin/uwsgi
  Reason: tried: '/install/lib/libpython3.13.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/install/lib/libpython3.13.dylib' (no such file), '/install/lib/libpython3.13.dylib' (no such file)
```

---

_Comment by @samypr100 on 2024-12-19 00:52_

Is it only an OSX issue?

```shell
> docker run --platform linux/x86_64 -it --rm ghcr.io/astral-sh/uv:0.5.10-bookworm /bin/bash -c "uvx --python 3.13.1 uwsgi"
   Built uwsgi==2.0.28
Installed 1 package in 1ms
```

---

_Comment by @zanieb on 2024-12-19 01:06_

For completeness:

```
❯ uv run -p 3.13 python -msysconfig | grep "/install"
	CONFIG_ARGS = "'--build=aarch64-apple-darwin' '--host=aarch64-apple-darwin' '--prefix=/install' '--with-openssl=/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/tools/deps' '--with-system-expat' '--with-system-libmpdec' '--without-ensurepip' '--enable-shared' '--enable-optimizations' '--with-lto' '--with-build-python=/var/folders/0w/4z5l9vds32nbkz7l22n8j6s80000gn/T/tmpbczfbq5n/tools/host/bin/python3.13' 'ac_cv_lib_intl_textdomain=no' 'ac_cv_func_ptsname_r=no' '--with-dbmliborder=ndbm' 'build_alias=aarch64-apple-darwin' 'host_alias=aarch64-apple-darwin' 'CC=clang' 'CFLAGS=-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new' 'LDFLAGS=-arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl' 'CPPFLAGS=-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new'"
	INSTALL = "/usr/bin/install -c"
	INSTALL_DATA = "/usr/bin/install -c -m 644"
	INSTALL_PROGRAM = "/usr/bin/install -c"
	INSTALL_SCRIPT = "/usr/bin/install -c"
	INSTALL_SHARED = "/usr/bin/install -c -m 755"
	MKDIR_P = "./install-sh -c -d"
	WASM_ASSETS_DIR = "./install"
	WASM_STDLIB = "./install/lib/python3.13/os.py"
```

---

_Comment by @zanieb on 2024-12-19 01:13_

I tried getting a bit more output

```
❯ export DYLD_PRINT_LIBRARIES=1
export DYLD_PRINT_APIS=1
export DYLD_PRINT_WARNINGS=1
export DYLD_PRINT_BINDINGS=1

❯ /Users/zb/.cache/uv/archive-v0/OpyXzZ7V4vIBs8_rJ33gg/bin/uwsgi
dyld[90953]: <3909DC74-45DD-36D0-A1CA-68225017613B> /Users/zb/.cache/uv/archive-v0/OpyXzZ7V4vIBs8_rJ33gg/bin/uwsgi
dyld[90953]: <548B061B-EF80-3713-94E9-78E7A5969EF5> /opt/homebrew/Cellar/pcre2/10.44/lib/libpcre2-8.0.dylib
dyld[90953]: Library not loaded: /install/lib/libpython3.13.dylib
  Referenced from: <3909DC74-45DD-36D0-A1CA-68225017613B> /Users/zb/.cache/uv/archive-v0/OpyXzZ7V4vIBs8_rJ33gg/bin/uwsgi
  Reason: tried: '/install/lib/libpython3.13.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/install/lib/libpython3.13.dylib' (no such file), '/install/lib/libpython3.13.dylib' (no such file)
zsh: abort      /Users/zb/.cache/uv/archive-v0/OpyXzZ7V4vIBs8_rJ33gg/bin/uwsgi
``

---

_Comment by @davidszotten on 2024-12-22 17:39_

Should we reopen this or one of the linked issues?

---

_Referenced in [astral-sh/uv#11028](../../astral-sh/uv/issues/11028.md) on 2025-01-28 17:48_

---

_Referenced in [astral-sh/uv#11811](../../astral-sh/uv/issues/11811.md) on 2025-02-26 21:26_

---

_Referenced in [Jij-Inc/pyo3-stub-gen#161](../../Jij-Inc/pyo3-stub-gen/issues/161.md) on 2025-04-05 08:32_

---

_Comment by @sabonerune on 2025-04-30 15:06_

It seems the problem has reappeared in v0.6.15.
It seems that the process of replacing `clang` and `clang++` with `cc` and `c++` for `CC` and `CXX` is not being done.

---

_Comment by @zanieb on 2025-04-30 15:27_

@sabonerune please open a new issue with more details.

---

_Referenced in [astral-sh/uv#13236](../../astral-sh/uv/issues/13236.md) on 2025-04-30 16:47_

---

_Referenced in [astral-sh/uv#13544](../../astral-sh/uv/issues/13544.md) on 2025-05-19 20:51_

---
