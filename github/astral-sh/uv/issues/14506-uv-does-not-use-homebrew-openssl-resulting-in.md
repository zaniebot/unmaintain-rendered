---
number: 14506
title: "uv does not use Homebrew OpenSSL, resulting in \"fatal error: 'openssl/crypto.h' file not found\""
type: issue
state: open
author: slhck
labels:
  - documentation
  - error messages
assignees: []
created_at: 2025-07-08T12:56:31Z
updated_at: 2025-07-09T18:55:46Z
url: https://github.com/astral-sh/uv/issues/14506
synced_at: 2026-01-07T13:12:18-06:00
---

# uv does not use Homebrew OpenSSL, resulting in "fatal error: 'openssl/crypto.h' file not found"

---

_Issue opened by @slhck on 2025-07-08 12:56_

### Summary

Perhaps this is not a bug, but I am trying to install a package under macOS:

```
➜ uv add XXXXXXX
Resolved 72 packages in 1.47s
  × Failed to build `securestring==0.2`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_ext
      building 'SecureString' extension
      clang -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -I/Users/werner/.cache/uv/builds-v0/.tmpkkcKgq/include -I/Users/werner/.pyenv/versions/3.13.2/include/python3.13 -c pysecstr.c -o
      build/temp.macosx-15.4-arm64-cpython-313/pysecstr.o

      [stderr]
      pysecstr.c:4:10: fatal error: 'openssl/crypto.h' file not found
          4 | #include <openssl/crypto.h>
            |          ^~~~~~~~~~~~~~~~~~
      1 error generated.
      error: command '/usr/bin/clang' failed with exit code 1

      hint: This error likely indicates that you need to install a library that provides "openssl/crypto.h" for `securestring@0.2`
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

That is despite having `openssl` installed through Homebrew:

```
➜ brew info openssl   
==> openssl@3: stable 3.5.1 (bottled)
Cryptography and SSL/TLS Toolkit
https://openssl-library.org
Installed
/opt/homebrew/Cellar/openssl@3/3.5.1 (7,563 files, 35.4MB) *
  Poured from bottle using the formulae.brew.sh API on 2025-07-08 at 14:40:50
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/o/openssl@3.rb
License: Apache-2.0
==> Dependencies
Required: ca-certificates ✔
==> Caveats
A CA file has been bootstrapped using certificates from the system
keychain. To add additional certificates, place .pem files in
  /opt/homebrew/etc/openssl@3/certs

and run
  /opt/homebrew/opt/openssl@3/bin/c_rehash
==> Analytics
install: 415,091 (30 days), 1,255,991 (90 days), 4,962,357 (365 days)
install-on-request: 64,517 (30 days), 198,141 (90 days), 699,819 (365 days)
build-error: 13,147 (30 days)
```

I would have _expected_ `uv` to just use that for installing the package.

For example, I am used to installing Python via `pyenv`, where you just install [the build dependencies](https://github.com/pyenv/pyenv/wiki#suggested-build-environment), and then you can use `pyenv install` to compile a Python version linked to Homebrew's OpenSSL.

Once I set the compiler flags manually, it works:

```bash
$ export CFLAGS="-I/opt/homebrew/opt/openssl@3/include"
$ export LDFLAGS="-L/opt/homebrew/opt/openssl@3/lib"
$ export CPPFLAGS="-I/opt/homebrew/opt/openssl@3/include"
$ export PKG_CONFIG_PATH="/opt/homebrew/opt/openssl@3/lib/pkgconfig"
$ uv add XXXXXXXXX
Resolved 72 packages in 1.92s
      Built securestring==0.2
      Built pyspark==3.1.1
Prepared 2 packages in 7.95s
Uninstalled 2 packages in 100ms
Installed 28 packages in 217ms
...
```

I am posting this because I could not find any related issues here when searching for `openssl` and Homebrew.

### Platform

macOS 15

### Version

uv 0.7.14 (Homebrew 2025-06-23)

### Python version

3.13

---

_Label `bug` added by @slhck on 2025-07-08 12:56_

---

_Comment by @zanieb on 2025-07-09 12:14_

Yeah, unlike pyenv we don't link to the system OpenSSL in our Python distributions so package builds will not automatically detect it. I don't think we can do anything to improve this, unfortunately.

---

_Comment by @slhck on 2025-07-09 12:16_

I see. Could this be addressed somewhere in the documentation?
For example, [here it says](https://docs.astral.sh/uv/reference/troubleshooting/build-failures/#header-or-library-is-missing) that apt-installing a package would be enough to fix a build error with a missing header file. Apparently this is not the case here.

---

_Comment by @zanieb on 2025-07-09 12:24_

Discovery of package headers is... complicated. I think it's actually uncommon for build systems to be able to discover things installed with Homebrew without taking the sort of action you've done there, whereas it's more common for installed packages to be globally discoverable on Linux.

We can add a note to the managed Python distribution documentation though, yes. We could also probably adjust our build failure hint here for macOS.

---

_Label `bug` removed by @zanieb on 2025-07-09 12:24_

---

_Label `error messages` added by @zanieb on 2025-07-09 12:24_

---

_Label `documentation` added by @zanieb on 2025-07-09 12:24_

---

_Comment by @slhck on 2025-07-09 14:18_

Yeah, sounds great, I think a nudge in the error would have been helpful.

(Just dropping this here: `uv` is awesome and has, despite that error, tremendously helped in getting dependencies working and switching between Python versions for a legacy project!)

---

_Comment by @geofft on 2025-07-09 18:23_

So the usual answer to this is that most libraries that have native code have precompiled wheels. `securestring` doesn't, and having taken a look at it, I would like to encourage you not to use it.

[The package](https://github.com/dnet/pysecstr) is very small and provides a single function:

```c
static PyObject* SecureString_clearmem(PyObject *self, PyObject *args) {
	char *buffer;
	Py_ssize_t length;

	if(!PyArg_ParseTuple(args, "s#", &buffer, &length)) {
		return NULL;
	}
	OPENSSL_cleanse(buffer, length);
	return Py_BuildValue("");
}
```

There's a couple of problems with this.

First, [the `s#` format gets you a `const char *`, not a `char *`](https://docs.python.org/3/c-api/arg.html). You're not supposed to modify it, and the behavior on the Python side if you modify an object like `bytes` or `str` that is immutable is undefined. In practice, Python `str` objects can be stored in multiple encodings _simultaneously_, and this code will modify at most one of them:

```
$ uv run --with securestring python
Python 3.13.2 (main, Feb 12 2025, 14:38:11) [GCC 6.3.0 20170516] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import SecureString
>>> string_stored_1byte = "Hello"
>>> SecureString.clearmem(string_stored_1byte)
>>> string_stored_1byte
'\x00\x00\x00\x00\x00'
>>> string_stored_2byte = "\uffff\uffff"
>>> SecureString.clearmem(string_stored_2byte)
>>> string_stored_2byte
'\uffff\uffff'
```

Because `string_stored_2byte` contains Unicode characters that don't fit in a one-byte encoding, Python primarily stores it in a two-byte encoding, and makes a cached UTF-8 copy for code that asks for it, such as `securestring`... which securely clears the cache but not the original string. :(

Also, [`OPENSSL_cleanse` is implemented](https://github.com/openssl/openssl/blob/openssl-3.5.1/crypto/mem_clr.c) as just a call to `memset(ptr, 0, len)` (through a volatile function pointer to attempt to prevent it from getting optimized out). At the time `securestring` was written, it did something more complicated with writing various patterns through assembly, because it was believed that it theoretically helped erase something from disk in unusual circumstances. It no longer does that, so there's not really any point in using OpenSSL's wrapper over `memset`.

Finally, given that this is Python, you have to be very careful that you're not getting extra copies of the string around. For instance, if you're reading a file from disk, you have to use `readinto()` on a raw unbuffered I/O object (`open(...buffering=False)`) or Python 3.14's `os.readinto()`, or it will make extra copies, and `securestring` can't guarantee that there aren't unwanted copies somewhere else. (Both `readinto()` and `readinto1()` on a buffered I/O object will make extra copies.) The `OPENSSL_cleanse` API is designed for C users who are being very careful about memory management and probably doing clever things to avoid unwanted copies in other ways through coredumps etc.

So, if you're in pure Python, as far as I can tell, you can do as good a job as `securestring` can do by just setting bytes in a writable buffer to zero yourself using normal Python assignment (something like `b[:] = bytearray(len(b))` should work), which also has the benefit of forcing you to actually use a writable byte buffer in the first place (`bytearray`, `array.array`, `mmap.mmap`, etc.), not an immutable `bytes` or `str`. Alternatively, you may want to move a little more of the handling of the sensitive data into C or some other compiled language (and calling `memset` yourself instead of using `OPENSSL_cleanse`).

---

Anyway, to the actual issue: I suspect when you ran `brew install openssl` you got a message saying something like

```
openssl@3 is keg-only, which means it was not symlinked into /opt/homebrew,
because macOS provides LibreSSL.
 
If you need to have openssl@3 first in your PATH, run:
  echo 'export PATH="/opt/homebrew/opt/openssl@3/bin:$PATH"' >> ~/.zshrc
 
For compilers to find openssl@3 you may need to set:
  export LDFLAGS="-L/opt/homebrew/opt/openssl@3/lib"
  export CPPFLAGS="-I/opt/homebrew/opt/openssl@3/include"
```

This advice is accurate.  (Although it would be entirely understandable if this message displayed long ago and you totally forgot it ever showed up. :) )

If I'm understanding right, I would almost say this is a bug in pyenv :) though it is something of a happy accident. I think what's going on is that
1) pyenv specifically knows about Homebrew and how to find Homebrew's OpenSSL, even though it is not on the default compiler search paths: https://github.com/pyenv/pyenv/blob/v2.6.4/plugins/python-build/bin/python-build#L1687-L1708
2) The build process for Python stores the build flags that were used for Python itself, so that extension modules can use the same flags. Specifically I think that if you run `python3 -m sysconfig` with a pyenv interpreter, it will show paths to Homebrew's OpenSSL.
3) pyenv doesn't distribute binaries that it has built in advance, it builds Python on your machine, so those paths are valid and useful when you're compiling extension modules.

We want to be able to distribute pre-built Python binaries so that it installs quickly and reliably. We also don't have a dependency on Homebrew; we want to be able to run on machines that don't have Homebrew installed at all. So there's nothing that will cause package builds under uv to pick up libraries installed from Homebrew (or Macports, or Nix, or whatever else) that are not on the default compiler search paths.

Honestly, item 2 is a little weird—it's pyenv's decision to go and find OpenSSL for its own build in a certain way, which is fine, but Python then causes those flags to apply to all package builds, too. (Because we build Python binaries in CI we actually have to go out of our way to work around this behavior so that compiler flags that exist in CI _don't_ show up when building packages on local systems. If you do `uv run --managed-python python -m sysconfig` you'll probably see a few lingering harmless references to paths on our CI infrastructure that don't exist on your machine.)

Apart from docs/error message improvements, in theory we could expose our own internal build of OpenSSL for downstream packages to use. That's a bit complicated for technical reasons at the moment, and it would also be a little more of a commitment to a generally-usable OpenSSL than we do at the moment—for instance, what happens across version upgrades? What if a package needs an older or newer version of OpenSSL? Because pyenv builds against whatever's on your system, those issues don't come up for it.

(Another theoretical answer is that, just like pyenv has special logic to go find Homebrew OpenSSL, securestring should also have this logic in its own build process. Then it won't depend on what the Python interpreter is telling it about its own build. But then there's the stuff above about whether you should use this library at all.)

---

_Comment by @slhck on 2025-07-09 18:55_

Thanks for the detailed information! I am actually just trying to make a (somewhat outdated) code base work that I have no control over, so while I appreciate your notes regarding the `securestring` package, unfortunately there is nothing I can do about its use…

-------

Regarding Homebrew, I am aware of these caveats (I wrote some myself for other Homebrew formulae), and I did actually check it, but it does not show up when you do a `brew info`:

```
➜ brew info openssl
==> openssl@3: stable 3.5.1 (bottled)
Cryptography and SSL/TLS Toolkit
https://openssl-library.org
Installed
/opt/homebrew/Cellar/openssl@3/3.5.1 (7,563 files, 35.4MB) *
  Poured from bottle using the formulae.brew.sh API on 2025-07-08 at 14:40:50
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/o/openssl@3.rb
License: Apache-2.0
==> Dependencies
Required: ca-certificates ✔
==> Caveats
A CA file has been bootstrapped using certificates from the system
keychain. To add additional certificates, place .pem files in
  /opt/homebrew/etc/openssl@3/certs

and run
  /opt/homebrew/opt/openssl@3/bin/c_rehash
==> Analytics
install: 425,241 (30 days), 1,263,613 (90 days), 4,967,033 (365 days)
install-on-request: 66,802 (30 days), 200,328 (90 days), 701,856 (365 days)
build-error: 13,995 (30 days)
```

I wonder where you got that post-install message from? It's [not there in the formula](https://github.com/Homebrew/homebrew-core/blob/main/Formula/o/openssl%403.rb#L125).

I guess the `use_homebrew_ssl` function is a "quality of life" hack that I, as an end user, appreciate a lot. But I completely understand how your build process is different.

I guess what I'm saying is, a simple note in the FAQ/Troubleshooting should help a lot.

---
