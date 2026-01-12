```yaml
number: 8426
title: "[BUILD] Windows: linking with `x86_64-w64-mingw32-gcc` failed: exit code: 1"
type: issue
state: closed
author: T-256
labels: []
assignees: []
created_at: 2023-11-01T22:33:26Z
updated_at: 2024-08-03T06:43:13Z
url: https://github.com/astral-sh/ruff/issues/8426
synced_at: 2026-01-12T15:54:48Z
```

# [BUILD] Windows: linking with `x86_64-w64-mingw32-gcc` failed: exit code: 1

---

_@T-256_

Rust: 1.73
Ruff: 1.3 (main branch cloned)

Full log:
[err.zip](https://github.com/astral-sh/ruff/files/13232328/err.zip)

I also used mingw64 for compile `ring` crate.
```cmd
E:\crates\ruff>gcc --version
gcc (Rev2, Built by MSYS2 project) 12.1.0
Copyright (C) 2022 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```



---

_Comment by @zanieb on 2023-11-01 22:37_

Maybe helpful https://github.com/rust-lang/rust/issues/91146

---

_Comment by @T-256 on 2023-11-01 23:25_

Thank you @zanieb 
I was using mingw compiled with msvcrt, which it seems was not compatible.
Using UCRT version solved the problem

---

_Closed by @T-256 on 2023-11-01 23:25_

---

_Comment by @zzy444626905 on 2024-08-03 06:43_

$ pacman -Q
base 2022.06-1
bash 5.2.026-1
bash-completion 2.14.0-1
binutils 2.42-1
brotli 1.1.0-1
bsdtar 3.7.4-1
bzip2 1.0.8-4
ca-certificates 20240203-2
coreutils 8.32-5
curl 8.9.0-1
dash 0.5.12-1
db 6.2.32-5
diffutils 3.10-1
file 5.45-3
filesystem 2023.02.07-2
findutils 4.10.0-2
gawk 5.3.0-1
gcc 13.3.0-1
gcc-libs 13.3.0-1
gdbm 1.24-1
getent 2.18.90-4
gettext 0.22.4-1
gmp 6.3.0-1
gnupg 2.4.5-1
grep 1~3.0-6
gzip 1.13-1
heimdal-libs 7.8.0-5
inetutils 2.5-2
info 7.1-3
isl 0.26-1
less 661-1
libargp 20110921-4
libasprintf 0.22.4-1
libassuan 2.5.7-1
libbz2 1.0.8-4
libcurl 8.9.0-1
libdb 6.2.32-5
libedit 20240517_3.1-1
libexpat 2.6.2-1
libffi 3.4.6-1
libgcrypt 1.11.0-1
libgdbm 1.24-1
libgettextpo 0.22.4-1
libgnutls 3.8.6-1
libgpg-error 1.50-1
libhogweed 3.10-1
libiconv 1.17-1
libidn2 2.3.7-1
libintl 0.22.4-1
libksba 1.6.7-1
liblz4 1.10.0-1
liblzma 5.6.2-1
libnettle 3.10-1
libnghttp2 1.62.1-1
libnpth 1.7-1
libopenssl 3.3.1-1
libp11-kit 0.25.5-2
libpcre 8.45-4
libpcre2_8 10.44-1
libpsl 0.21.5-2
libreadline 8.2.010-1
libsqlite 3.46.0-2
libssh2 1.11.0-1
libtasn1 4.19.0-1
libunistring 1.2-1
libutil-linux 2.35.2-4
libxcrypt 4.4.36-1
libzstd 1.5.6-1
llvm 11.0.0-5
m4 1.4.19-2
make 4.4.1-2
mingw-w64-x86_64-binutils 2.42-2
mingw-w64-x86_64-crt-git 12.0.0.r81.g90abf784a-1
mingw-w64-x86_64-gcc 14.1.0-3
mingw-w64-x86_64-gcc-libs 14.1.0-3
mingw-w64-x86_64-gettext-runtime 0.22.5-2
mingw-w64-x86_64-gmp 6.3.0-2
mingw-w64-x86_64-headers-git 12.0.0.r81.g90abf784a-1
mingw-w64-x86_64-isl 0.26-1
mingw-w64-x86_64-libiconv 1.17-4
mingw-w64-x86_64-libwinpthread-git 12.0.0.r81.g90abf784a-1
mingw-w64-x86_64-mpc 1.3.1-2
mingw-w64-x86_64-mpfr 4.2.1-2
mingw-w64-x86_64-windows-default-manifest 6.4-4
mingw-w64-x86_64-winpthreads-git 12.0.0.r81.g90abf784a-1
mingw-w64-x86_64-zlib 1.3.1-1
mingw-w64-x86_64-zstd 1.5.6-2
mintty 1~3.7.4-1
mpc 1.3.1-1
mpfr 4.2.1-1
msys2-keyring 1~20240410-2
msys2-launcher 1.5-1
msys2-runtime 3.5.3-5
msys2-runtime-devel 3.5.3-5
msys2-w32api-headers 12.0.0.r0.g819a6ec2e-1
msys2-w32api-runtime 12.0.0.r0.g819a6ec2e-1
nano 8.1-1
ncurses 6.5-1
nettle 3.10-1
openssl 3.3.1-1
p11-kit 0.25.5-2
pacman 6.1.0-4
pacman-contrib 1.10.6-1
pacman-mirrors 20240523-1
perl 5.38.2-2
pinentry 1.3.1-1
rebase 4.5.0-4
sed 4.9-1
tar 1.35-2
time 1.9-3
tzcode 2024a-1
util-linux 2.35.2-4
wget 1.24.5-2
which 2.21-4
windows-default-manifest 6.4-2
xz 5.6.2-1
zlib 1.3.1-1
zstd 1.5.6-1

i don't understand, what happened

---
