---
number: 7689
title: "Periodic compatibility issues with `uv` crates and `reqwest` crate"
type: issue
state: closed
author: fyusifov
labels: []
assignees: []
created_at: 2024-09-25T17:49:07Z
updated_at: 2024-09-25T18:54:14Z
url: https://github.com/astral-sh/uv/issues/7689
synced_at: 2026-01-10T01:24:18Z
---

# Periodic compatibility issues with `uv` crates and `reqwest` crate

---

_Issue opened by @fyusifov on 2024-09-25 17:49_

I am using `uv` crates to resolve dependencies from `pyproject.toml` programatically and I am periodically having issues with compatibility between `reqwest` and `uv` creates.  Last time when I had similar issue, it was recommended to pin to some commit and it worked. Now I am having different compatibility issue: 

These are my dependencies:

```toml
[package]
name = "uv-dep-testing"
version = "0.1.0"
edition = "2021"

[dependencies]
#reqwest = { version = "0.12", features = ["json", "rustls-tls"] }
tokio = { version = "1.38", features = ["full"] }

requirements-txt = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-client = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
pep508_rs = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
pypi-types = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-python = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-cache = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-installer = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
distribution-types = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-requirements = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-configuration = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-types = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-resolver = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-distribution = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-build = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-dispatch = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
platform-tags = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
uv-git = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }
install-wheel-rs = { git = "https://github.com/astral-sh/uv.git",  rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }

[patch.crates-io]
reqwest-middleware = { git = "https://github.com/astral-sh/reqwest-middleware", rev = "21ceec9a5fd2e8d6f71c3ea2999078fecbd13cbe"}
reqwest-retry = { git = "https://github.com/astral-sh/reqwest-middleware", rev = "21ceec9a5fd2e8d6f71c3ea2999078fecbd13cbe"}
```

And this is some test code that I am using to resolve dependencies and exploring in general parsing/resolving dependencies with `uv` programatically:

```rust
use std::path::PathBuf;

use tokio;

use uv_cache::Cache;
use uv_client::{BaseClientBuilder, RegistryClientBuilder};
use uv_python::{EnvironmentPreference, PythonEnvironment, PythonInstallation, PythonPreference, PythonRequest};
use uv_requirements::{RequirementsSource, RequirementsSpecification, SourceTreeResolver};
use uv_configuration::{BuildOptions, ExtrasSpecification, PreviewMode, NoBinary, NoBuild, IndexStrategy, SetupPyStrategy, ConfigSettings, SourceStrategy, Concurrency};
use uv_types::{HashStrategy, BuildContext, InFlight, BuildIsolation, EmptyInstalledPackages};
use uv_resolver::{InMemoryIndex, FlatIndex, Resolver, Manifest, OptionsBuilder, PythonRequirement, ResolverMarkers};
use uv_distribution::DistributionDatabase;
use uv_dispatch::BuildDispatch;
use platform_tags::Tags;
use uv_git::GitResolver;
use install_wheel_rs::linker::LinkMode;
use pep508_rs::MarkerTree;
use distribution_types::IndexLocations;


#[tokio::main]
async fn main() {
    let filename = PathBuf::from("./pyproject.toml");
    let reqs = RequirementsSource::from_requirements_file(filename);

    let base_client = BaseClientBuilder::new().native_tls(true);
    let requirement_specifications = RequirementsSpecification::from_source(&reqs, &base_client).await.unwrap();
    let in_memory_index = InMemoryIndex::default();
    let cache = Cache::temp().unwrap();
    let registry_client = RegistryClientBuilder::new(cache.clone()).build();

    let python_installation = PythonInstallation::find(
        &PythonRequest::Any,
        EnvironmentPreference::Any,
        PythonPreference::System,
        &cache,
    ).unwrap();

    let platform = python_installation.interpreter().platform();
    let python_version = python_installation.interpreter().implementation_tuple();
    let implementation_name = python_installation.interpreter().implementation_name();
    let python_environment = PythonEnvironment::from_installation(python_installation.clone());
    let interpreter = python_environment.interpreter();
    let index_locations = IndexLocations::default();
    let hasher = HashStrategy::None;
    let build_options = BuildOptions::new(NoBinary::None, NoBuild::None);
    let tags = Tags::from_env(platform, python_version, implementation_name, python_version, true).unwrap();

    let flat_index = {
        let client = uv_client::FlatIndexClient::new(&registry_client, &cache);
        let entries = client.fetch(index_locations.flat_index()).await.unwrap();
        FlatIndex::from_entries(entries, Some(&tags), &hasher, &build_options)
    };

    let git_resolver = GitResolver::default();
    let in_flight = InFlight::default();
    let index_strategy = IndexStrategy::default();
    let setup_py_strategy = SetupPyStrategy::default();
    let config_settings = ConfigSettings::default();
    let build_isolation = BuildIsolation::default();
    let link_mode = LinkMode::default();
    let source_strategy = SourceStrategy::default();
    let concurrency = Concurrency::default();
    let preview_mode = PreviewMode::default();

    let dispatch = BuildDispatch::new(
        &registry_client,
        &cache,
        &requirement_specifications.constraints,
        interpreter,
        &index_locations,
        &flat_index,
        &in_memory_index,
        &git_resolver,
        &in_flight,
        index_strategy,
        setup_py_strategy,
        &config_settings,
        build_isolation,
        link_mode,
        &build_options,
        None,
        source_strategy,
        concurrency,
        preview_mode,
    );

    let db = DistributionDatabase::new(&registry_client, &dispatch, 5, PreviewMode::Enabled);
    let src_tree_resolver = SourceTreeResolver::new(requirement_specifications.source_trees, &ExtrasSpecification::None, &HashStrategy::None, &in_memory_index, db);

    let resolve_result = src_tree_resolver.resolve().await.unwrap();

    let manifest = Manifest::simple(resolve_result[0].clone().requirements);
    let options = OptionsBuilder::new().build();
    let python_requirement = PythonRequirement::from_interpreter(interpreter);
    let markers = ResolverMarkers::Fork(MarkerTree::default());
    // let markers = ResolverMarkers::universal(vec![]);
    let installed_packages = EmptyInstalledPackages;
    let db_2 = DistributionDatabase::new(&registry_client, &dispatch, 5, PreviewMode::Enabled);

    let resolver = Resolver::new(
        manifest,
        options,
        &python_requirement,
        markers,
        None,
        &flat_index,
        &in_memory_index,
        &hasher,
        &dispatch,
        installed_packages,
        db_2,
    );

    let resolver_result = resolver.unwrap().resolve().await.unwrap();

    println!("{:?}", resolver_result)
}
```

If I run this code all works as expected, but if I uncomment `reqwest = { version = "0.12", features = ["json", "rustls-tls"] }` line in `Cargo.toml` above then I receive this error message:

```shell
➜  uv-dep-testing git:(master) ✗ cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.82s
     Running `target/debug/uv-dep-testing`
thread 'main' panicked at src/main.rs:115:61:
called `Result::unwrap()` on an `Err` value: Client(Error { kind: WrappedReqwestError(WrappedReqwestError(Middleware(Request failed after 3 retries

Caused by:
    0: error sending request for url (https://pypi.org/simple/jinja2/)
    1: client error (Connect)
    2: The certificate was not trusted.))) })
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Could you please have a look into this compatibility issue and help me to find a stable way to to build these dependencies

---

_Comment by @BurntSushi on 2024-09-25 17:59_

Have you tries using the same dependency specification on `reqwest` as uv itself uses?

https://github.com/astral-sh/uv/blob/f5601e261015cf18d8ea69688e4c99a0e634963a/Cargo.toml#L131

It looks like there's something with TLS going on here based on your error, and your TLS features are different than what uv itself enables.

---

_Comment by @fyusifov on 2024-09-25 18:26_

Thanks @BurntSushi for prompt response. I have changed my `reqwest` dependency to be one to one like in `uv`:

```toml
reqwest = { version = "0.12.7", default-features = false, features = ["json", "gzip", "stream", "rustls-tls", "rustls-tls-native-roots", "socks", "multipart"] }
```

and indeed it fixed this issue. 

Could you please also let me know is there any way also remove dependency on some specific commits? This is what was recommended to do on this issue: #6185 

And last question/favour will be is there any documentation on how internal parsing and dependency resolution of `uv` work. I am mostly getting needed info from source code analysis.

---

_Renamed from "Periodic compatibility issues with `uv` creates and `reqwest` crate" to "Periodic compatibility issues with `uv` crates and `reqwest` crate" by @fyusifov on 2024-09-25 18:39_

---

_Comment by @BurntSushi on 2024-09-25 18:53_

Using uv as a Rust library isn't really a well supported thing, and I don't think we have plans to make it well supported. The recommended way to use uv is by subprocess. So to that end, pinning to a specific commit seems pretty reasonable.

As far as docs at the source code level go, what you see is what you get. You can run something like `cargo doc --all-features --document-private-items --workspace`, but I'd say our source code docs can be hit or miss.

---

_Closed by @BurntSushi on 2024-09-25 18:54_

---
