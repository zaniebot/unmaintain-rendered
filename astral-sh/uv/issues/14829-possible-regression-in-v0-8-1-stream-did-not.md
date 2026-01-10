```yaml
number: 14829
title: Possible regression in v0.8.1 - stream did not contain valid UTF-8
type: issue
state: closed
author: jaredmarks-allspring
labels:
  - bug
assignees: []
created_at: 2025-07-22T18:22:50Z
updated_at: 2025-07-22T19:11:16Z
url: https://github.com/astral-sh/uv/issues/14829
synced_at: 2026-01-10T03:32:45Z
```

# Possible regression in v0.8.1 - stream did not contain valid UTF-8

---

_Issue opened by @jaredmarks-allspring on 2025-07-22 18:22_

### Summary

Our docker builds began failing after the latest release.

```
FROM public.ecr.aws/amazonlinux/amazonlinux:2023-minimal

ENV HOME=/home
ENV UV_NO_CACHE=True UV_LINK_MODE=copy

WORKDIR $HOME

# Install uv
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

# Needed for building locally
RUN uv run --with showcert showcert -i --chain -o pem cdn.amazonlinux.com:443 > /etc/pki/ca-trust/source/anchors/al2023cdn.pem && \
    update-ca-trust
```

Yields:

#16 [stage-0  7/14] RUN uv run --with showcert showcert -i --chain -o pem cdn.amazonlinux.com:443 > /etc/pki/ca-trust/source/anchors/al2023cdn.pem &&     update-ca-trust
#16 0.475 Downloading cpython-3.13.5-linux-x86_64-gnu (download) (33.8MiB)
#16 0.532 Downloading cpython-3.13.5-linux-x86_64-gnu (download) (33.8MiB)
#16 2.077  Downloading cpython-3.13.5-linux-x86_64-gnu (download)
#16 2.525 Downloading cryptography (4.2MiB)
#16 3.528  Downloading cpython-3.13.5-linux-x86_64-gnu (download)
#16 3.949 Downloading cryptography (4.2MiB)
#16 3.666  Downloading cryptography
#16 3.682 Installed 8 packages in 15ms
#16 3.894 error: failed to read from file `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13`: stream did not contain valid UTF-8
#16 4.041  Downloading cryptography
#16 4.072 Installed 8 packages in 29ms
#16 4.266 error: failed to read from file `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13`: stream did not contain valid UTF-8
#16 ERROR: process "/bin/sh -c uv run --with showcert showcert -i --chain -o pem cdn.amazonlinux.com:443 > /etc/pki/ca-trust/source/anchors/al2023cdn.pem &&     update-ca-trust" did not complete successfully: exit code: 2

### Platform

linux

### Version

0.8.1

### Python version

3.13.5

---

_Label `bug` added by @jaredmarks-allspring on 2025-07-22 18:22_

---

_Comment by @jaredmarks-allspring on 2025-07-22 18:26_

@zanieb 

---

_Comment by @zanieb on 2025-07-22 18:26_

Can you share `-vv` logs please?

---

_Comment by @jaredmarks-allspring on 2025-07-22 18:29_

 > [stage-0  7/15] RUN uv run -vv --with showcert showcert -i --chain -o pem cdn.amazonlinux.com:443 > /etc/pki/ca-trust/source/anchors/al2023cdn.pem &&     update-ca-trust:
0.407 DEBUG uv 0.8.1
0.407 DEBUG No project found; searching for Python interpreter
0.407 DEBUG No Python version file found in ancestors of working directory: /home
0.407 DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
0.407 DEBUG Searching for managed installations at `.local/share/uv/python`
0.408 TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
0.408 TRACE stdout output from `ld`: ""
0.409 TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
0.409 TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
0.409 TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to determine libc
0.409 TRACE stdout output from `ld.so --version`: "ld.so (GNU libc) stable release version 2.34.\nCopyright (C) 2021 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
0.409 TRACE Found manylinux 2.34 in stdout of ld.so version
0.409 TRACE Searching PATH for executables: python, python3
0.409 TRACE Checking `PATH` directory for interpreters: /usr/local/sbin
0.409 TRACE Checking `PATH` directory for interpreters: /usr/local/bin
0.409 TRACE Checking `PATH` directory for interpreters: /usr/sbin
0.409 TRACE Checking `PATH` directory for interpreters: /usr/bin
0.410 TRACE Checking `PATH` directory for interpreters: /sbin
0.410 TRACE Skipping already seen directory: /sbin
0.410 TRACE Checking `PATH` directory for interpreters: /bin
0.410 TRACE Skipping already seen directory: /bin
0.410 TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
0.411 TRACE stdout output from `ld`: ""
0.411 TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
0.411 TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
0.411 TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to determine libc
0.411 TRACE stdout output from `ld.so --version`: "ld.so (GNU libc) stable release version 2.34.\nCopyright (C) 2021 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
0.411 TRACE Found manylinux 2.34 in stdout of ld.so version
0.421 TRACE Checking lock for `.local/share/uv/python` at `.local/share/uv/python/.lock`
0.421 DEBUG Acquired lock for `.local/share/uv/python`
0.421 DEBUG Using request timeout of 30s
0.421 INFO Fetching requested Python...
0.421 DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
0.421 DEBUG Extracting cpython-3.13.5-20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz to temporary location: /home/.local/share/uv/python/.temp/.tmpgYLVpK
0.421 TRACE Handling request for https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz with authentication policy auto
0.421 TRACE Request for https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz is unauthenticated, checking cache 
0.421 TRACE No credentials in cache for URL https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
0.421 TRACE Attempting unauthenticated request for https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.13.5%2B20250712-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
0.679 Downloading cpython-3.13.5-linux-x86_64-gnu (download) (33.8MiB)
2.650  Downloading cpython-3.13.5-linux-x86_64-gnu (download)
2.650 DEBUG Moving /home/.local/share/uv/python/.temp/.tmpgYLVpK/python to .local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu
2.650 TRACE Discovered `sysconfig` data at: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13/_sysconfigdata__linux_x86_64-linux-gnu.py
2.651 TRACE Updated `AR` from `/tools/llvm/bin/llvm-ar` to `ar`
2.651 TRACE Updated `BINDIR` from `/install/bin` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin`
2.651 TRACE Updated `BINLIBDEST` from `/install/lib/python3.13` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13`
2.651 TRACE Updated `BLDSHARED` from `clang -pthread -shared  -Wl,--exclude-libs,ALL -LModules/_hacl` to `cc -pthread -shared -Wl,--exclude-libs,ALL -LModules/_hacl`
2.651 TRACE Updated `CC` from `clang -pthread` to `cc -pthread`
2.651 TRACE Updated `CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC  ` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC`
2.651 TRACE Updated `CONFIGURE_CFLAGS` from ` -fPIC  ` to `-fPIC`
2.651 TRACE Updated `CONFIGURE_CPPFLAGS` from ` -fPIC  ` to `-fPIC`
2.651 TRACE Updated `CONFIGURE_LDFLAGS` from ` -Wl,--exclude-libs,ALL -LModules/_hacl` to `-Wl,--exclude-libs,ALL -LModules/_hacl`
2.651 TRACE Updated `CONFIG_ARGS` from `'--build=x86_64-unknown-linux-gnu' '--host=x86_64-unknown-linux-gnu' '--prefix=/install' '--with-openssl=/tools/deps' '--with-system-expat' '--with-system-libmpdec' '--without-ens
urepip' '--with-readline=editline' '--enable-static-libpython-for-interpreter' '--enable-shared' '--with-mimalloc' '--enable-optimizations' '--enable-bolt' '--enable-experimental-jit=yes-off' '--with-lto' '--with-build-
python=/tools/host/bin/python3.13' '--with-dbmliborder=bdb' 'build_alias=x86_64-unknown-linux-gnu' 'host_alias=x86_64-unknown-linux-gnu' 'PKG_CONFIG=pkg-config --static' 'PKG_CONFIG_PATH=/tools/deps/share/pkgconfig:/too
ls/deps/lib/pkgconfig' 'CC=clang' 'CFLAGS=  -fPIC  ' 'LDFLAGS=  -Wl,--exclude-libs,ALL -LModules/_hacl' 'CPPFLAGS=  -fPIC  '` to `'--build=x86_64-unknown-linux-gnu' '--host=x86_64-unknown-linux-gnu' '--prefix=/install' 
'--with-openssl=/tools/deps' '--with-system-expat' '--with-system-libmpdec' '--without-ensurepip' '--with-readline=editline' '--enable-static-libpython-for-interpreter' '--enable-shared' '--with-mimalloc' '--enable-opti
mizations' '--enable-bolt' '--enable-experimental-jit=yes-off' '--with-lto' '--with-build-python=/tools/host/bin/python3.13' '--with-dbmliborder=bdb' 'build_alias=x86_64-unknown-linux-gnu' 'host_alias=x86_64-unknown-linux-gnu' 'PKG_CONFIG=pkg-config --static' 'PKG_CONFIG_PATH=/tools/deps/share/pkgconfig:/tools/deps/lib/pkgconfig' 'CC=clang' 'CFLAGS= -fPIC ' 'LDFLAGS= -Wl,--exclude-libs,ALL -LModules/_hacl' 'CPPFLAGS= -fPIC '`
2.651 TRACE Updated `CONFINCLUDEDIR` from `/install/include` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/include`
2.651 TRACE Updated `CONFINCLUDEPY` from `/install/include/python3.13` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/include/python3.13`
2.651 TRACE Updated `CPPFLAGS` from `-I. -I./Include  -fPIC  ` to `-I. -I./Include -fPIC`
2.651 TRACE Updated `CXX` from `clang++ -pthread` to `c++ -pthread`
2.651 TRACE Updated `DESTDIRS` from `/install /install/lib /install/lib/python3.13 /install/lib/python3.13/lib-dynload` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13 /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13/lib-dynload`
2.651 TRACE Updated `DESTLIB` from `/install/lib/python3.13` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13`
2.651 TRACE Updated `DESTSHARED` from `/install/lib/python3.13/lib-dynload` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13/lib-dynload`
2.651 TRACE Updated `EXENAME` from `/install/bin/python3.13` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13`
2.651 TRACE Updated `INCLDIRSTOMAKE` from `/install/include /install/include /install/include/python3.13 /install/include/python3.13` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/include /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/include /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/include/python3.13 /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/include/python3.13`  
2.651 TRACE Updated `INCLUDEDIR` from `/install/include` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/include`
2.651 TRACE Updated `INCLUDEPY` from `/install/include/python3.13` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/include/python3.13`
2.651 TRACE Updated `LDCXXSHARED` from `clang++ -pthread -shared  -Wl,--exclude-libs,ALL -LModules/_hacl` to `c++ -pthread -shared -Wl,--exclude-libs,ALL -LModules/_hacl`
2.651 TRACE Updated `LDFLAGS` from ` -Wl,--exclude-libs,ALL -LModules/_hacl` to `-Wl,--exclude-libs,ALL -LModules/_hacl`
2.651 TRACE Updated `LDSHARED` from `clang -pthread -shared  -Wl,--exclude-libs,ALL -LModules/_hacl` to `cc -pthread -shared -Wl,--exclude-libs,ALL -LModules/_hacl`
2.651 TRACE Updated `LIBDEST` from `/install/lib/python3.13` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13`
2.651 TRACE Updated `LIBDIR` from `/install/lib` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib`
2.651 TRACE Updated `LIBEXPAT_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC   -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -Wn
o-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. 
-I./Include  -fPIC   -fPIC -fPIC` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -Wno-missing
-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -fPIC -fPIC -fPIC`
2.651 TRACE Updated `LIBHACL_CFLAGS` from `-I./Modules/_hacl/include -D_BSD_SOURCE -D_DEFAULT_SOURCE -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC   -D_Py_TIER2=3 -D_Py_JIT -flto=th
in -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclang
d -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include  -fPIC   -fPIC -fPIC` to `-I./Modules/_hacl/include -D_BSD_SOURCE -D_DEFAULT_SOURCE -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g
 -O3 -Wall -fPIC -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -fPIC -fPIC -fPIC`
2.651 TRACE Updated `LIBMPDEC_CFLAGS` from `  -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC   -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -
Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I
. -I./Include  -fPIC   -fPIC -fPIC` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -Wno-missi
ng-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -fPIC -fPIC -fPIC`
2.651 TRACE Updated `LIBPC` from `/install/lib/pkgconfig` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/pkgconfig`
2.651 TRACE Updated `LIBPL` from `/install/lib/python3.13/config-3.13-x86_64-linux-gnu` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13/config-3.13-x86_64-linux-gnu`
2.651 TRACE Updated `LIBS` from `-lpthread -ldl  -lutil` to `-lpthread -ldl -lutil`
2.651 TRACE Updated `LINKCC` from `clang -pthread -fno-pie -no-pie` to `cc -pthread -fno-pie -no-pie`
2.651 TRACE Updated `LOCALMODLIBS` from `-lbz2          -lffi -ldl  -lm  -lncursesw  -lpanelw -lncursesw   -ldb  -lmpdec  -lexpat  -lcrypto -l:libatomic.a        -llzma       -lrt         -lsqlite3  -lssl -lcrypto -l:li
batomic.a            -ltcl8.6 -ltk8.6 -lX11 -lxcb -lXau   -luuid      -lm    -lm   -lexpat  -ledit -lncursesw        -lz` to `-lbz2 -lffi -ldl -lm -lncursesw -lpanelw -lncursesw -ldb -lmpdec -lexpat -lcrypto -l:libatomic.a -llzma -lrt -lsqlite3 -lssl -lcrypto -l:libatomic.a -ltcl8.6 -ltk8.6 -lX11 -lxcb -lXau -luuid -lm -lm -lexpat -ledit -lncursesw -lz`
2.651 TRACE Updated `MACHDESTLIB` from `/install/lib/python3.13` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/python3.13`
2.651 TRACE Updated `MANDIR` from `/install/share/man` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/share/man`
2.651 TRACE Updated `MODBUILT_NAMES` from `_asyncio  _bisect  _blake2  _bz2  _codecs_cn  _codecs_hk  _codecs_iso2022  _codecs_jp  _codecs_kr  _codecs_tw  _contextvars  _csv  _ctypes  _ctypes_test  _curses  _curses_panel
  _datetime  _dbm  _decimal  _elementtree  _hashlib  _heapq  _interpchannels  _interpqueues  _interpreters  _json  _lsprof  _lzma  _md5  _multibytecodec  _multiprocessing  _opcode  _pickle  _posixshmem  _posixsubprocess
  _queue  _random  _sha1  _sha2  _sha3  _socket  _sqlite3  _ssl  _statistics  _struct  _suggestions  _sysconfig  _testbuffer  _testexternalinspection  _testimportmultiple  _testinternalcapi  _testmultiphase  _testsingle
phase  _tkinter  _typing  _uuid  _xxtestfuzz  _zoneinfo  array  binascii  cmath  fcntl  grp  math  mmap  pyexpat  readline  resource  select  syslog  termios  unicodedata  xxsubtype  zlib  atexit  faulthandler  posix  _
signal  _tracemalloc  _codecs  _collections  errno  _io  itertools  _sre  _thread  time  _weakref  _abc  _functools  _locale  _operator  _stat  _symtable  pwd` to `_asyncio _bisect _blake2 _bz2 _codecs_cn _codecs_hk _co
decs_iso2022 _codecs_jp _codecs_kr _codecs_tw _contextvars _csv _ctypes _ctypes_test _curses _curses_panel _datetime _dbm _decimal _elementtree _hashlib _heapq _interpchannels _interpqueues _interpreters _json _lsprof _
lzma _md5 _multibytecodec _multiprocessing _opcode _pickle _posixshmem _posixsubprocess _queue _random _sha1 _sha2 _sha3 _socket _sqlite3 _ssl _statistics _struct _suggestions _sysconfig _testbuffer _testexternalinspect
ion _testimportmultiple _testinternalcapi _testmultiphase _testsinglephase _tkinter _typing _uuid _xxtestfuzz _zoneinfo array binascii cmath fcntl grp math mmap pyexpat readline resource select syslog termios unicodedata xxsubtype zlib atexit faulthandler posix _signal _tracemalloc _codecs _collections errno _io itertools _sre _thread time _weakref _abc _functools _locale _operator _stat _symtable pwd`
2.651 TRACE Updated `MODDISABLED_NAMES` from `_gdbm  _scproxy  _testcapi  xx  xxlimited  xxlimited_35` to `_gdbm _scproxy _testcapi xx xxlimited xxlimited_35`
2.651 TRACE Updated `MODLIBS` from `-lbz2          -lffi -ldl  -lm  -lncursesw  -lpanelw -lncursesw   -ldb  -lmpdec  -lexpat  -lcrypto -l:libatomic.a        -llzma       -lrt         -lsqlite3  -lssl -lcrypto -l:libatom
ic.a            -ltcl8.6 -ltk8.6 -lX11 -lxcb -lXau   -luuid      -lm    -lm   -lexpat  -ledit -lncursesw        -lz` to `-lbz2 -lffi -ldl -lm -lncursesw -lpanelw -lncursesw -ldb -lmpdec -lexpat -lcrypto -l:libatomic.a -llzma -lrt -lsqlite3 -lssl -lcrypto -l:libatomic.a -ltcl8.6 -ltk8.6 -lX11 -lxcb -lXau -luuid -lm -lm -lexpat -ledit -lncursesw -lz`
2.651 TRACE Updated `MODOBJS` from `Modules/_asynciomodule.o  Modules/_bisectmodule.o  Modules/_blake2/blake2module.o Modules/_blake2/blake2b_impl.o Modules/_blake2/blake2s_impl.o  Modules/_bz2module.o  Modules/cjkcodec
s/_codecs_cn.o  Modules/cjkcodecs/_codecs_hk.o  Modules/cjkcodecs/_codecs_iso2022.o  Modules/cjkcodecs/_codecs_jp.o  Modules/cjkcodecs/_codecs_kr.o  Modules/cjkcodecs/_codecs_tw.o  Modules/_contextvarsmodule.o  Modules/
_csv.o  Modules/_ctypes/_ctypes.o Modules/_ctypes/callbacks.o Modules/_ctypes/callproc.o Modules/_ctypes/stgdict.o Modules/_ctypes/cfield.o  Modules/_ctypes/_ctypes_test.o  Modules/_cursesmodule.o  Modules/_curses_panel
.o  Modules/_datetimemodule.o  Modules/_dbmmodule.o  Modules/_decimal/_decimal.o  Modules/_elementtree.o  Modules/_hashopenssl.o  Modules/_heapqmodule.o  Modules/_interpchannelsmodule.o  Modules/_interpqueuesmodule.o  M
odules/_interpretersmodule.o  Modules/_json.o  Modules/_lsprof.o Modules/rotatingtree.o  Modules/_lzmamodule.o  Modules/md5module.o Modules/_hacl/Hacl_Hash_MD5.o  Modules/cjkcodecs/multibytecodec.o  Modules/_multiproces
sing/multiprocessing.o Modules/_multiprocessing/semaphore.o  Modules/_opcode.o  Modules/_pickle.o  Modules/_multiprocessing/posixshmem.o  Modules/_posixsubprocess.o  Modules/_queuemodule.o  Modules/_randommodule.o  Modu
les/sha1module.o Modules/_hacl/Hacl_Hash_SHA1.o  Modules/sha2module.o Modules/_hacl/Hacl_Hash_SHA2.o  Modules/sha3module.o Modules/_hacl/Hacl_Hash_SHA3.o  Modules/socketmodule.o  Modules/_sqlite/connection.o Modules/_sq
lite/cursor.o Modules/_sqlite/microprotocols.o Modules/_sqlite/module.o Modules/_sqlite/prepare_protocol.o Modules/_sqlite/row.o Modules/_sqlite/statement.o Modules/_sqlite/util.o Modules/_sqlite/blob.o  Modules/_ssl.o 
 Modules/_statisticsmodule.o  Modules/_struct.o  Modules/_suggestions.o  Modules/_sysconfig.o  Modules/_testbuffer.o  Modules/_testexternalinspection.o  Modules/_testimportmultiple.o  Modules/_testinternalcapi.o Modules
/_testinternalcapi/pytime.o Modules/_testinternalcapi/set.o Modules/_testinternalcapi/test_critical_sections.o Modules/_testinternalcapi/test_lock.o  Modules/_testmultiphase.o  Modules/_testsinglephase.o  Modules/_tkint
er.o Modules/tkappinit.o  Modules/_typingmodule.o  Modules/_uuidmodule.o  Modules/_xxtestfuzz/_xxtestfuzz.o Modules/_xxtestfuzz/fuzzer.o  Modules/_zoneinfo.o  Modules/arraymodule.o  Modules/binascii.o  Modules/cmathmodu
le.o  Modules/fcntlmodule.o  Modules/grpmodule.o  Modules/mathmodule.o  Modules/mmapmodule.o  Modules/pyexpat.o  Modules/readline.o  Modules/resource.o  Modules/selectmodule.o  Modules/syslogmodule.o  Modules/termios.o 
 Modules/unicodedata.o  Modules/xxsubtype.o  Modules/zlibmodule.o  Modules/atexitmodule.o  Modules/faulthandler.o  Modules/posixmodule.o  Modules/signalmodule.o  Modules/_tracemalloc.o  Modules/_codecsmodule.o  Modules/
_collectionsmodule.o  Modules/errnomodule.o  Modules/_io/_iomodule.o Modules/_io/iobase.o Modules/_io/fileio.o Modules/_io/bytesio.o Modules/_io/bufferedio.o Modules/_io/textio.o Modules/_io/stringio.o  Modules/itertool
smodule.o  Modules/_sre/sre.o  Modules/_threadmodule.o  Modules/timemodule.o  Modules/_weakref.o  Modules/_abc.o  Modules/_functoolsmodule.o  Modules/_localemodule.o  Modules/_operator.o  Modules/_stat.o  Modules/symtab
lemodule.o  Modules/pwdmodule.o` to `Modules/_asynciomodule.o Modules/_bisectmodule.o Modules/_blake2/blake2module.o Modules/_blake2/blake2b_impl.o Modules/_blake2/blake2s_impl.o Modules/_bz2module.o Modules/cjkcodecs/_
codecs_cn.o Modules/cjkcodecs/_codecs_hk.o Modules/cjkcodecs/_codecs_iso2022.o Modules/cjkcodecs/_codecs_jp.o Modules/cjkcodecs/_codecs_kr.o Modules/cjkcodecs/_codecs_tw.o Modules/_contextvarsmodule.o Modules/_csv.o Mod
ules/_ctypes/_ctypes.o Modules/_ctypes/callbacks.o Modules/_ctypes/callproc.o Modules/_ctypes/stgdict.o Modules/_ctypes/cfield.o Modules/_ctypes/_ctypes_test.o Modules/_cursesmodule.o Modules/_curses_panel.o Modules/_da
tetimemodule.o Modules/_dbmmodule.o Modules/_decimal/_decimal.o Modules/_elementtree.o Modules/_hashopenssl.o Modules/_heapqmodule.o Modules/_interpchannelsmodule.o Modules/_interpqueuesmodule.o Modules/_interpretersmod
ule.o Modules/_json.o Modules/_lsprof.o Modules/rotatingtree.o Modules/_lzmamodule.o Modules/md5module.o Modules/_hacl/Hacl_Hash_MD5.o Modules/cjkcodecs/multibytecodec.o Modules/_multiprocessing/multiprocessing.o Module
s/_multiprocessing/semaphore.o Modules/_opcode.o Modules/_pickle.o Modules/_multiprocessing/posixshmem.o Modules/_posixsubprocess.o Modules/_queuemodule.o Modules/_randommodule.o Modules/sha1module.o Modules/_hacl/Hacl_
Hash_SHA1.o Modules/sha2module.o Modules/_hacl/Hacl_Hash_SHA2.o Modules/sha3module.o Modules/_hacl/Hacl_Hash_SHA3.o Modules/socketmodule.o Modules/_sqlite/connection.o Modules/_sqlite/cursor.o Modules/_sqlite/microproto
cols.o Modules/_sqlite/module.o Modules/_sqlite/prepare_protocol.o Modules/_sqlite/row.o Modules/_sqlite/statement.o Modules/_sqlite/util.o Modules/_sqlite/blob.o Modules/_ssl.o Modules/_statisticsmodule.o Modules/_stru
ct.o Modules/_suggestions.o Modules/_sysconfig.o Modules/_testbuffer.o Modules/_testexternalinspection.o Modules/_testimportmultiple.o Modules/_testinternalcapi.o Modules/_testinternalcapi/pytime.o Modules/_testinternal
capi/set.o Modules/_testinternalcapi/test_critical_sections.o Modules/_testinternalcapi/test_lock.o Modules/_testmultiphase.o Modules/_testsinglephase.o Modules/_tkinter.o Modules/tkappinit.o Modules/_typingmodule.o Mod
ules/_uuidmodule.o Modules/_xxtestfuzz/_xxtestfuzz.o Modules/_xxtestfuzz/fuzzer.o Modules/_zoneinfo.o Modules/arraymodule.o Modules/binascii.o Modules/cmathmodule.o Modules/fcntlmodule.o Modules/grpmodule.o Modules/math
module.o Modules/mmapmodule.o Modules/pyexpat.o Modules/readline.o Modules/resource.o Modules/selectmodule.o Modules/syslogmodule.o Modules/termios.o Modules/unicodedata.o Modules/xxsubtype.o Modules/zlibmodule.o Module
s/atexitmodule.o Modules/faulthandler.o Modules/posixmodule.o Modules/signalmodule.o Modules/_tracemalloc.o Modules/_codecsmodule.o Modules/_collectionsmodule.o Modules/errnomodule.o Modules/_io/_iomodule.o Modules/_io/
iobase.o Modules/_io/fileio.o Modules/_io/bytesio.o Modules/_io/bufferedio.o Modules/_io/textio.o Modules/_io/stringio.o Modules/itertoolsmodule.o Modules/_sre/sre.o Modules/_threadmodule.o Modules/timemodule.o Modules/_weakref.o Modules/_abc.o Modules/_functoolsmodule.o Modules/_localemodule.o Modules/_operator.o Modules/_stat.o Modules/symtablemodule.o Modules/pwdmodule.o`
2.651 TRACE Updated `MODULE__CODECS_HK_DEPS` from `./Modules/cjkcodecs/mappings_hk.h  ./Modules/cjkcodecs/multibytecodec.h ./Modules/cjkcodecs/cjkcodecs.h` to `./Modules/cjkcodecs/mappings_hk.h ./Modules/cjkcodecs/multibytecodec.h ./Modules/cjkcodecs/cjkcodecs.h`
2.651 TRACE Updated `PY_BUILTIN_MODULE_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC   -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-para
meter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mima
lloc -I. -I./Include  -fPIC   -fPIC -DPy_BUILD_CORE_BUILTIN` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-un
used-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -fPIC -fPIC -DPy_BUILD_CORE_BUILTIN`
2.651 TRACE Updated `PY_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC  ` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC`       
2.651 TRACE Updated `PY_CORE_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC   -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -Wno
-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -
I./Include  -fPIC   -fPIC -DPy_BUILD_CORE` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -Wn
o-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -fPIC -fPIC -DPy_BUILD_CORE`
2.651 TRACE Updated `PY_CORE_LDFLAGS` from ` -Wl,--exclude-libs,ALL -LModules/_hacl -flto=thin -g -Wl,--emit-relocs` to `-Wl,--exclude-libs,ALL -LModules/_hacl -flto=thin -g -Wl,--emit-relocs`
2.651 TRACE Updated `PY_CPPFLAGS` from `-I. -I./Include  -fPIC  ` to `-I. -I./Include -fPIC`
2.651 TRACE Updated `PY_LDFLAGS` from ` -Wl,--exclude-libs,ALL -LModules/_hacl` to `-Wl,--exclude-libs,ALL -LModules/_hacl`
2.651 TRACE Updated `PY_LDFLAGS_NOLTO` from ` -Wl,--exclude-libs,ALL -LModules/_hacl -flto=thin` to `-Wl,--exclude-libs,ALL -LModules/_hacl -flto=thin`
2.651 TRACE Updated `PY_STDMODULE_CFLAGS` from `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall  -fPIC   -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter
 -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc 
-I. -I./Include  -fPIC   -fPIC` to `-fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -D_Py_TIER2=3 -D_Py_JIT -flto=thin -g -fno-pie -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-f
ield-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-instr-use=/build/Python-3.13.5/code.profclangd -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -fPIC -fPIC`
2.651 TRACE Updated `SCRIPTDIR` from `/install/lib` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib`
2.651 TRACE Updated `SHLIBS` from `-lpthread -ldl  -lutil` to `-lpthread -ldl -lutil`
2.651 TRACE Updated `SRCDIRS` from `Modules   Modules/_blake2   Modules/_ctypes   Modules/_decimal   Modules/_decimal/libmpdec   Modules/_hacl   Modules/_io   Modules/_multiprocessing   Modules/_sqlite   Modules/_sre   
Modules/_testcapi   Modules/_testinternalcapi   Modules/_testlimitedcapi   Modules/_xxtestfuzz   Modules/cjkcodecs   Modules/expat   Objects   Objects/mimalloc   Objects/mimalloc/prim   Parser   Parser/tokenizer   Parse
r/lexer   Programs   Python   Python/frozen_modules` to `Modules Modules/_blake2 Modules/_ctypes Modules/_decimal Modules/_decimal/libmpdec Modules/_hacl Modules/_io Modules/_multiprocessing Modules/_sqlite Modules/_sre
 Modules/_testcapi Modules/_testinternalcapi Modules/_testlimitedcapi Modules/_xxtestfuzz Modules/cjkcodecs Modules/expat Objects Objects/mimalloc Objects/mimalloc/prim Parser Parser/tokenizer Parser/lexer Programs Python Python/frozen_modules`
2.651 TRACE Updated `datarootdir` from `/install/share` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/share`
2.651 TRACE Updated `exec_prefix` from `/install` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu`
2.651 TRACE Updated `prefix` from `/install` to `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu`
2.651 TRACE Updated 55 values
2.669 TRACE Discovered `pkgconfig` data at: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/pkgconfig/python-3.13-embed.pc
2.669 TRACE Discovered `pkgconfig` data at: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib/pkgconfig/python-3.13.pc
2.669 TRACE Querying interpreter executable at /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
2.779 DEBUG Released lock at `/home/.local/share/uv/python/.lock`
2.779 DEBUG Using Python 3.13.5 interpreter at: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
2.779 DEBUG At least one requirement is not satisfied in the base environment: showcert
2.779 DEBUG Syncing `--with` requirements to cached environment
2.779 DEBUG Assessing Python executable as base candidate: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
2.779 DEBUG Caching via base interpreter: `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13`
2.779 DEBUG Using request timeout of 30s
2.782 DEBUG Solving with installed Python version: 3.13.5
2.782 DEBUG Solving with target Python version: >=3.13.5
2.782 TRACE Assigned packages:
2.783 TRACE Chose package for decision: root. remaining choices:
2.783 DEBUG Adding direct dependency: showcert*
2.783 TRACE Assigned packages: root==0a0.dev0
2.783 TRACE Fetching metadata for showcert from https://pypi.org/simple/showcert/
2.783 TRACE Chose package for decision: showcert. remaining choices:
2.783 TRACE No cache entry exists for /tmp/.tmpBXR6qN/simple-v16/pypi/showcert.rkyv
2.783 DEBUG No cache entry for: https://pypi.org/simple/showcert/
2.783 TRACE Sending fresh GET request for https://pypi.org/simple/showcert/
2.783 TRACE Handling request for https://pypi.org/simple/showcert/ with authentication policy auto
2.783 TRACE Request for https://pypi.org/simple/showcert/ is unauthenticated, checking cache
2.783 TRACE No credentials in cache for URL https://pypi.org/simple/showcert/
2.783 TRACE Attempting unauthenticated request for https://pypi.org/simple/showcert/
3.191 TRACE Cached request https://pypi.org/simple/showcert/ is storable because its response has a 'public' cache-control directive
3.193 TRACE Received package metadata for: showcert
3.193 TRACE Selecting candidate for showcert with range * with 50 remote versions
3.193 DEBUG Searching for a compatible version of showcert (*)
3.193 TRACE Selecting candidate for showcert with range * with 50 remote versions
3.193 TRACE Found candidate for package showcert with range * after 1 steps: 0.4.4 version
3.193 TRACE Returning candidate for package showcert with range * after 1 steps
3.194 TRACE Found candidate for package showcert with range * after 1 steps: 0.4.4 version
3.194 TRACE Returning candidate for package showcert with range * after 1 steps
3.194 DEBUG Selecting: showcert==0.4.4 [compatible] (showcert-0.4.4-py3-none-any.whl)
3.194 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/showcert/0.4.4-py3-none-any.msgpack
3.194 DEBUG No cache entry for: https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl.metadata
3.194 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl.metadata
3.194 TRACE Handling request for https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl.metadata with authentication policy auto       
3.194 TRACE Request for https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl.metadata is unauthenticated, checking cache
3.194 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl.metadata
3.194 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl.metadata
3.384 TRACE Cached request https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
3.386 TRACE Received built distribution metadata for: showcert==0.4.4
3.386 DEBUG Adding transitive dependency for showcert==0.4.4: certifi>=2018.10.15
3.386 DEBUG Adding transitive dependency for showcert==0.4.4: cryptography>=41.0.0
3.386 DEBUG Adding transitive dependency for showcert==0.4.4: pem>=23.1.0
3.386 DEBUG Adding transitive dependency for showcert==0.4.4: pyopenssl>=24.0.0
3.386 DEBUG Adding transitive dependency for showcert==0.4.4: python-magic*
3.386 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4
3.386 TRACE Chose package for decision: certifi. remaining choices: python-magic, cryptography, pem, pyopenssl
3.386 TRACE Fetching metadata for certifi from https://pypi.org/simple/certifi/
3.386 TRACE Fetching metadata for cryptography from https://pypi.org/simple/cryptography/
3.386 TRACE Fetching metadata for pem from https://pypi.org/simple/pem/
3.386 TRACE Fetching metadata for pyopenssl from https://pypi.org/simple/pyopenssl/
3.386 TRACE Fetching metadata for python-magic from https://pypi.org/simple/python-magic/
3.388 TRACE No cache entry exists for /tmp/.tmpBXR6qN/simple-v16/pypi/certifi.rkyv
3.388 DEBUG No cache entry for: https://pypi.org/simple/certifi/
3.388 TRACE Sending fresh GET request for https://pypi.org/simple/certifi/
3.388 TRACE Handling request for https://pypi.org/simple/certifi/ with authentication policy auto
3.388 TRACE Request for https://pypi.org/simple/certifi/ is unauthenticated, checking cache
3.388 TRACE No credentials in cache for URL https://pypi.org/simple/certifi/
3.388 TRACE Attempting unauthenticated request for https://pypi.org/simple/certifi/
3.388 TRACE No cache entry exists for /tmp/.tmpBXR6qN/simple-v16/pypi/cryptography.rkyv
3.388 DEBUG No cache entry for: https://pypi.org/simple/cryptography/
3.388 TRACE Sending fresh GET request for https://pypi.org/simple/cryptography/
3.388 TRACE Handling request for https://pypi.org/simple/cryptography/ with authentication policy auto
3.388 TRACE Request for https://pypi.org/simple/cryptography/ is unauthenticated, checking cache
3.388 TRACE No credentials in cache for URL https://pypi.org/simple/cryptography/
3.388 TRACE Attempting unauthenticated request for https://pypi.org/simple/cryptography/
3.388 TRACE No cache entry exists for /tmp/.tmpBXR6qN/simple-v16/pypi/pem.rkyv
3.388 DEBUG No cache entry for: https://pypi.org/simple/pem/
3.388 TRACE Sending fresh GET request for https://pypi.org/simple/pem/
3.388 TRACE Handling request for https://pypi.org/simple/pem/ with authentication policy auto
3.388 TRACE Request for https://pypi.org/simple/pem/ is unauthenticated, checking cache
3.388 TRACE No credentials in cache for URL https://pypi.org/simple/pem/
3.388 TRACE Attempting unauthenticated request for https://pypi.org/simple/pem/
3.388 TRACE No cache entry exists for /tmp/.tmpBXR6qN/simple-v16/pypi/pyopenssl.rkyv
3.388 DEBUG No cache entry for: https://pypi.org/simple/pyopenssl/
3.388 TRACE Sending fresh GET request for https://pypi.org/simple/pyopenssl/
3.388 TRACE Handling request for https://pypi.org/simple/pyopenssl/ with authentication policy auto
3.388 TRACE Request for https://pypi.org/simple/pyopenssl/ is unauthenticated, checking cache
3.388 TRACE No credentials in cache for URL https://pypi.org/simple/pyopenssl/
3.388 TRACE Attempting unauthenticated request for https://pypi.org/simple/pyopenssl/
3.388 TRACE No cache entry exists for /tmp/.tmpBXR6qN/simple-v16/pypi/python-magic.rkyv
3.388 DEBUG No cache entry for: https://pypi.org/simple/python-magic/
3.388 TRACE Sending fresh GET request for https://pypi.org/simple/python-magic/
3.388 TRACE Handling request for https://pypi.org/simple/python-magic/ with authentication policy auto
3.388 TRACE Request for https://pypi.org/simple/python-magic/ is unauthenticated, checking cache
3.388 TRACE No credentials in cache for URL https://pypi.org/simple/python-magic/
3.388 TRACE Attempting unauthenticated request for https://pypi.org/simple/python-magic/
3.429 TRACE Cached request https://pypi.org/simple/certifi/ is storable because its response has a 'public' cache-control directive
3.431 TRACE Cached request https://pypi.org/simple/pyopenssl/ is storable because its response has a 'public' cache-control directive
3.432 TRACE Received package metadata for: certifi
3.432 TRACE Selecting candidate for certifi with range >=2018.10.15 with 66 remote versions
3.432 DEBUG Searching for a compatible version of certifi (>=2018.10.15)
3.432 TRACE Selecting candidate for certifi with range >=2018.10.15 with 66 remote versions
3.432 TRACE Found candidate for package certifi with range >=2018.10.15 after 1 steps: 2025.7.14 version
3.432 TRACE Returning candidate for package certifi with range >=2018.10.15 after 1 steps
3.432 TRACE Found candidate for package certifi with range >=2018.10.15 after 1 steps: 2025.7.14 version
3.432 TRACE Returning candidate for package certifi with range >=2018.10.15 after 1 steps
3.432 DEBUG Selecting: certifi==2025.7.14 [compatible] (certifi-2025.7.14-py3-none-any.whl)
3.433 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/certifi/2025.7.14-py3-none-any.msgpack
3.433 DEBUG No cache entry for: https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl.metadata
3.433 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl.metadata
3.433 TRACE Handling request for https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl.metadata with authentication policy auto    
3.433 TRACE Request for https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl.metadata is unauthenticated, checking cache
3.433 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl.metadata
3.433 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl.metadata
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.10-py2.4-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.10-py2.5-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.10-py2.6-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.10.winxp32-py2.4.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.10.winxp32-py2.5.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.10.winxp32-py2.5.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.10.winxp32-py2.6.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.10.winxp32-py2.6.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12-py2.4-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12-py2.5-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12-py2.6-win-amd64.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12-py2.6-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12-py2.7-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12-py3.2-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12.winxp32-py2.5.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12.winxp32-py2.6.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12.winxp32-py2.7.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.12.winxp32-py3.2.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13-py2.4-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13-py2.5-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13-py2.6-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13-py2.7-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13-py3.2-win32.egg
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.1.win-amd64-py2.6.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.1.win-amd64-py2.7.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.1.win-amd64-py3.2.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.1.win32-py2.6.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.1.win32-py2.7.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.1.win32-py3.2.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py2.4.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py2.5.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py2.5.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py2.6.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py2.6.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py2.7.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py2.7.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py3.2.exe
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.13.winxp32-py3.2.msi
3.451 WARN Skipping file for pyopenssl: pyOpenSSL-0.9.win32-py2.6.exe
3.451 TRACE Cached request https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
3.452 TRACE Cached request https://pypi.org/simple/cryptography/ is storable because its response has a 'public' cache-control directive
3.452 TRACE Received package metadata for: pyopenssl
3.452 TRACE Selecting candidate for pyopenssl with range >=24.0.0 with 39 remote versions
3.452 TRACE Found candidate for package pyopenssl with range >=24.0.0 after 1 steps: 25.1.0 version
3.452 TRACE Returning candidate for package pyopenssl with range >=24.0.0 after 1 steps
3.452 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/pyopenssl/25.1.0-py3-none-any.msgpack
3.452 DEBUG No cache entry for: https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl.metadata
3.452 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl.metadata
3.452 TRACE Handling request for https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl.metadata with authentication policy auto     
3.452 TRACE Request for https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl.metadata is unauthenticated, checking cache
3.452 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl.metadata
3.452 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl.metadata
3.452 TRACE Received built distribution metadata for: certifi==2025.7.14
3.452 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14
3.452 TRACE Chose package for decision: cryptography. remaining choices: python-magic, pyopenssl, pem
3.463 TRACE Cached request https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
3.464 TRACE Received built distribution metadata for: pyopenssl==25.1.0
3.472 TRACE Cached request https://pypi.org/simple/python-magic/ is storable because its response has a 'public' cache-control directive
3.472 WARN Skipping file for python-magic: python_magic-0.4.0-py2.5.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.0-py2.6.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.10-py2.7.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.5-py2.6.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.5-py2.7.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.5-py3.1.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.5-py3.2.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.6-py2.6.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.6-py2.7.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.6-py3.2.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.7-py2.6.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.7-py2.7.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.7-py3.2.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.8-py2.6.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.8-py2.7.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.8-py3.2.egg
3.472 WARN Skipping file for python-magic: python_magic-0.4.9-py2.7.egg
3.473 TRACE Received package metadata for: python-magic
3.473 TRACE Selecting candidate for python-magic with range * with 27 remote versions
3.473 TRACE Found candidate for package python-magic with range * after 1 steps: 0.4.27 version
3.473 TRACE Returning candidate for package python-magic with range * after 1 steps
3.473 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/python-magic/0.4.27-py2.py3-none-any.msgpack
3.473 DEBUG No cache entry for: https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl.metadata
3.473 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl.metadata
3.473 TRACE Handling request for https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl.metadata with authentication policy auto
3.473 TRACE Request for https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl.metadata is unauthenticated, checking cache    
3.473 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl.metadata
3.473 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl.metadata
3.475 TRACE Cached request https://pypi.org/simple/pem/ is storable because its response has a 'public' cache-control directive
3.476 TRACE Received package metadata for: pem
3.476 TRACE Selecting candidate for pem with range >=23.1.0 with 16 remote versions
3.476 TRACE Found candidate for package pem with range >=23.1.0 after 1 steps: 23.1.0 version
3.476 TRACE Returning candidate for package pem with range >=23.1.0 after 1 steps
3.476 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/pem/23.1.0-py3-none-any.msgpack
3.476 DEBUG No cache entry for: https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl.metadata
3.476 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl.metadata
3.476 TRACE Handling request for https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl.metadata with authentication policy auto
3.476 TRACE Request for https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl.metadata is unauthenticated, checking cache
3.476 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl.metadata
3.476 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl.metadata
3.491 TRACE Cached request https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
3.494 TRACE Cached request https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
3.494 TRACE Received built distribution metadata for: python-magic==0.4.27
3.494 TRACE Received built distribution metadata for: pem==23.1.0
3.495 TRACE Received package metadata for: cryptography
3.495 TRACE Selecting candidate for cryptography with range >=41.0.0 with 143 remote versions
3.495 DEBUG Searching for a compatible version of cryptography (>=41.0.0)
3.495 TRACE Selecting candidate for cryptography with range >=41.0.0 with 143 remote versions
3.495 TRACE Found candidate for package cryptography with range >=41.0.0 after 1 steps: 45.0.5 version
3.495 TRACE Returning candidate for package cryptography with range >=41.0.0 after 1 steps
3.495 TRACE Found candidate for package cryptography with range >=41.0.0 after 1 steps: 45.0.5 version
3.495 TRACE Returning candidate for package cryptography with range >=41.0.0 after 1 steps
3.495 DEBUG Selecting: cryptography==45.0.5 [compatible] (cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl)
3.495 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/cryptography/45.0.5-cp311-abi3-manylinux_2_34_x86_64.msgpack
3.495 DEBUG No cache entry for: https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl.metadata
3.495 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl.metadata     
3.495 TRACE Handling request for https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl.metadata with authentication policy auto
3.495 TRACE Request for https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl.metadata is unauthenticated, checking cache
3.495 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl.metadata   
3.495 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl.metadata
3.507 TRACE Cached request https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl.metadata is storable because its response has a 'public' cache-control directive
3.508 TRACE Received built distribution metadata for: cryptography==45.0.5
3.508 DEBUG Adding transitive dependency for cryptography==45.0.5: cffi{platform_python_implementation != 'PyPy'}>=1.14
3.508 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14, cryptography==45.0.5
3.508 TRACE Fetching metadata for cffi from https://pypi.org/simple/cffi/
3.508 TRACE Chose package for decision: pem. remaining choices: python-magic, pyopenssl
3.508 DEBUG Searching for a compatible version of pem (>=23.1.0)
3.508 TRACE Selecting candidate for pem with range >=23.1.0 with 16 remote versions
3.508 TRACE Found candidate for package pem with range >=23.1.0 after 1 steps: 23.1.0 version
3.508 TRACE Returning candidate for package pem with range >=23.1.0 after 1 steps
3.508 DEBUG Selecting: pem==23.1.0 [compatible] (pem-23.1.0-py3-none-any.whl)
3.508 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14, cryptography==45.0.5, pem==23.1.0
3.508 TRACE Chose package for decision: pyopenssl. remaining choices: python-magic
3.508 DEBUG Searching for a compatible version of pyopenssl (>=24.0.0)
3.508 TRACE Selecting candidate for pyopenssl with range >=24.0.0 with 39 remote versions
3.508 TRACE Found candidate for package pyopenssl with range >=24.0.0 after 1 steps: 25.1.0 version
3.508 TRACE Returning candidate for package pyopenssl with range >=24.0.0 after 1 steps
3.508 DEBUG Selecting: pyopenssl==25.1.0 [compatible] (pyopenssl-25.1.0-py3-none-any.whl)
3.509 TRACE No cache entry exists for /tmp/.tmpBXR6qN/simple-v16/pypi/cffi.rkyv
3.509 DEBUG No cache entry for: https://pypi.org/simple/cffi/
3.509 TRACE Sending fresh GET request for https://pypi.org/simple/cffi/
3.509 DEBUG Adding transitive dependency for pyopenssl==25.1.0: cryptography>=41.0.5, <46
3.509 TRACE Handling request for https://pypi.org/simple/cffi/ with authentication policy auto
3.509 TRACE Request for https://pypi.org/simple/cffi/ is unauthenticated, checking cache
3.509 TRACE No credentials in cache for URL https://pypi.org/simple/cffi/
3.509 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14, cryptography==45.0.5, pem==23.1.0, pyopenssl==25.1.0
3.509 TRACE Chose package for decision: python-magic. remaining choices:
3.509 DEBUG Searching for a compatible version of python-magic (*)
3.509 TRACE Selecting candidate for python-magic with range * with 27 remote versions
3.509 TRACE Found candidate for package python-magic with range * after 1 steps: 0.4.27 version
3.509 TRACE Returning candidate for package python-magic with range * after 1 steps
3.509 TRACE Attempting unauthenticated request for https://pypi.org/simple/cffi/
3.509 DEBUG Selecting: python-magic==0.4.27 [compatible] (python_magic-0.4.27-py2.py3-none-any.whl)
3.509 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14, cryptography==45.0.5, pem==23.1.0, pyopenssl==25.1.0, python-magic==0.4.27
3.509 TRACE Chose package for decision: cffi{platform_python_implementation != 'PyPy'}. remaining choices:
3.521 TRACE Cached request https://pypi.org/simple/cffi/ is storable because its response has a 'public' cache-control directive
3.536 TRACE Received package metadata for: cffi
3.536 DEBUG Searching for a compatible version of cffi{platform_python_implementation != 'PyPy'} (>=1.14)
3.536 TRACE Selecting candidate for cffi with range >=1.14 with 78 remote versions
3.536 TRACE Found candidate for package cffi with range >=1.14 after 1 steps: 1.17.1 version
3.536 TRACE Returning candidate for package cffi with range >=1.14 after 1 steps
3.536 DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
3.536 DEBUG Adding transitive dependency for cffi==1.17.1: cffi==1.17.1
3.536 DEBUG Adding transitive dependency for cffi==1.17.1: cffi{platform_python_implementation != 'PyPy'}==1.17.1
3.536 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14, cryptography==45.0.5, pem==23.1.0, pyopenssl==25.1.0, python-magic==0.4.27
3.536 TRACE Selecting candidate for cffi with range ==1.17.1 with 78 remote versions
3.536 TRACE Found candidate for package cffi with range ==1.17.1 after 1 steps: 1.17.1 version
3.536 TRACE Returning candidate for package cffi with range ==1.17.1 after 1 steps
3.536 TRACE Chose package for decision: cffi. remaining choices: cffi{platform_python_implementation != 'PyPy'}
3.536 DEBUG Searching for a compatible version of cffi (==1.17.1)
3.536 TRACE Selecting candidate for cffi with range ==1.17.1 with 78 remote versions
3.536 TRACE Found candidate for package cffi with range ==1.17.1 after 1 steps: 1.17.1 version
3.536 TRACE Returning candidate for package cffi with range ==1.17.1 after 1 steps
3.536 DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
3.536 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/cffi/1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.msgpack
3.536 DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata 
3.536 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
3.536 TRACE Handling request for https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata with authentication policy auto
3.536 TRACE Request for https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata is unauthenticated, checking cache
3.536 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
3.536 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
3.548 TRACE Cached request https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata is storable because its response has a 'public' cache-control directive
3.549 TRACE Received built distribution metadata for: cffi==1.17.1
3.549 DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
3.549 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14, cryptography==45.0.5, pem==23.1.0, pyopenssl==25.1.0, python-magic==0.4.27, cffi==1.17.1
3.549 TRACE Fetching metadata for pycparser from https://pypi.org/simple/pycparser/
3.549 TRACE Chose package for decision: cffi{platform_python_implementation != 'PyPy'}. remaining choices: pycparser
3.549 DEBUG Searching for a compatible version of cffi{platform_python_implementation != 'PyPy'} (==1.17.1)
3.549 TRACE Selecting candidate for cffi with range ==1.17.1 with 78 remote versions
3.549 TRACE Found candidate for package cffi with range ==1.17.1 after 1 steps: 1.17.1 version
3.549 TRACE Returning candidate for package cffi with range ==1.17.1 after 1 steps
3.549 DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
3.549 DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
3.549 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14, cryptography==45.0.5, pem==23.1.0, pyopenssl==25.1.0, python-magic==0.4.27, cffi==1.17.1, cffi{platform_python_implementation != 'PyPy'}==1.17.1
3.549 TRACE No cache entry exists for /tmp/.tmpBXR6qN/simple-v16/pypi/pycparser.rkyv
3.549 TRACE Chose package for decision: pycparser. remaining choices:
3.549 DEBUG No cache entry for: https://pypi.org/simple/pycparser/
3.549 TRACE Sending fresh GET request for https://pypi.org/simple/pycparser/
3.549 TRACE Handling request for https://pypi.org/simple/pycparser/ with authentication policy auto
3.549 TRACE Request for https://pypi.org/simple/pycparser/ is unauthenticated, checking cache
3.549 TRACE No credentials in cache for URL https://pypi.org/simple/pycparser/
3.549 TRACE Attempting unauthenticated request for https://pypi.org/simple/pycparser/
3.561 TRACE Cached request https://pypi.org/simple/pycparser/ is storable because its response has a 'public' cache-control directive
3.562 TRACE Received package metadata for: pycparser
3.562 TRACE Selecting candidate for pycparser with range * with 22 remote versions
3.562 TRACE Found candidate for package pycparser with range * after 1 steps: 2.22 version
3.562 TRACE Returning candidate for package pycparser with range * after 1 steps
3.562 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/pycparser/2.22-py3-none-any.msgpack
3.562 DEBUG No cache entry for: https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata
3.562 DEBUG Searching for a compatible version of pycparser (*)
3.562 TRACE Selecting candidate for pycparser with range * with 22 remote versions
3.562 TRACE Found candidate for package pycparser with range * after 1 steps: 2.22 version
3.562 TRACE Returning candidate for package pycparser with range * after 1 steps
3.562 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata
3.562 DEBUG Selecting: pycparser==2.22 [compatible] (pycparser-2.22-py3-none-any.whl)
3.562 TRACE Handling request for https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata with authentication policy auto       
3.562 TRACE Request for https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata is unauthenticated, checking cache
3.562 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata
3.562 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata
3.574 TRACE Cached request https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
3.574 TRACE Received built distribution metadata for: pycparser==2.22
3.574 TRACE Assigned packages: root==0a0.dev0, showcert==0.4.4, certifi==2025.7.14, cryptography==45.0.5, pem==23.1.0, pyopenssl==25.1.0, python-magic==0.4.27, cffi==1.17.1, cffi{platform_python_implementation != 'PyPy'}==1.17.1, pycparser==2.22
3.574 DEBUG Tried 8 versions: certifi 1, cffi 1, cryptography 1, pem 1, pycparser 1, pyopenssl 1, python-magic 1, showcert 1
3.574 DEBUG marker environment resolution took 0.792s
3.574 TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVers
ion { string: "3.13.5", version: "3.13.5" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "5.15.153.1-microsoft-standard-WSL2", platform_system: "Linux", pla
tform_version: "#1 SMP Fri Mar 29 23:14:13 UTC 2024", python_full_version: StringVersion { string: "3.13.5", version: "3.13.5" }, python_version: StringVersion { string: "3.13", version: "3.13" }, sys_platform: "linux" } }) } }
3.574 TRACE Resolution edge: ROOT -> showcert
3.574 TRACE Resolution edge:     0a0.dev0 -> 0.4.4
3.574 TRACE Resolution edge: cryptography -> cffi
3.574 TRACE Resolution edge:     45.0.5 -> 1.17.1 ; platform_python_implementation != 'PyPy'
3.574 TRACE Resolution edge: cffi -> pycparser
3.574 TRACE Resolution edge:     1.17.1 -> 2.22
3.574 TRACE Resolution edge: pyopenssl -> cryptography
3.574 TRACE Resolution edge:     25.1.0 -> 45.0.5
3.574 TRACE Resolution edge: showcert -> certifi
3.574 TRACE Resolution edge:     0.4.4 -> 2025.7.14
3.574 TRACE Resolution edge: showcert -> cryptography
3.574 TRACE Resolution edge:     0.4.4 -> 45.0.5
3.574 TRACE Resolution edge: showcert -> pem
3.574 TRACE Resolution edge:     0.4.4 -> 23.1.0
3.574 TRACE Resolution edge: showcert -> pyopenssl
3.574 TRACE Resolution edge:     0.4.4 -> 25.1.0
3.574 TRACE Resolution edge: showcert -> python-magic
3.574 TRACE Resolution edge:     0.4.4 -> 0.4.27
3.574 TRACE Resolution edge: cffi -> pycparser
3.574 TRACE Resolution edge:     1.17.1 -> 2.22
3.575 Resolved 8 packages in 795ms
3.575 DEBUG Assessing Python executable as base candidate: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
3.575 DEBUG Using base executable for virtual environment: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
3.575 DEBUG Removing existing directory due to `--clear`
3.576 DEBUG Using request timeout of 30s
3.576 DEBUG Identified uncached distribution: showcert==0.4.4
3.576 DEBUG Identified uncached distribution: pem==23.1.0
3.576 DEBUG Identified uncached distribution: pycparser==2.22
3.576 DEBUG Identified uncached distribution: python-magic==0.4.27
3.576 DEBUG Identified uncached distribution: pyopenssl==25.1.0
3.576 DEBUG Identified uncached distribution: certifi==2025.7.14
3.576 DEBUG Identified uncached distribution: cryptography==45.0.5
3.576 DEBUG Identified uncached distribution: cffi==1.17.1
3.577 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/cryptography/45.0.5-cp311-abi3-manylinux_2_34_x86_64.http
3.577 DEBUG No cache entry for: https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl
3.577 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl
3.577 TRACE Handling request for https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl with authentication policy auto
3.577 TRACE Request for https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl is unauthenticated, checking cache
3.577 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl
3.577 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl     
3.577 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/cffi/1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.http
3.577 DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
3.577 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
3.577 TRACE Handling request for https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl with authentication policy auto
3.577 TRACE Request for https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl is unauthenticated, checking cache
3.577 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
3.577 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
3.577 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/certifi/2025.7.14-py3-none-any.http
3.577 DEBUG No cache entry for: https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl
3.577 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl
3.577 TRACE Handling request for https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl with authentication policy auto
3.577 TRACE Request for https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl is unauthenticated, checking cache
3.577 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl
3.577 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl
3.577 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/pycparser/2.22-py3-none-any.http
3.577 DEBUG No cache entry for: https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl
3.577 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl
3.577 TRACE Handling request for https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl with authentication policy auto
3.577 TRACE Request for https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl is unauthenticated, checking cache
3.577 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl
3.577 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl
3.577 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/pyopenssl/25.1.0-py3-none-any.http
3.577 DEBUG No cache entry for: https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl
3.577 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl
3.577 TRACE Handling request for https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl with authentication policy auto
3.577 TRACE Request for https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl is unauthenticated, checking cache
3.577 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl
3.577 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl
3.577 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/showcert/0.4.4-py3-none-any.http
3.577 DEBUG No cache entry for: https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl
3.577 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl
3.577 TRACE Handling request for https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl with authentication policy auto
3.577 TRACE Request for https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl is unauthenticated, checking cache
3.577 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl
3.577 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl
3.578 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/python-magic/0.4.27-py2.py3-none-any.http
3.578 DEBUG No cache entry for: https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl
3.578 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl
3.578 TRACE Handling request for https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl with authentication policy auto       
3.578 TRACE Request for https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl is unauthenticated, checking cache
3.578 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl
3.578 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl
3.579 TRACE No cache entry exists for /tmp/.tmpBXR6qN/wheels-v5/pypi/pem/23.1.0-py3-none-any.http
3.579 DEBUG No cache entry for: https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl
3.579 TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl
3.579 TRACE Handling request for https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl with authentication policy auto
3.579 TRACE Request for https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl is unauthenticated, checking cache
3.579 TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl
3.579 TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl
3.630 TRACE Cached request https://files.pythonhosted.org/packages/6c/73/9f872cb81fc5c3bb48f7227872c28975f998f3e7c2b1c16e95e6432bbb90/python_magic-0.4.27-py2.py3-none-any.whl is storable because its response has a 'public' cache-control directive
3.630 TRACE Cached request https://files.pythonhosted.org/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl is storable because its response has a 'public' cache-control directive
3.648 TRACE Cached request https://files.pythonhosted.org/packages/80/28/2659c02301b9500751f8d42f9a6632e1508aa5120de5e43042b8b30f8d5d/pyopenssl-25.1.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
3.649 TRACE Cached request https://files.pythonhosted.org/packages/4f/52/34c6cf5bb9285074dc3531c437b3919e825d976fde097a7a73f79e726d03/certifi-2025.7.14-py3-none-any.whl is storable because its response has a 'public' cache-control directive
3.678 TRACE Cached request https://files.pythonhosted.org/packages/c9/97/8299a481ae6c08494b5d53511e6a4746775d8a354c685c69d8796b2ed482/pem-23.1.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
3.681 TRACE Cached request https://files.pythonhosted.org/packages/26/9f/1aab65a6c0db35f43c4d1b4f580e8df53914310afc10ae0397d29d697af4/cffi-1.17.1-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl is storable because its response has a 'public' cache-control directive
3.688 TRACE Cached request https://files.pythonhosted.org/packages/c9/d8/0749f7d39f53f8258e5c18a93131919ac465ee1f9dccaf1b3f420235e0b5/cryptography-45.0.5-cp311-abi3-manylinux_2_34_x86_64.whl is storable because its response has a 'public' cache-control directive
3.688 Downloading cryptography (4.2MiB)
3.688 TRACE Cached request https://files.pythonhosted.org/packages/29/22/cbba6758244c4fad42fdccd92e399d7fcbbba88744519840a0b7148f2e9b/showcert-0.4.4-py3-none-any.whl is storable because its response has a 'public' cache-control directive
3.940  Downloading cryptography
3.940 Prepared 8 packages in 363ms
3.967 TRACE Extracting file name=PackageName("python-magic")
3.967 TRACE Extracting file name=PackageName("pem")
3.967 TRACE Extracting file name=PackageName("pyopenssl")
3.967 TRACE Extracting file name=PackageName("certifi")
3.967 TRACE Extracting file name=PackageName("pycparser")
3.967 TRACE Extracting file name=PackageName("showcert")
3.967 TRACE Extracting file name=PackageName("cffi")
3.967 TRACE Extracting file name=PackageName("cryptography")
3.970 TRACE Extracted 10 files name=PackageName("python-magic")
3.970 TRACE No entrypoints name=PackageName("python-magic")
3.972 TRACE Extracted 9 files name=PackageName("pem")
3.972 TRACE No entrypoints name=PackageName("pem")
3.972 TRACE No data name=PackageName("python-magic")
3.972 TRACE Writing installer metadata name=PackageName("python-magic")
3.972 TRACE Writing record name=PackageName("python-magic")
3.972 TRACE Extracted 13 files name=PackageName("pyopenssl")
3.972 TRACE Extracted 16 files name=PackageName("showcert")
3.972 TRACE No data name=PackageName("pem")
3.972 TRACE Writing installer metadata name=PackageName("pem")
3.972 TRACE No entrypoints name=PackageName("pyopenssl")
3.972 TRACE No data name=PackageName("pyopenssl")
3.972 TRACE Writing installer metadata name=PackageName("pyopenssl")
3.973 TRACE Extracted 10 files name=PackageName("certifi")
3.973 TRACE Writing record name=PackageName("pyopenssl")
3.973 TRACE No entrypoints name=PackageName("certifi")
3.973 TRACE Writing record name=PackageName("pem")
3.973 TRACE No data name=PackageName("certifi")
3.973 TRACE Writing installer metadata name=PackageName("certifi")
3.973 TRACE Writing record name=PackageName("certifi")
3.973 TRACE Extracted 23 files name=PackageName("pycparser")
3.973 TRACE No entrypoints name=PackageName("pycparser")
3.973 TRACE No data name=PackageName("pycparser")
3.973 TRACE Writing installer metadata name=PackageName("pycparser")
3.973 TRACE Writing record name=PackageName("pycparser")
3.974 TRACE Writing entrypoints name=PackageName("showcert")
3.974 TRACE No data name=PackageName("showcert")
3.974 TRACE Writing installer metadata name=PackageName("showcert")
3.974 TRACE Extracted 29 files name=PackageName("cffi")
3.974 TRACE Writing record name=PackageName("showcert")
3.974 TRACE No entrypoints name=PackageName("cffi")
3.974 TRACE No data name=PackageName("cffi")
3.974 TRACE Writing installer metadata name=PackageName("cffi")
3.975 TRACE Writing record name=PackageName("cffi")
3.983 TRACE Extracted 104 files name=PackageName("cryptography")
3.983 TRACE No entrypoints name=PackageName("cryptography")
3.983 TRACE No data name=PackageName("cryptography")
3.983 TRACE Writing installer metadata name=PackageName("cryptography")
3.983 TRACE Writing record name=PackageName("cryptography")
3.983 Installed 8 packages in 43ms
3.983  + certifi==2025.7.14
3.983  + cffi==1.17.1
3.983  + cryptography==45.0.5
3.983  + pem==23.1.0
3.983  + pycparser==2.22
3.983  + pyopenssl==25.1.0
3.983  + python-magic==0.4.27
3.983  + showcert==0.4.4
3.984 DEBUG Checking for Python environment at: `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX`
3.984 TRACE Querying interpreter executable at /tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/python3
4.095 DEBUG Creating ephemeral environment at: `/tmp/.tmpBXR6qN/builds-v0/.tmpRIqxV1`
4.095 DEBUG Assessing Python executable as base candidate: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
4.095 DEBUG Using base executable for virtual environment: /home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13
4.095 DEBUG Removing existing directory due to `--clear`
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/activate.bat`: does not start with expected shebang
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/activate.csh`: does not start with expected shebang
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/activate.ps1`: does not start with expected shebang
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/activate`: does not start with expected shebang
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/deactivate.bat`: does not start with expected shebang
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/activate.fish`: does not start with expected shebang
4.095 TRACE Updated entrypoint at /tmp/.tmpBXR6qN/builds-v0/.tmpRIqxV1/bin/showcert
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/activate_this.py`: does not start with expected shebang
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/pydoc.bat`: does not start with expected shebang
4.095 TRACE Skipping copy of entrypoint `/tmp/.tmpBXR6qN/archive-v0/_JLZA9Iwco9TnneZiSOhX/bin/activate.nu`: does not start with expected shebang
4.095 TRACE Updated entrypoint at /tmp/.tmpBXR6qN/builds-v0/.tmpRIqxV1/bin/gencert
4.095 TRACE Skipping copy of entrypoint `.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/pip3.13`: does not start with expected shebang
4.118 error: failed to read from file `/home/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/bin/python3.13`: stream did not contain valid UTF-8


---

_Comment by @zanieb on 2025-07-22 18:30_

Thanks! I can reproduce. #14790 is at fault here.

---

_Closed by @zanieb on 2025-07-22 19:11_

---
