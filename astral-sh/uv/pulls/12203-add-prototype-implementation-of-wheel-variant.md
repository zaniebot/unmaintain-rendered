---
number: 12203
title: Add prototype implementation of wheel variant specification
type: pull_request
state: open
author: charliermarsh
labels:
  - no-build
  - no-test
assignees: []
draft: true
base: main
head: charlie/wheel-variant
created_at: 2025-03-16T16:35:56Z
updated_at: 2025-12-08T17:01:08Z
url: https://github.com/astral-sh/uv/pull/12203
synced_at: 2026-01-10T01:26:17Z
---

# Add prototype implementation of wheel variant specification

---

_Pull request opened by @charliermarsh on 2025-03-16 16:35_

(Branch taken over by @konstin)

This branch contains the prototype implementation of wheel variants proposal. It support `uv pip install` and `uv sync` for basic test cases and torch. Included are:
 * Installing and running providers
 * Variant ordering
 * Basic lockfile support
 * Static providers, with a custom format
 * Variant markers markers
 * Basic universal resolution
 
It does not implement `abi_dependency` yet, nor does it consider whether features are multi-value or single value.

Naturally, it is not ready for any serious usage yet. Expect that things break, please report problems.

**release** Installing the latest release, potentially a bit behind this branch:

Unix:

```
curl -LsSf https://astral.sh/uv/install.sh | INSTALLER_DOWNLOAD_URL=https://wheelnext.astral.sh/v0.0.2 sh
```

Windows:

```
powershell -c { $env:INSTALLER_DOWNLOAD_URL = 'https://wheelnext.astral.sh/v0.0.3'; irm https://astral.sh/uv/install.ps1 | iex }
```

**latest** The best way to test is to clone the repo, checkout the branch (charlie/wheel-variant) and run:

```
export RUST_LOG=uv_distribution_types=debug,uv_distribution::distribution_database=debug
cargo run pip install torch --index https://wheelnext.github.io/variants-index/v0.0.3/ --refresh
```

If you don’t have Rust installed, there's a bootstrapping process from current uv, though it's slow:

```
export RUST_LOG=uv_distribution_types=debug,uv_distribution::distribution_database=debug
uvx --from "uv @ git+https://github.com/astral-sh/uv@charlie/wheel-variant" uv pip install torch --index https://wheelnext.github.io/variants-index/v0.0.3/ --refresh
```

To use a static variant declaration, i.e. not query any providers, you pass a JSON file by settings its path in `UV_VARIANT_LOCK`. It assumes that you don't want to run any additional providers with it, i.e. no third party code execution, unless you set `UV_VARIANT_LOCK_INCOMPLETE=1`. The file format has to follow the schema below:

```toml
[metadata]
created-by = "tool-that-created-the-file" # Freeform string
version = "0.1" # PEP 440 version

[[provider]]
resolved = ["cpu_level_provider==0.1"]
plugin-api = "cpu_level_provider"
namespace = "cpu_level"

[provider.properties]
x86_64_level = ["v1", "v2"]

[[provider]]
resolved = ["nvidia-variant-provider==0.0.1"]
plugin-api = "nvidia_variant_provider.plugin:NvidiaVariantPlugin"
namespace = "nvidia"

[provider.properties]
cuda_version_lower_bound = ["12.8"]
sm_arch = ["100_real", "120_real", "70_real", "75_real", "80_real", "86_real", "90_real"]
```

---

_Label `no-build` added by @charliermarsh on 2025-03-16 16:35_

---

_Label `no-test` added by @charliermarsh on 2025-03-16 16:35_

---

_Comment by @warsawnv on 2025-03-17 22:30_

The `build-wheels.sh` step gives:

```
Successfully built wheels/provider_fictional_hw-1.0.0-py3-none-any.whl
+ uv build --out-dir ./wheels --project ./vendor/variantlib
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 101, column 11
      |
  101 | [[tool.uv.variant]]
      |           ^^^^^^^
  unknown field `variant`, expected one of `required-version`, `native-tls`, `offline`, `no-cache`, `cache-dir`, `preview`, `python-preference`, `python-downloads`, `concurrent-downloads`, `concurrent-builds`, `concurrent-installs`, `index`, `index-url`, `extra-index-url`, `no-index`, `find-links`, `index-strategy`, `keyring-provider`, `allow-insecure-host`, `resolution`, `prerelease`, `fork-strategy`, `dependency-metadata`, `config-settings`, `no-build-isolation`, `no-build-isolation-package`, `exclude-newer`, `link-mode`, `compile-bytecode`, `no-sources`, `upgrade`, `upgrade-package`, `reinstall`, `reinstall-package`, `no-build`, `no-build-package`, `no-binary`, `no-binary-package`, `python-install-mirror`, `pypy-install-mirror`, `publish-url`, `trusted-publishing`, `check-url`, `pip`, `cache-keys`, `override-dependencies`, `constraint-dependencies`, `build-constraint-dependencies`, `environments`, `required-environments`, `conflicts`, `workspace`, `sources`, `managed`, `package`, `default-groups`, `dev-dependencies`, `build-backend`

```

But then it seems to succeed.

---

_Comment by @charliermarsh on 2025-03-17 22:36_

Ah sorry. I’ll update the instructions. You can either remove that section from the pyproject.toml, then run the build script, then re-add it. Or run cargo build, then point the build script to ./target/debug/uv instead of your global uv.

---

_Comment by @warsawnv on 2025-03-17 22:36_

And the `cargo run` step fails with:

```
     Running `target/debug/uv pip install dummy_project --find-links ./wheels --extra-index-url 'https://mockhouse.wheelnext.dev/pep-xxx-variants/' --no-cache --reinstall`
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

---

_Comment by @warsawnv on 2025-03-17 22:36_

> Or run cargo build, then point the build script to ./target/debug/uv instead of your global uv

I had a feeling about that!

---

_Comment by @charliermarsh on 2025-03-17 22:38_

You can run uv venv prior to the cargo run command to create a virtual environment in the current working directory. cargo run will then install into that virtual environment by default.

---

_Comment by @warsawnv on 2025-03-17 22:43_

```
% git diff 
diff --git a/build-wheels.sh b/build-wheels.sh
index 96d89524d..193eed495 100755
--- a/build-wheels.sh
+++ b/build-wheels.sh
@@ -13,12 +13,14 @@
 
 set -euxo pipefail
 
+UV=./target/debug/uv
+
 # Create the destination directory if it doesn't exist.
 rm -rf wheels
 mkdir wheels
 
 # Build the wheels for the fictional hardware provider package.
-uv build --out-dir ./wheels --project ./vendor/provider_fictional_hw
+$UV build --out-dir ./wheels --project ./vendor/provider_fictional_hw
 
 # Build the wheels for the variant library.
-uv build --out-dir ./wheels --project ./vendor/variantlib
+$UV build --out-dir ./wheels --project ./vendor/variantlib
```

---

_Comment by @seemethere on 2025-03-17 23:06_

```toml
[[tool.uv.variant]]
backend = "provider_fictional_hw.plugin:build"
requires = ["provider_fictional_hw"]
```

For this configuration I'm curious if this would be end-user facing or would this be package maintainer facing?

From my POV I feel like this would be best served at the package maintainer side where a potential PyTorch packaging config might look like:

```toml
[[tool.uv.variant]]
backend = "torch.build.variants:detect_variants"
requires = ["cuda_variants", "rocm_variants", "xpu_variants", "your_fav_hardware_variants"]
```

fwiw I don't know if requiring people to install variant plugins themselves is necessarily going to be a way to adopt this heavily since this should be something that reduces the friction towards someone installing the correct package for their machine instead of them having to remember which variant plugins they're installing vs. not.

---

_Comment by @charliermarsh on 2025-03-17 23:11_

I was thinking it would be declared by the package maintainer, but possibly overridable by the end user (so, like, if they don't care about `your_fav_hardware_variants`, they can "skip" that variant provider).

---

_Comment by @charliermarsh on 2025-03-17 23:15_

(Applied your changes, thanks @warsawnv.)

---

_Referenced in [wheelnext/pep_xxx_wheel_variants#11](../../wheelnext/pep_xxx_wheel_variants/issues/11.md) on 2025-03-19 12:44_

---

_@charliermarsh reviewed on 2025-07-23 20:18_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/lib.rs`:105 on 2025-07-23 20:18_

Note: I'd want to resolve this prior to merging.

---

_@charliermarsh reviewed on 2025-07-23 20:19_

---

_Review comment by @charliermarsh on `crates/uv-resolver/Cargo.toml`:46 on 2025-07-23 20:19_

Good to resolve prior to merging.

---

_@charliermarsh reviewed on 2025-07-23 20:19_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:681 on 2025-07-23 20:19_

What's this mean? I see it elsewhere too.

---

_@konstin reviewed on 2025-07-23 20:40_

---

_Review comment by @konstin on `crates/uv-resolver/src/candidate_selector.rs`:681 on 2025-07-23 20:40_

It's a notion that this kind of mutability method shouldn't exist on this type. Here it means that we should ideally refactor it in a way that doesn't edit the `Candidate`; You can think of it as marker that this should be refactored prior to merging.

---

_Referenced in [wheelnext/variant-repack#4](../../wheelnext/variant-repack/pulls/4.md) on 2025-09-23 22:41_

---
