---
number: 9744
title: "uv add chromadb fails, error Compiling with an SDK that doesn't seem to exist /Applications/Xcode"
type: issue
state: closed
author: bodadotsh
labels:
  - bug
assignees: []
created_at: 2024-12-09T18:57:23Z
updated_at: 2025-06-11T12:07:06Z
url: https://github.com/astral-sh/uv/issues/9744
synced_at: 2026-01-10T01:24:45Z
---

# uv add chromadb fails, error Compiling with an SDK that doesn't seem to exist /Applications/Xcode

---

_Issue opened by @bodadotsh on 2024-12-09 18:57_

Hi, when I tried to run `uv add chromadb` on macOS, it fails and returns error:

```
esolved 91 packages in 261ms
  × Failed to download and build `chroma-hnswlib==0.7.6`
  ╰─▶ Build backend failed to build wheel through `build_wheel` (exit status: 1)
 37 warnings generated.
      Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      clang++: warning: no such sysroot directory: '/Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk' [-Wmissing-sysroot]
      ld: warning: search path 'Modules/_hacl' not found
      ld: warning: search path '/install/lib' not found
      ld: library 'c++' not found
      clang++: error: linker command failed with exit code 1 (use -v to see invocation)
      error: command '/usr/bin/g++' failed with exit code 1
```

When I looked for solutions, some suggested to install `xcode` from App Store but I'm on a work laptop, and doesn't have iCloud logged in. I tried to install `xcode` with `xcode-select --install` from the terminal. Also tried `CC=$(which gcc) CXX=$(which g++) uv add chromadb` and no luck.

Software versions
- `uv 0.5.5 (95cd8b8b3 2024-11-27)`
- `macOS Sequoia 15.1.1 arm64`
- `chroma-hnswlib v0.7.6`
- `chromadb v0.5.23`
- `python 3.13`

---

_Comment by @zanieb on 2024-12-09 18:58_

I think we have a fix for this upstream

ref https://github.com/indygreg/python-build-standalone/pull/414

---

_Label `bug` added by @zanieb on 2024-12-09 18:58_

---

_Comment by @charliermarsh on 2024-12-09 19:08_

You can also try unsetting `CFLAGS`? That might work.

---

_Comment by @bodadotsh on 2024-12-09 20:18_

> You can also try unsetting `CFLAGS`? That might work.

Got a different error

```
  × Failed to download and build `chroma-hnswlib==0.7.6`
  ╰─▶ Build backend failed to build wheel through `build_wheel` (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_ext
      clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix
      -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC -Werror=unguarded-availability-new -I/opt/homebrew/opt/llvm/include
      -I/Users/boda/.cache/uv/builds-v0/.tmpsIu7i0/include -I/Users/boda/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/include/python3.13 -c /var/folders/4q/xj6y7_fn4_xd7c6zglq850jm0000gn/T/tmp477pvn97.cpp -o
      var/folders/4q/xj6y7_fn4_xd7c6zglq850jm0000gn/T/tmp477pvn97.o -std=c++14
      clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix
      -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC -Werror=unguarded-availability-new -I/opt/homebrew/opt/llvm/include
      -I/Users/boda/.cache/uv/builds-v0/.tmpsIu7i0/include -I/Users/boda/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/include/python3.13 -c /var/folders/4q/xj6y7_fn4_xd7c6zglq850jm0000gn/T/tmp4vrqq8p_.cpp -o
      var/folders/4q/xj6y7_fn4_xd7c6zglq850jm0000gn/T/tmp4vrqq8p_.o -fvisibility=hidden
      building 'hnswlib' extension
      clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix
      -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -fPIC -Werror=unguarded-availability-new -I/opt/homebrew/opt/llvm/include
      -I/Users/boda/.cache/uv/builds-v0/.tmpsIu7i0/lib/python3.13/site-packages/pybind11/include -I/Users/boda/.cache/uv/builds-v0/.tmpsIu7i0/lib/python3.13/site-packages/numpy/_core/include
      -I./hnswlib/ -I/Users/boda/.cache/uv/builds-v0/.tmpsIu7i0/include -I/Users/boda/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/include/python3.13 -c ./python_bindings/bindings.cpp -o
      build/temp.macosx-11.0-arm64-cpython-313/./python_bindings/bindings.o -O3 -stdlib=libc++ -mmacosx-version-min=10.7 -DVERSION_INFO=\"0.7.6\" -std=c++14 -fvisibility=hidden

      [stderr]
      Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      clang++: warning: no such sysroot directory: '/Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk' [-Wmissing-sysroot]
      Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      clang++: warning: no such sysroot directory: '/Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk' [-Wmissing-sysroot]
      Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      clang++: warning: no such sysroot directory: '/Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk' [-Wmissing-sysroot]
      In file included from ./python_bindings/bindings.cpp:1:
      In file included from /opt/homebrew/Cellar/llvm/19.1.5/bin/../include/c++/v1/iostream:42:
      In file included from /opt/homebrew/Cellar/llvm/19.1.5/bin/../include/c++/v1/ios:220:
      In file included from /opt/homebrew/Cellar/llvm/19.1.5/bin/../include/c++/v1/__locale:14:
      /opt/homebrew/Cellar/llvm/19.1.5/bin/../include/c++/v1/__locale_dir/locale_base_api.h:29:12: fatal error: 'xlocale.h' file not found
         29 | #  include <xlocale.h>
            |            ^~~~~~~~~~~
      1 error generated.
      error: command '/opt/homebrew/opt/llvm/bin/clang++' failed with exit code 1

      hint: This error likely indicates that you need to install a library that provides "xlocale.h" for `chroma-hnswlib@0.7.6`
```

---

_Comment by @charliermarsh on 2024-12-09 20:26_

Anyway, that looks like ~the same problem that https://github.com/indygreg/python-build-standalone/pull/414 would resolve.

---

_Comment by @bodadotsh on 2024-12-09 21:49_

Just found that `chromadb` doesn't support python 3.13 yet so downgraded python to 3.11 `uv python pin 3.11`, and everything works now. 

Thanks both for the help.

---

_Closed by @bodadotsh on 2024-12-09 21:49_

---

_Referenced in [astral-sh/python-build-standalone#414](../../astral-sh/python-build-standalone/pulls/414.md) on 2024-12-10 13:27_

---

_Comment by @hhamud on 2025-01-08 23:46_

if anyone had the same problem with hnswlib failing to build, just updating uv and reinstalling the python version that used it fixed it. 

---

_Referenced in [GoogleCloudPlatform/agent-starter-pack#23](../../GoogleCloudPlatform/agent-starter-pack/pulls/23.md) on 2025-03-12 11:07_

---

_Comment by @aliraza108 on 2025-04-25 23:46_

also get same error

---

_Comment by @juliancoffee on 2025-06-10 22:07_

Just hit this with `PyICU`.
As it seems to still be a problem for people, yes, try to reinstall the `uv` Python.
Alternatively, check out `uv python list` and if you see something suspicious, just uninstall everything from `uv`.
I only had `python3.13.1` and everything else installed from `brew`.
After I removed `python3.13.1`, `uv` switched to system python, and it just worked.

---

_Comment by @konstin on 2025-06-11 12:07_

@aliraza108 @juliancoffee Please open a new issue with a minimal reproduction and the logs attached.

---

_Referenced in [langflow-ai/langflow#8224](../../langflow-ai/langflow/issues/8224.md) on 2025-06-12 12:16_

---
