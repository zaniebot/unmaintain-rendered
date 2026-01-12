```yaml
number: 8635
title: "Can't use uv docker image as a command-line tool"
type: issue
state: open
author: mjpieters
labels:
  - bug
assignees: []
created_at: 2024-10-28T13:31:10Z
updated_at: 2025-10-18T17:59:49Z
url: https://github.com/astral-sh/uv/issues/8635
synced_at: 2026-01-12T15:59:30Z
```

# Can't use uv docker image as a command-line tool

---

_@mjpieters_

Given that the documentation suggests that you [_can_ use the docker container as a command-line tool](https://docs.astral.sh/uv/guides/integration/docker/#running-uv-in-a-container) I found it very surprising that you can't, in fact, use `uv` in this manner:

```sh
$ uv lock  # ensure there is an up-to-date lockfile
docker run --rm \
      --mount type=bind,source="$(pwd)",destination=/app --mount destination=/app/.venv \         
      ghcr.io/astral-sh/uv:latest \
           --directory /app -vv tree
    0.000473s DEBUG uv uv 0.4.27
    0.001380s DEBUG uv_workspace::workspace Found workspace root: `/app`
    0.001392s DEBUG uv_workspace::workspace Adding current workspace member: `/app`
    0.001800s DEBUG uv_python::discovery Searching for Python >=3.12 in managed installations or system path
    0.001841s DEBUG uv_python::discovery Searching for managed installations at `/.local/share/uv/python`
error: Could not read ELF interpreter from any of the following paths: /bin/sh, /usr/bin/env, /bin/dash, /bin/ls
```

The `tree` command should not require a Python interpreter (see #8634), but the `ELF` error here is especially confusing.

---

_Comment by @zanieb on 2024-10-28 13:36_

We are attempting to sniff the platform to determine which managed installations are relevant. We might be able to fix that, I'm not quite sure how though.

You probably want one of the images that isn't distroless.

---

_Label `bug` added by @zanieb on 2024-10-28 13:37_

---

_Comment by @zanieb on 2024-10-28 13:37_

cc @konstin should we set an environment variable in the distroless images for glibc? (or something else?) 

---

_Comment by @konstin on 2024-10-28 16:14_

We can't reliably sniff the glibc version from inside the container, at least if we want to also provision Python. What about something like `-e UV_LIBC="$(/lib/ld-linux.so.2 --version)"`/`-e UV_LIBC="$(/lib/ld-musl-x86_64.so.1 --version)"` (is there a platform independent one?) for docker where you pass in the context and we parse our the right libc kind and version?

---

_Comment by @mjpieters on 2024-10-28 17:01_

Note that on MacOS, with Docker Desktop, docker itself runs inside a virtualised linux emulator, and so you can't trivially access `/lib/ld-linux.so.2` or `/lib/ld-musl-x86_64.so.1` from the host OS, at least not trivially.

---

_Comment by @konstin on 2024-10-28 18:52_

Does that mean you want to use the docker uv with a host mac os python, not another linux Python?

---

_Comment by @zanieb on 2024-10-29 13:53_

> docker run --rm -it ghcr.io/astral-sh/uv:latest /bin/sh
> If this command fails, it means that the image itself may have problems.

That will always fail, and not because our image has problems — the image is distroless and only contains the uv binary.

---

_Comment by @samypr100 on 2024-10-30 02:02_

> You probably want one of the images that isn't distroless.

^this


---

_Comment by @mjpieters on 2024-11-06 10:57_

> Does that mean you want to use the docker uv with a host mac os python, not another linux Python?

Exactly. But, we'll use `uv` locally, instead.

(To be exact: `uv` is used to manage venvs inside docker containers, but for certain operations having `uv` run locally is just much much nicer, given how fast the tool is).

---

_Comment by @codethief on 2025-03-27 22:27_

For other people (like me) who ended up here through their favorite search engine and need some context: The error message

```
> error: Could not read ELF interpreter from any of the following paths: /bin/sh, /usr/bin/env, /bin/dash, /bin/ls
``` 

is defined [here](https://github.com/astral-sh/uv/blob/6a13e4ceddf983e55b12e0bf5d0c354301e9e809/crates/uv-python/src/libc.rs#L37) and [is emitted when](https://github.com/astral-sh/uv/blob/6a13e4ceddf983e55b12e0bf5d0c354301e9e809/crates/uv-python/src/libc.rs#L205) the path to the linker / `ld` cannot be determined from looking at the binaries `"/bin/sh", "/usr/bin/env", "/bin/dash", "/bin/ls"`. This might happen when the binaries are not present (e.g. when building a Docker image from scratch or using a distroless image, see above) or when they are statically linked, e.g.:

```
$ file /bin/sh
> /bin/sh: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, BuildID[sha1]=96ee037a361464bb7e73dd6bcf0e37e63dc85dd6, not stripped
```

(This is what I ran into – I'm on NixOS.)

One (likely temporary) workaround can be to not have uv install Python in the first place and install it through some other means, e.g.

```
export UV_NO_MANAGED_PYTHON=true
export UV_SYSTEM_PYTHON=true
uv sync  # Won't try to find ld now
```


---

_Label `error messages` added by @zanieb on 2025-03-28 02:55_

---

_Label `help wanted` added by @zanieb on 2025-03-28 02:55_

---

_Comment by @zanieb on 2025-03-28 02:56_

We should improve this error message, at least.

cc @geofft on what our behavior should be on NixOS

---

_Comment by @declension on 2025-03-31 15:44_

I'm running into a similar problem within a small Nix derivation (itself in a Flake), one that calls `uv lock --locked`. The workaround from @codethief doesn't help (explanation is useful though, thanks!); still it wants to probe those files and I can't find a workaround currently.


---

_Comment by @codethief on 2025-04-02 10:02_

@declension Even with my workaround from above (i.e. using a system-installed Python instance) I ended up running into issues when using uv to install certain pip packages that include binaries. In the end, I just enabled [nix-ld](https://github.com/nix-community/nix-ld) in my system config to provide both uv and the packages it installs with a somewhat standard environment to run in and so far that seems to be doing the trick. Maybe that could help you, too?

---

_Comment by @declension on 2025-04-04 08:17_

Thanks @codethief. I'm not on NixOS though so not sure if / how that'd help me.
Either way, I hacked a workaround [to run `uv lock --locked` as required] via [`buildFHSUserEnv`](https://ryantm.github.io/nixpkgs/builders/special/fhs-environments/) that seems to be a step forwards (has its own issues now though). I'm guess it could be improved too:

```nix
let
  customUv = pkgs.buildFHSUserEnv {
    name = "uv";
    targetPkgs = pkgs: [pkgs.uv pkgs.python3];
    runScript = "uv";
  };
in
  pkgs.runCommandLocal "uv-lock" {
    inherit src;
    nativeBuildInputs = [customUv];
  } ''
    cd ${./.}
    ${customUv}/bin/uv --help  # or whatever
    exit 1  # to debug output
  '';
```

---

_Comment by @oconnor663 on 2025-05-29 17:40_

https://github.com/astral-sh/uv/commit/08f2fa48ff5af4dfd422cfa0ccc8b36499bbf149 doesn't change the behavior here, but it at least improves the error message:

```
error: Failed to discover managed Python installations
  Caused by: Failed to determine the libc used on the current platform
  Caused by: Failed to find any common binaries to determine libc from: /bin/sh, /usr/bin/env, /bin/dash, /bin/ls
```

---

_Label `help wanted` removed by @konstin on 2025-05-30 08:39_

---

_Label `error messages` removed by @konstin on 2025-05-30 08:39_

---

_Comment by @jmesterh on 2025-10-18 17:59_

Just ran into this as we are being asked to use [chainguard](https://www.chainguard.dev/) images as part of regulatory compliance. 

As mentioned above, fix was to add this to the final stage of our Dockerfile:
```
ENV UV_NO_MANAGED_PYTHON=true \
        UV_SYSTEM_PYTHON=true
```



---
