---
number: 1699
title: Installing ruff via pip on NixOs does not work
type: issue
state: closed
author: heitorPB
labels:
  - bug
assignees: []
created_at: 2023-01-06T21:41:30Z
updated_at: 2025-05-29T18:01:35Z
url: https://github.com/astral-sh/ruff/issues/1699
synced_at: 2026-01-07T13:12:14-06:00
---

# Installing ruff via pip on NixOs does not work

---

_Issue opened by @heitorPB on 2023-01-06 21:41_

Installing ruff via `pip` on NixOs systems does not work:

```bash
$ cd /tmp
$ python3 -m venv venv
$ source venv/bin/activate
(venv)
$ pip install ruff
Collecting ruff
  Using cached ruff-0.0.212-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (4.3 MB)
Installing collected packages: ruff
Successfully installed ruff-0.0.212

[notice] A new release of pip available: 22.2.2 -> 22.3.1
[notice] To update, run: pip install --upgrade pip
(venv)
$ ruff
-bash: /tmp/venv/bin/ruff: No such file or directory
(venv)

$ file /tmp/venv/bin/ruff
/tmp/venv/bin/ruff: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=1ec33df614fc5020de42d625366e4684ae45a0b6, for GNU/Linux 2.6.32, stripped
(venv)
```

The dynamic loader ("ELF interpreter") is not in the path hardcoded in the binary, although `ldd` shows the right path:

```bash
$ ldd /tmp/venv/bin/ruff
        linux-vdso.so.1 (0x00007f8623e51000)
        libgcc_s.so.1 => /nix/store/hsk71z8admvgykn7vzjy11dfnar9f4r1-glibc-2.35-163/lib/libgcc_s.so.1 (0x00007f86233eb000)
        librt.so.1 => /nix/store/hsk71z8admvgykn7vzjy11dfnar9f4r1-glibc-2.35-163/lib/librt.so.1 (0x00007f86233e6000)
        libpthread.so.0 => /nix/store/hsk71z8admvgykn7vzjy11dfnar9f4r1-glibc-2.35-163/lib/libpthread.so.0 (0x00007f86233e1000)
        libm.so.6 => /nix/store/hsk71z8admvgykn7vzjy11dfnar9f4r1-glibc-2.35-163/lib/libm.so.6 (0x00007f8623301000)
        libdl.so.2 => /nix/store/hsk71z8admvgykn7vzjy11dfnar9f4r1-glibc-2.35-163/lib/libdl.so.2 (0x00007f86232fc000)
        libc.so.6 => /nix/store/hsk71z8admvgykn7vzjy11dfnar9f4r1-glibc-2.35-163/lib/libc.so.6 (0x00007f86230f1000)
        /lib64/ld-linux-x86-64.so.2 => /nix/store/hsk71z8admvgykn7vzjy11dfnar9f4r1-glibc-2.35-163/lib64/ld-linux-x86-64.so.2 (0x00007f8623e53000)
(venv)
```

I was able to get it working using `patchelf`, but it is not a good workflow when using a Python package manager (e.g. hatch, pdm, poetry, etc) to handle all the dev dependencies of the project.

---

_Label `bug` added by @charliermarsh on 2023-01-06 22:03_

---

_Comment by @messense on 2023-01-07 02:29_

Can you run it with `LD_DEBUG=libs,files` env var to see what's going on?

```bash
LD_DEBUG=libs,files /tmp/venv/bin/ruff
```

---

_Comment by @messense on 2023-01-07 02:35_

https://nixos.wiki/wiki/Packaging/Binaries

> Downloading and attempting to run a binary on NixOS will almost never work. This is due to hard-coded paths in the executable.

https://nixos.wiki/wiki/Packaging/Binaries#The_Dynamic_Loader

I think it's by design in NixOS? You should probably look into packaging `ruff` in [nixpkgs](https://github.com/NixOS/nixpkgs).

Edit: Looks like it's already packaged: https://search.nixos.org/packages?channel=unstable&show=ruff&from=0&size=50&sort=relevance&type=packages&query=ruff

---

_Comment by @charliermarsh on 2023-01-08 03:18_

Given the above, going to close for now.

---

_Closed by @charliermarsh on 2023-01-08 03:18_

---

_Comment by @heitorPB on 2023-01-09 15:16_

@messense,
> Can you run it with `LD_DEBUG=libs,files` env var to see what's going on?

The binary does not run, due to the different path of the Dynamic Loader.

@charliermarsh,

`ruff` does work if I install it on my machine via the OS package manager, but that is not a solution. The issue is that many Python projects use `ruff` as a linter defined in `pyproject.toml`/`requirements.txt`, and that gets installed via pip/pdm/hatch/poetry/etc in a local environment (a.k.a. _virtual env_). And then running the linter/test/format scripts simply does not work.

As an example:
```bash
$ git clone git@github.com:tiangolo/fastapi.git
$ cd fastapi/
$ python -m venv env
$ source ./env/bin/activate
$ pip install -e ."[dev,doc,test]"
$ bash ./scripts/format.sh
+ ruff fastapi tests docs_src scripts --fix
scripts/format.sh: line 4: /home/h/projects/fastapi/venv/bin/ruff: No such file or directory
+ black fastapi tests docs_src scripts
All done! ✨ 🍰 ✨
804 files left unchanged.
+ isort fastapi tests docs_src scripts
```

As you can see, `ruff` is just erroring out :(

In some cases, we can install `ruff` via the OS package manager, but some Python projects "embed" it, or use/require/pin a specific version.

---

_Comment by @messense on 2023-01-09 15:34_

IMHO since you choose NixOS you should embrace [their rules](https://nixos.wiki/wiki/Python), it is known that `pip install` native binaries and extension modules may not work on NixOS, there are some existing issues in nixpkgs:

* https://github.com/NixOS/nixpkgs/issues/142383
* https://github.com/NixOS/nixpkgs/issues/10597

Ideally it should be fixed in NixOS, for now as a workaround I think you can install `ruff` from sdist instead

```bash
pip install --no-binary :all: ruff
```

That said, `ruff` may be able to provide fully static musl libc binaries but it might run much slower than the current glibc ones.

---

_Comment by @heitorPB on 2023-01-09 17:12_

> for now as a workaround I think you can install `ruff` from sdist instead
> 
> ```shell
> pip install --no-binary :all: ruff
> ```

Unfortunately, that did not work: I don't have rust compilers. I could install cargo/rustc/etc, but that does not provide a "work out of the box" experience. `pip install flake8` just works and I was expecting that for `ruff`.

> That said, `ruff` may be able to provide fully static musl libc binaries but it might run much slower than the current glibc ones.

Is this on the roadmap? Would it be possible to have an "alternate" installation for `ruff` that uses musl statically linked instead? For example: `pip install ruff[musl]`?

---

_Comment by @messense on 2023-01-10 02:03_

> `pip install flake8` just works and I was expecting that for `ruff`.

`flake8` is a pure Python wheel so it's very different than `ruff` which is a compiled Rust binary.

> Is this on the roadmap?

That's a question for @charliermarsh, IMO the default should be glibc version unless we can prove that musl libc version is on par with its performance.


> Would it be possible to have an "alternate" installation for `ruff` that uses musl statically linked instead? For example: `pip install ruff[musl]`?

`pip install ruff[musl]` won't work because `[musl]` is for specifying extra dependencies. A new `ruff-musl`/`ruff-static` pip package could work.

Maybe `ruff` eventual provides musl libc binaries that works for you but there are more similar pip binary projects that won't, so IMHO the best solution is to ask/[sponsor](https://opencollective.com/nixos) nixpkgs to fix https://github.com/NixOS/nixpkgs/issues/142383.

---

_Referenced in [astral-sh/ruff-pre-commit#22](../../astral-sh/ruff-pre-commit/issues/22.md) on 2023-01-21 22:50_

---

_Referenced in [NixOS/nixpkgs#142383](../../NixOS/nixpkgs/issues/142383.md) on 2023-03-23 20:02_

---

_Referenced in [pennersr/django-allauth#3291](../../pennersr/django-allauth/pulls/3291.md) on 2023-03-31 14:21_

---

_Comment by @akx on 2023-04-03 13:54_

I guess a musl-linked ruff could be shipped in a [`musllinux`](https://peps.python.org/pep-0656/) tagged wheel... `musllinux` seems to be supported by `cibuildwheel`, for what it's worth.

---

_Referenced in [matrix-org/synapse#15939](../../matrix-org/synapse/issues/15939.md) on 2023-07-14 10:10_

---

_Referenced in [element-hq/synapse#15939](../../element-hq/synapse/issues/15939.md) on 2023-12-22 00:13_

---

_Comment by @andrew-isakov on 2024-03-17 10:43_

`nix-shell -p ruff` is work
installed by `configuration.nix` or `pip install` dont work  (nix-ld error)

---

_Referenced in [pennersr/django-allauth#3768](../../pennersr/django-allauth/pulls/3768.md) on 2024-05-02 07:06_

---

_Comment by @snowdrop4 on 2024-06-04 19:44_

> I was able to get it working using patchelf

@heitorPB how did you achieve that?

---

_Comment by @heitorPB on 2024-06-04 22:41_

@snowdrop4 something like `patchelf path/to/bin/ruff`. See [Packaging/Binaries](https://nixos.wiki/wiki/Packaging/Binaries).

But I recommend to set the env var `PIP_NO_BINARY=ruff` and have `cargo` and `rustc` available to compile Ruff instaled via Pip. I use this Flake for this:

```nix
{
  description = "A Python flake";

  inputs = {
    nixpkgs.url = "nixpkgs/nixos-unstable";
    flake-utils.url = "github:numtide/flake-utils";
  };

  outputs = { self, nixpkgs, flake-utils }:
    flake-utils.lib.eachSystem [ "x86_64-linux" ] (system:
      let
        pkgs = import nixpkgs { inherit system; };
      in
      {
        # For nix develop
        devShell = pkgs.mkShell {
          shellHook = ''
            export PIP_NO_BINARY="ruff"
          '';

          packages = with pkgs; [
            (python311.withPackages (p: with p; [
              pip
              python-lsp-server
              python-lsp-ruff
            ]))

            rustc
            cargo
          ];
        };
      });
}
```

---

_Comment by @snowdrop4 on 2024-06-05 12:29_

Thank you!

---

_Referenced in [astral-sh/ruff#12216](../../astral-sh/ruff/issues/12216.md) on 2024-07-06 14:40_

---

_Comment by @vanchaxy on 2024-10-21 18:21_

The best way I found to make it work is using nix-ld.

Just add these two lines to your configuration, rebuild, reboot and you are all set.

```
programs.nix-ld.enable = true;
programs.nix-ld.libraries = options.programs.nix-ld.libraries.default;
```

---

_Comment by @anadon on 2025-04-21 21:40_

What about this?
```Python
def handle_nixos() -> List[str]:
    result = subprocess.run(["nix", "help", "run", ">", "/dev/null"]
    return ["ruff"] if result.returncode else ["nix", "run", "nixpkgs#ruff", "--"]
```
  

---

_Comment by @akx on 2025-04-22 10:44_

@anadon `if shutil.which("nix")` would do the trick to figure out whether `nix` is available at all, if that's the idea...

---

_Referenced in [copier-org/copier#2100](../../copier-org/copier/pulls/2100.md) on 2025-04-24 12:28_

---

_Referenced in [gradbench/gradbench#574](../../gradbench/gradbench/pulls/574.md) on 2025-05-16 11:52_

---

_Comment by @andrew-isakov on 2025-05-29 18:01_

открыл для себя devbox :)

---
