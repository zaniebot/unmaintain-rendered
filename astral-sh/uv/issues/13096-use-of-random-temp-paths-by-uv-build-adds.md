---
number: 13096
title: "Use of random temp paths by `uv build` adds nondeterminism into build environments"
type: issue
state: open
author: tabbyrobin
labels:
  - bug
  - external
assignees: []
created_at: 2025-04-25T03:01:27Z
updated_at: 2025-04-28T21:29:06Z
url: https://github.com/astral-sh/uv/issues/13096
synced_at: 2026-01-10T01:25:28Z
---

# Use of random temp paths by `uv build` adds nondeterminism into build environments

---

_Issue opened by @tabbyrobin on 2025-04-25 03:01_

### Summary

When `uv build` runs, it produces a temp dir for its cache. This temp dir is
named randomly, and so is different every time. As such, it introduces a source
of non-determinism, which can impact reproducible builds.

This can cause builds which are otherwise reproducible, to fail to build
reproducibly.

This is particularly relevant with compiled builds, where the `.tmp` paths may
end up in the final binary (e.g. in debug info...). This causes sequential runs
to produce different binaries every time, simply because the temp paths are
different; it may also lead to other things, including different ordering of ELF
symbol table entries, and different `NT_GNU_BUILD_ID`.

I have confirmed that this behavior affects at least some Rust/Maturin builds.

These temp paths have made their way into real-world build artifacts, for example,
[cryptography-44.0.2-cp39-abi3-manylinux_2_34_x86_64.whl](https://files.pythonhosted.org/packages/89/33/c1cf182c152e1d262cac56850939530c05ca6c8d149aa0dcee490b417e99/cryptography-44.0.2-cp39-abi3-manylinux_2_34_x86_64.whl)
contains paths such as
`/__w/cryptography/cryptography/tmpwheelhouse/.tmpmbdRiZ/cryptography-44.0.2/src/rust/cryptography-x509/src/common.rs`:

`> strings --all --bytes=8 cryptography/hazmat/bindings/_rust.abi3.so | grep '\.tmp' | head -n 1`

`encrypt_and_serializebuilderoptions_data_recipientspublic_keyencryptdecrypt_smimecertificateprivate_keyThe provided PEM data does not have the PKCS7 tag.Failed to parse PEM datadecrypt_pemNo recipient found that matches the given certificate.The EnvelopedData structure does not contain encrypted content.Only AES-128-CBC is currently supported for content decryption.Only RSA with PKCS #1 v1.5 padding is currently supported for key decryption.unwrap_read called on a Write value/__w/cryptography/cryptography/tmpwheelhouse/.tmpmbdRiZ/cryptography-44.0.2/src/rust/cryptography-x509/src/common.rsThe PKCS7 data is not an EnvelopedData structure.decrypt_derdecryptblock_size`

Here is a minimal example to reproduce `uv build` non-reproducible behavior.

The middle command, with `uv build --wheel ...`, is reproducible in my minimal
testing script, but does *not* appear to behave reproducibly when run as part of
the `cryptography` Github Actions yaml. I am not sure why.

```shell
# SETUP (note: you will also need various build deps, omitted here for brevity):
git clone https://github.com/pyca/cryptography "$PROJ_DIR"
cd "$PROJ_DIR" && git checkout refs/tags/44.0.2

# Reproducible.
    uv venv --relocatable
    uv pip install maturin[patchelf]
    uv pip install --require-hashes -r .github/requirements/build-requirements.txt
    uv run maturin build --interpreter "python3.11" -o tmpwheelhouse

# Reproducible sometimes?
    uv build --wheel \
      --require-hashes --build-constraints=.github/requirements/build-requirements.txt \
      -o tmpwheelhouse \
      --python "python3.11"

# Not Reproducible!
    uv build \
      --require-hashes --build-constraints=.github/requirements/build-requirements.txt \
      -o tmpwheelhouse \
      --python "python3.11"
```

I have created a script which tries to pin as much dependencies as possible, so
as to minimize variables. It also pulls in various necessary dependencies, for
example openssl-dev headers etc.:
https://gist.github.com/tabbyrobin/d54978d9d69bc08699683e50dc86adf9#file-uv_build_repro_issue-sh

That script produces results such as this:

```shell
## Running from git repo ##
### Running: uvBuildDefault ... 1/2 ###
4870e3ba77f4b5aff99a235c0bc09506b50a4f94a0dcea2e5c7633195beadb4b  ./cryptography/hazmat/bindings/_rust.abi3.so
### Running: uvBuildDefault ... 2/2 ###
9b16d4b2c29632aa01a49323f6a892908553a3f7cbc3d7f4ef4f30cba0ec39b9  ./cryptography/hazmat/bindings/_rust.abi3.so
### Running: uvBuildWheel ... 1/2 ###
50002d49b45124c67f54f04f45cb2e22e585fa4de3a379c9cedd802cf197238b  ./cryptography/hazmat/bindings/_rust.abi3.so
### Running: uvBuildWheel ... 2/2 ###
50002d49b45124c67f54f04f45cb2e22e585fa4de3a379c9cedd802cf197238b  ./cryptography/hazmat/bindings/_rust.abi3.so
### Running: uvVenvMaturinBuild ... 1/2 ###
b3ed77ab8808245060c34ce521012998bb2b9eab86843b51b7ffdf24d6b135fb  ./cryptography/hazmat/bindings/_rust.abi3.so
### Running: uvVenvMaturinBuild ... 2/2 ###
b3ed77ab8808245060c34ce521012998bb2b9eab86843b51b7ffdf24d6b135fb  ./cryptography/hazmat/bindings/_rust.abi3.so
```

Other notes...

I think this function is what's responsible for creating the temp dir:
https://github.com/astral-sh/uv/blob/7a18e4429d6750365c2f2ad98907a90d97e67fb7/crates/uv-cache/src/lib.rs#L214
That function is called here:
https://github.com/astral-sh/uv/blob/7a18e4429d6750365c2f2ad98907a90d97e67fb7/crates/uv-build-frontend/src/lib.rs#L282

Note: I am not affiliated with PyCA `cryptography` project in any way; I'm just
using that project as an example.


### Platform

Debian GNU/Linux 12 (bookworm) on Qubes

### Version

0.6.14

### Python version

Python 3.11

---

_Label `bug` added by @tabbyrobin on 2025-04-25 03:01_

---

_Comment by @konstin on 2025-04-25 07:54_

This is an upstream Rust bug, where rustc does not trim paths and leaks them into the final binary: https://github.com/rust-lang/rust/issues/111540

---

_Label `external` added by @konstin on 2025-04-25 07:54_

---

_Referenced in [rust-lang/rust#111540](../../rust-lang/rust/issues/111540.md) on 2025-04-25 17:55_

---

_Comment by @tabbyrobin on 2025-04-25 18:24_

Hi, thanks for your response. I agree that rustc behavior is a bug and should be fixed. On the other hand, the issue I filed here has different scope:

- It is not specific to Rust. Rust/Maturin is used as an example, but this can affect arbitrary build systems for arbitrary languages.
- The use of *strongly random* paths creates a thornier problem than many other non-deterministic issues. Because the paths cannot be predicted in advance (even by brute force enumeration), it means that usual strategies such as `--remap-path-prefix` do not work. (I am not actually sure whether Rust's proposed `--remap-path-scope` flag from the issue you linked will address this?)

This issue is about uv itself: ideally, `uv build` should avoid introducing additional sources of non-determinism into build environments, so as to be a "good steward" of the (imperfect) build tools that may be subsequently invoked.

There are at least a few possibilities for fixes:

- Provide an option that triggers uv to use a fixed path instead of a temp directory.
- Someone I talked to suggested: "the .tmp* bits could be changed to use a .tmp-HASH where the hash is the hash of the source code or something (unless multiple things will touch the same source code in parallel)."

---

_Comment by @Urgau on 2025-04-25 19:42_

Couldn't `uv` pass `--remap-path-prefix` with the temp directory to `rustc`/`cargo`? maybe inside `RUSTFLAGS`?

---

_Comment by @tabbyrobin on 2025-04-25 20:37_

Some related issues: #3684, #12036

@Urgau Thanks for the confirmation you provided (in the rust-lang issue) that `--remap-path-scope` would not address this issue.

---

_Referenced in [pyca/cryptography#12811](../../pyca/cryptography/issues/12811.md) on 2025-04-27 00:35_

---

_Comment by @tabbyrobin on 2025-04-27 21:54_

A couple notes for future reference, I believe that these options also do not address this issue:

`--no-build-isolation` / `UV_NO_BUILD_ISOLATION` -- This only affects sdists, not wheels? ("Disable isolation when building source distributions.")

`--cache-dir cache-dir` /  `UV_CACHE_DIR` -- I tried this, but it sets the root dir for the cache; the temp dir is created inside it.

---

_Comment by @konstin on 2025-04-28 08:35_

[build](https://github.com/pypa/build), the reference implementation of building Python packages through PEP 517, has the same problem (I've checked that `python -m build .` wheels for Rust-based packages changes hashes on each build).

After setting `SOURCE_DATE_EPOCH`, both `python -m build --no-isolation --wheel` and `uv build --no-build-isolation --wheel` build consistent wheels for cryptography. Passing `--wheel` means that we use the existing source tree directly without going through a source dist that needs to be unpacked somewhere. With these options on my machine I can build reproducible wheels for cryptography, given that you control the source path between machines.

> `--no-build-isolation` / `UV_NO_BUILD_ISOLATION` -- This only affects sdists, not wheels? ("Disable isolation when building source distributions.")

It refers to building from source, but it includes building to wheel. More accurately, it means that uv uses the current environment instead of an isolated environment for all calls to PEP 517 hooks.

---

_Comment by @tabbyrobin on 2025-04-28 21:05_

@konstin Thanks for looking into this!

Would `--no-build-isolation` be considered the recommended workaround?

> Passing `--wheel` means that we use the existing source tree directly without going through a source dist that needs to be unpacked somewhere. 

I may have realized the reason for the behavior discrepancy of `uv build --wheel ...` in the minimal testing script vs the GHA yaml. The PyCA cryptography build logic [invokes](https://github.com/pyca/cryptography/blob/44.0.2/.github/workflows/wheel-builder.yml#L146) `uv build ... --wheel ... cryptography*.tar.gz ...`. That is, it passes an sdist `.tar.gz` archive as argument -- not a directory. 


---

_Comment by @konstin on 2025-04-28 21:07_

> Would `--no-build-isolation` be considered the recommended workaround?

yes; Usually, deactivating build isolation is a workaround for legacy packages that don't declare their build dependencies properly, but for your case, it's also a workaround for rustc et al being ~~non-deterministic~~ source path dependent.

---

_Comment by @Urgau on 2025-04-28 21:18_

> it's also a workaround for rustc et al being non-deterministic.

`rustc` is deterministic (modulo bugs) and embedding paths in debuginfo is not a bug, it's a feature.

If you don't like the embedded paths, either don't build in such build directory (which apparently can be done with `--no-build-isolation`) or (as I said earlier) provide `rustc` with the proper remapping with `--remap-path-prefix`.

---
