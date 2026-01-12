```yaml
number: 8588
title: uv pip install compiles for wrong architecture if used in nix shell with rosetta-python
type: issue
state: open
author: dwt
labels:
  - question
assignees: []
created_at: 2024-10-26T08:14:17Z
updated_at: 2024-11-27T10:41:27Z
url: https://github.com/astral-sh/uv/issues/8588
synced_at: 2026-01-12T15:59:30Z
```

# uv pip install compiles for wrong architecture if used in nix shell with rosetta-python

---

_@dwt_

Hi there,

I'm having trouble with a slightly cursed setup. Since we have some binary closed source dependencies (ibm_db... cause... IBM is too small to support ARM? Who knew!).

This means that we have to use an x86 python, even though we work on ARM Notebooks.

The current workaround is to do everything in docker - which works, but sucks.

Trying to get this to work outside of docker, I've tried replicating the setup with nix, which does work reasonably well, but fails in one crucial aspect: If `uv` tries to compile a python package, it always builds for ARM, instead of for the python it's actually working for. Even if it is installed itself in the x86 version.

I have provided this example:

```nix
{
    description = "Reproduction of a build failure of pymssql which compiles arm64 even though it compiles for x86 python";

    inputs.nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    inputs.flake-utils.url = "github:numtide/flake-utils";

    outputs = { self, nixpkgs, flake-utils }:
    with flake-utils.lib; eachSystem allSystems (system:
        let
            pkgs = nixpkgs.legacyPackages.${system};
            rosettaPkgs =
                if pkgs.stdenv.isDarwin && pkgs.stdenv.isAarch64
                then pkgs.pkgsx86_64Darwin
                else pkgs;

        in
        {
            devShells.default = rosettaPkgs.mkShell {
                buildInputs = with rosettaPkgs; [
                    # make setuptoos-scm work with --ignore-environment
                    pkgs.git

                    python312Full

                    # dependencies of mssql
                    freetds
                    krb5
                    openssl

                    uv  # actually does work if the stdenv is x86. But only barely.
                    # `uv pip install -e .` (for pymssql) works,
                    # but `uv pip install --force-reinstall pymssql` still builds the arm version
                    # even with `nix develop --ignore-environment`
                ];

                shellHook = ''
                    # export DYLD_FALLBACK_LIBRARY_PATH="${rosettaPkgs.lib.makeLibraryPath [ rosettaPkgs.gcc14.cc] }:$DYLD_FALLBACK_LIBRARY_PATH"
                    rm -rf venv
                    uv venv --python ${rosettaPkgs.python312Full}/bin/python3  venv
                    source venv/bin/activate
                    uv pip install pymssql
                    python -c "import pymssql"
                '';
            };
        }
    );
}
```

This will most likely just work on anything other than an apple silicon machine, so take care.

To execute create a directory, putt this as the `flake.nix` inside and then execute `nix develop --ignore-environment` (`--ignore-environment` because I suspected that something bleeds in from my base system, but hat does not seem to be the case).

This generates the following output for me:

```shell
~/C/Projekte/nix/uv-repro  4m46s
❯ nix develop --ignore-environment
Using CPython 3.12.6 interpreter at: /nix/store/ibgkv0sx9b09j0p1wlv26hb5wspcyvn8-python3-3.12.6/bin/python3
Creating virtual environment at: venv
Activate with: source venv/bin/activate
Using Python 3.12.6 environment at venv
Resolved 1 package in 2ms
   Built pymssql==2.3.1
Prepared 1 package in 4.88s
Installed 1 package in 0.82ms
 + pymssql==2.3.1
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/dwt/Code/Projekte/nix/uv-repro/venv/lib/python3.12/site-packages/pymssql/__init__.py", line 3, in <module>
    from ._pymssql import *
ImportError: dlopen(/Users/dwt/Code/Projekte/nix/uv-repro/venv/lib/python3.12/site-packages/pymssql/_pymssql.cpython-312-darwin.so, 0x0002): tried: '/Users/dwt/Code/Projekte/nix/uv-repro/venv/lib/python3.12/site-packages/pymssql/_pymssql.cpython-312-darwin.so' (mach-o file, but is an incompatible architecture (have 'arm64', need 'x86_64')), '/System/Volumes/Preboot/Cryptexes/OS/Users/dwt/Code/Projekte/nix/uv-repro/venv/lib/python3.12/site-packages/pymssql/_pymssql.cpython-312-darwin.so' (no such file), '/Users/dwt/Code/Projekte/nix/uv-repro/venv/lib/python3.12/site-packages/pymssql/_pymssql.cpython-312-darwin.so' (mach-o file, but is an incompatible architecture (have 'arm64', need 'x86_64'))
```

Some observations: When I checkout the source for pymmsql and call `uv pip install --editable .` on that, that works and builds for the right architecture. Only 'pip install pymmsql' which pulls the package from pypi seems to be affected.

Does this output make sense for you? Can I provide any additional information?

---

_Comment by @zanieb on 2024-10-26 16:13_

Thanks for the thorough report. I am pretty sure we pull all the platform markers from the target Python interpreter. It's possible the build system `pymssql` uses detects the wrong architecture in a way you haven't isolated?

@konstin do you have any ideas?

> This will most likely just work on anything other than an apple silicon machine, so take care.

Like, I can only reproduce this on an Apple silicon machine?


---

_Comment by @dwt on 2024-10-27 09:27_

> > This will most likely just work on anything other than an apple silicon machine, so take care.
> 
> Like, I can only reproduce this on an Apple silicon machine?

That ist my guess, because (I think) that the bug manifests, because I use Rosetta to use an x86 python on ARM.

Part of what is bugging me, is that this also happens if I use nix isolation mode, which should ensure, that the Mac compiler is not even in the path anymore. But also that the compile works correctly if I invoke 'uv pip install -e .' On the checked out source of pymssql, but fails when uv installs and compiles from pypi?

---

_Comment by @zanieb on 2024-10-27 13:54_

>  But also that the compile works correctly if I invoke 'uv pip install -e .' On the checked out source of pymssql, but fails when uv installs and compiles from pypi?

That's pretty weird. Sorry I don't have any more ideas here. Hopefully someone else from my team will have some insight.

---

_Comment by @konstin on 2024-10-28 08:56_

I'm surprised pymssql is built at all, it has wheels for linux x86_64, linux aarch64 and macOS universal2 (aka a single file with both x86_64 and arm64 code), see https://pypi.org/project/pymssql/#files. Can you share the output of `<path to python> -m pip debug --verbose`? It shows you the tags which should match the tags of a pymssql wheel. Otherwise uv reads what python tells it, can you check and potentially share `<path to python> -m sysconfig`?

---

_Comment by @dwt on 2024-10-31 09:04_

Of course, this is what I see:

<details>

<summary> shell output</summary>

```shell
❯ nix develop --impure --ignore-environment
warning: Git tree '/Users/dwt/Code/Projekte/mkk/api' is dirty
Start app with
CI_COMMIT_SHA= FLASK_DEBUG=1 gunicorn --reload --conf src/gunicorn_conf.py --bind 127.0.0.1:5001 main:app
(nix:nix-shell-env) (venv) Sokrates:api dwt$ which python
bash: which: command not found
(nix:nix-shell-env) (venv) Sokrates:api dwt$ type python
python is /Users/dwt/Code/Projekte/mkk/api/venv/bin/python
<t$ /Users/dwt/Code/Projekte/mkk/api/venv/bin/python -m pip debug --verbose
WARNING: This command is only meant for debugging. Do not use this with automation for parsing and getting these details, since the output and options of this command may change without notice.
pip version: pip 24.2 from /Users/dwt/Code/Projekte/mkk/api/venv/lib/python3.12/site-packages/pip (python 3.12)
sys.version: 3.12.6 (main, Sep  6 2024, 19:03:47) [Clang 16.0.6 ]
sys.executable: /Users/dwt/Code/Projekte/mkk/api/venv/bin/python
sys.getdefaultencoding: utf-8
sys.getfilesystemencoding: utf-8
locale.getpreferredencoding: utf-8
sys.platform: darwin
sys.implementation:
  name: cpython
'cert' config value: Not specified
REQUESTS_CA_BUNDLE: None
CURL_CA_BUNDLE: None
pip._vendor.certifi.where(): /Users/dwt/Code/Projekte/mkk/api/venv/lib/python3.12/site-packages/pip/_vendor/certifi/cacert.pem
pip._vendor.DEBUNDLED: False
vendored library versions:
  CacheControl==0.14.0
  distlib==0.3.8
  distro==1.9.0
  msgpack==1.0.8
  packaging==24.1
  platformdirs==4.2.2
  pyproject-hooks==1.0.0
  requests==2.32.3
  certifi==2024.07.04
  idna==3.7
  urllib3==1.26.18
  rich==13.7.1 (Unable to locate actual module version, using vendor.txt specified version)
  pygments==2.18.0
  typing_extensions==4.12.2 (Unable to locate actual module version, using vendor.txt specified version)
  resolvelib==1.0.1
  setuptools==70.3.0 (Unable to locate actual module version, using vendor.txt specified version)
  tomli==2.0.1
  truststore==0.9.1
Compatible tags: 2931
  cp312-cp312-macosx_15_0_x86_64
  cp312-cp312-macosx_15_0_intel
  cp312-cp312-macosx_15_0_fat64
  cp312-cp312-macosx_15_0_fat32
  cp312-cp312-macosx_15_0_universal2
  cp312-cp312-macosx_15_0_universal
  cp312-cp312-macosx_14_0_x86_64
  cp312-cp312-macosx_14_0_intel
  cp312-cp312-macosx_14_0_fat64
  cp312-cp312-macosx_14_0_fat32
  cp312-cp312-macosx_14_0_universal2
  cp312-cp312-macosx_14_0_universal
  cp312-cp312-macosx_13_0_x86_64
  cp312-cp312-macosx_13_0_intel
  cp312-cp312-macosx_13_0_fat64
  cp312-cp312-macosx_13_0_fat32
  cp312-cp312-macosx_13_0_universal2
  cp312-cp312-macosx_13_0_universal
  cp312-cp312-macosx_12_0_x86_64
  cp312-cp312-macosx_12_0_intel
  cp312-cp312-macosx_12_0_fat64
  cp312-cp312-macosx_12_0_fat32
  cp312-cp312-macosx_12_0_universal2
  cp312-cp312-macosx_12_0_universal
  cp312-cp312-macosx_11_0_x86_64
  cp312-cp312-macosx_11_0_intel
  cp312-cp312-macosx_11_0_fat64
  cp312-cp312-macosx_11_0_fat32
  cp312-cp312-macosx_11_0_universal2
  cp312-cp312-macosx_11_0_universal
  cp312-cp312-macosx_10_16_x86_64
  cp312-cp312-macosx_10_16_intel
  cp312-cp312-macosx_10_16_fat64
  cp312-cp312-macosx_10_16_fat32
  cp312-cp312-macosx_10_16_universal2
  cp312-cp312-macosx_10_16_universal
  cp312-cp312-macosx_10_15_x86_64
  cp312-cp312-macosx_10_15_intel
  cp312-cp312-macosx_10_15_fat64
  cp312-cp312-macosx_10_15_fat32
  cp312-cp312-macosx_10_15_universal2
  cp312-cp312-macosx_10_15_universal
  cp312-cp312-macosx_10_14_x86_64
  cp312-cp312-macosx_10_14_intel
  cp312-cp312-macosx_10_14_fat64
  cp312-cp312-macosx_10_14_fat32
  cp312-cp312-macosx_10_14_universal2
  cp312-cp312-macosx_10_14_universal
  cp312-cp312-macosx_10_13_x86_64
  cp312-cp312-macosx_10_13_intel
  cp312-cp312-macosx_10_13_fat64
  cp312-cp312-macosx_10_13_fat32
  cp312-cp312-macosx_10_13_universal2
  cp312-cp312-macosx_10_13_universal
  cp312-cp312-macosx_10_12_x86_64
  cp312-cp312-macosx_10_12_intel
  cp312-cp312-macosx_10_12_fat64
  cp312-cp312-macosx_10_12_fat32
  cp312-cp312-macosx_10_12_universal2
  cp312-cp312-macosx_10_12_universal
  cp312-cp312-macosx_10_11_x86_64
  cp312-cp312-macosx_10_11_intel
  cp312-cp312-macosx_10_11_fat64
  cp312-cp312-macosx_10_11_fat32
  cp312-cp312-macosx_10_11_universal2
  cp312-cp312-macosx_10_11_universal
  cp312-cp312-macosx_10_10_x86_64
  cp312-cp312-macosx_10_10_intel
  cp312-cp312-macosx_10_10_fat64
  cp312-cp312-macosx_10_10_fat32
  cp312-cp312-macosx_10_10_universal2
  cp312-cp312-macosx_10_10_universal
  cp312-cp312-macosx_10_9_x86_64
  cp312-cp312-macosx_10_9_intel
  cp312-cp312-macosx_10_9_fat64
  cp312-cp312-macosx_10_9_fat32
  cp312-cp312-macosx_10_9_universal2
  cp312-cp312-macosx_10_9_universal
  cp312-cp312-macosx_10_8_x86_64
  cp312-cp312-macosx_10_8_intel
  cp312-cp312-macosx_10_8_fat64
  cp312-cp312-macosx_10_8_fat32
  cp312-cp312-macosx_10_8_universal2
  cp312-cp312-macosx_10_8_universal
  cp312-cp312-macosx_10_7_x86_64
  cp312-cp312-macosx_10_7_intel
  cp312-cp312-macosx_10_7_fat64
  cp312-cp312-macosx_10_7_fat32
  cp312-cp312-macosx_10_7_universal2
  cp312-cp312-macosx_10_7_universal
  cp312-cp312-macosx_10_6_x86_64
  cp312-cp312-macosx_10_6_intel
  cp312-cp312-macosx_10_6_fat64
  cp312-cp312-macosx_10_6_fat32
  cp312-cp312-macosx_10_6_universal2
  cp312-cp312-macosx_10_6_universal
  cp312-cp312-macosx_10_5_x86_64
  cp312-cp312-macosx_10_5_intel
  cp312-cp312-macosx_10_5_fat64
  cp312-cp312-macosx_10_5_fat32
  cp312-cp312-macosx_10_5_universal2
  cp312-cp312-macosx_10_5_universal
  cp312-cp312-macosx_10_4_x86_64
  cp312-cp312-macosx_10_4_intel
  cp312-cp312-macosx_10_4_fat64
  cp312-cp312-macosx_10_4_fat32
  cp312-cp312-macosx_10_4_universal2
  cp312-cp312-macosx_10_4_universal
  cp312-abi3-macosx_15_0_x86_64
  cp312-abi3-macosx_15_0_intel
  cp312-abi3-macosx_15_0_fat64
  cp312-abi3-macosx_15_0_fat32
  cp312-abi3-macosx_15_0_universal2
  cp312-abi3-macosx_15_0_universal
  cp312-abi3-macosx_14_0_x86_64
  cp312-abi3-macosx_14_0_intel
  cp312-abi3-macosx_14_0_fat64
  cp312-abi3-macosx_14_0_fat32
  cp312-abi3-macosx_14_0_universal2
  cp312-abi3-macosx_14_0_universal
  cp312-abi3-macosx_13_0_x86_64
  cp312-abi3-macosx_13_0_intel
  cp312-abi3-macosx_13_0_fat64
  cp312-abi3-macosx_13_0_fat32
  cp312-abi3-macosx_13_0_universal2
  cp312-abi3-macosx_13_0_universal
  cp312-abi3-macosx_12_0_x86_64
  cp312-abi3-macosx_12_0_intel
  cp312-abi3-macosx_12_0_fat64
  cp312-abi3-macosx_12_0_fat32
  cp312-abi3-macosx_12_0_universal2
  cp312-abi3-macosx_12_0_universal
  cp312-abi3-macosx_11_0_x86_64
  cp312-abi3-macosx_11_0_intel
  cp312-abi3-macosx_11_0_fat64
  cp312-abi3-macosx_11_0_fat32
  cp312-abi3-macosx_11_0_universal2
  cp312-abi3-macosx_11_0_universal
  cp312-abi3-macosx_10_16_x86_64
  cp312-abi3-macosx_10_16_intel
  cp312-abi3-macosx_10_16_fat64
  cp312-abi3-macosx_10_16_fat32
  cp312-abi3-macosx_10_16_universal2
  cp312-abi3-macosx_10_16_universal
  cp312-abi3-macosx_10_15_x86_64
  cp312-abi3-macosx_10_15_intel
  cp312-abi3-macosx_10_15_fat64
  cp312-abi3-macosx_10_15_fat32
  cp312-abi3-macosx_10_15_universal2
  cp312-abi3-macosx_10_15_universal
  cp312-abi3-macosx_10_14_x86_64
  cp312-abi3-macosx_10_14_intel
  cp312-abi3-macosx_10_14_fat64
  cp312-abi3-macosx_10_14_fat32
  cp312-abi3-macosx_10_14_universal2
  cp312-abi3-macosx_10_14_universal
  cp312-abi3-macosx_10_13_x86_64
  cp312-abi3-macosx_10_13_intel
  cp312-abi3-macosx_10_13_fat64
  cp312-abi3-macosx_10_13_fat32
  cp312-abi3-macosx_10_13_universal2
  cp312-abi3-macosx_10_13_universal
  cp312-abi3-macosx_10_12_x86_64
  cp312-abi3-macosx_10_12_intel
  cp312-abi3-macosx_10_12_fat64
  cp312-abi3-macosx_10_12_fat32
  cp312-abi3-macosx_10_12_universal2
  cp312-abi3-macosx_10_12_universal
  cp312-abi3-macosx_10_11_x86_64
  cp312-abi3-macosx_10_11_intel
  cp312-abi3-macosx_10_11_fat64
  cp312-abi3-macosx_10_11_fat32
  cp312-abi3-macosx_10_11_universal2
  cp312-abi3-macosx_10_11_universal
  cp312-abi3-macosx_10_10_x86_64
  cp312-abi3-macosx_10_10_intel
  cp312-abi3-macosx_10_10_fat64
  cp312-abi3-macosx_10_10_fat32
  cp312-abi3-macosx_10_10_universal2
  cp312-abi3-macosx_10_10_universal
  cp312-abi3-macosx_10_9_x86_64
  cp312-abi3-macosx_10_9_intel
  cp312-abi3-macosx_10_9_fat64
  cp312-abi3-macosx_10_9_fat32
  cp312-abi3-macosx_10_9_universal2
  cp312-abi3-macosx_10_9_universal
  cp312-abi3-macosx_10_8_x86_64
  cp312-abi3-macosx_10_8_intel
  cp312-abi3-macosx_10_8_fat64
  cp312-abi3-macosx_10_8_fat32
  cp312-abi3-macosx_10_8_universal2
  cp312-abi3-macosx_10_8_universal
  cp312-abi3-macosx_10_7_x86_64
  cp312-abi3-macosx_10_7_intel
  cp312-abi3-macosx_10_7_fat64
  cp312-abi3-macosx_10_7_fat32
  cp312-abi3-macosx_10_7_universal2
  cp312-abi3-macosx_10_7_universal
  cp312-abi3-macosx_10_6_x86_64
  cp312-abi3-macosx_10_6_intel
  cp312-abi3-macosx_10_6_fat64
  cp312-abi3-macosx_10_6_fat32
  cp312-abi3-macosx_10_6_universal2
  cp312-abi3-macosx_10_6_universal
  cp312-abi3-macosx_10_5_x86_64
  cp312-abi3-macosx_10_5_intel
  cp312-abi3-macosx_10_5_fat64
  cp312-abi3-macosx_10_5_fat32
  cp312-abi3-macosx_10_5_universal2
  cp312-abi3-macosx_10_5_universal
  cp312-abi3-macosx_10_4_x86_64
  cp312-abi3-macosx_10_4_intel
  cp312-abi3-macosx_10_4_fat64
  cp312-abi3-macosx_10_4_fat32
  cp312-abi3-macosx_10_4_universal2
  cp312-abi3-macosx_10_4_universal
  cp312-none-macosx_15_0_x86_64
  cp312-none-macosx_15_0_intel
  cp312-none-macosx_15_0_fat64
  cp312-none-macosx_15_0_fat32
  cp312-none-macosx_15_0_universal2
  cp312-none-macosx_15_0_universal
  cp312-none-macosx_14_0_x86_64
  cp312-none-macosx_14_0_intel
  cp312-none-macosx_14_0_fat64
  cp312-none-macosx_14_0_fat32
  cp312-none-macosx_14_0_universal2
  cp312-none-macosx_14_0_universal
  cp312-none-macosx_13_0_x86_64
  cp312-none-macosx_13_0_intel
  cp312-none-macosx_13_0_fat64
  cp312-none-macosx_13_0_fat32
  cp312-none-macosx_13_0_universal2
  cp312-none-macosx_13_0_universal
  cp312-none-macosx_12_0_x86_64
  cp312-none-macosx_12_0_intel
  cp312-none-macosx_12_0_fat64
  cp312-none-macosx_12_0_fat32
  cp312-none-macosx_12_0_universal2
  cp312-none-macosx_12_0_universal
  cp312-none-macosx_11_0_x86_64
  cp312-none-macosx_11_0_intel
  cp312-none-macosx_11_0_fat64
  cp312-none-macosx_11_0_fat32
  cp312-none-macosx_11_0_universal2
  cp312-none-macosx_11_0_universal
  cp312-none-macosx_10_16_x86_64
  cp312-none-macosx_10_16_intel
  cp312-none-macosx_10_16_fat64
  cp312-none-macosx_10_16_fat32
  cp312-none-macosx_10_16_universal2
  cp312-none-macosx_10_16_universal
  cp312-none-macosx_10_15_x86_64
  cp312-none-macosx_10_15_intel
  cp312-none-macosx_10_15_fat64
  cp312-none-macosx_10_15_fat32
  cp312-none-macosx_10_15_universal2
  cp312-none-macosx_10_15_universal
  cp312-none-macosx_10_14_x86_64
  cp312-none-macosx_10_14_intel
  cp312-none-macosx_10_14_fat64
  cp312-none-macosx_10_14_fat32
  cp312-none-macosx_10_14_universal2
  cp312-none-macosx_10_14_universal
  cp312-none-macosx_10_13_x86_64
  cp312-none-macosx_10_13_intel
  cp312-none-macosx_10_13_fat64
  cp312-none-macosx_10_13_fat32
  cp312-none-macosx_10_13_universal2
  cp312-none-macosx_10_13_universal
  cp312-none-macosx_10_12_x86_64
  cp312-none-macosx_10_12_intel
  cp312-none-macosx_10_12_fat64
  cp312-none-macosx_10_12_fat32
  cp312-none-macosx_10_12_universal2
  cp312-none-macosx_10_12_universal
  cp312-none-macosx_10_11_x86_64
  cp312-none-macosx_10_11_intel
  cp312-none-macosx_10_11_fat64
  cp312-none-macosx_10_11_fat32
  cp312-none-macosx_10_11_universal2
  cp312-none-macosx_10_11_universal
  cp312-none-macosx_10_10_x86_64
  cp312-none-macosx_10_10_intel
  cp312-none-macosx_10_10_fat64
  cp312-none-macosx_10_10_fat32
  cp312-none-macosx_10_10_universal2
  cp312-none-macosx_10_10_universal
  cp312-none-macosx_10_9_x86_64
  cp312-none-macosx_10_9_intel
  cp312-none-macosx_10_9_fat64
  cp312-none-macosx_10_9_fat32
  cp312-none-macosx_10_9_universal2
  cp312-none-macosx_10_9_universal
  cp312-none-macosx_10_8_x86_64
  cp312-none-macosx_10_8_intel
  cp312-none-macosx_10_8_fat64
  cp312-none-macosx_10_8_fat32
  cp312-none-macosx_10_8_universal2
  cp312-none-macosx_10_8_universal
  cp312-none-macosx_10_7_x86_64
  cp312-none-macosx_10_7_intel
  cp312-none-macosx_10_7_fat64
  cp312-none-macosx_10_7_fat32
  cp312-none-macosx_10_7_universal2
  cp312-none-macosx_10_7_universal
  cp312-none-macosx_10_6_x86_64
  cp312-none-macosx_10_6_intel
  cp312-none-macosx_10_6_fat64
  cp312-none-macosx_10_6_fat32
  cp312-none-macosx_10_6_universal2
  cp312-none-macosx_10_6_universal
  cp312-none-macosx_10_5_x86_64
  cp312-none-macosx_10_5_intel
  cp312-none-macosx_10_5_fat64
  cp312-none-macosx_10_5_fat32
  cp312-none-macosx_10_5_universal2
  cp312-none-macosx_10_5_universal
  cp312-none-macosx_10_4_x86_64
  cp312-none-macosx_10_4_intel
  cp312-none-macosx_10_4_fat64
  cp312-none-macosx_10_4_fat32
  cp312-none-macosx_10_4_universal2
  cp312-none-macosx_10_4_universal
  cp311-abi3-macosx_15_0_x86_64
  cp311-abi3-macosx_15_0_intel
  cp311-abi3-macosx_15_0_fat64
  cp311-abi3-macosx_15_0_fat32
  cp311-abi3-macosx_15_0_universal2
  cp311-abi3-macosx_15_0_universal
  cp311-abi3-macosx_14_0_x86_64
  cp311-abi3-macosx_14_0_intel
  cp311-abi3-macosx_14_0_fat64
  cp311-abi3-macosx_14_0_fat32
  cp311-abi3-macosx_14_0_universal2
  cp311-abi3-macosx_14_0_universal
  cp311-abi3-macosx_13_0_x86_64
  cp311-abi3-macosx_13_0_intel
  cp311-abi3-macosx_13_0_fat64
  cp311-abi3-macosx_13_0_fat32
  cp311-abi3-macosx_13_0_universal2
  cp311-abi3-macosx_13_0_universal
  cp311-abi3-macosx_12_0_x86_64
  cp311-abi3-macosx_12_0_intel
  cp311-abi3-macosx_12_0_fat64
  cp311-abi3-macosx_12_0_fat32
  cp311-abi3-macosx_12_0_universal2
  cp311-abi3-macosx_12_0_universal
  cp311-abi3-macosx_11_0_x86_64
  cp311-abi3-macosx_11_0_intel
  cp311-abi3-macosx_11_0_fat64
  cp311-abi3-macosx_11_0_fat32
  cp311-abi3-macosx_11_0_universal2
  cp311-abi3-macosx_11_0_universal
  cp311-abi3-macosx_10_16_x86_64
  cp311-abi3-macosx_10_16_intel
  cp311-abi3-macosx_10_16_fat64
  cp311-abi3-macosx_10_16_fat32
  cp311-abi3-macosx_10_16_universal2
  cp311-abi3-macosx_10_16_universal
  cp311-abi3-macosx_10_15_x86_64
  cp311-abi3-macosx_10_15_intel
  cp311-abi3-macosx_10_15_fat64
  cp311-abi3-macosx_10_15_fat32
  cp311-abi3-macosx_10_15_universal2
  cp311-abi3-macosx_10_15_universal
  cp311-abi3-macosx_10_14_x86_64
  cp311-abi3-macosx_10_14_intel
  cp311-abi3-macosx_10_14_fat64
  cp311-abi3-macosx_10_14_fat32
  cp311-abi3-macosx_10_14_universal2
  cp311-abi3-macosx_10_14_universal
  cp311-abi3-macosx_10_13_x86_64
  cp311-abi3-macosx_10_13_intel
  cp311-abi3-macosx_10_13_fat64
  cp311-abi3-macosx_10_13_fat32
  cp311-abi3-macosx_10_13_universal2
  cp311-abi3-macosx_10_13_universal
  cp311-abi3-macosx_10_12_x86_64
  cp311-abi3-macosx_10_12_intel
  cp311-abi3-macosx_10_12_fat64
  cp311-abi3-macosx_10_12_fat32
  cp311-abi3-macosx_10_12_universal2
  cp311-abi3-macosx_10_12_universal
  cp311-abi3-macosx_10_11_x86_64
  cp311-abi3-macosx_10_11_intel
  cp311-abi3-macosx_10_11_fat64
  cp311-abi3-macosx_10_11_fat32
  cp311-abi3-macosx_10_11_universal2
  cp311-abi3-macosx_10_11_universal
  cp311-abi3-macosx_10_10_x86_64
  cp311-abi3-macosx_10_10_intel
  cp311-abi3-macosx_10_10_fat64
  cp311-abi3-macosx_10_10_fat32
  cp311-abi3-macosx_10_10_universal2
  cp311-abi3-macosx_10_10_universal
  cp311-abi3-macosx_10_9_x86_64
  cp311-abi3-macosx_10_9_intel
  cp311-abi3-macosx_10_9_fat64
  cp311-abi3-macosx_10_9_fat32
  cp311-abi3-macosx_10_9_universal2
  cp311-abi3-macosx_10_9_universal
  cp311-abi3-macosx_10_8_x86_64
  cp311-abi3-macosx_10_8_intel
  cp311-abi3-macosx_10_8_fat64
  cp311-abi3-macosx_10_8_fat32
  cp311-abi3-macosx_10_8_universal2
  cp311-abi3-macosx_10_8_universal
  cp311-abi3-macosx_10_7_x86_64
  cp311-abi3-macosx_10_7_intel
  cp311-abi3-macosx_10_7_fat64
  cp311-abi3-macosx_10_7_fat32
  cp311-abi3-macosx_10_7_universal2
  cp311-abi3-macosx_10_7_universal
  cp311-abi3-macosx_10_6_x86_64
  cp311-abi3-macosx_10_6_intel
  cp311-abi3-macosx_10_6_fat64
  cp311-abi3-macosx_10_6_fat32
  cp311-abi3-macosx_10_6_universal2
  cp311-abi3-macosx_10_6_universal
  cp311-abi3-macosx_10_5_x86_64
  cp311-abi3-macosx_10_5_intel
  cp311-abi3-macosx_10_5_fat64
  cp311-abi3-macosx_10_5_fat32
  cp311-abi3-macosx_10_5_universal2
  cp311-abi3-macosx_10_5_universal
  cp311-abi3-macosx_10_4_x86_64
  cp311-abi3-macosx_10_4_intel
  cp311-abi3-macosx_10_4_fat64
  cp311-abi3-macosx_10_4_fat32
  cp311-abi3-macosx_10_4_universal2
  cp311-abi3-macosx_10_4_universal
  cp310-abi3-macosx_15_0_x86_64
  cp310-abi3-macosx_15_0_intel
  cp310-abi3-macosx_15_0_fat64
  cp310-abi3-macosx_15_0_fat32
  cp310-abi3-macosx_15_0_universal2
  cp310-abi3-macosx_15_0_universal
  cp310-abi3-macosx_14_0_x86_64
  cp310-abi3-macosx_14_0_intel
  cp310-abi3-macosx_14_0_fat64
  cp310-abi3-macosx_14_0_fat32
  cp310-abi3-macosx_14_0_universal2
  cp310-abi3-macosx_14_0_universal
  cp310-abi3-macosx_13_0_x86_64
  cp310-abi3-macosx_13_0_intel
  cp310-abi3-macosx_13_0_fat64
  cp310-abi3-macosx_13_0_fat32
  cp310-abi3-macosx_13_0_universal2
  cp310-abi3-macosx_13_0_universal
  cp310-abi3-macosx_12_0_x86_64
  cp310-abi3-macosx_12_0_intel
  cp310-abi3-macosx_12_0_fat64
  cp310-abi3-macosx_12_0_fat32
  cp310-abi3-macosx_12_0_universal2
  cp310-abi3-macosx_12_0_universal
  cp310-abi3-macosx_11_0_x86_64
  cp310-abi3-macosx_11_0_intel
  cp310-abi3-macosx_11_0_fat64
  cp310-abi3-macosx_11_0_fat32
  cp310-abi3-macosx_11_0_universal2
  cp310-abi3-macosx_11_0_universal
  cp310-abi3-macosx_10_16_x86_64
  cp310-abi3-macosx_10_16_intel
  cp310-abi3-macosx_10_16_fat64
  cp310-abi3-macosx_10_16_fat32
  cp310-abi3-macosx_10_16_universal2
  cp310-abi3-macosx_10_16_universal
  cp310-abi3-macosx_10_15_x86_64
  cp310-abi3-macosx_10_15_intel
  cp310-abi3-macosx_10_15_fat64
  cp310-abi3-macosx_10_15_fat32
  cp310-abi3-macosx_10_15_universal2
  cp310-abi3-macosx_10_15_universal
  cp310-abi3-macosx_10_14_x86_64
  cp310-abi3-macosx_10_14_intel
  cp310-abi3-macosx_10_14_fat64
  cp310-abi3-macosx_10_14_fat32
  cp310-abi3-macosx_10_14_universal2
  cp310-abi3-macosx_10_14_universal
  cp310-abi3-macosx_10_13_x86_64
  cp310-abi3-macosx_10_13_intel
  cp310-abi3-macosx_10_13_fat64
  cp310-abi3-macosx_10_13_fat32
  cp310-abi3-macosx_10_13_universal2
  cp310-abi3-macosx_10_13_universal
  cp310-abi3-macosx_10_12_x86_64
  cp310-abi3-macosx_10_12_intel
  cp310-abi3-macosx_10_12_fat64
  cp310-abi3-macosx_10_12_fat32
  cp310-abi3-macosx_10_12_universal2
  cp310-abi3-macosx_10_12_universal
  cp310-abi3-macosx_10_11_x86_64
  cp310-abi3-macosx_10_11_intel
  cp310-abi3-macosx_10_11_fat64
  cp310-abi3-macosx_10_11_fat32
  cp310-abi3-macosx_10_11_universal2
  cp310-abi3-macosx_10_11_universal
  cp310-abi3-macosx_10_10_x86_64
  cp310-abi3-macosx_10_10_intel
  cp310-abi3-macosx_10_10_fat64
  cp310-abi3-macosx_10_10_fat32
  cp310-abi3-macosx_10_10_universal2
  cp310-abi3-macosx_10_10_universal
  cp310-abi3-macosx_10_9_x86_64
  cp310-abi3-macosx_10_9_intel
  cp310-abi3-macosx_10_9_fat64
  cp310-abi3-macosx_10_9_fat32
  cp310-abi3-macosx_10_9_universal2
  cp310-abi3-macosx_10_9_universal
  cp310-abi3-macosx_10_8_x86_64
  cp310-abi3-macosx_10_8_intel
  cp310-abi3-macosx_10_8_fat64
  cp310-abi3-macosx_10_8_fat32
  cp310-abi3-macosx_10_8_universal2
  cp310-abi3-macosx_10_8_universal
  cp310-abi3-macosx_10_7_x86_64
  cp310-abi3-macosx_10_7_intel
  cp310-abi3-macosx_10_7_fat64
  cp310-abi3-macosx_10_7_fat32
  cp310-abi3-macosx_10_7_universal2
  cp310-abi3-macosx_10_7_universal
  cp310-abi3-macosx_10_6_x86_64
  cp310-abi3-macosx_10_6_intel
  cp310-abi3-macosx_10_6_fat64
  cp310-abi3-macosx_10_6_fat32
  cp310-abi3-macosx_10_6_universal2
  cp310-abi3-macosx_10_6_universal
  cp310-abi3-macosx_10_5_x86_64
  cp310-abi3-macosx_10_5_intel
  cp310-abi3-macosx_10_5_fat64
  cp310-abi3-macosx_10_5_fat32
  cp310-abi3-macosx_10_5_universal2
  cp310-abi3-macosx_10_5_universal
  cp310-abi3-macosx_10_4_x86_64
  cp310-abi3-macosx_10_4_intel
  cp310-abi3-macosx_10_4_fat64
  cp310-abi3-macosx_10_4_fat32
  cp310-abi3-macosx_10_4_universal2
  cp310-abi3-macosx_10_4_universal
  cp39-abi3-macosx_15_0_x86_64
  cp39-abi3-macosx_15_0_intel
  cp39-abi3-macosx_15_0_fat64
  cp39-abi3-macosx_15_0_fat32
  cp39-abi3-macosx_15_0_universal2
  cp39-abi3-macosx_15_0_universal
  cp39-abi3-macosx_14_0_x86_64
  cp39-abi3-macosx_14_0_intel
  cp39-abi3-macosx_14_0_fat64
  cp39-abi3-macosx_14_0_fat32
  cp39-abi3-macosx_14_0_universal2
  cp39-abi3-macosx_14_0_universal
  cp39-abi3-macosx_13_0_x86_64
  cp39-abi3-macosx_13_0_intel
  cp39-abi3-macosx_13_0_fat64
  cp39-abi3-macosx_13_0_fat32
  cp39-abi3-macosx_13_0_universal2
  cp39-abi3-macosx_13_0_universal
  cp39-abi3-macosx_12_0_x86_64
  cp39-abi3-macosx_12_0_intel
  cp39-abi3-macosx_12_0_fat64
  cp39-abi3-macosx_12_0_fat32
  cp39-abi3-macosx_12_0_universal2
  cp39-abi3-macosx_12_0_universal
  cp39-abi3-macosx_11_0_x86_64
  cp39-abi3-macosx_11_0_intel
  cp39-abi3-macosx_11_0_fat64
  cp39-abi3-macosx_11_0_fat32
  cp39-abi3-macosx_11_0_universal2
  cp39-abi3-macosx_11_0_universal
  cp39-abi3-macosx_10_16_x86_64
  cp39-abi3-macosx_10_16_intel
  cp39-abi3-macosx_10_16_fat64
  cp39-abi3-macosx_10_16_fat32
  cp39-abi3-macosx_10_16_universal2
  cp39-abi3-macosx_10_16_universal
  cp39-abi3-macosx_10_15_x86_64
  cp39-abi3-macosx_10_15_intel
  cp39-abi3-macosx_10_15_fat64
  cp39-abi3-macosx_10_15_fat32
  cp39-abi3-macosx_10_15_universal2
  cp39-abi3-macosx_10_15_universal
  cp39-abi3-macosx_10_14_x86_64
  cp39-abi3-macosx_10_14_intel
  cp39-abi3-macosx_10_14_fat64
  cp39-abi3-macosx_10_14_fat32
  cp39-abi3-macosx_10_14_universal2
  cp39-abi3-macosx_10_14_universal
  cp39-abi3-macosx_10_13_x86_64
  cp39-abi3-macosx_10_13_intel
  cp39-abi3-macosx_10_13_fat64
  cp39-abi3-macosx_10_13_fat32
  cp39-abi3-macosx_10_13_universal2
  cp39-abi3-macosx_10_13_universal
  cp39-abi3-macosx_10_12_x86_64
  cp39-abi3-macosx_10_12_intel
  cp39-abi3-macosx_10_12_fat64
  cp39-abi3-macosx_10_12_fat32
  cp39-abi3-macosx_10_12_universal2
  cp39-abi3-macosx_10_12_universal
  cp39-abi3-macosx_10_11_x86_64
  cp39-abi3-macosx_10_11_intel
  cp39-abi3-macosx_10_11_fat64
  cp39-abi3-macosx_10_11_fat32
  cp39-abi3-macosx_10_11_universal2
  cp39-abi3-macosx_10_11_universal
  cp39-abi3-macosx_10_10_x86_64
  cp39-abi3-macosx_10_10_intel
  cp39-abi3-macosx_10_10_fat64
  cp39-abi3-macosx_10_10_fat32
  cp39-abi3-macosx_10_10_universal2
  cp39-abi3-macosx_10_10_universal
  cp39-abi3-macosx_10_9_x86_64
  cp39-abi3-macosx_10_9_intel
  cp39-abi3-macosx_10_9_fat64
  cp39-abi3-macosx_10_9_fat32
  cp39-abi3-macosx_10_9_universal2
  cp39-abi3-macosx_10_9_universal
  cp39-abi3-macosx_10_8_x86_64
  cp39-abi3-macosx_10_8_intel
  cp39-abi3-macosx_10_8_fat64
  cp39-abi3-macosx_10_8_fat32
  cp39-abi3-macosx_10_8_universal2
  cp39-abi3-macosx_10_8_universal
  cp39-abi3-macosx_10_7_x86_64
  cp39-abi3-macosx_10_7_intel
  cp39-abi3-macosx_10_7_fat64
  cp39-abi3-macosx_10_7_fat32
  cp39-abi3-macosx_10_7_universal2
  cp39-abi3-macosx_10_7_universal
  cp39-abi3-macosx_10_6_x86_64
  cp39-abi3-macosx_10_6_intel
  cp39-abi3-macosx_10_6_fat64
  cp39-abi3-macosx_10_6_fat32
  cp39-abi3-macosx_10_6_universal2
  cp39-abi3-macosx_10_6_universal
  cp39-abi3-macosx_10_5_x86_64
  cp39-abi3-macosx_10_5_intel
  cp39-abi3-macosx_10_5_fat64
  cp39-abi3-macosx_10_5_fat32
  cp39-abi3-macosx_10_5_universal2
  cp39-abi3-macosx_10_5_universal
  cp39-abi3-macosx_10_4_x86_64
  cp39-abi3-macosx_10_4_intel
  cp39-abi3-macosx_10_4_fat64
  cp39-abi3-macosx_10_4_fat32
  cp39-abi3-macosx_10_4_universal2
  cp39-abi3-macosx_10_4_universal
  cp38-abi3-macosx_15_0_x86_64
  cp38-abi3-macosx_15_0_intel
  cp38-abi3-macosx_15_0_fat64
  cp38-abi3-macosx_15_0_fat32
  cp38-abi3-macosx_15_0_universal2
  cp38-abi3-macosx_15_0_universal
  cp38-abi3-macosx_14_0_x86_64
  cp38-abi3-macosx_14_0_intel
  cp38-abi3-macosx_14_0_fat64
  cp38-abi3-macosx_14_0_fat32
  cp38-abi3-macosx_14_0_universal2
  cp38-abi3-macosx_14_0_universal
  cp38-abi3-macosx_13_0_x86_64
  cp38-abi3-macosx_13_0_intel
  cp38-abi3-macosx_13_0_fat64
  cp38-abi3-macosx_13_0_fat32
  cp38-abi3-macosx_13_0_universal2
  cp38-abi3-macosx_13_0_universal
  cp38-abi3-macosx_12_0_x86_64
  cp38-abi3-macosx_12_0_intel
  cp38-abi3-macosx_12_0_fat64
  cp38-abi3-macosx_12_0_fat32
  cp38-abi3-macosx_12_0_universal2
  cp38-abi3-macosx_12_0_universal
  cp38-abi3-macosx_11_0_x86_64
  cp38-abi3-macosx_11_0_intel
  cp38-abi3-macosx_11_0_fat64
  cp38-abi3-macosx_11_0_fat32
  cp38-abi3-macosx_11_0_universal2
  cp38-abi3-macosx_11_0_universal
  cp38-abi3-macosx_10_16_x86_64
  cp38-abi3-macosx_10_16_intel
  cp38-abi3-macosx_10_16_fat64
  cp38-abi3-macosx_10_16_fat32
  cp38-abi3-macosx_10_16_universal2
  cp38-abi3-macosx_10_16_universal
  cp38-abi3-macosx_10_15_x86_64
  cp38-abi3-macosx_10_15_intel
  cp38-abi3-macosx_10_15_fat64
  cp38-abi3-macosx_10_15_fat32
  cp38-abi3-macosx_10_15_universal2
  cp38-abi3-macosx_10_15_universal
  cp38-abi3-macosx_10_14_x86_64
  cp38-abi3-macosx_10_14_intel
  cp38-abi3-macosx_10_14_fat64
  cp38-abi3-macosx_10_14_fat32
  cp38-abi3-macosx_10_14_universal2
  cp38-abi3-macosx_10_14_universal
  cp38-abi3-macosx_10_13_x86_64
  cp38-abi3-macosx_10_13_intel
  cp38-abi3-macosx_10_13_fat64
  cp38-abi3-macosx_10_13_fat32
  cp38-abi3-macosx_10_13_universal2
  cp38-abi3-macosx_10_13_universal
  cp38-abi3-macosx_10_12_x86_64
  cp38-abi3-macosx_10_12_intel
  cp38-abi3-macosx_10_12_fat64
  cp38-abi3-macosx_10_12_fat32
  cp38-abi3-macosx_10_12_universal2
  cp38-abi3-macosx_10_12_universal
  cp38-abi3-macosx_10_11_x86_64
  cp38-abi3-macosx_10_11_intel
  cp38-abi3-macosx_10_11_fat64
  cp38-abi3-macosx_10_11_fat32
  cp38-abi3-macosx_10_11_universal2
  cp38-abi3-macosx_10_11_universal
  cp38-abi3-macosx_10_10_x86_64
  cp38-abi3-macosx_10_10_intel
  cp38-abi3-macosx_10_10_fat64
  cp38-abi3-macosx_10_10_fat32
  cp38-abi3-macosx_10_10_universal2
  cp38-abi3-macosx_10_10_universal
  cp38-abi3-macosx_10_9_x86_64
  cp38-abi3-macosx_10_9_intel
  cp38-abi3-macosx_10_9_fat64
  cp38-abi3-macosx_10_9_fat32
  cp38-abi3-macosx_10_9_universal2
  cp38-abi3-macosx_10_9_universal
  cp38-abi3-macosx_10_8_x86_64
  cp38-abi3-macosx_10_8_intel
  cp38-abi3-macosx_10_8_fat64
  cp38-abi3-macosx_10_8_fat32
  cp38-abi3-macosx_10_8_universal2
  cp38-abi3-macosx_10_8_universal
  cp38-abi3-macosx_10_7_x86_64
  cp38-abi3-macosx_10_7_intel
  cp38-abi3-macosx_10_7_fat64
  cp38-abi3-macosx_10_7_fat32
  cp38-abi3-macosx_10_7_universal2
  cp38-abi3-macosx_10_7_universal
  cp38-abi3-macosx_10_6_x86_64
  cp38-abi3-macosx_10_6_intel
  cp38-abi3-macosx_10_6_fat64
  cp38-abi3-macosx_10_6_fat32
  cp38-abi3-macosx_10_6_universal2
  cp38-abi3-macosx_10_6_universal
  cp38-abi3-macosx_10_5_x86_64
  cp38-abi3-macosx_10_5_intel
  cp38-abi3-macosx_10_5_fat64
  cp38-abi3-macosx_10_5_fat32
  cp38-abi3-macosx_10_5_universal2
  cp38-abi3-macosx_10_5_universal
  cp38-abi3-macosx_10_4_x86_64
  cp38-abi3-macosx_10_4_intel
  cp38-abi3-macosx_10_4_fat64
  cp38-abi3-macosx_10_4_fat32
  cp38-abi3-macosx_10_4_universal2
  cp38-abi3-macosx_10_4_universal
  cp37-abi3-macosx_15_0_x86_64
  cp37-abi3-macosx_15_0_intel
  cp37-abi3-macosx_15_0_fat64
  cp37-abi3-macosx_15_0_fat32
  cp37-abi3-macosx_15_0_universal2
  cp37-abi3-macosx_15_0_universal
  cp37-abi3-macosx_14_0_x86_64
  cp37-abi3-macosx_14_0_intel
  cp37-abi3-macosx_14_0_fat64
  cp37-abi3-macosx_14_0_fat32
  cp37-abi3-macosx_14_0_universal2
  cp37-abi3-macosx_14_0_universal
  cp37-abi3-macosx_13_0_x86_64
  cp37-abi3-macosx_13_0_intel
  cp37-abi3-macosx_13_0_fat64
  cp37-abi3-macosx_13_0_fat32
  cp37-abi3-macosx_13_0_universal2
  cp37-abi3-macosx_13_0_universal
  cp37-abi3-macosx_12_0_x86_64
  cp37-abi3-macosx_12_0_intel
  cp37-abi3-macosx_12_0_fat64
  cp37-abi3-macosx_12_0_fat32
  cp37-abi3-macosx_12_0_universal2
  cp37-abi3-macosx_12_0_universal
  cp37-abi3-macosx_11_0_x86_64
  cp37-abi3-macosx_11_0_intel
  cp37-abi3-macosx_11_0_fat64
  cp37-abi3-macosx_11_0_fat32
  cp37-abi3-macosx_11_0_universal2
  cp37-abi3-macosx_11_0_universal
  cp37-abi3-macosx_10_16_x86_64
  cp37-abi3-macosx_10_16_intel
  cp37-abi3-macosx_10_16_fat64
  cp37-abi3-macosx_10_16_fat32
  cp37-abi3-macosx_10_16_universal2
  cp37-abi3-macosx_10_16_universal
  cp37-abi3-macosx_10_15_x86_64
  cp37-abi3-macosx_10_15_intel
  cp37-abi3-macosx_10_15_fat64
  cp37-abi3-macosx_10_15_fat32
  cp37-abi3-macosx_10_15_universal2
  cp37-abi3-macosx_10_15_universal
  cp37-abi3-macosx_10_14_x86_64
  cp37-abi3-macosx_10_14_intel
  cp37-abi3-macosx_10_14_fat64
  cp37-abi3-macosx_10_14_fat32
  cp37-abi3-macosx_10_14_universal2
  cp37-abi3-macosx_10_14_universal
  cp37-abi3-macosx_10_13_x86_64
  cp37-abi3-macosx_10_13_intel
  cp37-abi3-macosx_10_13_fat64
  cp37-abi3-macosx_10_13_fat32
  cp37-abi3-macosx_10_13_universal2
  cp37-abi3-macosx_10_13_universal
  cp37-abi3-macosx_10_12_x86_64
  cp37-abi3-macosx_10_12_intel
  cp37-abi3-macosx_10_12_fat64
  cp37-abi3-macosx_10_12_fat32
  cp37-abi3-macosx_10_12_universal2
  cp37-abi3-macosx_10_12_universal
  cp37-abi3-macosx_10_11_x86_64
  cp37-abi3-macosx_10_11_intel
  cp37-abi3-macosx_10_11_fat64
  cp37-abi3-macosx_10_11_fat32
  cp37-abi3-macosx_10_11_universal2
  cp37-abi3-macosx_10_11_universal
  cp37-abi3-macosx_10_10_x86_64
  cp37-abi3-macosx_10_10_intel
  cp37-abi3-macosx_10_10_fat64
  cp37-abi3-macosx_10_10_fat32
  cp37-abi3-macosx_10_10_universal2
  cp37-abi3-macosx_10_10_universal
  cp37-abi3-macosx_10_9_x86_64
  cp37-abi3-macosx_10_9_intel
  cp37-abi3-macosx_10_9_fat64
  cp37-abi3-macosx_10_9_fat32
  cp37-abi3-macosx_10_9_universal2
  cp37-abi3-macosx_10_9_universal
  cp37-abi3-macosx_10_8_x86_64
  cp37-abi3-macosx_10_8_intel
  cp37-abi3-macosx_10_8_fat64
  cp37-abi3-macosx_10_8_fat32
  cp37-abi3-macosx_10_8_universal2
  cp37-abi3-macosx_10_8_universal
  cp37-abi3-macosx_10_7_x86_64
  cp37-abi3-macosx_10_7_intel
  cp37-abi3-macosx_10_7_fat64
  cp37-abi3-macosx_10_7_fat32
  cp37-abi3-macosx_10_7_universal2
  cp37-abi3-macosx_10_7_universal
  cp37-abi3-macosx_10_6_x86_64
  cp37-abi3-macosx_10_6_intel
  cp37-abi3-macosx_10_6_fat64
  cp37-abi3-macosx_10_6_fat32
  cp37-abi3-macosx_10_6_universal2
  cp37-abi3-macosx_10_6_universal
  cp37-abi3-macosx_10_5_x86_64
  cp37-abi3-macosx_10_5_intel
  cp37-abi3-macosx_10_5_fat64
  cp37-abi3-macosx_10_5_fat32
  cp37-abi3-macosx_10_5_universal2
  cp37-abi3-macosx_10_5_universal
  cp37-abi3-macosx_10_4_x86_64
  cp37-abi3-macosx_10_4_intel
  cp37-abi3-macosx_10_4_fat64
  cp37-abi3-macosx_10_4_fat32
  cp37-abi3-macosx_10_4_universal2
  cp37-abi3-macosx_10_4_universal
  cp36-abi3-macosx_15_0_x86_64
  cp36-abi3-macosx_15_0_intel
  cp36-abi3-macosx_15_0_fat64
  cp36-abi3-macosx_15_0_fat32
  cp36-abi3-macosx_15_0_universal2
  cp36-abi3-macosx_15_0_universal
  cp36-abi3-macosx_14_0_x86_64
  cp36-abi3-macosx_14_0_intel
  cp36-abi3-macosx_14_0_fat64
  cp36-abi3-macosx_14_0_fat32
  cp36-abi3-macosx_14_0_universal2
  cp36-abi3-macosx_14_0_universal
  cp36-abi3-macosx_13_0_x86_64
  cp36-abi3-macosx_13_0_intel
  cp36-abi3-macosx_13_0_fat64
  cp36-abi3-macosx_13_0_fat32
  cp36-abi3-macosx_13_0_universal2
  cp36-abi3-macosx_13_0_universal
  cp36-abi3-macosx_12_0_x86_64
  cp36-abi3-macosx_12_0_intel
  cp36-abi3-macosx_12_0_fat64
  cp36-abi3-macosx_12_0_fat32
  cp36-abi3-macosx_12_0_universal2
  cp36-abi3-macosx_12_0_universal
  cp36-abi3-macosx_11_0_x86_64
  cp36-abi3-macosx_11_0_intel
  cp36-abi3-macosx_11_0_fat64
  cp36-abi3-macosx_11_0_fat32
  cp36-abi3-macosx_11_0_universal2
  cp36-abi3-macosx_11_0_universal
  cp36-abi3-macosx_10_16_x86_64
  cp36-abi3-macosx_10_16_intel
  cp36-abi3-macosx_10_16_fat64
  cp36-abi3-macosx_10_16_fat32
  cp36-abi3-macosx_10_16_universal2
  cp36-abi3-macosx_10_16_universal
  cp36-abi3-macosx_10_15_x86_64
  cp36-abi3-macosx_10_15_intel
  cp36-abi3-macosx_10_15_fat64
  cp36-abi3-macosx_10_15_fat32
  cp36-abi3-macosx_10_15_universal2
  cp36-abi3-macosx_10_15_universal
  cp36-abi3-macosx_10_14_x86_64
  cp36-abi3-macosx_10_14_intel
  cp36-abi3-macosx_10_14_fat64
  cp36-abi3-macosx_10_14_fat32
  cp36-abi3-macosx_10_14_universal2
  cp36-abi3-macosx_10_14_universal
  cp36-abi3-macosx_10_13_x86_64
  cp36-abi3-macosx_10_13_intel
  cp36-abi3-macosx_10_13_fat64
  cp36-abi3-macosx_10_13_fat32
  cp36-abi3-macosx_10_13_universal2
  cp36-abi3-macosx_10_13_universal
  cp36-abi3-macosx_10_12_x86_64
  cp36-abi3-macosx_10_12_intel
  cp36-abi3-macosx_10_12_fat64
  cp36-abi3-macosx_10_12_fat32
  cp36-abi3-macosx_10_12_universal2
  cp36-abi3-macosx_10_12_universal
  cp36-abi3-macosx_10_11_x86_64
  cp36-abi3-macosx_10_11_intel
  cp36-abi3-macosx_10_11_fat64
  cp36-abi3-macosx_10_11_fat32
  cp36-abi3-macosx_10_11_universal2
  cp36-abi3-macosx_10_11_universal
  cp36-abi3-macosx_10_10_x86_64
  cp36-abi3-macosx_10_10_intel
  cp36-abi3-macosx_10_10_fat64
  cp36-abi3-macosx_10_10_fat32
  cp36-abi3-macosx_10_10_universal2
  cp36-abi3-macosx_10_10_universal
  cp36-abi3-macosx_10_9_x86_64
  cp36-abi3-macosx_10_9_intel
  cp36-abi3-macosx_10_9_fat64
  cp36-abi3-macosx_10_9_fat32
  cp36-abi3-macosx_10_9_universal2
  cp36-abi3-macosx_10_9_universal
  cp36-abi3-macosx_10_8_x86_64
  cp36-abi3-macosx_10_8_intel
  cp36-abi3-macosx_10_8_fat64
  cp36-abi3-macosx_10_8_fat32
  cp36-abi3-macosx_10_8_universal2
  cp36-abi3-macosx_10_8_universal
  cp36-abi3-macosx_10_7_x86_64
  cp36-abi3-macosx_10_7_intel
  cp36-abi3-macosx_10_7_fat64
  cp36-abi3-macosx_10_7_fat32
  cp36-abi3-macosx_10_7_universal2
  cp36-abi3-macosx_10_7_universal
  cp36-abi3-macosx_10_6_x86_64
  cp36-abi3-macosx_10_6_intel
  cp36-abi3-macosx_10_6_fat64
  cp36-abi3-macosx_10_6_fat32
  cp36-abi3-macosx_10_6_universal2
  cp36-abi3-macosx_10_6_universal
  cp36-abi3-macosx_10_5_x86_64
  cp36-abi3-macosx_10_5_intel
  cp36-abi3-macosx_10_5_fat64
  cp36-abi3-macosx_10_5_fat32
  cp36-abi3-macosx_10_5_universal2
  cp36-abi3-macosx_10_5_universal
  cp36-abi3-macosx_10_4_x86_64
  cp36-abi3-macosx_10_4_intel
  cp36-abi3-macosx_10_4_fat64
  cp36-abi3-macosx_10_4_fat32
  cp36-abi3-macosx_10_4_universal2
  cp36-abi3-macosx_10_4_universal
  cp35-abi3-macosx_15_0_x86_64
  cp35-abi3-macosx_15_0_intel
  cp35-abi3-macosx_15_0_fat64
  cp35-abi3-macosx_15_0_fat32
  cp35-abi3-macosx_15_0_universal2
  cp35-abi3-macosx_15_0_universal
  cp35-abi3-macosx_14_0_x86_64
  cp35-abi3-macosx_14_0_intel
  cp35-abi3-macosx_14_0_fat64
  cp35-abi3-macosx_14_0_fat32
  cp35-abi3-macosx_14_0_universal2
  cp35-abi3-macosx_14_0_universal
  cp35-abi3-macosx_13_0_x86_64
  cp35-abi3-macosx_13_0_intel
  cp35-abi3-macosx_13_0_fat64
  cp35-abi3-macosx_13_0_fat32
  cp35-abi3-macosx_13_0_universal2
  cp35-abi3-macosx_13_0_universal
  cp35-abi3-macosx_12_0_x86_64
  cp35-abi3-macosx_12_0_intel
  cp35-abi3-macosx_12_0_fat64
  cp35-abi3-macosx_12_0_fat32
  cp35-abi3-macosx_12_0_universal2
  cp35-abi3-macosx_12_0_universal
  cp35-abi3-macosx_11_0_x86_64
  cp35-abi3-macosx_11_0_intel
  cp35-abi3-macosx_11_0_fat64
  cp35-abi3-macosx_11_0_fat32
  cp35-abi3-macosx_11_0_universal2
  cp35-abi3-macosx_11_0_universal
  cp35-abi3-macosx_10_16_x86_64
  cp35-abi3-macosx_10_16_intel
  cp35-abi3-macosx_10_16_fat64
  cp35-abi3-macosx_10_16_fat32
  cp35-abi3-macosx_10_16_universal2
  cp35-abi3-macosx_10_16_universal
  cp35-abi3-macosx_10_15_x86_64
  cp35-abi3-macosx_10_15_intel
  cp35-abi3-macosx_10_15_fat64
  cp35-abi3-macosx_10_15_fat32
  cp35-abi3-macosx_10_15_universal2
  cp35-abi3-macosx_10_15_universal
  cp35-abi3-macosx_10_14_x86_64
  cp35-abi3-macosx_10_14_intel
  cp35-abi3-macosx_10_14_fat64
  cp35-abi3-macosx_10_14_fat32
  cp35-abi3-macosx_10_14_universal2
  cp35-abi3-macosx_10_14_universal
  cp35-abi3-macosx_10_13_x86_64
  cp35-abi3-macosx_10_13_intel
  cp35-abi3-macosx_10_13_fat64
  cp35-abi3-macosx_10_13_fat32
  cp35-abi3-macosx_10_13_universal2
  cp35-abi3-macosx_10_13_universal
  cp35-abi3-macosx_10_12_x86_64
  cp35-abi3-macosx_10_12_intel
  cp35-abi3-macosx_10_12_fat64
  cp35-abi3-macosx_10_12_fat32
  cp35-abi3-macosx_10_12_universal2
  cp35-abi3-macosx_10_12_universal
  cp35-abi3-macosx_10_11_x86_64
  cp35-abi3-macosx_10_11_intel
  cp35-abi3-macosx_10_11_fat64
  cp35-abi3-macosx_10_11_fat32
  cp35-abi3-macosx_10_11_universal2
  cp35-abi3-macosx_10_11_universal
  cp35-abi3-macosx_10_10_x86_64
  cp35-abi3-macosx_10_10_intel
  cp35-abi3-macosx_10_10_fat64
  cp35-abi3-macosx_10_10_fat32
  cp35-abi3-macosx_10_10_universal2
  cp35-abi3-macosx_10_10_universal
  cp35-abi3-macosx_10_9_x86_64
  cp35-abi3-macosx_10_9_intel
  cp35-abi3-macosx_10_9_fat64
  cp35-abi3-macosx_10_9_fat32
  cp35-abi3-macosx_10_9_universal2
  cp35-abi3-macosx_10_9_universal
  cp35-abi3-macosx_10_8_x86_64
  cp35-abi3-macosx_10_8_intel
  cp35-abi3-macosx_10_8_fat64
  cp35-abi3-macosx_10_8_fat32
  cp35-abi3-macosx_10_8_universal2
  cp35-abi3-macosx_10_8_universal
  cp35-abi3-macosx_10_7_x86_64
  cp35-abi3-macosx_10_7_intel
  cp35-abi3-macosx_10_7_fat64
  cp35-abi3-macosx_10_7_fat32
  cp35-abi3-macosx_10_7_universal2
  cp35-abi3-macosx_10_7_universal
  cp35-abi3-macosx_10_6_x86_64
  cp35-abi3-macosx_10_6_intel
  cp35-abi3-macosx_10_6_fat64
  cp35-abi3-macosx_10_6_fat32
  cp35-abi3-macosx_10_6_universal2
  cp35-abi3-macosx_10_6_universal
  cp35-abi3-macosx_10_5_x86_64
  cp35-abi3-macosx_10_5_intel
  cp35-abi3-macosx_10_5_fat64
  cp35-abi3-macosx_10_5_fat32
  cp35-abi3-macosx_10_5_universal2
  cp35-abi3-macosx_10_5_universal
  cp35-abi3-macosx_10_4_x86_64
  cp35-abi3-macosx_10_4_intel
  cp35-abi3-macosx_10_4_fat64
  cp35-abi3-macosx_10_4_fat32
  cp35-abi3-macosx_10_4_universal2
  cp35-abi3-macosx_10_4_universal
  cp34-abi3-macosx_15_0_x86_64
  cp34-abi3-macosx_15_0_intel
  cp34-abi3-macosx_15_0_fat64
  cp34-abi3-macosx_15_0_fat32
  cp34-abi3-macosx_15_0_universal2
  cp34-abi3-macosx_15_0_universal
  cp34-abi3-macosx_14_0_x86_64
  cp34-abi3-macosx_14_0_intel
  cp34-abi3-macosx_14_0_fat64
  cp34-abi3-macosx_14_0_fat32
  cp34-abi3-macosx_14_0_universal2
  cp34-abi3-macosx_14_0_universal
  cp34-abi3-macosx_13_0_x86_64
  cp34-abi3-macosx_13_0_intel
  cp34-abi3-macosx_13_0_fat64
  cp34-abi3-macosx_13_0_fat32
  cp34-abi3-macosx_13_0_universal2
  cp34-abi3-macosx_13_0_universal
  cp34-abi3-macosx_12_0_x86_64
  cp34-abi3-macosx_12_0_intel
  cp34-abi3-macosx_12_0_fat64
  cp34-abi3-macosx_12_0_fat32
  cp34-abi3-macosx_12_0_universal2
  cp34-abi3-macosx_12_0_universal
  cp34-abi3-macosx_11_0_x86_64
  cp34-abi3-macosx_11_0_intel
  cp34-abi3-macosx_11_0_fat64
  cp34-abi3-macosx_11_0_fat32
  cp34-abi3-macosx_11_0_universal2
  cp34-abi3-macosx_11_0_universal
  cp34-abi3-macosx_10_16_x86_64
  cp34-abi3-macosx_10_16_intel
  cp34-abi3-macosx_10_16_fat64
  cp34-abi3-macosx_10_16_fat32
  cp34-abi3-macosx_10_16_universal2
  cp34-abi3-macosx_10_16_universal
  cp34-abi3-macosx_10_15_x86_64
  cp34-abi3-macosx_10_15_intel
  cp34-abi3-macosx_10_15_fat64
  cp34-abi3-macosx_10_15_fat32
  cp34-abi3-macosx_10_15_universal2
  cp34-abi3-macosx_10_15_universal
  cp34-abi3-macosx_10_14_x86_64
  cp34-abi3-macosx_10_14_intel
  cp34-abi3-macosx_10_14_fat64
  cp34-abi3-macosx_10_14_fat32
  cp34-abi3-macosx_10_14_universal2
  cp34-abi3-macosx_10_14_universal
  cp34-abi3-macosx_10_13_x86_64
  cp34-abi3-macosx_10_13_intel
  cp34-abi3-macosx_10_13_fat64
  cp34-abi3-macosx_10_13_fat32
  cp34-abi3-macosx_10_13_universal2
  cp34-abi3-macosx_10_13_universal
  cp34-abi3-macosx_10_12_x86_64
  cp34-abi3-macosx_10_12_intel
  cp34-abi3-macosx_10_12_fat64
  cp34-abi3-macosx_10_12_fat32
  cp34-abi3-macosx_10_12_universal2
  cp34-abi3-macosx_10_12_universal
  cp34-abi3-macosx_10_11_x86_64
  cp34-abi3-macosx_10_11_intel
  cp34-abi3-macosx_10_11_fat64
  cp34-abi3-macosx_10_11_fat32
  cp34-abi3-macosx_10_11_universal2
  cp34-abi3-macosx_10_11_universal
  cp34-abi3-macosx_10_10_x86_64
  cp34-abi3-macosx_10_10_intel
  cp34-abi3-macosx_10_10_fat64
  cp34-abi3-macosx_10_10_fat32
  cp34-abi3-macosx_10_10_universal2
  cp34-abi3-macosx_10_10_universal
  cp34-abi3-macosx_10_9_x86_64
  cp34-abi3-macosx_10_9_intel
  cp34-abi3-macosx_10_9_fat64
  cp34-abi3-macosx_10_9_fat32
  cp34-abi3-macosx_10_9_universal2
  cp34-abi3-macosx_10_9_universal
  cp34-abi3-macosx_10_8_x86_64
  cp34-abi3-macosx_10_8_intel
  cp34-abi3-macosx_10_8_fat64
  cp34-abi3-macosx_10_8_fat32
  cp34-abi3-macosx_10_8_universal2
  cp34-abi3-macosx_10_8_universal
  cp34-abi3-macosx_10_7_x86_64
  cp34-abi3-macosx_10_7_intel
  cp34-abi3-macosx_10_7_fat64
  cp34-abi3-macosx_10_7_fat32
  cp34-abi3-macosx_10_7_universal2
  cp34-abi3-macosx_10_7_universal
  cp34-abi3-macosx_10_6_x86_64
  cp34-abi3-macosx_10_6_intel
  cp34-abi3-macosx_10_6_fat64
  cp34-abi3-macosx_10_6_fat32
  cp34-abi3-macosx_10_6_universal2
  cp34-abi3-macosx_10_6_universal
  cp34-abi3-macosx_10_5_x86_64
  cp34-abi3-macosx_10_5_intel
  cp34-abi3-macosx_10_5_fat64
  cp34-abi3-macosx_10_5_fat32
  cp34-abi3-macosx_10_5_universal2
  cp34-abi3-macosx_10_5_universal
  cp34-abi3-macosx_10_4_x86_64
  cp34-abi3-macosx_10_4_intel
  cp34-abi3-macosx_10_4_fat64
  cp34-abi3-macosx_10_4_fat32
  cp34-abi3-macosx_10_4_universal2
  cp34-abi3-macosx_10_4_universal
  cp33-abi3-macosx_15_0_x86_64
  cp33-abi3-macosx_15_0_intel
  cp33-abi3-macosx_15_0_fat64
  cp33-abi3-macosx_15_0_fat32
  cp33-abi3-macosx_15_0_universal2
  cp33-abi3-macosx_15_0_universal
  cp33-abi3-macosx_14_0_x86_64
  cp33-abi3-macosx_14_0_intel
  cp33-abi3-macosx_14_0_fat64
  cp33-abi3-macosx_14_0_fat32
  cp33-abi3-macosx_14_0_universal2
  cp33-abi3-macosx_14_0_universal
  cp33-abi3-macosx_13_0_x86_64
  cp33-abi3-macosx_13_0_intel
  cp33-abi3-macosx_13_0_fat64
  cp33-abi3-macosx_13_0_fat32
  cp33-abi3-macosx_13_0_universal2
  cp33-abi3-macosx_13_0_universal
  cp33-abi3-macosx_12_0_x86_64
  cp33-abi3-macosx_12_0_intel
  cp33-abi3-macosx_12_0_fat64
  cp33-abi3-macosx_12_0_fat32
  cp33-abi3-macosx_12_0_universal2
  cp33-abi3-macosx_12_0_universal
  cp33-abi3-macosx_11_0_x86_64
  cp33-abi3-macosx_11_0_intel
  cp33-abi3-macosx_11_0_fat64
  cp33-abi3-macosx_11_0_fat32
  cp33-abi3-macosx_11_0_universal2
  cp33-abi3-macosx_11_0_universal
  cp33-abi3-macosx_10_16_x86_64
  cp33-abi3-macosx_10_16_intel
  cp33-abi3-macosx_10_16_fat64
  cp33-abi3-macosx_10_16_fat32
  cp33-abi3-macosx_10_16_universal2
  cp33-abi3-macosx_10_16_universal
  cp33-abi3-macosx_10_15_x86_64
  cp33-abi3-macosx_10_15_intel
  cp33-abi3-macosx_10_15_fat64
  cp33-abi3-macosx_10_15_fat32
  cp33-abi3-macosx_10_15_universal2
  cp33-abi3-macosx_10_15_universal
  cp33-abi3-macosx_10_14_x86_64
  cp33-abi3-macosx_10_14_intel
  cp33-abi3-macosx_10_14_fat64
  cp33-abi3-macosx_10_14_fat32
  cp33-abi3-macosx_10_14_universal2
  cp33-abi3-macosx_10_14_universal
  cp33-abi3-macosx_10_13_x86_64
  cp33-abi3-macosx_10_13_intel
  cp33-abi3-macosx_10_13_fat64
  cp33-abi3-macosx_10_13_fat32
  cp33-abi3-macosx_10_13_universal2
  cp33-abi3-macosx_10_13_universal
  cp33-abi3-macosx_10_12_x86_64
  cp33-abi3-macosx_10_12_intel
  cp33-abi3-macosx_10_12_fat64
  cp33-abi3-macosx_10_12_fat32
  cp33-abi3-macosx_10_12_universal2
  cp33-abi3-macosx_10_12_universal
  cp33-abi3-macosx_10_11_x86_64
  cp33-abi3-macosx_10_11_intel
  cp33-abi3-macosx_10_11_fat64
  cp33-abi3-macosx_10_11_fat32
  cp33-abi3-macosx_10_11_universal2
  cp33-abi3-macosx_10_11_universal
  cp33-abi3-macosx_10_10_x86_64
  cp33-abi3-macosx_10_10_intel
  cp33-abi3-macosx_10_10_fat64
  cp33-abi3-macosx_10_10_fat32
  cp33-abi3-macosx_10_10_universal2
  cp33-abi3-macosx_10_10_universal
  cp33-abi3-macosx_10_9_x86_64
  cp33-abi3-macosx_10_9_intel
  cp33-abi3-macosx_10_9_fat64
  cp33-abi3-macosx_10_9_fat32
  cp33-abi3-macosx_10_9_universal2
  cp33-abi3-macosx_10_9_universal
  cp33-abi3-macosx_10_8_x86_64
  cp33-abi3-macosx_10_8_intel
  cp33-abi3-macosx_10_8_fat64
  cp33-abi3-macosx_10_8_fat32
  cp33-abi3-macosx_10_8_universal2
  cp33-abi3-macosx_10_8_universal
  cp33-abi3-macosx_10_7_x86_64
  cp33-abi3-macosx_10_7_intel
  cp33-abi3-macosx_10_7_fat64
  cp33-abi3-macosx_10_7_fat32
  cp33-abi3-macosx_10_7_universal2
  cp33-abi3-macosx_10_7_universal
  cp33-abi3-macosx_10_6_x86_64
  cp33-abi3-macosx_10_6_intel
  cp33-abi3-macosx_10_6_fat64
  cp33-abi3-macosx_10_6_fat32
  cp33-abi3-macosx_10_6_universal2
  cp33-abi3-macosx_10_6_universal
  cp33-abi3-macosx_10_5_x86_64
  cp33-abi3-macosx_10_5_intel
  cp33-abi3-macosx_10_5_fat64
  cp33-abi3-macosx_10_5_fat32
  cp33-abi3-macosx_10_5_universal2
  cp33-abi3-macosx_10_5_universal
  cp33-abi3-macosx_10_4_x86_64
  cp33-abi3-macosx_10_4_intel
  cp33-abi3-macosx_10_4_fat64
  cp33-abi3-macosx_10_4_fat32
  cp33-abi3-macosx_10_4_universal2
  cp33-abi3-macosx_10_4_universal
  cp32-abi3-macosx_15_0_x86_64
  cp32-abi3-macosx_15_0_intel
  cp32-abi3-macosx_15_0_fat64
  cp32-abi3-macosx_15_0_fat32
  cp32-abi3-macosx_15_0_universal2
  cp32-abi3-macosx_15_0_universal
  cp32-abi3-macosx_14_0_x86_64
  cp32-abi3-macosx_14_0_intel
  cp32-abi3-macosx_14_0_fat64
  cp32-abi3-macosx_14_0_fat32
  cp32-abi3-macosx_14_0_universal2
  cp32-abi3-macosx_14_0_universal
  cp32-abi3-macosx_13_0_x86_64
  cp32-abi3-macosx_13_0_intel
  cp32-abi3-macosx_13_0_fat64
  cp32-abi3-macosx_13_0_fat32
  cp32-abi3-macosx_13_0_universal2
  cp32-abi3-macosx_13_0_universal
  cp32-abi3-macosx_12_0_x86_64
  cp32-abi3-macosx_12_0_intel
  cp32-abi3-macosx_12_0_fat64
  cp32-abi3-macosx_12_0_fat32
  cp32-abi3-macosx_12_0_universal2
  cp32-abi3-macosx_12_0_universal
  cp32-abi3-macosx_11_0_x86_64
  cp32-abi3-macosx_11_0_intel
  cp32-abi3-macosx_11_0_fat64
  cp32-abi3-macosx_11_0_fat32
  cp32-abi3-macosx_11_0_universal2
  cp32-abi3-macosx_11_0_universal
  cp32-abi3-macosx_10_16_x86_64
  cp32-abi3-macosx_10_16_intel
  cp32-abi3-macosx_10_16_fat64
  cp32-abi3-macosx_10_16_fat32
  cp32-abi3-macosx_10_16_universal2
  cp32-abi3-macosx_10_16_universal
  cp32-abi3-macosx_10_15_x86_64
  cp32-abi3-macosx_10_15_intel
  cp32-abi3-macosx_10_15_fat64
  cp32-abi3-macosx_10_15_fat32
  cp32-abi3-macosx_10_15_universal2
  cp32-abi3-macosx_10_15_universal
  cp32-abi3-macosx_10_14_x86_64
  cp32-abi3-macosx_10_14_intel
  cp32-abi3-macosx_10_14_fat64
  cp32-abi3-macosx_10_14_fat32
  cp32-abi3-macosx_10_14_universal2
  cp32-abi3-macosx_10_14_universal
  cp32-abi3-macosx_10_13_x86_64
  cp32-abi3-macosx_10_13_intel
  cp32-abi3-macosx_10_13_fat64
  cp32-abi3-macosx_10_13_fat32
  cp32-abi3-macosx_10_13_universal2
  cp32-abi3-macosx_10_13_universal
  cp32-abi3-macosx_10_12_x86_64
  cp32-abi3-macosx_10_12_intel
  cp32-abi3-macosx_10_12_fat64
  cp32-abi3-macosx_10_12_fat32
  cp32-abi3-macosx_10_12_universal2
  cp32-abi3-macosx_10_12_universal
  cp32-abi3-macosx_10_11_x86_64
  cp32-abi3-macosx_10_11_intel
  cp32-abi3-macosx_10_11_fat64
  cp32-abi3-macosx_10_11_fat32
  cp32-abi3-macosx_10_11_universal2
  cp32-abi3-macosx_10_11_universal
  cp32-abi3-macosx_10_10_x86_64
  cp32-abi3-macosx_10_10_intel
  cp32-abi3-macosx_10_10_fat64
  cp32-abi3-macosx_10_10_fat32
  cp32-abi3-macosx_10_10_universal2
  cp32-abi3-macosx_10_10_universal
  cp32-abi3-macosx_10_9_x86_64
  cp32-abi3-macosx_10_9_intel
  cp32-abi3-macosx_10_9_fat64
  cp32-abi3-macosx_10_9_fat32
  cp32-abi3-macosx_10_9_universal2
  cp32-abi3-macosx_10_9_universal
  cp32-abi3-macosx_10_8_x86_64
  cp32-abi3-macosx_10_8_intel
  cp32-abi3-macosx_10_8_fat64
  cp32-abi3-macosx_10_8_fat32
  cp32-abi3-macosx_10_8_universal2
  cp32-abi3-macosx_10_8_universal
  cp32-abi3-macosx_10_7_x86_64
  cp32-abi3-macosx_10_7_intel
  cp32-abi3-macosx_10_7_fat64
  cp32-abi3-macosx_10_7_fat32
  cp32-abi3-macosx_10_7_universal2
  cp32-abi3-macosx_10_7_universal
  cp32-abi3-macosx_10_6_x86_64
  cp32-abi3-macosx_10_6_intel
  cp32-abi3-macosx_10_6_fat64
  cp32-abi3-macosx_10_6_fat32
  cp32-abi3-macosx_10_6_universal2
  cp32-abi3-macosx_10_6_universal
  cp32-abi3-macosx_10_5_x86_64
  cp32-abi3-macosx_10_5_intel
  cp32-abi3-macosx_10_5_fat64
  cp32-abi3-macosx_10_5_fat32
  cp32-abi3-macosx_10_5_universal2
  cp32-abi3-macosx_10_5_universal
  cp32-abi3-macosx_10_4_x86_64
  cp32-abi3-macosx_10_4_intel
  cp32-abi3-macosx_10_4_fat64
  cp32-abi3-macosx_10_4_fat32
  cp32-abi3-macosx_10_4_universal2
  cp32-abi3-macosx_10_4_universal
  py312-none-macosx_15_0_x86_64
  py312-none-macosx_15_0_intel
  py312-none-macosx_15_0_fat64
  py312-none-macosx_15_0_fat32
  py312-none-macosx_15_0_universal2
  py312-none-macosx_15_0_universal
  py312-none-macosx_14_0_x86_64
  py312-none-macosx_14_0_intel
  py312-none-macosx_14_0_fat64
  py312-none-macosx_14_0_fat32
  py312-none-macosx_14_0_universal2
  py312-none-macosx_14_0_universal
  py312-none-macosx_13_0_x86_64
  py312-none-macosx_13_0_intel
  py312-none-macosx_13_0_fat64
  py312-none-macosx_13_0_fat32
  py312-none-macosx_13_0_universal2
  py312-none-macosx_13_0_universal
  py312-none-macosx_12_0_x86_64
  py312-none-macosx_12_0_intel
  py312-none-macosx_12_0_fat64
  py312-none-macosx_12_0_fat32
  py312-none-macosx_12_0_universal2
  py312-none-macosx_12_0_universal
  py312-none-macosx_11_0_x86_64
  py312-none-macosx_11_0_intel
  py312-none-macosx_11_0_fat64
  py312-none-macosx_11_0_fat32
  py312-none-macosx_11_0_universal2
  py312-none-macosx_11_0_universal
  py312-none-macosx_10_16_x86_64
  py312-none-macosx_10_16_intel
  py312-none-macosx_10_16_fat64
  py312-none-macosx_10_16_fat32
  py312-none-macosx_10_16_universal2
  py312-none-macosx_10_16_universal
  py312-none-macosx_10_15_x86_64
  py312-none-macosx_10_15_intel
  py312-none-macosx_10_15_fat64
  py312-none-macosx_10_15_fat32
  py312-none-macosx_10_15_universal2
  py312-none-macosx_10_15_universal
  py312-none-macosx_10_14_x86_64
  py312-none-macosx_10_14_intel
  py312-none-macosx_10_14_fat64
  py312-none-macosx_10_14_fat32
  py312-none-macosx_10_14_universal2
  py312-none-macosx_10_14_universal
  py312-none-macosx_10_13_x86_64
  py312-none-macosx_10_13_intel
  py312-none-macosx_10_13_fat64
  py312-none-macosx_10_13_fat32
  py312-none-macosx_10_13_universal2
  py312-none-macosx_10_13_universal
  py312-none-macosx_10_12_x86_64
  py312-none-macosx_10_12_intel
  py312-none-macosx_10_12_fat64
  py312-none-macosx_10_12_fat32
  py312-none-macosx_10_12_universal2
  py312-none-macosx_10_12_universal
  py312-none-macosx_10_11_x86_64
  py312-none-macosx_10_11_intel
  py312-none-macosx_10_11_fat64
  py312-none-macosx_10_11_fat32
  py312-none-macosx_10_11_universal2
  py312-none-macosx_10_11_universal
  py312-none-macosx_10_10_x86_64
  py312-none-macosx_10_10_intel
  py312-none-macosx_10_10_fat64
  py312-none-macosx_10_10_fat32
  py312-none-macosx_10_10_universal2
  py312-none-macosx_10_10_universal
  py312-none-macosx_10_9_x86_64
  py312-none-macosx_10_9_intel
  py312-none-macosx_10_9_fat64
  py312-none-macosx_10_9_fat32
  py312-none-macosx_10_9_universal2
  py312-none-macosx_10_9_universal
  py312-none-macosx_10_8_x86_64
  py312-none-macosx_10_8_intel
  py312-none-macosx_10_8_fat64
  py312-none-macosx_10_8_fat32
  py312-none-macosx_10_8_universal2
  py312-none-macosx_10_8_universal
  py312-none-macosx_10_7_x86_64
  py312-none-macosx_10_7_intel
  py312-none-macosx_10_7_fat64
  py312-none-macosx_10_7_fat32
  py312-none-macosx_10_7_universal2
  py312-none-macosx_10_7_universal
  py312-none-macosx_10_6_x86_64
  py312-none-macosx_10_6_intel
  py312-none-macosx_10_6_fat64
  py312-none-macosx_10_6_fat32
  py312-none-macosx_10_6_universal2
  py312-none-macosx_10_6_universal
  py312-none-macosx_10_5_x86_64
  py312-none-macosx_10_5_intel
  py312-none-macosx_10_5_fat64
  py312-none-macosx_10_5_fat32
  py312-none-macosx_10_5_universal2
  py312-none-macosx_10_5_universal
  py312-none-macosx_10_4_x86_64
  py312-none-macosx_10_4_intel
  py312-none-macosx_10_4_fat64
  py312-none-macosx_10_4_fat32
  py312-none-macosx_10_4_universal2
  py312-none-macosx_10_4_universal
  py3-none-macosx_15_0_x86_64
  py3-none-macosx_15_0_intel
  py3-none-macosx_15_0_fat64
  py3-none-macosx_15_0_fat32
  py3-none-macosx_15_0_universal2
  py3-none-macosx_15_0_universal
  py3-none-macosx_14_0_x86_64
  py3-none-macosx_14_0_intel
  py3-none-macosx_14_0_fat64
  py3-none-macosx_14_0_fat32
  py3-none-macosx_14_0_universal2
  py3-none-macosx_14_0_universal
  py3-none-macosx_13_0_x86_64
  py3-none-macosx_13_0_intel
  py3-none-macosx_13_0_fat64
  py3-none-macosx_13_0_fat32
  py3-none-macosx_13_0_universal2
  py3-none-macosx_13_0_universal
  py3-none-macosx_12_0_x86_64
  py3-none-macosx_12_0_intel
  py3-none-macosx_12_0_fat64
  py3-none-macosx_12_0_fat32
  py3-none-macosx_12_0_universal2
  py3-none-macosx_12_0_universal
  py3-none-macosx_11_0_x86_64
  py3-none-macosx_11_0_intel
  py3-none-macosx_11_0_fat64
  py3-none-macosx_11_0_fat32
  py3-none-macosx_11_0_universal2
  py3-none-macosx_11_0_universal
  py3-none-macosx_10_16_x86_64
  py3-none-macosx_10_16_intel
  py3-none-macosx_10_16_fat64
  py3-none-macosx_10_16_fat32
  py3-none-macosx_10_16_universal2
  py3-none-macosx_10_16_universal
  py3-none-macosx_10_15_x86_64
  py3-none-macosx_10_15_intel
  py3-none-macosx_10_15_fat64
  py3-none-macosx_10_15_fat32
  py3-none-macosx_10_15_universal2
  py3-none-macosx_10_15_universal
  py3-none-macosx_10_14_x86_64
  py3-none-macosx_10_14_intel
  py3-none-macosx_10_14_fat64
  py3-none-macosx_10_14_fat32
  py3-none-macosx_10_14_universal2
  py3-none-macosx_10_14_universal
  py3-none-macosx_10_13_x86_64
  py3-none-macosx_10_13_intel
  py3-none-macosx_10_13_fat64
  py3-none-macosx_10_13_fat32
  py3-none-macosx_10_13_universal2
  py3-none-macosx_10_13_universal
  py3-none-macosx_10_12_x86_64
  py3-none-macosx_10_12_intel
  py3-none-macosx_10_12_fat64
  py3-none-macosx_10_12_fat32
  py3-none-macosx_10_12_universal2
  py3-none-macosx_10_12_universal
  py3-none-macosx_10_11_x86_64
  py3-none-macosx_10_11_intel
  py3-none-macosx_10_11_fat64
  py3-none-macosx_10_11_fat32
  py3-none-macosx_10_11_universal2
  py3-none-macosx_10_11_universal
  py3-none-macosx_10_10_x86_64
  py3-none-macosx_10_10_intel
  py3-none-macosx_10_10_fat64
  py3-none-macosx_10_10_fat32
  py3-none-macosx_10_10_universal2
  py3-none-macosx_10_10_universal
  py3-none-macosx_10_9_x86_64
  py3-none-macosx_10_9_intel
  py3-none-macosx_10_9_fat64
  py3-none-macosx_10_9_fat32
  py3-none-macosx_10_9_universal2
  py3-none-macosx_10_9_universal
  py3-none-macosx_10_8_x86_64
  py3-none-macosx_10_8_intel
  py3-none-macosx_10_8_fat64
  py3-none-macosx_10_8_fat32
  py3-none-macosx_10_8_universal2
  py3-none-macosx_10_8_universal
  py3-none-macosx_10_7_x86_64
  py3-none-macosx_10_7_intel
  py3-none-macosx_10_7_fat64
  py3-none-macosx_10_7_fat32
  py3-none-macosx_10_7_universal2
  py3-none-macosx_10_7_universal
  py3-none-macosx_10_6_x86_64
  py3-none-macosx_10_6_intel
  py3-none-macosx_10_6_fat64
  py3-none-macosx_10_6_fat32
  py3-none-macosx_10_6_universal2
  py3-none-macosx_10_6_universal
  py3-none-macosx_10_5_x86_64
  py3-none-macosx_10_5_intel
  py3-none-macosx_10_5_fat64
  py3-none-macosx_10_5_fat32
  py3-none-macosx_10_5_universal2
  py3-none-macosx_10_5_universal
  py3-none-macosx_10_4_x86_64
  py3-none-macosx_10_4_intel
  py3-none-macosx_10_4_fat64
  py3-none-macosx_10_4_fat32
  py3-none-macosx_10_4_universal2
  py3-none-macosx_10_4_universal
  py311-none-macosx_15_0_x86_64
  py311-none-macosx_15_0_intel
  py311-none-macosx_15_0_fat64
  py311-none-macosx_15_0_fat32
  py311-none-macosx_15_0_universal2
  py311-none-macosx_15_0_universal
  py311-none-macosx_14_0_x86_64
  py311-none-macosx_14_0_intel
  py311-none-macosx_14_0_fat64
  py311-none-macosx_14_0_fat32
  py311-none-macosx_14_0_universal2
  py311-none-macosx_14_0_universal
  py311-none-macosx_13_0_x86_64
  py311-none-macosx_13_0_intel
  py311-none-macosx_13_0_fat64
  py311-none-macosx_13_0_fat32
  py311-none-macosx_13_0_universal2
  py311-none-macosx_13_0_universal
  py311-none-macosx_12_0_x86_64
  py311-none-macosx_12_0_intel
  py311-none-macosx_12_0_fat64
  py311-none-macosx_12_0_fat32
  py311-none-macosx_12_0_universal2
  py311-none-macosx_12_0_universal
  py311-none-macosx_11_0_x86_64
  py311-none-macosx_11_0_intel
  py311-none-macosx_11_0_fat64
  py311-none-macosx_11_0_fat32
  py311-none-macosx_11_0_universal2
  py311-none-macosx_11_0_universal
  py311-none-macosx_10_16_x86_64
  py311-none-macosx_10_16_intel
  py311-none-macosx_10_16_fat64
  py311-none-macosx_10_16_fat32
  py311-none-macosx_10_16_universal2
  py311-none-macosx_10_16_universal
  py311-none-macosx_10_15_x86_64
  py311-none-macosx_10_15_intel
  py311-none-macosx_10_15_fat64
  py311-none-macosx_10_15_fat32
  py311-none-macosx_10_15_universal2
  py311-none-macosx_10_15_universal
  py311-none-macosx_10_14_x86_64
  py311-none-macosx_10_14_intel
  py311-none-macosx_10_14_fat64
  py311-none-macosx_10_14_fat32
  py311-none-macosx_10_14_universal2
  py311-none-macosx_10_14_universal
  py311-none-macosx_10_13_x86_64
  py311-none-macosx_10_13_intel
  py311-none-macosx_10_13_fat64
  py311-none-macosx_10_13_fat32
  py311-none-macosx_10_13_universal2
  py311-none-macosx_10_13_universal
  py311-none-macosx_10_12_x86_64
  py311-none-macosx_10_12_intel
  py311-none-macosx_10_12_fat64
  py311-none-macosx_10_12_fat32
  py311-none-macosx_10_12_universal2
  py311-none-macosx_10_12_universal
  py311-none-macosx_10_11_x86_64
  py311-none-macosx_10_11_intel
  py311-none-macosx_10_11_fat64
  py311-none-macosx_10_11_fat32
  py311-none-macosx_10_11_universal2
  py311-none-macosx_10_11_universal
  py311-none-macosx_10_10_x86_64
  py311-none-macosx_10_10_intel
  py311-none-macosx_10_10_fat64
  py311-none-macosx_10_10_fat32
  py311-none-macosx_10_10_universal2
  py311-none-macosx_10_10_universal
  py311-none-macosx_10_9_x86_64
  py311-none-macosx_10_9_intel
  py311-none-macosx_10_9_fat64
  py311-none-macosx_10_9_fat32
  py311-none-macosx_10_9_universal2
  py311-none-macosx_10_9_universal
  py311-none-macosx_10_8_x86_64
  py311-none-macosx_10_8_intel
  py311-none-macosx_10_8_fat64
  py311-none-macosx_10_8_fat32
  py311-none-macosx_10_8_universal2
  py311-none-macosx_10_8_universal
  py311-none-macosx_10_7_x86_64
  py311-none-macosx_10_7_intel
  py311-none-macosx_10_7_fat64
  py311-none-macosx_10_7_fat32
  py311-none-macosx_10_7_universal2
  py311-none-macosx_10_7_universal
  py311-none-macosx_10_6_x86_64
  py311-none-macosx_10_6_intel
  py311-none-macosx_10_6_fat64
  py311-none-macosx_10_6_fat32
  py311-none-macosx_10_6_universal2
  py311-none-macosx_10_6_universal
  py311-none-macosx_10_5_x86_64
  py311-none-macosx_10_5_intel
  py311-none-macosx_10_5_fat64
  py311-none-macosx_10_5_fat32
  py311-none-macosx_10_5_universal2
  py311-none-macosx_10_5_universal
  py311-none-macosx_10_4_x86_64
  py311-none-macosx_10_4_intel
  py311-none-macosx_10_4_fat64
  py311-none-macosx_10_4_fat32
  py311-none-macosx_10_4_universal2
  py311-none-macosx_10_4_universal
  py310-none-macosx_15_0_x86_64
  py310-none-macosx_15_0_intel
  py310-none-macosx_15_0_fat64
  py310-none-macosx_15_0_fat32
  py310-none-macosx_15_0_universal2
  py310-none-macosx_15_0_universal
  py310-none-macosx_14_0_x86_64
  py310-none-macosx_14_0_intel
  py310-none-macosx_14_0_fat64
  py310-none-macosx_14_0_fat32
  py310-none-macosx_14_0_universal2
  py310-none-macosx_14_0_universal
  py310-none-macosx_13_0_x86_64
  py310-none-macosx_13_0_intel
  py310-none-macosx_13_0_fat64
  py310-none-macosx_13_0_fat32
  py310-none-macosx_13_0_universal2
  py310-none-macosx_13_0_universal
  py310-none-macosx_12_0_x86_64
  py310-none-macosx_12_0_intel
  py310-none-macosx_12_0_fat64
  py310-none-macosx_12_0_fat32
  py310-none-macosx_12_0_universal2
  py310-none-macosx_12_0_universal
  py310-none-macosx_11_0_x86_64
  py310-none-macosx_11_0_intel
  py310-none-macosx_11_0_fat64
  py310-none-macosx_11_0_fat32
  py310-none-macosx_11_0_universal2
  py310-none-macosx_11_0_universal
  py310-none-macosx_10_16_x86_64
  py310-none-macosx_10_16_intel
  py310-none-macosx_10_16_fat64
  py310-none-macosx_10_16_fat32
  py310-none-macosx_10_16_universal2
  py310-none-macosx_10_16_universal
  py310-none-macosx_10_15_x86_64
  py310-none-macosx_10_15_intel
  py310-none-macosx_10_15_fat64
  py310-none-macosx_10_15_fat32
  py310-none-macosx_10_15_universal2
  py310-none-macosx_10_15_universal
  py310-none-macosx_10_14_x86_64
  py310-none-macosx_10_14_intel
  py310-none-macosx_10_14_fat64
  py310-none-macosx_10_14_fat32
  py310-none-macosx_10_14_universal2
  py310-none-macosx_10_14_universal
  py310-none-macosx_10_13_x86_64
  py310-none-macosx_10_13_intel
  py310-none-macosx_10_13_fat64
  py310-none-macosx_10_13_fat32
  py310-none-macosx_10_13_universal2
  py310-none-macosx_10_13_universal
  py310-none-macosx_10_12_x86_64
  py310-none-macosx_10_12_intel
  py310-none-macosx_10_12_fat64
  py310-none-macosx_10_12_fat32
  py310-none-macosx_10_12_universal2
  py310-none-macosx_10_12_universal
  py310-none-macosx_10_11_x86_64
  py310-none-macosx_10_11_intel
  py310-none-macosx_10_11_fat64
  py310-none-macosx_10_11_fat32
  py310-none-macosx_10_11_universal2
  py310-none-macosx_10_11_universal
  py310-none-macosx_10_10_x86_64
  py310-none-macosx_10_10_intel
  py310-none-macosx_10_10_fat64
  py310-none-macosx_10_10_fat32
  py310-none-macosx_10_10_universal2
  py310-none-macosx_10_10_universal
  py310-none-macosx_10_9_x86_64
  py310-none-macosx_10_9_intel
  py310-none-macosx_10_9_fat64
  py310-none-macosx_10_9_fat32
  py310-none-macosx_10_9_universal2
  py310-none-macosx_10_9_universal
  py310-none-macosx_10_8_x86_64
  py310-none-macosx_10_8_intel
  py310-none-macosx_10_8_fat64
  py310-none-macosx_10_8_fat32
  py310-none-macosx_10_8_universal2
  py310-none-macosx_10_8_universal
  py310-none-macosx_10_7_x86_64
  py310-none-macosx_10_7_intel
  py310-none-macosx_10_7_fat64
  py310-none-macosx_10_7_fat32
  py310-none-macosx_10_7_universal2
  py310-none-macosx_10_7_universal
  py310-none-macosx_10_6_x86_64
  py310-none-macosx_10_6_intel
  py310-none-macosx_10_6_fat64
  py310-none-macosx_10_6_fat32
  py310-none-macosx_10_6_universal2
  py310-none-macosx_10_6_universal
  py310-none-macosx_10_5_x86_64
  py310-none-macosx_10_5_intel
  py310-none-macosx_10_5_fat64
  py310-none-macosx_10_5_fat32
  py310-none-macosx_10_5_universal2
  py310-none-macosx_10_5_universal
  py310-none-macosx_10_4_x86_64
  py310-none-macosx_10_4_intel
  py310-none-macosx_10_4_fat64
  py310-none-macosx_10_4_fat32
  py310-none-macosx_10_4_universal2
  py310-none-macosx_10_4_universal
  py39-none-macosx_15_0_x86_64
  py39-none-macosx_15_0_intel
  py39-none-macosx_15_0_fat64
  py39-none-macosx_15_0_fat32
  py39-none-macosx_15_0_universal2
  py39-none-macosx_15_0_universal
  py39-none-macosx_14_0_x86_64
  py39-none-macosx_14_0_intel
  py39-none-macosx_14_0_fat64
  py39-none-macosx_14_0_fat32
  py39-none-macosx_14_0_universal2
  py39-none-macosx_14_0_universal
  py39-none-macosx_13_0_x86_64
  py39-none-macosx_13_0_intel
  py39-none-macosx_13_0_fat64
  py39-none-macosx_13_0_fat32
  py39-none-macosx_13_0_universal2
  py39-none-macosx_13_0_universal
  py39-none-macosx_12_0_x86_64
  py39-none-macosx_12_0_intel
  py39-none-macosx_12_0_fat64
  py39-none-macosx_12_0_fat32
  py39-none-macosx_12_0_universal2
  py39-none-macosx_12_0_universal
  py39-none-macosx_11_0_x86_64
  py39-none-macosx_11_0_intel
  py39-none-macosx_11_0_fat64
  py39-none-macosx_11_0_fat32
  py39-none-macosx_11_0_universal2
  py39-none-macosx_11_0_universal
  py39-none-macosx_10_16_x86_64
  py39-none-macosx_10_16_intel
  py39-none-macosx_10_16_fat64
  py39-none-macosx_10_16_fat32
  py39-none-macosx_10_16_universal2
  py39-none-macosx_10_16_universal
  py39-none-macosx_10_15_x86_64
  py39-none-macosx_10_15_intel
  py39-none-macosx_10_15_fat64
  py39-none-macosx_10_15_fat32
  py39-none-macosx_10_15_universal2
  py39-none-macosx_10_15_universal
  py39-none-macosx_10_14_x86_64
  py39-none-macosx_10_14_intel
  py39-none-macosx_10_14_fat64
  py39-none-macosx_10_14_fat32
  py39-none-macosx_10_14_universal2
  py39-none-macosx_10_14_universal
  py39-none-macosx_10_13_x86_64
  py39-none-macosx_10_13_intel
  py39-none-macosx_10_13_fat64
  py39-none-macosx_10_13_fat32
  py39-none-macosx_10_13_universal2
  py39-none-macosx_10_13_universal
  py39-none-macosx_10_12_x86_64
  py39-none-macosx_10_12_intel
  py39-none-macosx_10_12_fat64
  py39-none-macosx_10_12_fat32
  py39-none-macosx_10_12_universal2
  py39-none-macosx_10_12_universal
  py39-none-macosx_10_11_x86_64
  py39-none-macosx_10_11_intel
  py39-none-macosx_10_11_fat64
  py39-none-macosx_10_11_fat32
  py39-none-macosx_10_11_universal2
  py39-none-macosx_10_11_universal
  py39-none-macosx_10_10_x86_64
  py39-none-macosx_10_10_intel
  py39-none-macosx_10_10_fat64
  py39-none-macosx_10_10_fat32
  py39-none-macosx_10_10_universal2
  py39-none-macosx_10_10_universal
  py39-none-macosx_10_9_x86_64
  py39-none-macosx_10_9_intel
  py39-none-macosx_10_9_fat64
  py39-none-macosx_10_9_fat32
  py39-none-macosx_10_9_universal2
  py39-none-macosx_10_9_universal
  py39-none-macosx_10_8_x86_64
  py39-none-macosx_10_8_intel
  py39-none-macosx_10_8_fat64
  py39-none-macosx_10_8_fat32
  py39-none-macosx_10_8_universal2
  py39-none-macosx_10_8_universal
  py39-none-macosx_10_7_x86_64
  py39-none-macosx_10_7_intel
  py39-none-macosx_10_7_fat64
  py39-none-macosx_10_7_fat32
  py39-none-macosx_10_7_universal2
  py39-none-macosx_10_7_universal
  py39-none-macosx_10_6_x86_64
  py39-none-macosx_10_6_intel
  py39-none-macosx_10_6_fat64
  py39-none-macosx_10_6_fat32
  py39-none-macosx_10_6_universal2
  py39-none-macosx_10_6_universal
  py39-none-macosx_10_5_x86_64
  py39-none-macosx_10_5_intel
  py39-none-macosx_10_5_fat64
  py39-none-macosx_10_5_fat32
  py39-none-macosx_10_5_universal2
  py39-none-macosx_10_5_universal
  py39-none-macosx_10_4_x86_64
  py39-none-macosx_10_4_intel
  py39-none-macosx_10_4_fat64
  py39-none-macosx_10_4_fat32
  py39-none-macosx_10_4_universal2
  py39-none-macosx_10_4_universal
  py38-none-macosx_15_0_x86_64
  py38-none-macosx_15_0_intel
  py38-none-macosx_15_0_fat64
  py38-none-macosx_15_0_fat32
  py38-none-macosx_15_0_universal2
  py38-none-macosx_15_0_universal
  py38-none-macosx_14_0_x86_64
  py38-none-macosx_14_0_intel
  py38-none-macosx_14_0_fat64
  py38-none-macosx_14_0_fat32
  py38-none-macosx_14_0_universal2
  py38-none-macosx_14_0_universal
  py38-none-macosx_13_0_x86_64
  py38-none-macosx_13_0_intel
  py38-none-macosx_13_0_fat64
  py38-none-macosx_13_0_fat32
  py38-none-macosx_13_0_universal2
  py38-none-macosx_13_0_universal
  py38-none-macosx_12_0_x86_64
  py38-none-macosx_12_0_intel
  py38-none-macosx_12_0_fat64
  py38-none-macosx_12_0_fat32
  py38-none-macosx_12_0_universal2
  py38-none-macosx_12_0_universal
  py38-none-macosx_11_0_x86_64
  py38-none-macosx_11_0_intel
  py38-none-macosx_11_0_fat64
  py38-none-macosx_11_0_fat32
  py38-none-macosx_11_0_universal2
  py38-none-macosx_11_0_universal
  py38-none-macosx_10_16_x86_64
  py38-none-macosx_10_16_intel
  py38-none-macosx_10_16_fat64
  py38-none-macosx_10_16_fat32
  py38-none-macosx_10_16_universal2
  py38-none-macosx_10_16_universal
  py38-none-macosx_10_15_x86_64
  py38-none-macosx_10_15_intel
  py38-none-macosx_10_15_fat64
  py38-none-macosx_10_15_fat32
  py38-none-macosx_10_15_universal2
  py38-none-macosx_10_15_universal
  py38-none-macosx_10_14_x86_64
  py38-none-macosx_10_14_intel
  py38-none-macosx_10_14_fat64
  py38-none-macosx_10_14_fat32
  py38-none-macosx_10_14_universal2
  py38-none-macosx_10_14_universal
  py38-none-macosx_10_13_x86_64
  py38-none-macosx_10_13_intel
  py38-none-macosx_10_13_fat64
  py38-none-macosx_10_13_fat32
  py38-none-macosx_10_13_universal2
  py38-none-macosx_10_13_universal
  py38-none-macosx_10_12_x86_64
  py38-none-macosx_10_12_intel
  py38-none-macosx_10_12_fat64
  py38-none-macosx_10_12_fat32
  py38-none-macosx_10_12_universal2
  py38-none-macosx_10_12_universal
  py38-none-macosx_10_11_x86_64
  py38-none-macosx_10_11_intel
  py38-none-macosx_10_11_fat64
  py38-none-macosx_10_11_fat32
  py38-none-macosx_10_11_universal2
  py38-none-macosx_10_11_universal
  py38-none-macosx_10_10_x86_64
  py38-none-macosx_10_10_intel
  py38-none-macosx_10_10_fat64
  py38-none-macosx_10_10_fat32
  py38-none-macosx_10_10_universal2
  py38-none-macosx_10_10_universal
  py38-none-macosx_10_9_x86_64
  py38-none-macosx_10_9_intel
  py38-none-macosx_10_9_fat64
  py38-none-macosx_10_9_fat32
  py38-none-macosx_10_9_universal2
  py38-none-macosx_10_9_universal
  py38-none-macosx_10_8_x86_64
  py38-none-macosx_10_8_intel
  py38-none-macosx_10_8_fat64
  py38-none-macosx_10_8_fat32
  py38-none-macosx_10_8_universal2
  py38-none-macosx_10_8_universal
  py38-none-macosx_10_7_x86_64
  py38-none-macosx_10_7_intel
  py38-none-macosx_10_7_fat64
  py38-none-macosx_10_7_fat32
  py38-none-macosx_10_7_universal2
  py38-none-macosx_10_7_universal
  py38-none-macosx_10_6_x86_64
  py38-none-macosx_10_6_intel
  py38-none-macosx_10_6_fat64
  py38-none-macosx_10_6_fat32
  py38-none-macosx_10_6_universal2
  py38-none-macosx_10_6_universal
  py38-none-macosx_10_5_x86_64
  py38-none-macosx_10_5_intel
  py38-none-macosx_10_5_fat64
  py38-none-macosx_10_5_fat32
  py38-none-macosx_10_5_universal2
  py38-none-macosx_10_5_universal
  py38-none-macosx_10_4_x86_64
  py38-none-macosx_10_4_intel
  py38-none-macosx_10_4_fat64
  py38-none-macosx_10_4_fat32
  py38-none-macosx_10_4_universal2
  py38-none-macosx_10_4_universal
  py37-none-macosx_15_0_x86_64
  py37-none-macosx_15_0_intel
  py37-none-macosx_15_0_fat64
  py37-none-macosx_15_0_fat32
  py37-none-macosx_15_0_universal2
  py37-none-macosx_15_0_universal
  py37-none-macosx_14_0_x86_64
  py37-none-macosx_14_0_intel
  py37-none-macosx_14_0_fat64
  py37-none-macosx_14_0_fat32
  py37-none-macosx_14_0_universal2
  py37-none-macosx_14_0_universal
  py37-none-macosx_13_0_x86_64
  py37-none-macosx_13_0_intel
  py37-none-macosx_13_0_fat64
  py37-none-macosx_13_0_fat32
  py37-none-macosx_13_0_universal2
  py37-none-macosx_13_0_universal
  py37-none-macosx_12_0_x86_64
  py37-none-macosx_12_0_intel
  py37-none-macosx_12_0_fat64
  py37-none-macosx_12_0_fat32
  py37-none-macosx_12_0_universal2
  py37-none-macosx_12_0_universal
  py37-none-macosx_11_0_x86_64
  py37-none-macosx_11_0_intel
  py37-none-macosx_11_0_fat64
  py37-none-macosx_11_0_fat32
  py37-none-macosx_11_0_universal2
  py37-none-macosx_11_0_universal
  py37-none-macosx_10_16_x86_64
  py37-none-macosx_10_16_intel
  py37-none-macosx_10_16_fat64
  py37-none-macosx_10_16_fat32
  py37-none-macosx_10_16_universal2
  py37-none-macosx_10_16_universal
  py37-none-macosx_10_15_x86_64
  py37-none-macosx_10_15_intel
  py37-none-macosx_10_15_fat64
  py37-none-macosx_10_15_fat32
  py37-none-macosx_10_15_universal2
  py37-none-macosx_10_15_universal
  py37-none-macosx_10_14_x86_64
  py37-none-macosx_10_14_intel
  py37-none-macosx_10_14_fat64
  py37-none-macosx_10_14_fat32
  py37-none-macosx_10_14_universal2
  py37-none-macosx_10_14_universal
  py37-none-macosx_10_13_x86_64
  py37-none-macosx_10_13_intel
  py37-none-macosx_10_13_fat64
  py37-none-macosx_10_13_fat32
  py37-none-macosx_10_13_universal2
  py37-none-macosx_10_13_universal
  py37-none-macosx_10_12_x86_64
  py37-none-macosx_10_12_intel
  py37-none-macosx_10_12_fat64
  py37-none-macosx_10_12_fat32
  py37-none-macosx_10_12_universal2
  py37-none-macosx_10_12_universal
  py37-none-macosx_10_11_x86_64
  py37-none-macosx_10_11_intel
  py37-none-macosx_10_11_fat64
  py37-none-macosx_10_11_fat32
  py37-none-macosx_10_11_universal2
  py37-none-macosx_10_11_universal
  py37-none-macosx_10_10_x86_64
  py37-none-macosx_10_10_intel
  py37-none-macosx_10_10_fat64
  py37-none-macosx_10_10_fat32
  py37-none-macosx_10_10_universal2
  py37-none-macosx_10_10_universal
  py37-none-macosx_10_9_x86_64
  py37-none-macosx_10_9_intel
  py37-none-macosx_10_9_fat64
  py37-none-macosx_10_9_fat32
  py37-none-macosx_10_9_universal2
  py37-none-macosx_10_9_universal
  py37-none-macosx_10_8_x86_64
  py37-none-macosx_10_8_intel
  py37-none-macosx_10_8_fat64
  py37-none-macosx_10_8_fat32
  py37-none-macosx_10_8_universal2
  py37-none-macosx_10_8_universal
  py37-none-macosx_10_7_x86_64
  py37-none-macosx_10_7_intel
  py37-none-macosx_10_7_fat64
  py37-none-macosx_10_7_fat32
  py37-none-macosx_10_7_universal2
  py37-none-macosx_10_7_universal
  py37-none-macosx_10_6_x86_64
  py37-none-macosx_10_6_intel
  py37-none-macosx_10_6_fat64
  py37-none-macosx_10_6_fat32
  py37-none-macosx_10_6_universal2
  py37-none-macosx_10_6_universal
  py37-none-macosx_10_5_x86_64
  py37-none-macosx_10_5_intel
  py37-none-macosx_10_5_fat64
  py37-none-macosx_10_5_fat32
  py37-none-macosx_10_5_universal2
  py37-none-macosx_10_5_universal
  py37-none-macosx_10_4_x86_64
  py37-none-macosx_10_4_intel
  py37-none-macosx_10_4_fat64
  py37-none-macosx_10_4_fat32
  py37-none-macosx_10_4_universal2
  py37-none-macosx_10_4_universal
  py36-none-macosx_15_0_x86_64
  py36-none-macosx_15_0_intel
  py36-none-macosx_15_0_fat64
  py36-none-macosx_15_0_fat32
  py36-none-macosx_15_0_universal2
  py36-none-macosx_15_0_universal
  py36-none-macosx_14_0_x86_64
  py36-none-macosx_14_0_intel
  py36-none-macosx_14_0_fat64
  py36-none-macosx_14_0_fat32
  py36-none-macosx_14_0_universal2
  py36-none-macosx_14_0_universal
  py36-none-macosx_13_0_x86_64
  py36-none-macosx_13_0_intel
  py36-none-macosx_13_0_fat64
  py36-none-macosx_13_0_fat32
  py36-none-macosx_13_0_universal2
  py36-none-macosx_13_0_universal
  py36-none-macosx_12_0_x86_64
  py36-none-macosx_12_0_intel
  py36-none-macosx_12_0_fat64
  py36-none-macosx_12_0_fat32
  py36-none-macosx_12_0_universal2
  py36-none-macosx_12_0_universal
  py36-none-macosx_11_0_x86_64
  py36-none-macosx_11_0_intel
  py36-none-macosx_11_0_fat64
  py36-none-macosx_11_0_fat32
  py36-none-macosx_11_0_universal2
  py36-none-macosx_11_0_universal
  py36-none-macosx_10_16_x86_64
  py36-none-macosx_10_16_intel
  py36-none-macosx_10_16_fat64
  py36-none-macosx_10_16_fat32
  py36-none-macosx_10_16_universal2
  py36-none-macosx_10_16_universal
  py36-none-macosx_10_15_x86_64
  py36-none-macosx_10_15_intel
  py36-none-macosx_10_15_fat64
  py36-none-macosx_10_15_fat32
  py36-none-macosx_10_15_universal2
  py36-none-macosx_10_15_universal
  py36-none-macosx_10_14_x86_64
  py36-none-macosx_10_14_intel
  py36-none-macosx_10_14_fat64
  py36-none-macosx_10_14_fat32
  py36-none-macosx_10_14_universal2
  py36-none-macosx_10_14_universal
  py36-none-macosx_10_13_x86_64
  py36-none-macosx_10_13_intel
  py36-none-macosx_10_13_fat64
  py36-none-macosx_10_13_fat32
  py36-none-macosx_10_13_universal2
  py36-none-macosx_10_13_universal
  py36-none-macosx_10_12_x86_64
  py36-none-macosx_10_12_intel
  py36-none-macosx_10_12_fat64
  py36-none-macosx_10_12_fat32
  py36-none-macosx_10_12_universal2
  py36-none-macosx_10_12_universal
  py36-none-macosx_10_11_x86_64
  py36-none-macosx_10_11_intel
  py36-none-macosx_10_11_fat64
  py36-none-macosx_10_11_fat32
  py36-none-macosx_10_11_universal2
  py36-none-macosx_10_11_universal
  py36-none-macosx_10_10_x86_64
  py36-none-macosx_10_10_intel
  py36-none-macosx_10_10_fat64
  py36-none-macosx_10_10_fat32
  py36-none-macosx_10_10_universal2
  py36-none-macosx_10_10_universal
  py36-none-macosx_10_9_x86_64
  py36-none-macosx_10_9_intel
  py36-none-macosx_10_9_fat64
  py36-none-macosx_10_9_fat32
  py36-none-macosx_10_9_universal2
  py36-none-macosx_10_9_universal
  py36-none-macosx_10_8_x86_64
  py36-none-macosx_10_8_intel
  py36-none-macosx_10_8_fat64
  py36-none-macosx_10_8_fat32
  py36-none-macosx_10_8_universal2
  py36-none-macosx_10_8_universal
  py36-none-macosx_10_7_x86_64
  py36-none-macosx_10_7_intel
  py36-none-macosx_10_7_fat64
  py36-none-macosx_10_7_fat32
  py36-none-macosx_10_7_universal2
  py36-none-macosx_10_7_universal
  py36-none-macosx_10_6_x86_64
  py36-none-macosx_10_6_intel
  py36-none-macosx_10_6_fat64
  py36-none-macosx_10_6_fat32
  py36-none-macosx_10_6_universal2
  py36-none-macosx_10_6_universal
  py36-none-macosx_10_5_x86_64
  py36-none-macosx_10_5_intel
  py36-none-macosx_10_5_fat64
  py36-none-macosx_10_5_fat32
  py36-none-macosx_10_5_universal2
  py36-none-macosx_10_5_universal
  py36-none-macosx_10_4_x86_64
  py36-none-macosx_10_4_intel
  py36-none-macosx_10_4_fat64
  py36-none-macosx_10_4_fat32
  py36-none-macosx_10_4_universal2
  py36-none-macosx_10_4_universal
  py35-none-macosx_15_0_x86_64
  py35-none-macosx_15_0_intel
  py35-none-macosx_15_0_fat64
  py35-none-macosx_15_0_fat32
  py35-none-macosx_15_0_universal2
  py35-none-macosx_15_0_universal
  py35-none-macosx_14_0_x86_64
  py35-none-macosx_14_0_intel
  py35-none-macosx_14_0_fat64
  py35-none-macosx_14_0_fat32
  py35-none-macosx_14_0_universal2
  py35-none-macosx_14_0_universal
  py35-none-macosx_13_0_x86_64
  py35-none-macosx_13_0_intel
  py35-none-macosx_13_0_fat64
  py35-none-macosx_13_0_fat32
  py35-none-macosx_13_0_universal2
  py35-none-macosx_13_0_universal
  py35-none-macosx_12_0_x86_64
  py35-none-macosx_12_0_intel
  py35-none-macosx_12_0_fat64
  py35-none-macosx_12_0_fat32
  py35-none-macosx_12_0_universal2
  py35-none-macosx_12_0_universal
  py35-none-macosx_11_0_x86_64
  py35-none-macosx_11_0_intel
  py35-none-macosx_11_0_fat64
  py35-none-macosx_11_0_fat32
  py35-none-macosx_11_0_universal2
  py35-none-macosx_11_0_universal
  py35-none-macosx_10_16_x86_64
  py35-none-macosx_10_16_intel
  py35-none-macosx_10_16_fat64
  py35-none-macosx_10_16_fat32
  py35-none-macosx_10_16_universal2
  py35-none-macosx_10_16_universal
  py35-none-macosx_10_15_x86_64
  py35-none-macosx_10_15_intel
  py35-none-macosx_10_15_fat64
  py35-none-macosx_10_15_fat32
  py35-none-macosx_10_15_universal2
  py35-none-macosx_10_15_universal
  py35-none-macosx_10_14_x86_64
  py35-none-macosx_10_14_intel
  py35-none-macosx_10_14_fat64
  py35-none-macosx_10_14_fat32
  py35-none-macosx_10_14_universal2
  py35-none-macosx_10_14_universal
  py35-none-macosx_10_13_x86_64
  py35-none-macosx_10_13_intel
  py35-none-macosx_10_13_fat64
  py35-none-macosx_10_13_fat32
  py35-none-macosx_10_13_universal2
  py35-none-macosx_10_13_universal
  py35-none-macosx_10_12_x86_64
  py35-none-macosx_10_12_intel
  py35-none-macosx_10_12_fat64
  py35-none-macosx_10_12_fat32
  py35-none-macosx_10_12_universal2
  py35-none-macosx_10_12_universal
  py35-none-macosx_10_11_x86_64
  py35-none-macosx_10_11_intel
  py35-none-macosx_10_11_fat64
  py35-none-macosx_10_11_fat32
  py35-none-macosx_10_11_universal2
  py35-none-macosx_10_11_universal
  py35-none-macosx_10_10_x86_64
  py35-none-macosx_10_10_intel
  py35-none-macosx_10_10_fat64
  py35-none-macosx_10_10_fat32
  py35-none-macosx_10_10_universal2
  py35-none-macosx_10_10_universal
  py35-none-macosx_10_9_x86_64
  py35-none-macosx_10_9_intel
  py35-none-macosx_10_9_fat64
  py35-none-macosx_10_9_fat32
  py35-none-macosx_10_9_universal2
  py35-none-macosx_10_9_universal
  py35-none-macosx_10_8_x86_64
  py35-none-macosx_10_8_intel
  py35-none-macosx_10_8_fat64
  py35-none-macosx_10_8_fat32
  py35-none-macosx_10_8_universal2
  py35-none-macosx_10_8_universal
  py35-none-macosx_10_7_x86_64
  py35-none-macosx_10_7_intel
  py35-none-macosx_10_7_fat64
  py35-none-macosx_10_7_fat32
  py35-none-macosx_10_7_universal2
  py35-none-macosx_10_7_universal
  py35-none-macosx_10_6_x86_64
  py35-none-macosx_10_6_intel
  py35-none-macosx_10_6_fat64
  py35-none-macosx_10_6_fat32
  py35-none-macosx_10_6_universal2
  py35-none-macosx_10_6_universal
  py35-none-macosx_10_5_x86_64
  py35-none-macosx_10_5_intel
  py35-none-macosx_10_5_fat64
  py35-none-macosx_10_5_fat32
  py35-none-macosx_10_5_universal2
  py35-none-macosx_10_5_universal
  py35-none-macosx_10_4_x86_64
  py35-none-macosx_10_4_intel
  py35-none-macosx_10_4_fat64
  py35-none-macosx_10_4_fat32
  py35-none-macosx_10_4_universal2
  py35-none-macosx_10_4_universal
  py34-none-macosx_15_0_x86_64
  py34-none-macosx_15_0_intel
  py34-none-macosx_15_0_fat64
  py34-none-macosx_15_0_fat32
  py34-none-macosx_15_0_universal2
  py34-none-macosx_15_0_universal
  py34-none-macosx_14_0_x86_64
  py34-none-macosx_14_0_intel
  py34-none-macosx_14_0_fat64
  py34-none-macosx_14_0_fat32
  py34-none-macosx_14_0_universal2
  py34-none-macosx_14_0_universal
  py34-none-macosx_13_0_x86_64
  py34-none-macosx_13_0_intel
  py34-none-macosx_13_0_fat64
  py34-none-macosx_13_0_fat32
  py34-none-macosx_13_0_universal2
  py34-none-macosx_13_0_universal
  py34-none-macosx_12_0_x86_64
  py34-none-macosx_12_0_intel
  py34-none-macosx_12_0_fat64
  py34-none-macosx_12_0_fat32
  py34-none-macosx_12_0_universal2
  py34-none-macosx_12_0_universal
  py34-none-macosx_11_0_x86_64
  py34-none-macosx_11_0_intel
  py34-none-macosx_11_0_fat64
  py34-none-macosx_11_0_fat32
  py34-none-macosx_11_0_universal2
  py34-none-macosx_11_0_universal
  py34-none-macosx_10_16_x86_64
  py34-none-macosx_10_16_intel
  py34-none-macosx_10_16_fat64
  py34-none-macosx_10_16_fat32
  py34-none-macosx_10_16_universal2
  py34-none-macosx_10_16_universal
  py34-none-macosx_10_15_x86_64
  py34-none-macosx_10_15_intel
  py34-none-macosx_10_15_fat64
  py34-none-macosx_10_15_fat32
  py34-none-macosx_10_15_universal2
  py34-none-macosx_10_15_universal
  py34-none-macosx_10_14_x86_64
  py34-none-macosx_10_14_intel
  py34-none-macosx_10_14_fat64
  py34-none-macosx_10_14_fat32
  py34-none-macosx_10_14_universal2
  py34-none-macosx_10_14_universal
  py34-none-macosx_10_13_x86_64
  py34-none-macosx_10_13_intel
  py34-none-macosx_10_13_fat64
  py34-none-macosx_10_13_fat32
  py34-none-macosx_10_13_universal2
  py34-none-macosx_10_13_universal
  py34-none-macosx_10_12_x86_64
  py34-none-macosx_10_12_intel
  py34-none-macosx_10_12_fat64
  py34-none-macosx_10_12_fat32
  py34-none-macosx_10_12_universal2
  py34-none-macosx_10_12_universal
  py34-none-macosx_10_11_x86_64
  py34-none-macosx_10_11_intel
  py34-none-macosx_10_11_fat64
  py34-none-macosx_10_11_fat32
  py34-none-macosx_10_11_universal2
  py34-none-macosx_10_11_universal
  py34-none-macosx_10_10_x86_64
  py34-none-macosx_10_10_intel
  py34-none-macosx_10_10_fat64
  py34-none-macosx_10_10_fat32
  py34-none-macosx_10_10_universal2
  py34-none-macosx_10_10_universal
  py34-none-macosx_10_9_x86_64
  py34-none-macosx_10_9_intel
  py34-none-macosx_10_9_fat64
  py34-none-macosx_10_9_fat32
  py34-none-macosx_10_9_universal2
  py34-none-macosx_10_9_universal
  py34-none-macosx_10_8_x86_64
  py34-none-macosx_10_8_intel
  py34-none-macosx_10_8_fat64
  py34-none-macosx_10_8_fat32
  py34-none-macosx_10_8_universal2
  py34-none-macosx_10_8_universal
  py34-none-macosx_10_7_x86_64
  py34-none-macosx_10_7_intel
  py34-none-macosx_10_7_fat64
  py34-none-macosx_10_7_fat32
  py34-none-macosx_10_7_universal2
  py34-none-macosx_10_7_universal
  py34-none-macosx_10_6_x86_64
  py34-none-macosx_10_6_intel
  py34-none-macosx_10_6_fat64
  py34-none-macosx_10_6_fat32
  py34-none-macosx_10_6_universal2
  py34-none-macosx_10_6_universal
  py34-none-macosx_10_5_x86_64
  py34-none-macosx_10_5_intel
  py34-none-macosx_10_5_fat64
  py34-none-macosx_10_5_fat32
  py34-none-macosx_10_5_universal2
  py34-none-macosx_10_5_universal
  py34-none-macosx_10_4_x86_64
  py34-none-macosx_10_4_intel
  py34-none-macosx_10_4_fat64
  py34-none-macosx_10_4_fat32
  py34-none-macosx_10_4_universal2
  py34-none-macosx_10_4_universal
  py33-none-macosx_15_0_x86_64
  py33-none-macosx_15_0_intel
  py33-none-macosx_15_0_fat64
  py33-none-macosx_15_0_fat32
  py33-none-macosx_15_0_universal2
  py33-none-macosx_15_0_universal
  py33-none-macosx_14_0_x86_64
  py33-none-macosx_14_0_intel
  py33-none-macosx_14_0_fat64
  py33-none-macosx_14_0_fat32
  py33-none-macosx_14_0_universal2
  py33-none-macosx_14_0_universal
  py33-none-macosx_13_0_x86_64
  py33-none-macosx_13_0_intel
  py33-none-macosx_13_0_fat64
  py33-none-macosx_13_0_fat32
  py33-none-macosx_13_0_universal2
  py33-none-macosx_13_0_universal
  py33-none-macosx_12_0_x86_64
  py33-none-macosx_12_0_intel
  py33-none-macosx_12_0_fat64
  py33-none-macosx_12_0_fat32
  py33-none-macosx_12_0_universal2
  py33-none-macosx_12_0_universal
  py33-none-macosx_11_0_x86_64
  py33-none-macosx_11_0_intel
  py33-none-macosx_11_0_fat64
  py33-none-macosx_11_0_fat32
  py33-none-macosx_11_0_universal2
  py33-none-macosx_11_0_universal
  py33-none-macosx_10_16_x86_64
  py33-none-macosx_10_16_intel
  py33-none-macosx_10_16_fat64
  py33-none-macosx_10_16_fat32
  py33-none-macosx_10_16_universal2
  py33-none-macosx_10_16_universal
  py33-none-macosx_10_15_x86_64
  py33-none-macosx_10_15_intel
  py33-none-macosx_10_15_fat64
  py33-none-macosx_10_15_fat32
  py33-none-macosx_10_15_universal2
  py33-none-macosx_10_15_universal
  py33-none-macosx_10_14_x86_64
  py33-none-macosx_10_14_intel
  py33-none-macosx_10_14_fat64
  py33-none-macosx_10_14_fat32
  py33-none-macosx_10_14_universal2
  py33-none-macosx_10_14_universal
  py33-none-macosx_10_13_x86_64
  py33-none-macosx_10_13_intel
  py33-none-macosx_10_13_fat64
  py33-none-macosx_10_13_fat32
  py33-none-macosx_10_13_universal2
  py33-none-macosx_10_13_universal
  py33-none-macosx_10_12_x86_64
  py33-none-macosx_10_12_intel
  py33-none-macosx_10_12_fat64
  py33-none-macosx_10_12_fat32
  py33-none-macosx_10_12_universal2
  py33-none-macosx_10_12_universal
  py33-none-macosx_10_11_x86_64
  py33-none-macosx_10_11_intel
  py33-none-macosx_10_11_fat64
  py33-none-macosx_10_11_fat32
  py33-none-macosx_10_11_universal2
  py33-none-macosx_10_11_universal
  py33-none-macosx_10_10_x86_64
  py33-none-macosx_10_10_intel
  py33-none-macosx_10_10_fat64
  py33-none-macosx_10_10_fat32
  py33-none-macosx_10_10_universal2
  py33-none-macosx_10_10_universal
  py33-none-macosx_10_9_x86_64
  py33-none-macosx_10_9_intel
  py33-none-macosx_10_9_fat64
  py33-none-macosx_10_9_fat32
  py33-none-macosx_10_9_universal2
  py33-none-macosx_10_9_universal
  py33-none-macosx_10_8_x86_64
  py33-none-macosx_10_8_intel
  py33-none-macosx_10_8_fat64
  py33-none-macosx_10_8_fat32
  py33-none-macosx_10_8_universal2
  py33-none-macosx_10_8_universal
  py33-none-macosx_10_7_x86_64
  py33-none-macosx_10_7_intel
  py33-none-macosx_10_7_fat64
  py33-none-macosx_10_7_fat32
  py33-none-macosx_10_7_universal2
  py33-none-macosx_10_7_universal
  py33-none-macosx_10_6_x86_64
  py33-none-macosx_10_6_intel
  py33-none-macosx_10_6_fat64
  py33-none-macosx_10_6_fat32
  py33-none-macosx_10_6_universal2
  py33-none-macosx_10_6_universal
  py33-none-macosx_10_5_x86_64
  py33-none-macosx_10_5_intel
  py33-none-macosx_10_5_fat64
  py33-none-macosx_10_5_fat32
  py33-none-macosx_10_5_universal2
  py33-none-macosx_10_5_universal
  py33-none-macosx_10_4_x86_64
  py33-none-macosx_10_4_intel
  py33-none-macosx_10_4_fat64
  py33-none-macosx_10_4_fat32
  py33-none-macosx_10_4_universal2
  py33-none-macosx_10_4_universal
  py32-none-macosx_15_0_x86_64
  py32-none-macosx_15_0_intel
  py32-none-macosx_15_0_fat64
  py32-none-macosx_15_0_fat32
  py32-none-macosx_15_0_universal2
  py32-none-macosx_15_0_universal
  py32-none-macosx_14_0_x86_64
  py32-none-macosx_14_0_intel
  py32-none-macosx_14_0_fat64
  py32-none-macosx_14_0_fat32
  py32-none-macosx_14_0_universal2
  py32-none-macosx_14_0_universal
  py32-none-macosx_13_0_x86_64
  py32-none-macosx_13_0_intel
  py32-none-macosx_13_0_fat64
  py32-none-macosx_13_0_fat32
  py32-none-macosx_13_0_universal2
  py32-none-macosx_13_0_universal
  py32-none-macosx_12_0_x86_64
  py32-none-macosx_12_0_intel
  py32-none-macosx_12_0_fat64
  py32-none-macosx_12_0_fat32
  py32-none-macosx_12_0_universal2
  py32-none-macosx_12_0_universal
  py32-none-macosx_11_0_x86_64
  py32-none-macosx_11_0_intel
  py32-none-macosx_11_0_fat64
  py32-none-macosx_11_0_fat32
  py32-none-macosx_11_0_universal2
  py32-none-macosx_11_0_universal
  py32-none-macosx_10_16_x86_64
  py32-none-macosx_10_16_intel
  py32-none-macosx_10_16_fat64
  py32-none-macosx_10_16_fat32
  py32-none-macosx_10_16_universal2
  py32-none-macosx_10_16_universal
  py32-none-macosx_10_15_x86_64
  py32-none-macosx_10_15_intel
  py32-none-macosx_10_15_fat64
  py32-none-macosx_10_15_fat32
  py32-none-macosx_10_15_universal2
  py32-none-macosx_10_15_universal
  py32-none-macosx_10_14_x86_64
  py32-none-macosx_10_14_intel
  py32-none-macosx_10_14_fat64
  py32-none-macosx_10_14_fat32
  py32-none-macosx_10_14_universal2
  py32-none-macosx_10_14_universal
  py32-none-macosx_10_13_x86_64
  py32-none-macosx_10_13_intel
  py32-none-macosx_10_13_fat64
  py32-none-macosx_10_13_fat32
  py32-none-macosx_10_13_universal2
  py32-none-macosx_10_13_universal
  py32-none-macosx_10_12_x86_64
  py32-none-macosx_10_12_intel
  py32-none-macosx_10_12_fat64
  py32-none-macosx_10_12_fat32
  py32-none-macosx_10_12_universal2
  py32-none-macosx_10_12_universal
  py32-none-macosx_10_11_x86_64
  py32-none-macosx_10_11_intel
  py32-none-macosx_10_11_fat64
  py32-none-macosx_10_11_fat32
  py32-none-macosx_10_11_universal2
  py32-none-macosx_10_11_universal
  py32-none-macosx_10_10_x86_64
  py32-none-macosx_10_10_intel
  py32-none-macosx_10_10_fat64
  py32-none-macosx_10_10_fat32
  py32-none-macosx_10_10_universal2
  py32-none-macosx_10_10_universal
  py32-none-macosx_10_9_x86_64
  py32-none-macosx_10_9_intel
  py32-none-macosx_10_9_fat64
  py32-none-macosx_10_9_fat32
  py32-none-macosx_10_9_universal2
  py32-none-macosx_10_9_universal
  py32-none-macosx_10_8_x86_64
  py32-none-macosx_10_8_intel
  py32-none-macosx_10_8_fat64
  py32-none-macosx_10_8_fat32
  py32-none-macosx_10_8_universal2
  py32-none-macosx_10_8_universal
  py32-none-macosx_10_7_x86_64
  py32-none-macosx_10_7_intel
  py32-none-macosx_10_7_fat64
  py32-none-macosx_10_7_fat32
  py32-none-macosx_10_7_universal2
  py32-none-macosx_10_7_universal
  py32-none-macosx_10_6_x86_64
  py32-none-macosx_10_6_intel
  py32-none-macosx_10_6_fat64
  py32-none-macosx_10_6_fat32
  py32-none-macosx_10_6_universal2
  py32-none-macosx_10_6_universal
  py32-none-macosx_10_5_x86_64
  py32-none-macosx_10_5_intel
  py32-none-macosx_10_5_fat64
  py32-none-macosx_10_5_fat32
  py32-none-macosx_10_5_universal2
  py32-none-macosx_10_5_universal
  py32-none-macosx_10_4_x86_64
  py32-none-macosx_10_4_intel
  py32-none-macosx_10_4_fat64
  py32-none-macosx_10_4_fat32
  py32-none-macosx_10_4_universal2
  py32-none-macosx_10_4_universal
  py31-none-macosx_15_0_x86_64
  py31-none-macosx_15_0_intel
  py31-none-macosx_15_0_fat64
  py31-none-macosx_15_0_fat32
  py31-none-macosx_15_0_universal2
  py31-none-macosx_15_0_universal
  py31-none-macosx_14_0_x86_64
  py31-none-macosx_14_0_intel
  py31-none-macosx_14_0_fat64
  py31-none-macosx_14_0_fat32
  py31-none-macosx_14_0_universal2
  py31-none-macosx_14_0_universal
  py31-none-macosx_13_0_x86_64
  py31-none-macosx_13_0_intel
  py31-none-macosx_13_0_fat64
  py31-none-macosx_13_0_fat32
  py31-none-macosx_13_0_universal2
  py31-none-macosx_13_0_universal
  py31-none-macosx_12_0_x86_64
  py31-none-macosx_12_0_intel
  py31-none-macosx_12_0_fat64
  py31-none-macosx_12_0_fat32
  py31-none-macosx_12_0_universal2
  py31-none-macosx_12_0_universal
  py31-none-macosx_11_0_x86_64
  py31-none-macosx_11_0_intel
  py31-none-macosx_11_0_fat64
  py31-none-macosx_11_0_fat32
  py31-none-macosx_11_0_universal2
  py31-none-macosx_11_0_universal
  py31-none-macosx_10_16_x86_64
  py31-none-macosx_10_16_intel
  py31-none-macosx_10_16_fat64
  py31-none-macosx_10_16_fat32
  py31-none-macosx_10_16_universal2
  py31-none-macosx_10_16_universal
  py31-none-macosx_10_15_x86_64
  py31-none-macosx_10_15_intel
  py31-none-macosx_10_15_fat64
  py31-none-macosx_10_15_fat32
  py31-none-macosx_10_15_universal2
  py31-none-macosx_10_15_universal
  py31-none-macosx_10_14_x86_64
  py31-none-macosx_10_14_intel
  py31-none-macosx_10_14_fat64
  py31-none-macosx_10_14_fat32
  py31-none-macosx_10_14_universal2
  py31-none-macosx_10_14_universal
  py31-none-macosx_10_13_x86_64
  py31-none-macosx_10_13_intel
  py31-none-macosx_10_13_fat64
  py31-none-macosx_10_13_fat32
  py31-none-macosx_10_13_universal2
  py31-none-macosx_10_13_universal
  py31-none-macosx_10_12_x86_64
  py31-none-macosx_10_12_intel
  py31-none-macosx_10_12_fat64
  py31-none-macosx_10_12_fat32
  py31-none-macosx_10_12_universal2
  py31-none-macosx_10_12_universal
  py31-none-macosx_10_11_x86_64
  py31-none-macosx_10_11_intel
  py31-none-macosx_10_11_fat64
  py31-none-macosx_10_11_fat32
  py31-none-macosx_10_11_universal2
  py31-none-macosx_10_11_universal
  py31-none-macosx_10_10_x86_64
  py31-none-macosx_10_10_intel
  py31-none-macosx_10_10_fat64
  py31-none-macosx_10_10_fat32
  py31-none-macosx_10_10_universal2
  py31-none-macosx_10_10_universal
  py31-none-macosx_10_9_x86_64
  py31-none-macosx_10_9_intel
  py31-none-macosx_10_9_fat64
  py31-none-macosx_10_9_fat32
  py31-none-macosx_10_9_universal2
  py31-none-macosx_10_9_universal
  py31-none-macosx_10_8_x86_64
  py31-none-macosx_10_8_intel
  py31-none-macosx_10_8_fat64
  py31-none-macosx_10_8_fat32
  py31-none-macosx_10_8_universal2
  py31-none-macosx_10_8_universal
  py31-none-macosx_10_7_x86_64
  py31-none-macosx_10_7_intel
  py31-none-macosx_10_7_fat64
  py31-none-macosx_10_7_fat32
  py31-none-macosx_10_7_universal2
  py31-none-macosx_10_7_universal
  py31-none-macosx_10_6_x86_64
  py31-none-macosx_10_6_intel
  py31-none-macosx_10_6_fat64
  py31-none-macosx_10_6_fat32
  py31-none-macosx_10_6_universal2
  py31-none-macosx_10_6_universal
  py31-none-macosx_10_5_x86_64
  py31-none-macosx_10_5_intel
  py31-none-macosx_10_5_fat64
  py31-none-macosx_10_5_fat32
  py31-none-macosx_10_5_universal2
  py31-none-macosx_10_5_universal
  py31-none-macosx_10_4_x86_64
  py31-none-macosx_10_4_intel
  py31-none-macosx_10_4_fat64
  py31-none-macosx_10_4_fat32
  py31-none-macosx_10_4_universal2
  py31-none-macosx_10_4_universal
  py30-none-macosx_15_0_x86_64
  py30-none-macosx_15_0_intel
  py30-none-macosx_15_0_fat64
  py30-none-macosx_15_0_fat32
  py30-none-macosx_15_0_universal2
  py30-none-macosx_15_0_universal
  py30-none-macosx_14_0_x86_64
  py30-none-macosx_14_0_intel
  py30-none-macosx_14_0_fat64
  py30-none-macosx_14_0_fat32
  py30-none-macosx_14_0_universal2
  py30-none-macosx_14_0_universal
  py30-none-macosx_13_0_x86_64
  py30-none-macosx_13_0_intel
  py30-none-macosx_13_0_fat64
  py30-none-macosx_13_0_fat32
  py30-none-macosx_13_0_universal2
  py30-none-macosx_13_0_universal
  py30-none-macosx_12_0_x86_64
  py30-none-macosx_12_0_intel
  py30-none-macosx_12_0_fat64
  py30-none-macosx_12_0_fat32
  py30-none-macosx_12_0_universal2
  py30-none-macosx_12_0_universal
  py30-none-macosx_11_0_x86_64
  py30-none-macosx_11_0_intel
  py30-none-macosx_11_0_fat64
  py30-none-macosx_11_0_fat32
  py30-none-macosx_11_0_universal2
  py30-none-macosx_11_0_universal
  py30-none-macosx_10_16_x86_64
  py30-none-macosx_10_16_intel
  py30-none-macosx_10_16_fat64
  py30-none-macosx_10_16_fat32
  py30-none-macosx_10_16_universal2
  py30-none-macosx_10_16_universal
  py30-none-macosx_10_15_x86_64
  py30-none-macosx_10_15_intel
  py30-none-macosx_10_15_fat64
  py30-none-macosx_10_15_fat32
  py30-none-macosx_10_15_universal2
  py30-none-macosx_10_15_universal
  py30-none-macosx_10_14_x86_64
  py30-none-macosx_10_14_intel
  py30-none-macosx_10_14_fat64
  py30-none-macosx_10_14_fat32
  py30-none-macosx_10_14_universal2
  py30-none-macosx_10_14_universal
  py30-none-macosx_10_13_x86_64
  py30-none-macosx_10_13_intel
  py30-none-macosx_10_13_fat64
  py30-none-macosx_10_13_fat32
  py30-none-macosx_10_13_universal2
  py30-none-macosx_10_13_universal
  py30-none-macosx_10_12_x86_64
  py30-none-macosx_10_12_intel
  py30-none-macosx_10_12_fat64
  py30-none-macosx_10_12_fat32
  py30-none-macosx_10_12_universal2
  py30-none-macosx_10_12_universal
  py30-none-macosx_10_11_x86_64
  py30-none-macosx_10_11_intel
  py30-none-macosx_10_11_fat64
  py30-none-macosx_10_11_fat32
  py30-none-macosx_10_11_universal2
  py30-none-macosx_10_11_universal
  py30-none-macosx_10_10_x86_64
  py30-none-macosx_10_10_intel
  py30-none-macosx_10_10_fat64
  py30-none-macosx_10_10_fat32
  py30-none-macosx_10_10_universal2
  py30-none-macosx_10_10_universal
  py30-none-macosx_10_9_x86_64
  py30-none-macosx_10_9_intel
  py30-none-macosx_10_9_fat64
  py30-none-macosx_10_9_fat32
  py30-none-macosx_10_9_universal2
  py30-none-macosx_10_9_universal
  py30-none-macosx_10_8_x86_64
  py30-none-macosx_10_8_intel
  py30-none-macosx_10_8_fat64
  py30-none-macosx_10_8_fat32
  py30-none-macosx_10_8_universal2
  py30-none-macosx_10_8_universal
  py30-none-macosx_10_7_x86_64
  py30-none-macosx_10_7_intel
  py30-none-macosx_10_7_fat64
  py30-none-macosx_10_7_fat32
  py30-none-macosx_10_7_universal2
  py30-none-macosx_10_7_universal
  py30-none-macosx_10_6_x86_64
  py30-none-macosx_10_6_intel
  py30-none-macosx_10_6_fat64
  py30-none-macosx_10_6_fat32
  py30-none-macosx_10_6_universal2
  py30-none-macosx_10_6_universal
  py30-none-macosx_10_5_x86_64
  py30-none-macosx_10_5_intel
  py30-none-macosx_10_5_fat64
  py30-none-macosx_10_5_fat32
  py30-none-macosx_10_5_universal2
  py30-none-macosx_10_5_universal
  py30-none-macosx_10_4_x86_64
  py30-none-macosx_10_4_intel
  py30-none-macosx_10_4_fat64
  py30-none-macosx_10_4_fat32
  py30-none-macosx_10_4_universal2
  py30-none-macosx_10_4_universal
  cp312-none-any
  py312-none-any
  py3-none-any
  py311-none-any
  py310-none-any
  py39-none-any
  py38-none-any
  py37-none-any
  py36-none-any
  py35-none-any
  py34-none-any
  py33-none-any
  py32-none-any
  py31-none-any
  py30-none-any
```

</detail>

This is what python thinks it is running on, not sure if it is relevant.

```
>>> import platform
>>> platform.architecture()
('64bit', '')
>>> platform.machine()     
'x86_64'
>>> platform.platform()
'macOS-10.16-x86_64-i386-64bit'
>>> platform.processor()
'i386'
```



---

_Comment by @dwt on 2024-10-31 09:11_

This is what it looks like when pip installs the same package:

```
(nix:nix-shell-env) (venv) Sokrates:api dwt$ pip --verbose --debug install pymssql --force-reinstall
Using pip 24.2 from /Users/dwt/Code/Projekte/mkk/api/venv/lib/python3.12/site-packages/pip (python 3.12)
Collecting pymssql
  Obtaining dependency information for pymssql from https://files.pythonhosted.org/packages/fd/cf/ac241b624b25e608f4f17f3f454cc34a8daea6fb1fe102572edd6b529d9d/pymssql-2.3.1-cp312-cp312-macosx_13_0_universal2.whl.metadata
  Using cached pymssql-2.3.1-cp312-cp312-macosx_13_0_universal2.whl.metadata (7.1 kB)
Using cached pymssql-2.3.1-cp312-cp312-macosx_13_0_universal2.whl (3.0 MB)
Installing collected packages: pymssql
  Attempting uninstall: pymssql
    Found existing installation: pymssql 2.3.1
    Uninstalling pymssql-2.3.1:
      Removing file or directory /Users/dwt/Code/Projekte/mkk/api/venv/lib/python3.12/site-packages/pymssql-2.3.1.dist-info/
      Removing file or directory /Users/dwt/Code/Projekte/mkk/api/venv/lib/python3.12/site-packages/pymssql/
      Successfully uninstalled pymssql-2.3.1
Successfully installed pymssql-2.3.1
```

---

_Comment by @dwt on 2024-10-31 09:13_

<details>
<summary>And this is what it looks like with uv</summary>

```
(nix:nix-shell-env) (venv) Sokrates:api dwt$ uv pip install --verbose  --force-reinstall pymssql       
DEBUG uv 0.4.29 (85f9a0d0e 2024-10-30)
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.12.6-macos-x86_64-none` at `/Users/dwt/Code/Projekte/mkk/api/venv/bin/python3` (active virtual environment)
Using Python 3.12.6 environment at venv
DEBUG Acquired lock for `venv`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: pymssql*
DEBUG Found stale response for: https://pypi.org/simple/pymssql/
DEBUG Sending revalidation request for: https://pypi.org/simple/pymssql/
DEBUG Found not-modified response for: https://pypi.org/simple/pymssql/
DEBUG Searching for a compatible version of pymssql (*)
DEBUG Selecting: pymssql==2.3.1 [compatible] (pymssql-2.3.1.tar.gz)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/49/9e/5342bdc6ad39506de7530329e976ec5631cb409fc7d64dc8a2613fb7df75/pymssql-2.3.1-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
DEBUG Tried 1 versions: pymssql 1
DEBUG Split specific environment resolution took 0.140s
Resolved 1 package in 153ms
DEBUG Must revalidate requirement: pymssql
DEBUG Unnecessary package: mako==1.3.6
DEBUG Unnecessary package: markupsafe==3.0.2
DEBUG Unnecessary package: pyyaml==6.0.2
DEBUG Unnecessary package: sqlalchemy==1.4.54
DEBUG Unnecessary package: alembic==1.13.3
DEBUG Unnecessary package: annotated-types==0.7.0
DEBUG Unnecessary package: anyio==4.6.2.post1
DEBUG Unnecessary package: attrs==24.2.0
DEBUG Unnecessary package: blinker==1.8.2
DEBUG Unnecessary package: certifi==2024.8.30
DEBUG Unnecessary package: cffi==1.17.1
DEBUG Unnecessary package: charset-normalizer==3.4.0
DEBUG Unnecessary package: click==8.1.7
DEBUG Unnecessary package: coverage==7.6.4
DEBUG Unnecessary package: cryptography==43.0.3
DEBUG Unnecessary package: deepdiff==8.0.1
DEBUG Unnecessary package: flask==3.0.3
DEBUG Unnecessary package: greenlet==3.1.1
DEBUG Unnecessary package: gunicorn==23.0.0
DEBUG Unnecessary package: h11==0.14.0
DEBUG Unnecessary package: httpcore==1.0.6
DEBUG Unnecessary package: httpx==0.27.2
DEBUG Unnecessary package: ibm-db==3.2.3
DEBUG Unnecessary package: ibm-db-sa==0.4.1
DEBUG Unnecessary package: idna==3.10
DEBUG Unnecessary package: iniconfig==2.0.0
DEBUG Unnecessary package: isodate==0.7.2
DEBUG Unnecessary package: itsdangerous==2.2.0
DEBUG Unnecessary package: jinja2==3.1.4
DEBUG Unnecessary package: json-logging==1.3.0
DEBUG Unnecessary package: lxml==5.3.0
DEBUG Unnecessary package: markdown-it-py==3.0.0
DEBUG Unnecessary package: marshmallow==3.23.0
DEBUG Unnecessary package: marshmallow-dataclass==8.7.1
DEBUG Unnecessary package: mdurl==0.1.2
DEBUG Unnecessary package: mypy-extensions==1.0.0
DEBUG Unnecessary package: openapi-python-client==0.21.6
DEBUG Unnecessary package: orderly-set==5.2.2
DEBUG Unnecessary package: packaging==24.1
DEBUG Unnecessary package: phonenumbers==8.13.48
DEBUG Preserving seed package: pip==24.2
DEBUG Unnecessary package: platformdirs==4.3.6
DEBUG Unnecessary package: pluggy==1.5.0
DEBUG Unnecessary package: psycopg2-binary==2.9.10
DEBUG Unnecessary package: pycparser==2.22
DEBUG Unnecessary package: pydantic==2.9.2
DEBUG Unnecessary package: pydantic-core==2.23.4
DEBUG Unnecessary package: pygments==2.18.0
DEBUG Unnecessary package: pyspnego==0.11.1
DEBUG Unnecessary package: pytest==8.3.3
DEBUG Unnecessary package: pytest-cov==5.0.0
DEBUG Unnecessary package: pytest-sugar==1.0.0
DEBUG Unnecessary package: python-dateutil==2.9.0.post0
DEBUG Unnecessary package: pytz==2024.2
DEBUG Unnecessary package: requests==2.32.3
DEBUG Unnecessary package: requests-file==2.1.0
DEBUG Unnecessary package: requests-toolbelt==1.0.0
DEBUG Unnecessary package: responses==0.25.3
DEBUG Unnecessary package: rich==13.9.3
DEBUG Unnecessary package: ruamel-yaml==0.18.6
DEBUG Unnecessary package: ruamel-yaml-clib==0.2.12
DEBUG Unnecessary package: ruff==0.7.1
DEBUG Unnecessary package: shellingham==1.5.4
DEBUG Unnecessary package: six==1.16.0
DEBUG Unnecessary package: smbprotocol==1.14.0
DEBUG Unnecessary package: sniffio==1.3.1
DEBUG Unnecessary package: sqlalchemy-cockroachdb==2.0.2
DEBUG Unnecessary package: tabulate==0.9.0
DEBUG Unnecessary package: termcolor==2.5.0
DEBUG Unnecessary package: typeguard==4.4.0
DEBUG Unnecessary package: typer==0.12.5
DEBUG Unnecessary package: typing-extensions==4.12.2
DEBUG Unnecessary package: typing-inspect==0.9.0
DEBUG Unnecessary package: urllib3==2.2.3
DEBUG Preserving seed package: uv==0.4.29
DEBUG Unnecessary package: vbu-service==1 (from file:///Users/dwt/Code/Projekte/mkk/api)
DEBUG Unnecessary package: watching-testrunner==1.2.2
DEBUG Unnecessary package: werkzeug==3.0.6
DEBUG Unnecessary package: zeep==4.3.1
DEBUG Acquired lock for `/Users/dwt/Library/Caches/uv/sdists-v5/pypi/pymssql/2.3.1`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/30/66/f98a16e1db6592b9ab7fa85a3cb5542b4304178b6ad67928e69927590493/pymssql-2.3.1.tar.gz
DEBUG Building: pymssql==2.3.1
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: setuptools>=54.0
DEBUG Adding direct dependency: setuptools-scm>=5.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0
DEBUG Adding direct dependency: wheel>=0.36.2
DEBUG Adding direct dependency: cython>=3.0.10
DEBUG Adding direct dependency: tomli*
DEBUG Found stale response for: https://pypi.org/simple/tomli/
DEBUG Sending revalidation request for: https://pypi.org/simple/tomli/
DEBUG Found stale response for: https://pypi.org/simple/setuptools-scm/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools-scm/
DEBUG Found stale response for: https://pypi.org/simple/wheel/
DEBUG Sending revalidation request for: https://pypi.org/simple/wheel/
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found stale response for: https://pypi.org/simple/cython/
DEBUG Sending revalidation request for: https://pypi.org/simple/cython/
DEBUG Found not-modified response for: https://pypi.org/simple/tomli/
DEBUG Found not-modified response for: https://pypi.org/simple/wheel/
DEBUG Found not-modified response for: https://pypi.org/simple/cython/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools-scm/
DEBUG Searching for a compatible version of setuptools (>=54.0)
DEBUG Selecting: setuptools==75.3.0 [compatible] (setuptools-75.3.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cf/db/ce8eda256fa131af12e0a76d481711abe4681b6923c27efb9a255c9e4594/tomli-2.0.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/58/50/fbb23239efe2183e4eaf76689270d6f5b3bbcf9be9ad1eb97cc34349e6fc/Cython-3.0.11-cp312-cp312-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/b9/1906bfeb30f2fc13bb39bf7ddb8749784c05faadbd18a21cf141ba37bff2/setuptools_scm-8.1.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/90/12/282ee9bce8b58130cb762fbc9beabd531549952cac11fc56add11dcb7ea0/setuptools-75.3.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of setuptools-scm (>=5.0)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (>=5.0)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm==8.1.0
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm[toml]==8.1.0
DEBUG Searching for a compatible version of setuptools-scm[toml] (==8.1.0)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of wheel (>=0.36.2)
DEBUG Selecting: wheel==0.44.0 [compatible] (wheel-0.44.0-py3-none-any.whl)
DEBUG Searching for a compatible version of cython (>=3.0.10)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found stale response for: https://pypi.org/simple/packaging/
DEBUG Sending revalidation request for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of tomli (*)
DEBUG Selecting: tomli==2.0.2 [compatible] (tomli-2.0.2-py3-none-any.whl)
DEBUG Found not-modified response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of packaging (>=20)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG Tried 6 versions: cython 1, packaging 1, setuptools 1, setuptools-scm 1, tomli 1, wheel 1
DEBUG Split specific environment resolution took 0.074s
DEBUG Installing in cython==3.0.11, packaging==24.1, setuptools==75.3.0, setuptools-scm==8.1.0, tomli==2.0.2, wheel==0.44.0 in /Users/dwt/Library/Caches/uv/builds-v0/.tmpUNF4ds
DEBUG Must revalidate requirement: cython
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: setuptools
DEBUG Must revalidate requirement: setuptools-scm
DEBUG Must revalidate requirement: tomli
DEBUG Must revalidate requirement: wheel
DEBUG Downloading and building requirements for build: cython==3.0.11, packaging==24.1, setuptools==75.3.0, setuptools-scm==8.1.0, tomli==2.0.2, wheel==0.44.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/58/50/fbb23239efe2183e4eaf76689270d6f5b3bbcf9be9ad1eb97cc34349e6fc/Cython-3.0.11-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/90/12/282ee9bce8b58130cb762fbc9beabd531549952cac11fc56add11dcb7ea0/setuptools-75.3.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/b9/1906bfeb30f2fc13bb39bf7ddb8749784c05faadbd18a21cf141ba37bff2/setuptools_scm-8.1.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cf/db/ce8eda256fa131af12e0a76d481711abe4681b6923c27efb9a255c9e4594/tomli-2.0.2-py3-none-any.whl
DEBUG Installing build requirements: cython==3.0.11, setuptools==75.3.0, wheel==0.44.0, setuptools-scm==8.1.0, packaging==24.1, tomli==2.0.2
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG prefix='/usr/local'
DEBUG PREFIX=/nix/store/m32rsgpxqpnc674ayk60iarj68s5q9lp-bash-5.2p32/bin/sh: line 1: brew: command not found
DEBUG setup.py: platform.system() => Darwin
DEBUG setup.py: platform.architecture() => ('64bit', '')
DEBUG setup.py: platform.libc_ver() => ('', '')
DEBUG setup.py: include_dirs => ['/usr/local/include', '/nix/store/m32rsgpxqpnc674ayk60iarj68s5q9lp-bash-5.2p32/bin/sh: line 1: brew: command not found/include']
DEBUG setup.py: library_dirs => ['/usr/local/lib', '/nix/store/m32rsgpxqpnc674ayk60iarj68s5q9lp-bash-5.2p32/bin/sh: line 1: brew: command not found/lib']
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.12.6
DEBUG Solving with target Python version: >=3.12.6
DEBUG Adding direct dependency: setuptools>=54.0
DEBUG Adding direct dependency: setuptools-scm>=5.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0
DEBUG Adding direct dependency: wheel>=0.36.2
DEBUG Adding direct dependency: cython>=3.0.10
DEBUG Adding direct dependency: tomli*
DEBUG Adding direct dependency: setuptools-scm>=5.0, <9.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0, <9.0
DEBUG Adding direct dependency: cython>=3.0.7
DEBUG Searching for a compatible version of setuptools (>=54.0)
DEBUG Selecting: setuptools==75.3.0 [compatible] (setuptools-75.3.0-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools-scm (>=5.0, <9.0)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (>=5.0, <9.0)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm==8.1.0
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm[toml]==8.1.0
DEBUG Searching for a compatible version of setuptools-scm[toml] (==8.1.0)
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of wheel (>=0.36.2)
DEBUG Selecting: wheel==0.44.0 [compatible] (wheel-0.44.0-py3-none-any.whl)
DEBUG Searching for a compatible version of cython (>=3.0.10)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of tomli (*)
DEBUG Selecting: tomli==2.0.2 [compatible] (tomli-2.0.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Tried 6 versions: cython 1, packaging 1, setuptools 1, setuptools-scm 1, tomli 1, wheel 1
DEBUG Split specific environment resolution took 0.001s
DEBUG Installing in cython==3.0.11, packaging==24.1, setuptools==75.3.0, setuptools-scm==8.1.0, tomli==2.0.2, wheel==0.44.0 in /Users/dwt/Library/Caches/uv/builds-v0/.tmpUNF4ds
DEBUG Requirement already installed: cython==3.0.11
DEBUG Requirement already installed: packaging==24.1
DEBUG Requirement already installed: setuptools==75.3.0
DEBUG Requirement already installed: setuptools-scm==8.1.0
DEBUG Requirement already installed: tomli==2.0.2
DEBUG Requirement already installed: wheel==0.44.0
DEBUG No build requirements to install for build
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("/Users/dwt/Library/Caches/uv/sdists-v5/pypi/pymssql/2.3.1/TlfTVzO3d4ZkTnTz-03tS/.tmp29ewJh", {}, None)`
DEBUG /Users/dwt/Library/Caches/uv/builds-v0/.tmpUNF4ds/lib/python3.12/site-packages/setuptools/_distutils/dist.py:261: UserWarning: Unknown distribution option: 'tests_require'
DEBUG   warnings.warn(msg)
DEBUG prefix='/usr/local'
DEBUG PREFIX=/nix/store/m32rsgpxqpnc674ayk60iarj68s5q9lp-bash-5.2p32/bin/sh: line 1: brew: command not found
DEBUG setup.py: platform.system() => Darwin
DEBUG setup.py: platform.architecture() => ('64bit', '')
DEBUG setup.py: platform.libc_ver() => ('', '')
DEBUG setup.py: include_dirs => ['/usr/local/include', '/nix/store/m32rsgpxqpnc674ayk60iarj68s5q9lp-bash-5.2p32/bin/sh: line 1: brew: command not found/include']
DEBUG setup.py: library_dirs => ['/usr/local/lib', '/nix/store/m32rsgpxqpnc674ayk60iarj68s5q9lp-bash-5.2p32/bin/sh: line 1: brew: command not found/lib']
DEBUG running bdist_wheel
DEBUG running build
DEBUG running build_py
DEBUG copying src/pymssql/__init__.py -> build/lib.macosx-10.12-x86_64-cpython-312/pymssql
DEBUG copying src/pymssql/exceptions.py -> build/lib.macosx-10.12-x86_64-cpython-312/pymssql
DEBUG copying src/pymssql/__init__.pyi -> build/lib.macosx-10.12-x86_64-cpython-312/pymssql
DEBUG copying src/pymssql/exceptions.pyi -> build/lib.macosx-10.12-x86_64-cpython-312/pymssql
DEBUG copying src/pymssql/_mssql.pyi -> build/lib.macosx-10.12-x86_64-cpython-312/pymssql
DEBUG copying src/pymssql/_pymssql.pyi -> build/lib.macosx-10.12-x86_64-cpython-312/pymssql
DEBUG running build_ext
DEBUG installing to build/bdist.macosx-10.12-x86_64/wheel
DEBUG running install
DEBUG running install_lib
DEBUG creating build/bdist.macosx-10.12-x86_64/wheel
DEBUG creating build/bdist.macosx-10.12-x86_64/wheel/pymssql
DEBUG copying build/lib.macosx-10.12-x86_64-cpython-312/pymssql/_mssql.cpython-312-darwin.so -> build/bdist.macosx-10.12-x86_64/wheel/./pymssql
DEBUG copying build/lib.macosx-10.12-x86_64-cpython-312/pymssql/__init__.pyi -> build/bdist.macosx-10.12-x86_64/wheel/./pymssql
DEBUG copying build/lib.macosx-10.12-x86_64-cpython-312/pymssql/_pymssql.cpython-312-darwin.so -> build/bdist.macosx-10.12-x86_64/wheel/./pymssql
DEBUG copying build/lib.macosx-10.12-x86_64-cpython-312/pymssql/exceptions.pyi -> build/bdist.macosx-10.12-x86_64/wheel/./pymssql
DEBUG copying build/lib.macosx-10.12-x86_64-cpython-312/pymssql/__init__.py -> build/bdist.macosx-10.12-x86_64/wheel/./pymssql
DEBUG copying build/lib.macosx-10.12-x86_64-cpython-312/pymssql/_mssql.pyi -> build/bdist.macosx-10.12-x86_64/wheel/./pymssql
DEBUG copying build/lib.macosx-10.12-x86_64-cpython-312/pymssql/exceptions.py -> build/bdist.macosx-10.12-x86_64/wheel/./pymssql
DEBUG copying build/lib.macosx-10.12-x86_64-cpython-312/pymssql/_pymssql.pyi -> build/bdist.macosx-10.12-x86_64/wheel/./pymssql
DEBUG running install_egg_info
DEBUG running egg_info
DEBUG writing src/pymssql.egg-info/PKG-INFO
DEBUG writing dependency_links to src/pymssql.egg-info/dependency_links.txt
DEBUG writing top-level names to src/pymssql.egg-info/top_level.txt
DEBUG ERROR setuptools_scm._file_finders.git listing git files failed - pretending there aren't any
DEBUG reading manifest file 'src/pymssql.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE'
DEBUG writing manifest file 'src/pymssql.egg-info/SOURCES.txt'
DEBUG Copying src/pymssql.egg-info to build/bdist.macosx-10.12-x86_64/wheel/./pymssql-2.3.1-py3.12.egg-info
DEBUG running install_scripts
DEBUG [WARNING] This wheel needs a higher macOS version than is set in MACOSX_DEPLOYMENT_TARGET variable.  To silence this warning, set MACOSX_DEPLOYMENT_TARGET to at least 11_0 or recreate these files with lower MACOSX_DEPLOYMENT_TARGET:  
DEBUG build/bdist.macosx-10.12-x86_64/wheel/pymssql/_mssql.cpython-312-darwin.so
DEBUG build/bdist.macosx-10.12-x86_64/wheel/pymssql/_pymssql.cpython-312-darwin.so[WARNING] This wheel needs a higher macOS version than is set in MACOSX_DEPLOYMENT_TARGET variable.  To silence this warning, set MACOSX_DEPLOYMENT_TARGET to at least 11_0 or recreate these files with lower MACOSX_DEPLOYMENT_TARGET:  
DEBUG build/bdist.macosx-10.12-x86_64/wheel/pymssql/_mssql.cpython-312-darwin.so
DEBUG creating build/bdist.macosx-10.12-x86_64/wheel/pymssql-2.3.1.dist-info/WHEEL
DEBUG creating '/Users/dwt/Library/Caches/uv/sdists-v5/pypi/pymssql/2.3.1/TlfTVzO3d4ZkTnTz-03tS/.tmp29ewJh/.tmp-7mpbdkmy/pymssql-2.3.1-cp312-cp312-macosx_11_0_x86_64.whl' and adding 'build/bdist.macosx-10.12-x86_64/wheel' to it
DEBUG adding 'pymssql/__init__.py'
DEBUG adding 'pymssql/__init__.pyi'
DEBUG adding 'pymssql/_mssql.cpython-312-darwin.so'
DEBUG adding 'pymssql/_mssql.pyi'
DEBUG adding 'pymssql/_pymssql.cpython-312-darwin.so'
DEBUG adding 'pymssql/_pymssql.pyi'
DEBUG adding 'pymssql/exceptions.py'
DEBUG adding 'pymssql/exceptions.pyi'
DEBUG adding 'pymssql-2.3.1.dist-info/LICENSE'
DEBUG adding 'pymssql-2.3.1.dist-info/METADATA'
DEBUG adding 'pymssql-2.3.1.dist-info/WHEEL'
DEBUG adding 'pymssql-2.3.1.dist-info/top_level.txt'
DEBUG adding 'pymssql-2.3.1.dist-info/RECORD'
DEBUG removing build/bdist.macosx-10.12-x86_64/wheel
DEBUG build/bdist.macosx-10.12-x86_64/wheel/pymssql/_pymssql.cpython-312-darwin.so
DEBUG Finished building: pymssql==2.3.1
DEBUG Released lock at `/Users/dwt/Library/Caches/uv/sdists-v5/pypi/pymssql/2.3.1/.lock`
Prepared 1 package in 5.13s
DEBUG Uninstalled pymssql (19 files, 4 directories)
Uninstalled 1 package in 4ms
Installed 1 package in 1ms
 ~ pymssql==2.3.1
DEBUG Released lock at `/Users/dwt/Code/Projekte/mkk/api/venv/.lock`
```
</details>

Looks a bit like that uv doesn't realize it is on MacOS X and could use the universal wheel?

---

_Comment by @dwt on 2024-11-17 14:36_

I have tried investigating this a bit further, and came up with this nix expression to show the problem more fully.

<details>
<summary>flake.nix and additional files to reproduce this</summary>

flake.nix
```nix
{

    description = "Flake based nix environment to try out isolation and still allow to get binary python dependencies (wheels) to run";

    inputs.nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    inputs.flake-utils.url = "github:numtide/flake-utils";

    outputs = { self, nixpkgs, flake-utils }:
    with flake-utils.lib; eachSystem allSystems (system:
        let
            pkgs = nixpkgs.legacyPackages.${system};
            rosettaPkgs =
                if pkgs.stdenv.isDarwin && pkgs.stdenv.isAarch64
                then pkgs.pkgsx86_64Darwin
                else pkgs;

        in
        {
            # need the rosettaPkgs.mkShell to inherit the x86 stdenv that includes the compiler!
            devShells.default = rosettaPkgs.mkShell {
                buildInputs = with rosettaPkgs; [
                    # make setuptoos-scm work with --ignore-environment
                    pkgs.git
                    python312Full
                    uv

                    # dependencies of mssql
                    freetds
                    krb5
                    openssl

                    # uv  # actually does work if the stdenv is x86. But only barely.
                    # `uv pip install -e .` (for pymssql) works,
                    # but `uv pip install --force-reinstall pymssql` still builds the arm version
                    # even with `nix develop --ignore-environment`
                ];

                shellHook = ''
                    export DYLD_FALLBACK_LIBRARY_PATH="${rosettaPkgs.lib.makeLibraryPath [ rosettaPkgs.gcc14.cc] }:$DYLD_FALLBACK_LIBRARY_PATH"

                    if test ! -d pymssql; then
                        git clone git@github.com:pymssql/pymssql.git
                    fi

                    function test() {
                        (
                        set -x
                        type python
                        python --version
                        python -c "import ibm_db" || echo "ibm_db failed"
                        python -c "import pymssql" || echo "pymssql failed"
                        )
                    }

                    echo ${rosettaPkgs.python312Full}/bin/python3
                    echo

                    echo testing native venv + pip
                    rm -rf venv
                    ${rosettaPkgs.python312Full}/bin/python3 -m venv venv
                    source venv/bin/activate
                    pip install --requirement requirements.txt
                    test

                    echo testing x86 compile
                    (
                        cd pymssql
                        pip install -e .
                    )
                    test


                    echo
                    echo testing uv
                    rm -rf venv
                    ${rosettaPkgs.uv}/bin/uv --version
                    ${rosettaPkgs.uv}/bin/uv venv --python ${rosettaPkgs.python312Full}/bin/python3 venv
                    source venv/bin/activate
                    ${rosettaPkgs.uv}/bin/uv pip install --requirement requirements.txt
                    test

                    echo testing uv x86 compile
                    (
                        cd pymssql
                        uv pip install --force-reinstall -e .
                    )
                    test

                    rm -rf venv
                '';

            };
        }
    );
}
```

requirements.txt
```
ibm_db_sa
pymssql
```

</details>

Activate the flake and run all the tests by executing: `nix develop --impure --ignore-environment` in a directory with those files.

This way ensures that no environment variables from the operating system stay around to disturb the build process.

What this shows to me is that: `uv pip install --editable ./pymssql` actually compiles the code correctly for x86, even though the system has an ARM chip, while `uv pip install -r reqs.txt` a) compiles for ARM even though it (nor python) should see any arm compiler, and b) doesn't use the readily compiled packages from pypi.

This means I seem to see multiple bugs:
1. uv does not use the pre-compiled version from pypi. I would be happy to provide further debugging information to track this down, as this would be a welcome speed boost
2. uv seems to have different ways to trigger a compilation, and the one triggered from `uv pip install -r req.txt` has a bug, that is already fixed or was never present in the way `uv pip install ./path` triggers compilation.

---

_Comment by @konstin on 2024-11-19 13:48_

There is a `pymssql-2.3.1-cp312-cp312-macosx_13_0_universal2.whl` on https://pypi.org/project/pymssql/#files, i'm surprised uv doesn't pick it up. Can you run with `RUST_LOG=uv_resolver=trace` and copy the line that looks like:

```
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.13.0", version: "3.13.0" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "6.8.0-48-generic", platform_system: "Linux", platform_version: "#48-Ubuntu SMP PREEMPT_DYNAMIC Fri Sep 27 14:04:52 UTC 2024", python_full_version: StringVersion { string: "3.13.0", version: "3.13.0" }, python_version: StringVersion { string: "3.13", version: "3.13" }, sys_platform: "linux" } }) } }
```

---

_Comment by @dwt on 2024-11-21 07:21_

I See multiple outputs that seem to fit this, but as far as I can tell they are all identical.

```log
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.7", version: "3.12.7" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "24.1.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.1.0: Thu Oct 10 21:05:23 PDT 2024; root:xnu-11215.41.3~2/RELEASE_ARM64_T6031", python_full_version: StringVersion { string: "3.12.7", version: "3.12.7" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "darwin" } }) } }
```

<details>
<summary>Full Build log</summary>

```
❯ ./enter-flake
/nix/store/ak0sh27jipas724h0c7raxvdfmw824k8-python3-3.12.7/bin/python3

testing native venv + pip
Collecting ibm_db_sa (from -r requirements.txt (line 1))
  Using cached ibm_db_sa-0.4.1-py3-none-any.whl.metadata (5.3 kB)
Collecting pymssql (from -r requirements.txt (line 2))
  Downloading pymssql-2.3.2-cp312-cp312-macosx_13_0_universal2.whl.metadata (4.7 kB)
Collecting sqlalchemy>=0.7.3 (from ibm_db_sa->-r requirements.txt (line 1))
  Using cached SQLAlchemy-2.0.36-cp312-cp312-macosx_10_13_x86_64.whl.metadata (9.7 kB)
Collecting ibm-db>=2.0.0 (from ibm_db_sa->-r requirements.txt (line 1))
  Using cached ibm_db-3.2.3-cp312-cp312-macosx_10_15_x86_64.whl.metadata (1.4 kB)
Collecting typing-extensions>=4.6.0 (from sqlalchemy>=0.7.3->ibm_db_sa->-r requirements.txt (line 1))
  Using cached typing_extensions-4.12.2-py3-none-any.whl.metadata (3.0 kB)
Collecting greenlet!=0.4.17 (from sqlalchemy>=0.7.3->ibm_db_sa->-r requirements.txt (line 1))
  Using cached greenlet-3.1.1-cp312-cp312-macosx_11_0_universal2.whl.metadata (3.8 kB)
Using cached ibm_db_sa-0.4.1-py3-none-any.whl (31 kB)
Downloading pymssql-2.3.2-cp312-cp312-macosx_13_0_universal2.whl (3.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.0/3.0 MB 9.2 MB/s eta 0:00:00
Using cached ibm_db-3.2.3-cp312-cp312-macosx_10_15_x86_64.whl (24.0 MB)
Using cached SQLAlchemy-2.0.36-cp312-cp312-macosx_10_13_x86_64.whl (2.1 MB)
Using cached greenlet-3.1.1-cp312-cp312-macosx_11_0_universal2.whl (274 kB)
Using cached typing_extensions-4.12.2-py3-none-any.whl (37 kB)
Installing collected packages: pymssql, ibm-db, typing-extensions, greenlet, sqlalchemy, ibm_db_sa
Successfully installed greenlet-3.1.1 ibm-db-3.2.3 ibm_db_sa-0.4.1 pymssql-2.3.2 sqlalchemy-2.0.36 typing-extensions-4.12.2

[notice] A new release of pip is available: 24.2 -> 24.3.1
[notice] To update, run: pip install --upgrade pip
++ type python
python is /Users/dwt/Code/Projekte/nix/devenv-reproduction/venv/bin/python
++ python --version
Python 3.12.7
++ python -c 'import ibm_db'
++ python -c 'import pymssql'
testing x86 compile
Obtaining file:///Users/dwt/Code/Projekte/nix/devenv-reproduction/pymssql
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Building wheels for collected packages: pymssql
  Building editable for pymssql (pyproject.toml) ... done
  Created wheel for pymssql: filename=pymssql-2.3.2.dev3-0.editable-cp312-cp312-macosx_10_12_x86_64.whl size=13615 sha256=fcf1ab43ca4f2d91b052d9b63130f6c396e455675e60a7b158c32457ca4e0c67
  Stored in directory: /private/tmp/nix-shell.z2pYNR/pip-ephem-wheel-cache-7txef2hn/wheels/ce/19/95/edf0e96a03cc462dfa4aa4e342bccdb376ea4ad475a25073c2
Successfully built pymssql
Installing collected packages: pymssql
  Attempting uninstall: pymssql
    Found existing installation: pymssql 2.3.2
    Uninstalling pymssql-2.3.2:
      Successfully uninstalled pymssql-2.3.2
Successfully installed pymssql-2.3.2.dev3

[notice] A new release of pip is available: 24.2 -> 24.3.1
[notice] To update, run: pip install --upgrade pip
++ type python
python is /Users/dwt/Code/Projekte/nix/devenv-reproduction/venv/bin/python
++ python --version
Python 3.12.7
++ python -c 'import ibm_db'
++ python -c 'import pymssql'

testing uv
uv 0.4.30
Using CPython 3.12.7 interpreter at: /nix/store/ak0sh27jipas724h0c7raxvdfmw824k8-python3-3.12.7/bin/python3
Creating virtual environment at: venv
Activate with: source venv/bin/activate
Using Python 3.12.7 environment at venv
⠙ Resolving dependencies...                                                                                                     DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: ibm-db-sa*
DEBUG Adding direct dependency: pymssql*
TRACE Received package metadata for: ibm-db-sa
TRACE Selecting candidate for ibm-db-sa with range * with 15 remote versions
DEBUG Searching for a compatible version of ibm-db-sa (*)
TRACE Selecting candidate for ibm-db-sa with range * with 15 remote versions
TRACE found candidate for package PackageName("ibm-db-sa") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.4.1" version
TRACE found candidate for package PackageName("ibm-db-sa") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.4.1" version
DEBUG Selecting: ibm-db-sa==0.4.1 [compatible] (ibm_db_sa-0.4.1-py3-none-any.whl)
⠙ ibm-db-sa==0.4.1                                                                                                              TRACE Received built distribution metadata for: ibm-db-sa==0.4.1
DEBUG Adding transitive dependency for ibm-db-sa==0.4.1: sqlalchemy>=0.7.3
DEBUG Adding transitive dependency for ibm-db-sa==0.4.1: ibm-db>=2.0.0
TRACE Received package metadata for: ibm-db
TRACE Received package metadata for: sqlalchemy
TRACE Received package metadata for: pymssql
TRACE Selecting candidate for ibm-db with range >=2.0.0 with 48 remote versions
DEBUG Searching for a compatible version of pymssql (*)
TRACE Selecting candidate for pymssql with range * with 29 remote versions
TRACE found candidate for package PackageName("ibm-db") with range Ranges { segments: [(Included("2.0.0"), Unbounded)] } after 1 steps: "3.2.3" version
TRACE found candidate for package PackageName("pymssql") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.3.2" version
DEBUG Selecting: pymssql==2.3.2 [compatible] (pymssql-2.3.2-cp312-cp312-macosx_13_0_universal2.whl)
TRACE Selecting candidate for sqlalchemy with range >=0.7.3 with 307 remote versions
⠙ pymssql==2.3.2                                                                                                                TRACE found candidate for package PackageName("sqlalchemy") with range Ranges { segments: [(Included("0.7.3"), Unbounded)] } after 1 steps: "2.0.36" version
TRACE Selecting candidate for pymssql with range * with 29 remote versions
TRACE found candidate for package PackageName("pymssql") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.3.2" version
TRACE Received built distribution metadata for: ibm-db==3.2.3
TRACE Received built distribution metadata for: sqlalchemy==2.0.36
⠹ pymssql==2.3.2                                                                                                                TRACE Received built distribution metadata for: pymssql==2.3.2
DEBUG Searching for a compatible version of sqlalchemy (>=0.7.3)
TRACE Selecting candidate for sqlalchemy with range >=0.7.3 with 307 remote versions
TRACE found candidate for package PackageName("sqlalchemy") with range Ranges { segments: [(Included("0.7.3"), Unbounded)] } after 1 steps: "2.0.36" version
DEBUG Selecting: sqlalchemy==2.0.36 [compatible] (SQLAlchemy-2.0.36-cp312-cp312-macosx_10_13_x86_64.whl)
⠹ sqlalchemy==2.0.36                                                                                                            TRACE skipping importlib-metadata ; python_full_version < '3.8' because of Requires-Python: >=3.12.7
DEBUG Adding transitive dependency for sqlalchemy==2.0.36: typing-extensions>=4.6.0
DEBUG Adding transitive dependency for sqlalchemy==2.0.36: greenlet{(python_full_version < '3.13' and platform_machine == 'AMD64') or (python_full_version < '3.13' and platform_machine == 'WIN32') or (python_full_version < '3.13' and platform_machine == 'aarch64') or (python_full_version < '3.13' and platform_machine == 'amd64') or (python_full_version < '3.13' and platform_machine == 'ppc64le') or (python_full_version < '3.13' and platform_machine == 'win32') or (python_full_version < '3.13' and platform_machine == 'x86_64')}<0.4.17 | >0.4.17
DEBUG Searching for a compatible version of ibm-db (>=2.0.0)
TRACE Selecting candidate for ibm-db with range >=2.0.0 with 48 remote versions
TRACE found candidate for package PackageName("ibm-db") with range Ranges { segments: [(Included("2.0.0"), Unbounded)] } after 1 steps: "3.2.3" version
DEBUG Selecting: ibm-db==3.2.3 [compatible] (ibm_db-3.2.3-cp312-cp312-macosx_10_15_x86_64.whl)
⠹ ibm-db==3.2.3                                                                                                                 TRACE Received package metadata for: greenlet
TRACE Received package metadata for: typing-extensions
TRACE Selecting candidate for typing-extensions with range >=4.6.0 with 40 remote versions
DEBUG Searching for a compatible version of typing-extensions (>=4.6.0)
TRACE Selecting candidate for typing-extensions with range >=4.6.0 with 40 remote versions
TRACE found candidate for package PackageName("typing-extensions") with range Ranges { segments: [(Included("4.6.0"), Unbounded)] } after 1 steps: "4.12.2" version
TRACE found candidate for package PackageName("typing-extensions") with range Ranges { segments: [(Included("4.6.0"), Unbounded)] } after 1 steps: "4.12.2" version
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
⠹ typing-extensions==4.12.2                                                                                                     TRACE Received built distribution metadata for: typing-extensions==4.12.2
DEBUG Searching for a compatible version of greenlet{(python_full_version < '3.13' and platform_machine == 'AMD64') or (python_full_version < '3.13' and platform_machine == 'WIN32') or (python_full_version < '3.13' and platform_machine == 'aarch64') or (python_full_version < '3.13' and platform_machine == 'amd64') or (python_full_version < '3.13' and platform_machine == 'ppc64le') or (python_full_version < '3.13' and platform_machine == 'win32') or (python_full_version < '3.13' and platform_machine == 'x86_64')} (<0.4.17 | >0.4.17)
TRACE Selecting candidate for greenlet with range <0.4.17 | >0.4.17 with 52 remote versions
TRACE found candidate for package PackageName("greenlet") with range Ranges { segments: [(Unbounded, Excluded("0.4.17")), (Excluded("0.4.17"), Unbounded)] } after 1 steps: "3.1.1" version
DEBUG Selecting: greenlet==3.1.1 [compatible] (greenlet-3.1.1-cp312-cp312-macosx_11_0_universal2.whl)
DEBUG Adding transitive dependency for greenlet==3.1.1: greenlet==3.1.1
DEBUG Adding transitive dependency for greenlet==3.1.1: greenlet{(python_full_version < '3.13' and platform_machine == 'AMD64') or (python_full_version < '3.13' and platform_machine == 'WIN32') or (python_full_version < '3.13' and platform_machine == 'aarch64') or (python_full_version < '3.13' and platform_machine == 'amd64') or (python_full_version < '3.13' and platform_machine == 'ppc64le') or (python_full_version < '3.13' and platform_machine == 'win32') or (python_full_version < '3.13' and platform_machine == 'x86_64')}==3.1.1
TRACE Selecting candidate for greenlet with range ==3.1.1 with 52 remote versions
TRACE found candidate for package PackageName("greenlet") with range Ranges { segments: [(Included("3.1.1"), Included("3.1.1"))] } after 1 steps: "3.1.1" version
DEBUG Searching for a compatible version of greenlet{(python_full_version < '3.13' and platform_machine == 'AMD64') or (python_full_version < '3.13' and platform_machine == 'WIN32') or (python_full_version < '3.13' and platform_machine == 'aarch64') or (python_full_version < '3.13' and platform_machine == 'amd64') or (python_full_version < '3.13' and platform_machine == 'ppc64le') or (python_full_version < '3.13' and platform_machine == 'win32') or (python_full_version < '3.13' and platform_machine == 'x86_64')} (==3.1.1)
TRACE Selecting candidate for greenlet with range ==3.1.1 with 52 remote versions
TRACE found candidate for package PackageName("greenlet") with range Ranges { segments: [(Included("3.1.1"), Included("3.1.1"))] } after 1 steps: "3.1.1" version
DEBUG Selecting: greenlet==3.1.1 [compatible] (greenlet-3.1.1-cp312-cp312-macosx_11_0_universal2.whl)
⠹ greenlet==3.1.1                                                                                                               TRACE Received built distribution metadata for: greenlet==3.1.1
DEBUG Searching for a compatible version of greenlet (==3.1.1)
TRACE Selecting candidate for greenlet with range ==3.1.1 with 52 remote versions
TRACE found candidate for package PackageName("greenlet") with range Ranges { segments: [(Included("3.1.1"), Included("3.1.1"))] } after 1 steps: "3.1.1" version
DEBUG Selecting: greenlet==3.1.1 [compatible] (greenlet-3.1.1-cp312-cp312-macosx_11_0_universal2.whl)
⠹ greenlet==3.1.1                                                                                                               DEBUG Tried 6 versions: greenlet 1, ibm-db 1, ibm-db-sa 1, pymssql 1, sqlalchemy 1, typing-extensions 1
DEBUG marker environment resolution took 0.404s
⠸ greenlet==3.1.1                                                                                                               TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.7", version: "3.12.7" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "24.1.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.1.0: Thu Oct 10 21:05:23 PDT 2024; root:xnu-11215.41.3~2/RELEASE_ARM64_T6031", python_full_version: StringVersion { string: "3.12.7", version: "3.12.7" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "darwin" } }) } }
TRACE Resolution edge: ibm-db-sa -> sqlalchemy
TRACE Resolution edge:     0.4.1 -> 2.0.36
TRACE Resolution edge: ibm-db-sa -> ibm-db
TRACE Resolution edge:     0.4.1 -> 3.2.3
TRACE Resolution edge: sqlalchemy -> typing-extensions
TRACE Resolution edge:     2.0.36 -> 4.12.2
TRACE Resolution edge: ROOT -> ibm-db-sa
TRACE Resolution edge:     0a0.dev0 -> 0.4.1
TRACE Resolution edge: sqlalchemy -> greenlet
TRACE Resolution edge:     2.0.36 -> 3.1.1 ; (python_full_version < '3.13' and platform_machine == 'AMD64') or (python_full_version < '3.13' and platform_machine == 'WIN32') or (python_full_version < '3.13' and platform_machine == 'aarch64') or (python_full_version < '3.13' and platform_machine == 'amd64') or (python_full_version < '3.13' and platform_machine == 'ppc64le') or (python_full_version < '3.13' and platform_machine == 'win32') or (python_full_version < '3.13' and platform_machine == 'x86_64')
TRACE Resolution edge: ROOT -> pymssql
TRACE Resolution edge:     0a0.dev0 -> 2.3.2
Resolved 6 packages in 407ms
Prepared 1 package in 255ms
Installed 6 packages in 18ms
 + greenlet==3.1.1
 + ibm-db==3.2.3
 + ibm-db-sa==0.4.1
 + pymssql==2.3.2
 + sqlalchemy==2.0.36
 + typing-extensions==4.12.2
++ type python
python is /Users/dwt/Code/Projekte/nix/devenv-reproduction/venv/bin/python
++ python --version
Python 3.12.7
++ python -c 'import ibm_db'
++ python -c 'import pymssql'
testing uv x86 compile
Using Python 3.12.7 environment at /Users/dwt/Code/Projekte/nix/devenv-reproduction/venv
⠙ Resolving dependencies...                                                                                                     DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: setuptools>=54.0
DEBUG Adding direct dependency: setuptools-scm>=5.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0
DEBUG Adding direct dependency: wheel>=0.36.2
DEBUG Adding direct dependency: cython>=3.0.10
DEBUG Adding direct dependency: tomli*
TRACE Received package metadata for: setuptools-scm
TRACE Received package metadata for: cython
TRACE Selecting candidate for setuptools-scm with range >=5.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Unbounded)] } after 1 steps: "8.1.0" version
TRACE Selecting candidate for cython with range >=3.0.10 with 140 remote versions
TRACE found candidate for package PackageName("cython") with range Ranges { segments: [(Included("3.0.10"), Unbounded)] } after 2 steps: "3.0.11" version
TRACE Received package metadata for: tomli
TRACE Received package metadata for: wheel
TRACE Selecting candidate for tomli with range * with 25 remote versions
TRACE found candidate for package PackageName("tomli") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.1.0" version
TRACE Selecting candidate for wheel with range >=0.36.2 with 77 remote versions
TRACE found candidate for package PackageName("wheel") with range Ranges { segments: [(Included("0.36.2"), Unbounded)] } after 1 steps: "0.45.0" version
TRACE Received built distribution metadata for: cython==3.0.11
TRACE Received built distribution metadata for: setuptools-scm==8.1.0
TRACE Received built distribution metadata for: tomli==2.1.0
TRACE Received built distribution metadata for: wheel==0.45.0
⠹ Resolving dependencies...                                                                                                     TRACE Received package metadata for: setuptools
TRACE Selecting candidate for setuptools with range >=54.0 with 585 remote versions
TRACE found candidate for package PackageName("setuptools") with range Ranges { segments: [(Included("54.0"), Unbounded)] } after 1 steps: "75.6.0" version
DEBUG Searching for a compatible version of setuptools (>=54.0)
TRACE Selecting candidate for setuptools with range >=54.0 with 585 remote versions
TRACE found candidate for package PackageName("setuptools") with range Ranges { segments: [(Included("54.0"), Unbounded)] } after 1 steps: "75.6.0" version
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
TRACE Received built distribution metadata for: setuptools==75.6.0
TRACE skipping importlib-metadata>=6 ; python_full_version < '3.10' and extra == 'core' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=2.0.1 ; python_full_version < '3.11' and extra == 'core' because of Requires-Python: >=3.12.7
TRACE skipping importlib-metadata>=7.0.2 ; python_full_version < '3.10' and extra == 'type' because of Requires-Python: >=3.12.7
DEBUG Searching for a compatible version of setuptools-scm (>=5.0)
TRACE Selecting candidate for setuptools-scm with range >=5.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Unbounded)] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE skipping typing-extensions ; python_full_version < '3.10' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=1 ; python_full_version < '3.11' because of Requires-Python: >=3.12.7
TRACE skipping typing-extensions ; python_full_version < '3.11' and extra == 'test' because of Requires-Python: >=3.12.7
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (>=5.0)
TRACE Selecting candidate for setuptools-scm with range >=5.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Unbounded)] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm==8.1.0
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm[toml]==8.1.0
DEBUG Searching for a compatible version of setuptools-scm[toml] (==8.1.0)
TRACE Selecting candidate for setuptools-scm with range ==8.1.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("8.1.0"), Included("8.1.0"))] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE skipping typing-extensions ; python_full_version < '3.10' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=1 ; python_full_version < '3.11' because of Requires-Python: >=3.12.7
TRACE skipping typing-extensions ; python_full_version < '3.11' and extra == 'test' because of Requires-Python: >=3.12.7
DEBUG Searching for a compatible version of wheel (>=0.36.2)
TRACE Selecting candidate for wheel with range >=0.36.2 with 77 remote versions
TRACE found candidate for package PackageName("wheel") with range Ranges { segments: [(Included("0.36.2"), Unbounded)] } after 1 steps: "0.45.0" version
DEBUG Selecting: wheel==0.45.0 [compatible] (wheel-0.45.0-py3-none-any.whl)
DEBUG Searching for a compatible version of cython (>=3.0.10)
TRACE Selecting candidate for cython with range >=3.0.10 with 140 remote versions
TRACE found candidate for package PackageName("cython") with range Ranges { segments: [(Included("3.0.10"), Unbounded)] } after 2 steps: "3.0.11" version
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of tomli (*)
TRACE Selecting candidate for tomli with range * with 25 remote versions
TRACE found candidate for package PackageName("tomli") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.1.0" version
DEBUG Selecting: tomli==2.1.0 [compatible] (tomli-2.1.0-py3-none-any.whl)
TRACE Received package metadata for: packaging
TRACE Selecting candidate for packaging with range >=20 with 46 remote versions
DEBUG Searching for a compatible version of packaging (>=20)
TRACE Selecting candidate for packaging with range >=20 with 46 remote versions
TRACE found candidate for package PackageName("packaging") with range Ranges { segments: [(Included("20"), Unbounded)] } after 1 steps: "24.2" version
TRACE found candidate for package PackageName("packaging") with range Ranges { segments: [(Included("20"), Unbounded)] } after 1 steps: "24.2" version
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
TRACE Received built distribution metadata for: packaging==24.2
DEBUG Tried 6 versions: cython 1, packaging 1, setuptools 1, setuptools-scm 1, tomli 1, wheel 1
DEBUG marker environment resolution took 0.303s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.7", version: "3.12.7" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "24.1.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.1.0: Thu Oct 10 21:05:23 PDT 2024; root:xnu-11215.41.3~2/RELEASE_ARM64_T6031", python_full_version: StringVersion { string: "3.12.7", version: "3.12.7" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "darwin" } }) } }
TRACE Resolution edge: ROOT -> setuptools
TRACE Resolution edge:     0a0.dev0 -> 75.6.0
TRACE Resolution edge: ROOT -> cython
TRACE Resolution edge:     0a0.dev0 -> 3.0.11
TRACE Resolution edge: ROOT -> setuptools-scm
TRACE Resolution edge:     0a0.dev0 -> 8.1.0 (extra: toml)
TRACE Resolution edge: ROOT -> wheel
TRACE Resolution edge:     0a0.dev0 -> 0.45.0
TRACE Resolution edge: setuptools-scm -> setuptools
TRACE Resolution edge:     8.1.0 -> 75.6.0
TRACE Resolution edge: ROOT -> tomli
TRACE Resolution edge:     0a0.dev0 -> 2.1.0
TRACE Resolution edge: ROOT -> setuptools-scm
TRACE Resolution edge:     0a0.dev0 -> 8.1.0
TRACE Resolution edge: setuptools-scm -> packaging
TRACE Resolution edge:     8.1.0 -> 24.2
⠧ Resolving dependencies...                                                                                                     DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: setuptools>=54.0
DEBUG Adding direct dependency: setuptools-scm>=5.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0
DEBUG Adding direct dependency: wheel>=0.36.2
DEBUG Adding direct dependency: cython>=3.0.10
DEBUG Adding direct dependency: tomli*
DEBUG Adding direct dependency: setuptools-scm>=5.0, <9.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0, <9.0
DEBUG Adding direct dependency: cython>=3.0.7
DEBUG Searching for a compatible version of setuptools (>=54.0)
TRACE Selecting candidate for setuptools with range >=54.0 with 585 remote versions
TRACE found candidate for package PackageName("setuptools") with range Ranges { segments: [(Included("54.0"), Unbounded)] } after 1 steps: "75.6.0" version
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
TRACE skipping importlib-metadata>=6 ; python_full_version < '3.10' and extra == 'core' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=2.0.1 ; python_full_version < '3.11' and extra == 'core' because of Requires-Python: >=3.12.7
TRACE skipping importlib-metadata>=7.0.2 ; python_full_version < '3.10' and extra == 'type' because of Requires-Python: >=3.12.7
DEBUG Searching for a compatible version of setuptools-scm (>=5.0, <9.0)
TRACE Selecting candidate for setuptools-scm with range >=5.0, <9.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Excluded("9.0"))] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE skipping typing-extensions ; python_full_version < '3.10' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=1 ; python_full_version < '3.11' because of Requires-Python: >=3.12.7
TRACE skipping typing-extensions ; python_full_version < '3.11' and extra == 'test' because of Requires-Python: >=3.12.7
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (>=5.0, <9.0)
TRACE Selecting candidate for setuptools-scm with range >=5.0, <9.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Excluded("9.0"))] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm==8.1.0
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm[toml]==8.1.0
DEBUG Searching for a compatible version of setuptools-scm[toml] (==8.1.0)
TRACE Selecting candidate for setuptools-scm with range ==8.1.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("8.1.0"), Included("8.1.0"))] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE skipping typing-extensions ; python_full_version < '3.10' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=1 ; python_full_version < '3.11' because of Requires-Python: >=3.12.7
TRACE skipping typing-extensions ; python_full_version < '3.11' and extra == 'test' because of Requires-Python: >=3.12.7
DEBUG Searching for a compatible version of wheel (>=0.36.2)
TRACE Selecting candidate for wheel with range >=0.36.2 with 77 remote versions
TRACE found candidate for package PackageName("wheel") with range Ranges { segments: [(Included("0.36.2"), Unbounded)] } after 1 steps: "0.45.0" version
DEBUG Selecting: wheel==0.45.0 [compatible] (wheel-0.45.0-py3-none-any.whl)
DEBUG Searching for a compatible version of cython (>=3.0.10)
TRACE Selecting candidate for cython with range >=3.0.10 with 140 remote versions
TRACE found candidate for package PackageName("cython") with range Ranges { segments: [(Included("3.0.10"), Unbounded)] } after 2 steps: "3.0.11" version
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of tomli (*)
TRACE Selecting candidate for tomli with range * with 25 remote versions
TRACE found candidate for package PackageName("tomli") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.1.0" version
DEBUG Selecting: tomli==2.1.0 [compatible] (tomli-2.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20)
TRACE Selecting candidate for packaging with range >=20 with 46 remote versions
TRACE found candidate for package PackageName("packaging") with range Ranges { segments: [(Included("20"), Unbounded)] } after 1 steps: "24.2" version
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Tried 6 versions: cython 1, packaging 1, setuptools 1, setuptools-scm 1, tomli 1, wheel 1
DEBUG marker environment resolution took 0.001s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.7", version: "3.12.7" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "24.1.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.1.0: Thu Oct 10 21:05:23 PDT 2024; root:xnu-11215.41.3~2/RELEASE_ARM64_T6031", python_full_version: StringVersion { string: "3.12.7", version: "3.12.7" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "darwin" } }) } }
TRACE Resolution edge: ROOT -> setuptools
TRACE Resolution edge:     0a0.dev0 -> 75.6.0
TRACE Resolution edge: ROOT -> cython
TRACE Resolution edge:     0a0.dev0 -> 3.0.11
TRACE Resolution edge: ROOT -> setuptools-scm
TRACE Resolution edge:     0a0.dev0 -> 8.1.0 (extra: toml)
TRACE Resolution edge: ROOT -> wheel
TRACE Resolution edge:     0a0.dev0 -> 0.45.0
TRACE Resolution edge: setuptools-scm -> setuptools
TRACE Resolution edge:     8.1.0 -> 75.6.0
TRACE Resolution edge: ROOT -> tomli
TRACE Resolution edge:     0a0.dev0 -> 2.1.0
TRACE Resolution edge: ROOT -> setuptools-scm
TRACE Resolution edge:     0a0.dev0 -> 8.1.0
TRACE Resolution edge: setuptools-scm -> packaging
TRACE Resolution edge:     8.1.0 -> 24.2
TRACE Selecting candidate for cython with range >=3.0.10 with 140 remote versions
TRACE found candidate for package PackageName("cython") with range Ranges { segments: [(Included("3.0.10"), Unbounded)] } after 2 steps: "3.0.11" version
TRACE Selecting candidate for setuptools-scm with range >=5.0, <9.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Excluded("9.0"))] } after 1 steps: "8.1.0" version
TRACE Selecting candidate for tomli with range * with 25 remote versions
TRACE found candidate for package PackageName("tomli") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.1.0" version
TRACE Selecting candidate for wheel with range >=0.36.2 with 77 remote versions
TRACE found candidate for package PackageName("wheel") with range Ranges { segments: [(Included("0.36.2"), Unbounded)] } after 1 steps: "0.45.0" version
TRACE Selecting candidate for setuptools with range >=54.0 with 585 remote versions
TRACE found candidate for package PackageName("setuptools") with range Ranges { segments: [(Included("54.0"), Unbounded)] } after 1 steps: "75.6.0" version
TRACE Selecting candidate for packaging with range >=20 with 46 remote versions
TRACE found candidate for package PackageName("packaging") with range Ranges { segments: [(Included("20"), Unbounded)] } after 1 steps: "24.2" version
⠙ Resolving dependencies...                                                                                                     DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: pymssql*
DEBUG Searching for a compatible version of pymssql @ file:///Users/dwt/Code/Projekte/nix/devenv-reproduction/pymssql (*)
⠙ pymssql==2.3.2.dev3                                                                                                           DEBUG Tried 1 versions: pymssql 1
DEBUG marker environment resolution took 0.003s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.7", version: "3.12.7" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "24.1.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.1.0: Thu Oct 10 21:05:23 PDT 2024; root:xnu-11215.41.3~2/RELEASE_ARM64_T6031", python_full_version: StringVersion { string: "3.12.7", version: "3.12.7" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "darwin" } }) } }
TRACE Resolution edge: ROOT -> pymssql
TRACE Resolution edge:     0a0.dev0 -> 2.3.2.dev3
Resolved 1 package in 1.71s
Building pymssql @ file:///Users/dwt/Code/Projekte/nix/devenv-reproduction/pymssql
⠙ Preparing packages... (0/1)                                                                                                   DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: setuptools>=54.0
DEBUG Adding direct dependency: setuptools-scm>=5.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0
DEBUG Adding direct dependency: wheel>=0.36.2
DEBUG Adding direct dependency: cython>=3.0.10
DEBUG Adding direct dependency: tomli*
DEBUG Searching for a compatible version of setuptools (>=54.0)
TRACE Selecting candidate for tomli with range * with 25 remote versions
TRACE Selecting candidate for setuptools with range >=54.0 with 585 remote versions
TRACE found candidate for package PackageName("tomli") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.1.0" version
TRACE found candidate for package PackageName("setuptools") with range Ranges { segments: [(Included("54.0"), Unbounded)] } after 1 steps: "75.6.0" version
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
TRACE Selecting candidate for cython with range >=3.0.10 with 140 remote versions
TRACE found candidate for package PackageName("cython") with range Ranges { segments: [(Included("3.0.10"), Unbounded)] } after 2 steps: "3.0.11" version
TRACE Selecting candidate for wheel with range >=0.36.2 with 77 remote versions
TRACE found candidate for package PackageName("wheel") with range Ranges { segments: [(Included("0.36.2"), Unbounded)] } after 1 steps: "0.45.0" version
TRACE Selecting candidate for setuptools-scm with range >=5.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Unbounded)] } after 1 steps: "8.1.0" version
TRACE Selecting candidate for setuptools with range >=54.0 with 585 remote versions
TRACE found candidate for package PackageName("setuptools") with range Ranges { segments: [(Included("54.0"), Unbounded)] } after 1 steps: "75.6.0" version
TRACE skipping importlib-metadata>=6 ; python_full_version < '3.10' and extra == 'core' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=2.0.1 ; python_full_version < '3.11' and extra == 'core' because of Requires-Python: >=3.12.7
TRACE skipping importlib-metadata>=7.0.2 ; python_full_version < '3.10' and extra == 'type' because of Requires-Python: >=3.12.7
DEBUG Searching for a compatible version of setuptools-scm (>=5.0)
TRACE Selecting candidate for setuptools-scm with range >=5.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Unbounded)] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE skipping typing-extensions ; python_full_version < '3.10' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=1 ; python_full_version < '3.11' because of Requires-Python: >=3.12.7
TRACE skipping typing-extensions ; python_full_version < '3.11' and extra == 'test' because of Requires-Python: >=3.12.7
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (>=5.0)
TRACE Selecting candidate for setuptools-scm with range >=5.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Unbounded)] } after 1 steps: "8.1.0" version
TRACE Selecting candidate for packaging with range >=20 with 46 remote versions
TRACE found candidate for package PackageName("packaging") with range Ranges { segments: [(Included("20"), Unbounded)] } after 1 steps: "24.2" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm==8.1.0
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm[toml]==8.1.0
DEBUG Searching for a compatible version of setuptools-scm[toml] (==8.1.0)
TRACE Selecting candidate for setuptools-scm with range ==8.1.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("8.1.0"), Included("8.1.0"))] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE skipping typing-extensions ; python_full_version < '3.10' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=1 ; python_full_version < '3.11' because of Requires-Python: >=3.12.7
TRACE skipping typing-extensions ; python_full_version < '3.11' and extra == 'test' because of Requires-Python: >=3.12.7
DEBUG Searching for a compatible version of wheel (>=0.36.2)
TRACE Selecting candidate for wheel with range >=0.36.2 with 77 remote versions
TRACE found candidate for package PackageName("wheel") with range Ranges { segments: [(Included("0.36.2"), Unbounded)] } after 1 steps: "0.45.0" version
DEBUG Selecting: wheel==0.45.0 [compatible] (wheel-0.45.0-py3-none-any.whl)
DEBUG Searching for a compatible version of cython (>=3.0.10)
TRACE Selecting candidate for cython with range >=3.0.10 with 140 remote versions
TRACE found candidate for package PackageName("cython") with range Ranges { segments: [(Included("3.0.10"), Unbounded)] } after 2 steps: "3.0.11" version
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of tomli (*)
TRACE Selecting candidate for tomli with range * with 25 remote versions
TRACE found candidate for package PackageName("tomli") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.1.0" version
DEBUG Selecting: tomli==2.1.0 [compatible] (tomli-2.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20)
TRACE Selecting candidate for packaging with range >=20 with 46 remote versions
TRACE found candidate for package PackageName("packaging") with range Ranges { segments: [(Included("20"), Unbounded)] } after 1 steps: "24.2" version
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Tried 6 versions: cython 1, packaging 1, setuptools 1, setuptools-scm 1, tomli 1, wheel 1
DEBUG marker environment resolution took 0.000s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.7", version: "3.12.7" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "24.1.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.1.0: Thu Oct 10 21:05:23 PDT 2024; root:xnu-11215.41.3~2/RELEASE_ARM64_T6031", python_full_version: StringVersion { string: "3.12.7", version: "3.12.7" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "darwin" } }) } }
TRACE Resolution edge: ROOT -> setuptools
TRACE Resolution edge:     0a0.dev0 -> 75.6.0
TRACE Resolution edge: ROOT -> cython
TRACE Resolution edge:     0a0.dev0 -> 3.0.11
TRACE Resolution edge: ROOT -> setuptools-scm
TRACE Resolution edge:     0a0.dev0 -> 8.1.0 (extra: toml)
TRACE Resolution edge: ROOT -> wheel
TRACE Resolution edge:     0a0.dev0 -> 0.45.0
TRACE Resolution edge: setuptools-scm -> setuptools
TRACE Resolution edge:     8.1.0 -> 75.6.0
TRACE Resolution edge: ROOT -> tomli
TRACE Resolution edge:     0a0.dev0 -> 2.1.0
TRACE Resolution edge: ROOT -> setuptools-scm
TRACE Resolution edge:     0a0.dev0 -> 8.1.0
TRACE Resolution edge: setuptools-scm -> packaging
Building pymssql @ file:///Users/dwt/Code/Projekte/nix/devenv-reproduction/pymssql
⠸ Preparing packages... (0/1)                                                                                                   DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: setuptools>=54.0
DEBUG Adding direct dependency: setuptools-scm>=5.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0
DEBUG Adding direct dependency: wheel>=0.36.2
DEBUG Adding direct dependency: cython>=3.0.10
DEBUG Adding direct dependency: tomli*
DEBUG Adding direct dependency: setuptools-scm>=5.0, <9.0
DEBUG Adding direct dependency: setuptools-scm[toml]>=5.0, <9.0
DEBUG Adding direct dependency: cython>=3.0.7
DEBUG Searching for a compatible version of setuptools (>=54.0)
TRACE Selecting candidate for setuptools with range >=54.0 with 585 remote versions
TRACE found candidate for package PackageName("setuptools") with range Ranges { segments: [(Included("54.0"), Unbounded)] } after 1 steps: "75.6.0" version
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
TRACE Selecting candidate for cython with range >=3.0.10 with 140 remote versions
TRACE found candidate for package PackageName("cython") with range Ranges { segments: [(Included("3.0.10"), Unbounded)] } after 2 steps: "3.0.11" version
TRACE Selecting candidate for setuptools-scm with range >=5.0, <9.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Excluded("9.0"))] } after 1 steps: "8.1.0" version
TRACE Selecting candidate for tomli with range * with 25 remote versions
TRACE found candidate for package PackageName("tomli") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.1.0" version
TRACE Selecting candidate for wheel with range >=0.36.2 with 77 remote versions
TRACE found candidate for package PackageName("wheel") with range Ranges { segments: [(Included("0.36.2"), Unbounded)] } after 1 steps: "0.45.0" version
TRACE Selecting candidate for setuptools with range >=54.0 with 585 remote versions
TRACE found candidate for package PackageName("setuptools") with range Ranges { segments: [(Included("54.0"), Unbounded)] } after 1 steps: "75.6.0" version
TRACE skipping importlib-metadata>=6 ; python_full_version < '3.10' and extra == 'core' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=2.0.1 ; python_full_version < '3.11' and extra == 'core' because of Requires-Python: >=3.12.7
TRACE skipping importlib-metadata>=7.0.2 ; python_full_version < '3.10' and extra == 'type' because of Requires-Python: >=3.12.7
DEBUG Searching for a compatible version of setuptools-scm (>=5.0, <9.0)
TRACE Selecting candidate for setuptools-scm with range >=5.0, <9.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Excluded("9.0"))] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE skipping typing-extensions ; python_full_version < '3.10' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=1 ; python_full_version < '3.11' because of Requires-Python: >=3.12.7
TRACE skipping typing-extensions ; python_full_version < '3.11' and extra == 'test' because of Requires-Python: >=3.12.7
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (>=5.0, <9.0)
TRACE Selecting candidate for setuptools-scm with range >=5.0, <9.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("5.0"), Excluded("9.0"))] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE Selecting candidate for packaging with range >=20 with 46 remote versions
TRACE found candidate for package PackageName("packaging") with range Ranges { segments: [(Included("20"), Unbounded)] } after 1 steps: "24.2" version
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm==8.1.0
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools-scm[toml]==8.1.0
DEBUG Searching for a compatible version of setuptools-scm[toml] (==8.1.0)
TRACE Selecting candidate for setuptools-scm with range ==8.1.0 with 84 remote versions
TRACE found candidate for package PackageName("setuptools-scm") with range Ranges { segments: [(Included("8.1.0"), Included("8.1.0"))] } after 1 steps: "8.1.0" version
DEBUG Selecting: setuptools-scm==8.1.0 [compatible] (setuptools_scm-8.1.0-py3-none-any.whl)
TRACE skipping typing-extensions ; python_full_version < '3.10' because of Requires-Python: >=3.12.7
TRACE skipping tomli>=1 ; python_full_version < '3.11' because of Requires-Python: >=3.12.7
TRACE skipping typing-extensions ; python_full_version < '3.11' and extra == 'test' because of Requires-Python: >=3.12.7
DEBUG Searching for a compatible version of wheel (>=0.36.2)
TRACE Selecting candidate for wheel with range >=0.36.2 with 77 remote versions
TRACE found candidate for package PackageName("wheel") with range Ranges { segments: [(Included("0.36.2"), Unbounded)] } after 1 steps: "0.45.0" version
DEBUG Selecting: wheel==0.45.0 [compatible] (wheel-0.45.0-py3-none-any.whl)
DEBUG Searching for a compatible version of cython (>=3.0.10)
TRACE Selecting candidate for cython with range >=3.0.10 with 140 remote versions
TRACE found candidate for package PackageName("cython") with range Ranges { segments: [(Included("3.0.10"), Unbounded)] } after 2 steps: "3.0.11" version
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Searching for a compatible version of tomli (*)
TRACE Selecting candidate for tomli with range * with 25 remote versions
TRACE found candidate for package PackageName("tomli") with range Ranges { segments: [(Unbounded, Unbounded)] } after 1 steps: "2.1.0" version
DEBUG Selecting: tomli==2.1.0 [compatible] (tomli-2.1.0-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (>=20)
TRACE Selecting candidate for packaging with range >=20 with 46 remote versions
TRACE found candidate for package PackageName("packaging") with range Ranges { segments: [(Included("20"), Unbounded)] } after 1 steps: "24.2" version
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Tried 6 versions: cython 1, packaging 1, setuptools 1, setuptools-scm 1, tomli 1, wheel 1
DEBUG marker environment resolution took 0.001s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.12.7", version: "3.12.7" }, os_name: "posix", platform_machine: "x86_64", platform_python_implementation: "CPython", platform_release: "24.1.0", platform_system: "Darwin", platform_version: "Darwin Kernel Version 24.1.0: Thu Oct 10 21:05:23 PDT 2024; root:xnu-11215.41.3~2/RELEASE_ARM64_T6031", python_full_version: StringVersion { string: "3.12.7", version: "3.12.7" }, python_version: StringVersion { string: "3.12", version: "3.12" }, sys_platform: "darwin" } }) } }
TRACE Resolution edge: ROOT -> setuptools
TRACE Resolution edge:     0a0.dev0 -> 75.6.0
TRACE Resolution edge: ROOT -> cython
TRACE Resolution edge:     0a0.dev0 -> 3.0.11
TRACE Resolution edge: ROOT -> setuptools-scm
TRACE Resolution edge:     0a0.dev0 -> 8.1.0 (extra: toml)
TRACE Resolution edge: ROOT -> wheel
TRACE Resolution edge:     0a0.dev0 -> 0.45.0
TRACE Resolution edge: setuptools-scm -> setuptools
TRACE Resolution edge:     8.1.0 -> 75.6.0
TRACE Resolution edge: ROOT -> tomli
TRACE Resolution edge:     0a0.dev0 -> 2.1.0
TRACE Resolution edge: ROOT -> setuptools-scm
TRACE Resolution edge:     0a0.dev0 -> 8.1.0
TRACE Resolution edge: setuptools-scm -> packaging
   Built pymssql @ file:///Users/dwt/Code/Projekte/nix/devenv-reproduction/pymssql
Prepared 1 package in 4.44s
Uninstalled 1 package in 1ms
Installed 1 package in 1ms
 - pymssql==2.3.2
 + pymssql==2.3.2.dev3 (from file:///Users/dwt/Code/Projekte/nix/devenv-reproduction/pymssql)
++ type python
python is /Users/dwt/Code/Projekte/nix/devenv-reproduction/venv/bin/python
++ python --version
Python 3.12.7
++ python -c 'import ibm_db'
++ python -c 'import pymssql'
```

</details>

---

_Comment by @dwt on 2024-11-23 16:11_

I'm not quite sure what happened, but I cannot reproduce the bug anymore with uv --version 0.4.25.

I'm quite sure this is the same uv version that showed the bug initially. Especially since this is all happening in a flake locked nix environment this doubly perplexes me.

Any Idea what could have triggered this?

I'll reopen when I am able to reproduce again.

---

_Closed by @dwt on 2024-11-23 16:11_

---

_Comment by @dwt on 2024-11-23 16:28_

The same for every version of uv released since. I also noticed an update to [pymmsql](https://pypi.org/project/pymssql/#history) two days ago with diskutil related fixes. So maybe it was a bug in that package all along.

---

_Comment by @dwt on 2024-11-23 18:55_

Seems I have been a bit too hasty. It still downloads and builds a version, instead of using the prebuilt version. But if it builds a version itself that version has the right architecture.

---

_Reopened by @dwt on 2024-11-23 18:55_

---

_Comment by @dwt on 2024-11-23 18:55_

So at least the bug is no longer blocking me.

---

_Comment by @dwt on 2024-11-25 07:26_

Now that I think about this some more, that the old version builds with the wrong compiler when installed from GitHub, but the right ones when installed from checkout is also definitely a bug, and one that is probably easier to track down with the knowledge what change caused it not choose the wrong code path there.

Do you guys want me to split this into two separate bugs, or do you want to track this in this one issue?

---

_Comment by @konstin on 2024-11-26 21:10_

uv shouldn't be involved in the compiler selection at all, we're just implementing PEP 517. You can check this by testing against e.g. `python -m build`, the reference implementation. If you find that this is still related to uv, feel free to open a separate issue.

---

_Comment by @dwt on 2024-11-27 08:12_

Seeing that this worked when self compiling before, my current theory is that the developers sat on that fix for some time before releasing it, and thus the pypi version was broken, while the tip of the tree was fine. Probably.

That would leave the question why uv compiles something in the first place, while a perfectly fine version was available on pypi. I did post the log results above, but am not sure how to interpret them.

Do they make sense to you as to why uv tries to compile something instead of using what is on pypi?

---

_Label `question` added by @konstin on 2024-11-27 09:28_

---

_Comment by @konstin on 2024-11-27 09:28_

If you provide a non-pypi source such as a git repo or a path, uv will use that and build. If you provide only pypi, uv will try to find a matching wheel on pypi and otherwise use the source distribution to build (if any)

---

_Comment by @dwt on 2024-11-27 10:41_

@konstin that is what I would expect. If you look into the history of this bug, you will notice that uv however did download the source distribution of `pymssql` from pypi and builds it instead of using the also provided binary which should be usable.

---
