```yaml
number: 4450
title: Venvs from uv and from native python works a little different on nix
type: issue
state: open
author: Rubikoid
labels:
  - wish
  - compatibility
  - virtualenv
assignees: []
created_at: 2024-06-22T19:41:15Z
updated_at: 2025-07-22T01:08:04Z
url: https://github.com/astral-sh/uv/issues/4450
synced_at: 2026-01-12T15:58:50Z
```

# Venvs from uv and from native python works a little different on nix

---

_@Rubikoid_

# About issue

uv platform: MacOS Sonoma 14.3.1
uv version: `uv 0.2.13`

`sys_prefix` on uv-created venvs points to the wrong place on nix-managed python.

I'm using Nix for python installation, so I have kinda two variations of python:
- `/nix/store/blahA-python3-3.12.3/` with pure python without any packages
- `/nix/store/blahB-python3-3.12.3-env/` with partially symlinks to `/nix/store/blahA-python3-3.12.3/` and have packages installed.

From the user view, all symlinks to python goes to `*-env` thing.

When creating venvs with uv they have wrong `sys_prefix` set, so it is impossible to install any package in read-only python site-packages.

# Reproducing

uv thinks that my `*-env` python is not "system" (which it quite right) so when I specify just `-p $version` to uv, it founds nothing
```sh
-> % uv venv uv-venv -vv -p 3.12
    0.002848s DEBUG uv_toolchain::discovery Searching for Python 3.12 in search path
    0.005586s DEBUG uv_toolchain::discovery Found CPython 3.12.3 at `/Users/rubikoid/.nix-profile/bin/python3.12` (search path)
    0.005749s DEBUG uv_toolchain::discovery Ignoring Python interpreter at `/nix/store/blahB-python3-3.12.3-env/bin/python3.12`: system interpreter required
    0.077956s DEBUG uv_toolchain::discovery Found CPython 3.12.3 at `/Users/rubikoid/.nix-profile/bin/python3` (search path)
    0.077974s DEBUG uv_toolchain::discovery Ignoring Python interpreter at `/nix/store/blahB-python3-3.12.3-env/bin/python3.12`: system interpreter required
    0.109480s DEBUG uv_toolchain::discovery Found CPython 3.12.3 at `/Users/rubikoid/.nix-profile/bin/python` (search path)
    0.109498s DEBUG uv_toolchain::discovery Ignoring Python interpreter at `/nix/store/blahB-python3-3.12.3-env/bin/python3.12`: system interpreter required
    0.306166s DEBUG uv_toolchain::discovery Found CPython 3.9.6 at `/usr/bin/python3` (search path)
  × No interpreter found for Python 3.12 in search path
```
Because of it i have to specify direct path to python.

## Creating venvs with vu

So for the first i create venv with `*-env` python:
```sh
-> % uv venv uv-venv -vv -p /nix/store/blahB-python3-3.12.3-env/bin/python3.12
    0.003919s DEBUG uv_toolchain::discovery Checking for Python interpreter at path `/nix/store/blahB-python3-3.12.3-env/bin/python3.12`
Using Python 3.12.3 interpreter at: /nix/store/blahB-python3-3.12.3-env/bin/python3.12
Creating virtualenv at: uv-venv
Activate with: source uv-venv/bin/activate
```

And then venv from "original" python:
```sh
-> % uv venv uv-venv-2 -vv -p /nix/store/blahA-python3-3.12.3/bin/python3.12
    0.003196s DEBUG uv_toolchain::discovery Checking for Python interpreter in directory `/nix/store/blahA-python3-3.12.3`
Using Python 3.12.3 interpreter at: /nix/store/blahA-python3-3.12.3/bin/python3
Creating virtualenv at: uv-venv-2
Activate with: source uv-venv-2/bin/activate
```

## Creating venvs with python venv module

And then the same thing, but with the native venv module:
```sh
/nix/store/blahB-python3-3.12.3-env/bin/python3.12 -m venv py-venv
/nix/store/blahA-python3-3.12.3/bin/python3.12 -m venv py-venv-2
```

## Checking results

Everything run at `uv/crates/uv-toolchain`:

### uv

uv venv created from `*-env` python:
```sh
-> % ~/test/uv-venv/bin/python3.12 -m python.get_interpreter_info | jq '. | {sys_prefix: .sys_prefix, sys_base_prefix: .sys_base_prefix, sys_path: .sys_path}'
{
  "sys_prefix": "/nix/store/blahB-python3-3.12.3-env",
  "sys_base_prefix": "/nix/store/blahA-python3-3.12.3",
  "sys_path": [
    "/Users/rubikoid/projects/git/uv/crates/uv-toolchain",
    "/nix/store/blahA-python3-3.12.3/lib/python312.zip",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12/lib-dynload",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12/site-packages",
    "/nix/store/blahB-python3-3.12.3-env/lib/python3.12/site-packages"
  ]
}
```

uv venv created from fresh python:
```sh
-> % ~/test/uv-venv-2/bin/python3.12 -m python.get_interpreter_info | jq '. | {sys_prefix: .sys_prefix, sys_base_prefix: .sys_base_prefix, sys_path: .sys_path}'
{
  "sys_prefix": "/Users/rubikoid/test/uv-venv-2",
  "sys_base_prefix": "/nix/store/blahA-python3-3.12.3",
  "sys_path": [
    "/Users/rubikoid/projects/git/uv/crates/uv-toolchain",
    "/nix/store/blahA-python3-3.12.3/lib/python312.zip",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12/lib-dynload",
    "/Users/rubikoid/test/uv-venv-2/lib/python3.12/site-packages"
  ]
}
```

### Native python

Native python venv, created from `*-env` python:
```sh
-> % ~/test/py-venv/bin/python3.12 -m python.get_interpreter_info | jq '. | {sys_prefix: .sys_prefix, sys_base_prefix: .sys_base_prefix, sys_path: .sys_path}'
{
  "sys_prefix": "/Users/rubikoid/test/py-venv",
  "sys_base_prefix": "/nix/store/blahA-python3-3.12.3",
  "sys_path": [
    "/Users/rubikoid/projects/git/uv/crates/uv-toolchain",
    "/nix/store/blahA-python3-3.12.3/lib/python312.zip",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12/lib-dynload",
    "/Users/rubikoid/test/py-venv/lib/python3.12/site-packages"
  ]
}
```

Native python venv, created from fresh python:
```sh
-> % ~/test/py-venv-2/bin/python3.12 -m python.get_interpreter_info | jq '. | {sys_prefix: .sys_prefix, sys_base_prefix: .sys_base_prefix, sys_path: .sys_path}'
{
  "sys_prefix": "/Users/rubikoid/test/py-venv-2",
  "sys_base_prefix": "/nix/store/65ackbgqn02p6fy75rksjbp17zj6440j-python3-3.12.3",
  "sys_path": [
    "/Users/rubikoid/projects/git/uv/crates/uv-toolchain",
    "/nix/store/blahA-python3-3.12.3/lib/python312.zip",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12",
    "/nix/store/blahA-python3-3.12.3/lib/python3.12/lib-dynload",
    "/Users/rubikoid/test/py-venv-2/lib/python3.12/site-packages"
  ]
}
```

## Result

`sys_prefix` for the first created venv different from others. 

Impossible to install package:

```sh
-> % uv pip install -vv -p ~/test/uv-venv uv
 uv_requirements::specification::from_source source=uv
    0.005069s DEBUG uv_toolchain::discovery Checking for Python interpreter in directory `/Users/rubikoid/test/uv-venv`
    0.007050s DEBUG uv::commands::pip::install Using Python 3.12.3 environment at /nix/store/blahB-python3-3.12.3-env/bin/python3.12
error: failed to create file `/nix/store/blahB-python3-3.12.3-env/.lock`
  Caused by: Permission denied (os error 13)
```

Maybe this is more a nix than uv issue, if so - feel free to say it and close this.

I don't find anything about patching venvs on nix, only `site` [customisation](https://github.com/NixOS/nixpkgs/blob/master/pkgs/development/interpreters/python/sitecustomize.py)

---

_Comment by @charliermarsh on 2024-06-23 17:20_

Is this roughly the same as https://github.com/astral-sh/uv/issues/1795 -- that if we resolved the symlink on the interpreter, we would match the behavior you're expecting?

---

_Label `question` added by @charliermarsh on 2024-06-23 17:20_

---

_Label `virtualenv` added by @charliermarsh on 2024-06-23 17:20_

---

_Label `compatibility` added by @charliermarsh on 2024-06-23 17:20_

---

_Comment by @Rubikoid on 2024-06-23 21:36_

> that if we resolved the symlink on the interpreter, we would match the behavior you're expecting

I'm not sure.

`/nix/store/blahB-python3-3.12.3-env/bin/python3.12` is not a symlink, in fact it is a wrapper, which updates few env vars (such as `NIX_PYTHONPREFIX`, they are used further in `site` customization thing) and then execve original `/nix/store/blahA-python3-3.12.3/bin/python3.12`.

Sounds like this more about creating venv from existing venv.

---

_Comment by @kreha1 on 2024-07-05 20:11_

The fact that python from nix store isn't treated as a system interpreter isn't really an issue imo, as we can easily set the UV_PYTHON env dynamically in a derivation.

The real issue is the fact that uv wants to create a `.lock` file in there, but `/nix/store` is read-only, so that won't be possible.
While looking for a workaround, I found a similar issue with flutter https://github.com/NixOS/nix/issues/3217

And with that said, I think we would need to at least be able to set where the lock file is created... unless that defeats the purpose of a lock file, but I don't know if that's a standard for python-related software to be honest...

---

_Comment by @charliermarsh on 2024-07-05 20:27_

We could make the `.lock` stuff fallible (just skip it if we can't write, or fallback to a different location).

---

_Comment by @kreha1 on 2024-07-05 20:39_

Hmm, I think the problem could be elsewhere. I've managed to make `uv pip install` work with newly created venv (via `uv venv`).

So for my setup which uses nix flake, the first fix was to make `uv` always use my selected python version, basically
`export UV_PYTHON=${supportedPython}` which then turns into `export UV_PYTHON=/nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env` when entering a shell.

Since this is a nix flake and not NixOS distro which has the entire /nix/store directory mounted as read-only, I just made that path `sudo chmod +w` and I thought that it would work, but packages still wanted to be installed to the nix store location.

Then I've realized that after I unset the variable, the lock gets created, as I suspect correctly, inside `.venv` directory. That would suggest it's a bug, where setting `UV_PYTHON` variable prevents `uv` from installing packages in a virtual environment.


---

_Comment by @Rubikoid on 2024-07-05 23:04_

> The real issue is the fact that uv wants to create a .lock file in there

I think, I quite disagree there.

.lock problem is kinda a symptom, while wrong pythons selection / strange venvs are disease.

> Then I've realized that after I unset the variable, the lock gets created, as I suspect correctly, inside .venv directory.

That’s interesting. I did not use environment variables at all, only cli args. I’ll recheck this issue using env vars. 

---

_Comment by @kreha1 on 2024-07-05 23:16_

Well, there is an issue with symlink resolution for sure, but I agree 100% with what you are saying. 

After reproducing this issue on a different machine, I realized that I didn't manage to make it work... What worked was actually a different tool (hatch), as since it can be configured to use uv, I mistakenly assumed that it also uses it for venv creation.
So, sorry for giving incorrect info.

I believe that hatch uses `python -m venv` under the hood, hence why it worked (but it's just a guess).

---

_Comment by @kreha1 on 2024-07-05 23:23_

And now I'm really confused. Creating venv via `python -m venv` and `uv venv` will resolve different nix store paths.
### python module
```
$ /nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3 -m venv .venv
$ readlink .venv/bin/python3
/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14/bin/python3
```
### uv venv
```
$ uv venv -p /nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3
Using Python 3.10.14 interpreter at: /nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3.10
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
$ readlink .venv/bin/python
/nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3.10
```

And fun fact, with venv module, `.venv/bin/python` points to `.venv/bin/python3` and for uv venv it's reversed

---

_Comment by @Rubikoid on 2024-07-05 23:42_

> And now I'm really confused. Creating venv via python -m venv and uv venv will resolve different nix store paths.

Yeah -)

This is literally the spirit of this issue)

uv’s venv have python pointing to python wrapper in nix’s python environment, while the native python venv have py pointing to original python binary (and derivation, ofc)

---

_Comment by @charliermarsh on 2024-07-05 23:45_

I just need to find time to setup Nix and play around with the behaviors. It’s a little hard for me to keep track of what’s going wrong from here.

---

_Comment by @darinkishore on 2024-07-06 06:09_

Please correct me if I'm wrong, @Rubikoid @kreha1 , but this is an issue if you're using uv and trying to point to a globally managed nix python installation, right? 

I think this is the case because I currently use a devshell for managing python/uv, and i haven't had a single installation issue. 

Isn't using a devshell the solution to this? I know @kreha1 you said you only got it working through hatch, but I don't think you need hatch, pretty sure you can use uv to install things alongside the python from nixpkgs. 

Here's the devshell `flake.nix` I'm using:

```nix

{
  description = "Python development environment with uv";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    flake-utils.url = "github:numtide/flake-utils";
  };

  outputs = { self, nixpkgs, flake-utils }:
    flake-utils.lib.eachDefaultSystem (system:
      let
        pkgs = import nixpkgs { inherit system; };
      in
      {
        devShells.default = pkgs.mkShell {
          buildInputs = with pkgs; [
            python312
            uv
          ];

          # this runs when we do `nix develop .` 
          shellHook = ''
            # Create a virtual environment if it doesn't exist
            if [ ! -d ".venv" ]; then
              uv venv .venv
            fi
            source .venv/bin/activate
            echo "uv pip env ready"
          '';
        };
      }
    );
}
```

direnv bonus copy paste: `echo "use flake" >> .envrc`

Have installed torch w/cuda support, jupyter, all sorts of things that are usually a massive pain on nixos with a modified version of this flake. 

Could you let me know if this solves or helps the issue, or if I'm just misunderstanding things? 

---

_Comment by @darinkishore on 2024-07-06 06:17_

> I just need to find time to setup Nix and play around with the behaviors. It’s a little hard for me to keep track of what’s going wrong from here.

I started a couple months ago! It's a LOT. 

Fastest way to hit the ground running and have a uv + py nix-managed environment is:

1. install nix via: https://github.com/DeterminateSystems/nix-installer (just run `curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s — install`, if you don't mind curl to sh)

2. make a new directory, copy the code in the previous response into a file called `flake.nix` inside your new dir (remove shellHook if you don't want a venv off the bat)

3. Run `nix develop .` and try `which uv`. To exit this temporary shell, just do `exit`!


4. (optional) if you think you're going to be doing this more often, `zero-to-nix` is a great starting resource for practical, usable nix. [direnv](https://direnv.net/docs/installation.html)  with [nix-direnv](https://github.com/nix-community/nix-direnv) (use the nix-profile install method) makes activating and deactivating the shell an afterthought. finally, [devenv](https://devenv.sh) is a great layer of abstraction over nix, especially for beginners. 

---

_Comment by @kreha1 on 2024-07-06 12:48_

@darinkishore This is exactly what I'm doing, but not on all systems this works.

```nix
{
  description = "A Nix-flake-based Python development environment";

  inputs.nixpkgs.url = "github:nixos/nixpkgs?ref=nixos-unstable";

  outputs = { self, nixpkgs }:
    let
      supportedSystems = [ "x86_64-linux" "aarch64-linux" "x86_64-darwin" "aarch64-darwin" ];
      forEachSupportedSystem = f: nixpkgs.lib.genAttrs supportedSystems (system: f rec {
        pkgs = import nixpkgs { inherit system; };
        supportedPython = pkgs.python310;
      });
    in
    {
      devShells = forEachSupportedSystem ({ pkgs, supportedPython }: {
        default = pkgs.mkShell {
          venvDir = ".venv";
          packages = [
            (supportedPython.withPackages(ps: [ps.uv]))
          ];
          shellHook = ''
            rm -rf .venv && echo " * Deleted previous .venv"
            uv venv -p ${supportedPython} -v
            . .venv/bin/activate
            echo " * Active python $(which python3) resolves to $(realpath $(which python3))"
            uv pip install numpy -v
            echo " * All python versions:"
            which -a python3 | while read path; do echo "$path is $($path -V)"; done
            echo " * System info:"
            lsb_release -a
          '';
        };
      });
    };
}
```
### Ubuntu 20.04 (has only system python 3.8.10 installed)
```
  * Deleted previous .venv
DEBUG uv 0.2.15
DEBUG Checking for Python interpreter in directory `/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14`
DEBUG Checking for Python interpreter at directory `/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14`
Using Python 3.10.14 interpreter at: /nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
 * Active python /home/kreha1/git/nix-python-template/.venv/bin/python3 resolves to /nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14/bin/python3.10
DEBUG uv 0.2.15
DEBUG Searching for Python interpreter in system toolchains
DEBUG Found cpython 3.10.14 at `/home/kreha1/git/nix-python-template/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.10.14 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: numpy
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.10.14
DEBUG Adding direct dependency: numpy*
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (*)
DEBUG Selecting: numpy==2.0.0 (numpy-2.0.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d6/a8/6a2419c40c7b6f7cb4ef52c532c88e55490c4fa92885964757d507adddce/numpy-2.0.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Tried 1 versions: numpy 1
Resolved 1 package in 8ms
DEBUG Requirement already cached: numpy==2.0.0
Installed 1 package in 10ms
 + numpy==2.0.0
 * All python versions:
/home/kreha1/git/nix-python-template/.venv/bin/python3 is Python 3.10.14
/nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3 is Python 3.10.14
/usr/bin/python3 is Python 3.8.10
 * System info:
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.6 LTS
Release:        20.04
Codename:       focal
```
### Ubuntu 22.04 (system python 3.10.12 and pyenv managed versions)
```
 * Deleted previous .venv
DEBUG uv 0.2.15
DEBUG Checking for Python interpreter in directory `/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14`
DEBUG Checking for Python interpreter at directory `/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14`
Using Python 3.10.14 interpreter at: /nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3.10
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
 * Active python /home/kreha1/git/nix-python-template/.venv/bin/python3 resolves to /nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3.10
DEBUG uv 0.2.15
DEBUG Searching for Python interpreter in system toolchains
DEBUG Found cpython 3.10.14 at `/home/kreha1/git/nix-python-template/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.10.14 environment at /nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3.10
error: failed to create file `/nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/.lock`
  Caused by: Permission denied (os error 13)
 * All python versions:
/home/kreha1/git/nix-python-template/.venv/bin/python3 is Python 3.10.14
/nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3 is Python 3.10.14
/home/kreha1/.pyenv/shims/python3 is Python 3.10.13
/usr/bin/python3 is Python 3.10.12
/bin/python3 is Python 3.10.12
 * System info:
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04.4 LTS
Release:        22.04
Codename:       jammy
```
So maybe it because of pyenv? @Rubikoid do you also have pyenv managed python on your system?

But still, without providing explicitly python version, on both system the non-nix version gets selected (pyenv > system version)

I might try to containerize this flake, as to eliminate any noise once I have more time.

---

_Comment by @Rubikoid on 2024-07-06 13:16_

@darinkishore,

> but this is an issue if you're using uv and trying to point to a globally managed nix python installation, right?

Partially. I have globally managed nix python installation _with some additional python packages_, builded using `python.withPackages`.
I add the most used packages to global python environment with nix, and want to manage per-project deps using uv.

Due to how nix work, when you use `python.withPackages`, you end up with two derivations in store: "clean" python and "dirty" python with requested packages and python wrapper, which points to "clean" python.

So my setup is more like this:

```nix
{
  inputs.nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";

  outputs = { self, nixpkgs }:
    let
      supportedSystems = [ "x86_64-linux" "aarch64-darwin" "x86_64-darwin" "aarch64-linux" ];
      forEachSystem = f: nixpkgs.lib.genAttrs supportedSystems (system: f {
        pkgs = import nixpkgs { inherit system; };
      });
    in
    {
      devShells = forEachSystem ({ pkgs }: {
        default = pkgs.mkShell (let 
          python = pkgs.python312;
          pythonWithEnv = (python.withPackages (ps: [ ps.pydantic ]));
        in {
          venvDir = ".venv";
          packages = [
            python
            pythonWithEnv
            pkgs.uv
          ];
          shellHook = ''
            rm -rf .venv && echo " * Deleted previous .venv"
            uv venv -p ${pythonWithEnv} -v
            . .venv/bin/activate
            echo " * Active python $(which python3) resolves to $(realpath $(which python3))"
            uv pip install numpy -v
            echo " * All python versions:"
            which -a python3 | while read path; do echo "$path is $($path -V)"; done
          '';
        });
      });
    };
}
```

With result of:
```
 * Deleted previous .venv
DEBUG Checking for Python interpreter in directory `/nix/store/g5s961289kggv2cah6wh3mc4alyailk7-python3-3.12.3-env`
Using Python 3.12.3 interpreter at: /nix/store/g5s961289kggv2cah6wh3mc4alyailk7-python3-3.12.3-env/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
 * Active python /Users/rubikoid/projects/personal/infra/py-repro/.venv/bin/python3 resolves to /nix/store/g5s961289kggv2cah6wh3mc4alyailk7-python3-3.12.3-env/bin/python3.12
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found CPython 3.12.3 at `/Users/rubikoid/projects/personal/infra/py-repro/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at /nix/store/g5s961289kggv2cah6wh3mc4alyailk7-python3-3.12.3-env/bin/python3.12
error: failed to create file `/nix/store/g5s961289kggv2cah6wh3mc4alyailk7-python3-3.12.3-env/.lock`
  Caused by: Permission denied (os error 13)
 * All python versions:
/Users/rubikoid/projects/personal/infra/py-repro/.venv/bin/python3 is Python 3.12.3
/nix/store/65ackbgqn02p6fy75rksjbp17zj6440j-python3-3.12.3/bin/python3 is Python 3.12.3
/nix/store/g5s961289kggv2cah6wh3mc4alyailk7-python3-3.12.3-env/bin/python3 is Python 3.12.3
/nix/store/327bf08j5b7l9cnzink3g4vp32y5352j-python3-3.11.9/bin/python3 is Python 3.11.9
/Users/rubikoid/.nix-profile/bin/python3 is Python 3.12.3
/usr/bin/python3 is Python 3.9.6
(py-repro) bash-5.2$
```

So here:
`/nix/store/65ackbgqn02p6fy75rksjbp17zj6440j-python3-3.12.3/bin/python3` - is "clean" python
`/nix/store/g5s961289kggv2cah6wh3mc4alyailk7-python3-3.12.3-env/bin/python3` - is "dirty" python env with pydantic, which i created with `python.withPackages`.

Also, if I replace `uv venv -p ${pythonWithEnv} -v` with `uv venv -p ${python} -v`, everything will work perfectly.

@kreha1, no, i don't use `pyenv` at all, only nix's `python.withPackages`.
The reason, why your example on ubuntu succeeded, is because you hardcoded "clean" python in `uv venv -p`, while adding "dirty" python with packages to shell environment

---

_Comment by @kreha1 on 2024-07-06 13:52_

That makes sense, thanks for explaining, as I don't have much experience with nix :).

But that is still odd, that this same flake results in different behaviour on two systems - on 20.04 uv venv chooses clean python as provided, but on 22.04 it will choose dirty python, resulting in writing to read-only directory.

```nix
shellHook = ''
  echo "Clean python is ${supportedPython}"
  which -a python3 | while read path; do echo "$path is $($path -V)"; done
  rm -rf .venv && echo " * Deleted previous .venv"
  uv venv -p ${supportedPython} -v
'';
```

### ubuntu 20.04

```
Clean python is /nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14
/nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3 is Python 3.10.14
/usr/bin/python3 is Python 3.8.10

 * Deleted previous .venv
DEBUG uv 0.2.15
DEBUG Checking for Python interpreter in directory `/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14`
DEBUG Checking for Python interpreter at directory `/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14`
Using Python 3.10.14 interpreter at: /nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14/bin/python3
...
```

### ubuntu 22.04

```
Clean python is /nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14
/nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3 is Python 3.10.14
/home/t.rymkiewicz/.pyenv/shims/python3 is Python 3.10.13
/usr/bin/python3 is Python 3.10.12
/bin/python3 is Python 3.10.12
 * Deleted previous .venv
DEBUG uv 0.2.15
DEBUG Checking for Python interpreter in directory `/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14`
DEBUG Checking for Python interpreter at directory `/nix/store/igfc19nl0zv95wj77sf7nnmkbykb4idj-python3-3.10.14`
Using Python 3.10.14 interpreter at: /nix/store/j6j5mv16lbx32m4qjhj5zsygqy0p5zcf-python3-3.10.14-env/bin/python3.10
...
```

---

_Comment by @akaihola on 2024-09-14 14:56_

I have this problem with all virtualenvs on NixOS, whether created with `uv venv` or `python -m venv`:
```shell
$ source .venv/bin/activate
$ pip install -U pip
error: failed to create file `/nix/store/pnavhjx4pdya95nx2apl2yxz6x46snh2-python3-3.12.4-env/.lock`
  Caused by: Read-only file system (os error 30)
```

I'm working around it by making sure I always
- create my virtualenv using `python -m venv`
- do this before using `uv pip`:
```shell
source .venv/bin/activate
export UV_PYTHON=$VIRTUAL_ENV/bin/python
```

See #7395 for more details.

---

_Comment by @Rubikoid on 2024-09-14 15:46_

So far, I've come to the following bizzare solution for creating working envs: 
```sh
uv venv -p `python3 -c 'import sys; from pathlib import Path; print(str(Path(sys.path[1]).parent.parent))'`
```

This works on `a58bc8ad779655e790115244571758e8de055e3d` rev of nixpkgs

---

_Comment by @ShaddyDC on 2024-09-18 09:19_

Instead of setting all the environment variables in bash, it seems much better to set them in your nix flake or whatever you're using for your environment. It's more robust and precise.
Something like
```nix
UV_PYTHON_PREFERENCE = "only-system";
UV_PYTHON = "${pkgs.python310}";
```

Unfortunately, the uv-installed numpy version isn't nixos compatible (`ImportError: libstdc++.so.6: cannot open shared object file: No such file or directory`).
I was thinking of using `uv sync --no-install-package numpy` and a nix python environment that pulls in numpy with `withPackages`, but that fails because it tries to create the lockfile in the nix store as others have reported.
For now I'm just using distrobox.

---

_Comment by @Rubikoid on 2024-09-18 17:33_

> UV_PYTHON

Yeah, great idea, I did't mind.

> Unfortunately, the uv-installed numpy version isn't nixos compatible

I guess, this can be fixed if (or when) the "using packages from other venv in venv" thing is implemented, which is, i think, core issue here.

---

_Comment by @jakob1379 on 2024-10-12 20:05_

Just to add to the plathora of solutions I usually just have this which haven't caused any issues so far

*EDIT*: tried in Ubuntu, NixOS, and PiOS. 

```nix
{
  inputs = { utils.url = "github:numtide/flake-utils"; };
  outputs = { self, nixpkgs, utils }:
    utils.lib.eachDefaultSystem (system:
      let pkgs = nixpkgs.legacyPackages.${system};
      in {
        devShell = pkgs.mkShell {
          buildInputs = with pkgs; [ uv python312 ];
          shellHook = ''
            export UV_PYTHON=${pkgs.python312}
            uv venv
          '';
        };
      });
}
```

---

_Label `question` removed by @zanieb on 2024-10-21 21:26_

---

_Label `wish` added by @zanieb on 2024-10-21 21:26_

---

_Comment by @purepani on 2024-11-13 15:49_

> Instead of setting all the environment variables in bash, it seems much better to set them in your nix flake or whatever you're using for your environment. It's more robust and precise. Something like
> 
> ```nix
> UV_PYTHON_PREFERENCE = "only-system";
> UV_PYTHON = "${pkgs.python310}";
> ```
> 
> Unfortunately, the uv-installed numpy version isn't nixos compatible (`ImportError: libstdc++.so.6: cannot open shared object file: No such file or directory`). I was thinking of using `uv sync --no-install-package numpy` and a nix python environment that pulls in numpy with `withPackages`, but that fails because it tries to create the lockfile in the nix store as others have reported. For now I'm just using distrobox.


I would like to point out that this is normal in nix and not something that is solvable by UV.
Numpy requires dynamic libraries to be installed, which wheels don't automatically install. The "correct" nix solution would be to expose whatever dynamic libraries are needed for whatever libraries to the Python interpreter when declaring the nix shell, or even better, using something like [dream2nix](https://github.com/nix-community/dream2nix)(which doesn't support UV yet but could) or [uv2nix](https://github.com/adisbladis/uv2nix) where you can declare the dynamic  libraries needed for whatever packages you're using. 





---

_Comment by @Alsiri0n on 2025-02-18 15:22_

uv platform: MacOS Sequoia 15.3.1
uv version: uv 0.6.1

Hello, I'm a little bit of confused behaviour of uv and creating venv. Possible I incorrectly create venv with help uv.
Before uv I'm used to default command for creating venv. And after I checked with help pip the location of my venv
```shell
-> % python3.11 -m venv .pyvenv
-> % source .pyvenv/bin/activate
-> % pip -V
pip 24.0 from /Users/alsiri0n/coding/python/test/.pyvenv/lib/python3.11/site-packages/pip (python 3.11)
```
But if I use uv, I got another path
```shell
-> % uv venv 
Using CPython 3.11.9 interpreter at: /Library/Frameworks/Python.framework/Versions/3.11/bin/python3.11
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
-> % source .venv/bin/activate 
-> % pip -V
pip 25.0.1 from /Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/pip (python 3.11)
```
This path some frustrated me and I don't know how to trust with path.
I didn't find any related topics instead this one. Maybe anyone know community chats or something else) or any related issue.
Maybe it's correct behaviour for pip, but it's differed at default path.

---

_Comment by @zanieb on 2025-02-18 15:31_

@Alsiri0n this issue is for using uv with Nix, which it looks like you're not using. We don't install `pip` in new virtual environments by default, you want `uv venv --seed` to include that. Please read #9452 and open a new issue if you have more questions.

---

_Comment by @Alsiri0n on 2025-02-18 22:41_

Thank you very much! I thought that standard behaviour at the moment of creation virtual environment is to add pip

---

_Comment by @iniw on 2025-02-19 02:08_

For reference, if anyone bumps into this issue, here's a devshell that allows you to mix nix-managed and uv-managed libraries:
```nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixpkgs-unstable";
    flake-utils.url = "github:numtide/flake-utils";
  };

  outputs =
    {
      self,
      nixpkgs,
      flake-utils,
    }:
    flake-utils.lib.eachDefaultSystem (
      system:
      let
        pkgs = import nixpkgs { inherit system; };
        python = pkgs.python3;
        pythonEnv = python.withPackages (p: [
          # Here goes all the libraries that can't be managed by uv because of dynamic linking issues
          # or that you just want to be managed by nix for one reason or another
          p.pandas
        ]);
      in
      {
        devShells.default =
          with pkgs;
          mkShell {
            packages = [
              uv
              python
              pythonEnv
            ];

            shellHook = ''
              export UV_PYTHON_PREFERENCE="only-system";
              export UV_PYTHON=${python}
            '';
          };
      }
    );
}
```
You can even do `uv add pandas` just to add it to the lockfile, which is great for working in projects with non-nix users :)

 _Originally posted in [#11529](https://github.com/astral-sh/uv/issues/11529#issuecomment-2667294194)_

---

_Comment by @lucasew on 2025-07-21 23:18_

Standard NixOS, for not being FHS compliant, often do not run binaries not packaged for it because how the programs do dynamic linking. You can use steam-run or make a FHS wrapper like how it's done in Conda.

I prefer this approach: https://github.com/lucasew/nixcfg/blob/master/nix/nodes/common/nix-ld.nix

It allows even appimages to work out of the box

Edit 1: as it's not NixOS, I'd suggest to just use the UV python directly. Mixing stuff up can cause weird bugs.

---

_Comment by @Rubikoid on 2025-07-22 01:08_

> Standard NixOS, for not being FHS compliant, often do not run binaries not packaged for it because how the programs do dynamic linking.

The problem is not with dynamic linking nor FHS compatibility, the problem in complex python system for package search, which somehow works different on python's venv and uv's venv.

This affects native libraries, but only as a side effect.

> as it's not NixOS, I'd suggest to just use the UV python directly. Mixing stuff up can cause weird bugs.

This is not depends on which distro/nix installation you use.

---
