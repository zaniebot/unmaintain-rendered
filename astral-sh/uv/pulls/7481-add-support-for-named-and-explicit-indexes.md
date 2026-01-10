```yaml
number: 7481
title: Add support for named and explicit indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/index-api
created_at: 2024-09-18T01:57:10Z
updated_at: 2024-11-15T03:52:57Z
url: https://github.com/astral-sh/uv/pull/7481
synced_at: 2026-01-10T11:59:59Z
```

# Add support for named and explicit indexes

---

_Pull request opened by @charliermarsh on 2024-09-18 01:57_

## Summary

This PR adds a first-class API for defining registry indexes, beyond our existing `--index-url` and `--extra-index-url` setup.

Specifically, you now define indexes like so in a `uv.toml` or `pyproject.toml` file:

```toml
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
```

You can also provide indexes via `--index` and `UV_INDEX`, and override the default index with `--default-index` and `UV_DEFAULT_INDEX`.

### Index priority

Indexes are prioritized in the order in which they're defined, such that the first-defined index has highest priority.

Indexes are also inherited from parent configuration (e.g., the user-level `uv.toml`), but are placed after any indexes in the current project, matching our semantics for other array-based configuration values.

You can mix `--index` and `--default-index` with the legacy `--index-url` and `--extra-index-url` settings; the latter two are merely treated as unnamed `[[tool.uv.index]]` entries.

### Index pinning

If an index includes a name (which is optional), it can then be referenced via `tool.uv.sources`:

```toml
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"

[tool.uv.sources]
torch = { index = "pytorch" }
```

If an index is marked as `explicit = true`, it can _only_ be used via such references, and will never be searched implicitly:

```toml
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch" }
```

Indexes defined outside of the current project (e.g., in the user-level `uv.toml`) can _not_ be explicitly selected.

(As of now, we only support using a single index for a given `tool.uv.sources` definition.)

### Default index

By default, we include PyPI as the default index. This remains true even if the user defines a `[[tool.uv.index]]` -- PyPI is still used as a fallback. You can mark an index as `default = true` to (1) disable the use of PyPI, and (2) bump it to the bottom of the prioritized list, such that it's used only if a package does not exist on a prior index:

```toml
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
default = true
```

### Name reuse

If a name is reused, the higher-priority index with that name is used, while the lower-priority indexes are ignored entirely. 

For example, given:

```toml
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"

[[tool.uv.index]]
name = "pytorch"
url = "https://test.pypi.org/simple"
```

The `https://test.pypi.org/simple` index would be ignored entirely, since it's lower-priority than `https://download.pytorch.org/whl/cu121` but shares the same name.

Closes #171.

## Future work

- Users should be able to provide authentication for named indexes via environment variables.
- `uv add` should automatically write `--index` entries to the `pyproject.toml` file.
- Users should be able to provide multiple indexes for a given package, stratified by platform:
```toml
[tool.uv.sources]
torch = [
  { index = "cpu", markers = "sys_platform == 'darwin'" },
  { index = "gpu", markers = "sys_platform != 'darwin'" },
]
```
- Users should be able to specify a proxy URL for a given index, to avoid writing user-specific URLs to a lockfile:
```toml
[[tool.uv.index]]
name = "test"
url = "https://private.org/simple"
proxy = "http://<omitted>/pypi/simple"
```



---

_Label `enhancement` added by @charliermarsh on 2024-09-18 01:57_

---

_Comment by @charliermarsh on 2024-09-18 02:08_

This still needs docs (beyond the inline documentation), but I'd like to align on the semantics described in the PR summary first.

---

_@charliermarsh reviewed on 2024-09-18 02:08_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index_source.rs`:60 on 2024-09-18 02:08_

I left this in for now but can remove before merging. Eventually I want this to support `--find-links`.

---

_Review requested from @zanieb by @charliermarsh on 2024-09-18 02:24_

---

_Comment by @zanieb on 2024-09-18 03:53_

> Any indexes defined via [[tool.uv.index]] take priority over any indexes defined via --index-url or --extra-index-url.
> 
> (This is problematic, since as of now, there's no way to provide a [[tool.uv.index]] on the command line, so you can never override in-file configuration with command-line configuration.)

Yeah this seems wrong. Why is it this way?

Do we need an explicit `explicit` tag or can we just use the presence of a name? I guess the name is useful for providing credentials externally so... I guess the explicit tag is important. 

Do we set the explicit tag during a `uv add --index-url <url> <pkg>` operation?

How does index pinning work for transitive dependencies?

How can we teach the PyPI fallback behavior? Are there any bad user experiences where we could suggest adding `default = true` to their index definition?

---

_Comment by @charliermarsh on 2024-09-18 04:00_

> Yeah this seems wrong. Why is it this way?

Well... we could just invert the priority, if that's what you mean (such that the new index stuff comes last, and the legacy arguments come first). Then the CLI arguments would work as expected. As-is, though, we don't have a way to differentiate between indexes passed on the command line (with `--index-url` or `--extra-index-url`) and indexes defined in a file via `index-url` and `extra-index-url`... So, like, we can't have `index-url` from the `uv.toml` be lower priority than `tool.uv.index`, but `--index-url` be higher priority. Alternatively, we could add a new command-line argument (`--index`?) for these `tool.uv.index` sources, which would also solve the problem?

> Do we need an explicit explicit tag or can we just use the presence of a name? I guess the name is useful for providing credentials externally so... I guess the explicit tag is important.

Yeah roughly this.

> Do we set the explicit tag during a uv add --index-url <url> <pkg> operation?

We don't as of this PR... We can though. I think we probably should? I guess by default we add the index, and make it explicit for that package? The unfortunate thing is that we need a name for the index in that case (as we discussed on Discord). Alternatively, we could just add a `tool.uv.index`, and not make it explicit.

> How does index pinning work for transitive dependencies?

It doesn't have any effect on transitive dependencies. I don't know that it can or should, honestly, because transitive dependencies can be required from multiple different first-party dependencies that could come from different indexes. I think this would be extremely hard to implement correctly and could lead to confusing behavior.


---

_Comment by @inflation on 2024-09-18 04:27_

I prefer to make `explicit=true` the default behavior, since the first foot gun in the current situation after adding `extra-index-url`, are packages in the `pytorch` channel taking precedence over `pypi`. 

---

_Comment by @smphhh on 2024-09-18 08:28_


> > How does index pinning work for transitive dependencies?
> 
> It doesn't have any effect on transitive dependencies. I don't know that it can or should, honestly, because transitive dependencies can be required from multiple different first-party dependencies that could come from different indexes. I think this would be extremely hard to implement correctly and could lead to confusing behavior.

Did you consider allowing pinning packages to indexes using glob-patterns as mentioned [here](https://github.com/astral-sh/uv/issues/171#issuecomment-2302093785)? AFAIK that is the only solution for pinning transitive dependencies that is straightforward enough but still useful for a set of real use-cases.


---

_Comment by @charliermarsh on 2024-09-18 14:13_

> Did you consider allowing pinning packages to indexes using glob-patterns as mentioned https://github.com/astral-sh/uv/issues/171#issuecomment-2302093785? AFAIK that is the only solution for pinning transitive dependencies that is straightforward enough but still useful for a set of real use-cases.

There's nothing stopping us from supporting that in addition to the schema described above. It strikes me as somewhat backwards but I understand why it's useful. My only concern is that we're complicating the schema and creating two ways to assign a package to an index.


---

_Comment by @corleyma on 2024-09-20 18:41_

@charliermarsh I know it's painful/stupid/messy, but the regex assignment is still really useful for handling transitive dependencies in corporate environments.  I don't think there are easy alternatives given the default assumption that python wants to make about indices being equivalent.

---

_Comment by @zanieb on 2024-09-20 18:48_

I wonder if we should have a separate schema for pinning transitive packages to a defined index? It could allow globs as well. I'm not sure what all the trade-offs are.

---

_Comment by @charliermarsh on 2024-09-20 19:43_

@zanieb -- That's a good call. Instead of putting this on the index schema, we could have a separate table for assigning packages to named indexes.

---

_Comment by @charliermarsh on 2024-09-26 18:27_

I'll probably be pushing to this branch a lot over the next few days, so you may want to unsubscribe if the notifications are annoying!

---

_Comment by @charliermarsh on 2024-09-27 01:41_

Ok @zanieb -- I think this is ready for review. It now includes docs too.

---

_Comment by @j178 on 2024-09-27 05:05_

While playing with this, I found an issue that the lockfile is always ignored due to missing remote index:
```console
DEBUG Ignoring existing lockfile due to missing remote index: `idna` `3.4` from `https://download.pytorch.org/whl/cu121`
```

A reproduction:
```console
$ cat pyproject.toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "idna>=3.4",
]

[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
idna = { index = "pytorch" }

$ cargo run -- lock
$ cargo run -- lock
```

Seems like `explict` indexes are not included in the `locations.indexes()`, so that `remotes` does not contain `explict` indexes:
https://github.com/astral-sh/uv/blob/805f1bd6f548f142acd592e5089597b31a012a7c/crates/uv-resolver/src/lock/mod.rs#L1046-L1067

---

_Comment by @charliermarsh on 2024-09-27 12:37_

Thanks, good catch.

---

_Review requested from @konstin by @charliermarsh on 2024-09-28 19:02_

---

_Review comment by @konstin on `docs/configuration/indexes.md`:17 on 2024-09-30 08:38_

Nit: We now prefer PEP 691 – JSON-based Simple API for Python Package Indexes because it's better to parse json than html.

---

_Review comment by @konstin on `docs/configuration/indexes.md`:23 on 2024-09-30 08:44_

Does that mean if I have an index in xdg config `uv.toml`, that one is tried first, then the one in my project? Is there a way to enumerate the indexes and their to be used by project in `pyproject.toml` explicitly? 

---

_Review comment by @konstin on `docs/configuration/indexes.md`:82 on 2024-09-30 08:48_

This is a bit more far-fetched, but can i use constraints + sources to pin transitive dependencies to an index?

---

_Review comment by @konstin on `crates/pypi-types/src/requirement.rs`:322 on 2024-09-30 08:49_

Shouldn't this refer to an index name, rather than a URL? By defining a name, we comment on the indexes meaning and avoid repeating the URL each time.

---

_Review comment by @konstin on `crates/uv/tests/show_settings.rs`:3682 on 2024-09-30 09:31_

Stale comment

---

_@konstin approved on 2024-09-30 09:38_

---

_@zanieb reviewed on 2024-09-30 22:37_

---

_Review comment by @zanieb on `crates/distribution-types/src/index.rs`:29 on 2024-09-30 22:37_

Perhaps

```suggestion
    /// Explicit indexes will _only_ be used when explicitly requested via a `[tool.uv.sources]`
```

---

_@zanieb reviewed on 2024-09-30 22:40_

---

_Review comment by @zanieb on `crates/distribution-types/src/index.rs`:51 on 2024-09-30 22:40_

I thought it was the opposite?

---

_Review comment by @zanieb on `crates/distribution-types/src/index.rs`:73 on 2024-09-30 22:41_

Is this addressed elsewhere?

---

_@zanieb reviewed on 2024-09-30 22:41_

---

_@zanieb reviewed on 2024-09-30 22:43_

---

_Review comment by @zanieb on `crates/distribution-types/src/index.rs`:117 on 2024-09-30 22:43_

I presume this check is intended to avoid collisions with usernames after the scheme?

---

_@zanieb reviewed on 2024-09-30 22:52_

---

_Review comment by @zanieb on `crates/distribution-types/src/index_url.rs`:455 on 2024-09-30 22:52_

I don't quite follow the "pip perspective" part.

---

_@charliermarsh reviewed on 2024-09-30 22:54_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index.rs`:73 on 2024-09-30 22:54_

No it's a TODO. I intentionally left it in but I don't feel strongly.

---

_@charliermarsh reviewed on 2024-09-30 22:54_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index.rs`:51 on 2024-09-30 22:54_

Yeah sorry, this comment looks stale. It is the opposite.

---

_@charliermarsh reviewed on 2024-09-30 22:55_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index.rs`:117 on 2024-09-30 22:55_

I think I just wouldn't want to detect `https://foo=bar.com` as a name `https://foo`.

---

_@zanieb reviewed on 2024-09-30 22:55_

---

_Review comment by @zanieb on `crates/pypi-types/src/requirement.rs`:322 on 2024-09-30 22:55_

The comment seems wrong as-is

---

_@zanieb reviewed on 2024-09-30 23:11_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:772 on 2024-09-30 23:11_

Same comment as earlier.

---

_@zanieb reviewed on 2024-09-30 23:12_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:754 on 2024-09-30 23:12_

Should we refer to index strategies here too?

---

_@zanieb reviewed on 2024-09-30 23:15_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:11825 on 2024-09-30 23:15_

Just "named" right? There's no explicit declaration here.

---

_@zanieb reviewed on 2024-09-30 23:15_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:11893 on 2024-09-30 23:15_

Might be safer to use `example.com` or something so the test fails if it's used?

---

_@zanieb reviewed on 2024-09-30 23:16_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:12154 on 2024-09-30 23:16_

What if I provided the named index _again_ via the CLI?

---

_@zanieb reviewed on 2024-09-30 23:18_

---

_Review comment by @zanieb on `docs/concepts/dependencies.md`:73 on 2024-09-30 23:18_

I worry the use of "explicit" here will be conflated with the `explicit` index marker

---

_@zanieb reviewed on 2024-09-30 23:19_

---

_Review comment by @zanieb on `docs/configuration/environment.md`:6 on 2024-09-30 23:19_

Is "base index" right right idea here?

---

_@zanieb reviewed on 2024-09-30 23:19_

---

_Review comment by @zanieb on `docs/configuration/environment.md`:6 on 2024-09-30 23:19_

Can't be multiple values right? I think these might be roughly swapped but both need some work.

---

_@zanieb reviewed on 2024-09-30 23:20_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:5 on 2024-09-30 23:20_

```suggestion
private indexes, via the `[[tool.uv.index]]` configuration option (and `--index`, the analogous
```

---

_@zanieb reviewed on 2024-09-30 23:21_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:15 on 2024-09-30 23:21_

Would avoid the use of "explicit" here

---

_@zanieb reviewed on 2024-09-30 23:21_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:17 on 2024-09-30 23:21_

I'd discuss the expected format somewhere else, I think. It seems too irrelevant to most users to put here.

---

_@zanieb reviewed on 2024-09-30 23:22_

---

_Review comment by @zanieb on `docs/configuration/index.md`:8 on 2024-09-30 23:22_

Just wondering, why here instead of in `Concepts`?

---

_@zanieb reviewed on 2024-09-30 23:23_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:23 on 2024-09-30 23:23_

I think project configuration always takes precedence over configuration outside the project.

---

_@zanieb reviewed on 2024-09-30 23:23_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:34 on 2024-09-30 23:23_

Interesting thing missing here. What if I want to disable having a default index entirely?

---

_@zanieb reviewed on 2024-09-30 23:24_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:57 on 2024-09-30 23:24_

Is this supposed to be

> ... to ensure that _only_ `torch` is installed from ...

---

_@zanieb reviewed on 2024-09-30 23:26_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:70 on 2024-09-30 23:26_

As far as I can tell, we silently ignore if we receive a command-line argument that interacts with a named index right?

Separately, why this limitation? What happened to being able to override a named index on a single invocation?

---

_@zanieb reviewed on 2024-09-30 23:27_

---

_Review comment by @zanieb on `docs/pip/compatibility.md`:151 on 2024-09-30 23:27_

Should we refer to "searching across multiple indexes" and deduplicate the content above?

---

_@zanieb reviewed on 2024-09-30 23:28_

---

_Review comment by @zanieb on `crates/distribution-types/src/index.rs`:117 on 2024-09-30 23:28_

Sorry I was thinking about `@` — anyway yeah this seems right.

---

_@charliermarsh reviewed on 2024-09-30 23:32_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:70 on 2024-09-30 23:32_

> Separately, why this limitation? What happened to being able to override a named index on a single invocation?

It's an implementation detail... It probably should work, but it's hard because those get merged into settings, and we don't look in the settings when resolving explicit, named indexes (we just look in the current workspace).


---

_@charliermarsh reviewed on 2024-10-01 00:29_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:23 on 2024-10-01 00:29_

Correct.

---

_@charliermarsh reviewed on 2024-10-01 00:29_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:82 on 2024-10-01 00:29_

I think you would need to make it a direct dependency.

---

_@charliermarsh reviewed on 2024-10-01 00:30_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:322 on 2024-10-01 00:30_

This is correct as a URL. This isn't the user-facing type. But the comment is wrong.

---

_@charliermarsh reviewed on 2024-10-01 00:37_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:12154 on 2024-10-01 00:37_

The CLI wins!

---

_@charliermarsh reviewed on 2024-10-01 00:40_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:34 on 2024-10-01 00:40_

`--no-index` does that, but otherwise there's no such thing.

---

_@charliermarsh reviewed on 2024-10-01 00:40_

---

_Review comment by @charliermarsh on `docs/configuration/index.md`:8 on 2024-10-01 00:40_

I'm not sure. To me it was similar to authentication.

---

_@charliermarsh reviewed on 2024-10-01 00:47_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:12154 on 2024-10-01 00:47_

Added `lock_repeat_named_index_cli` for this.

---

_@charliermarsh reviewed on 2024-10-01 00:49_

---

_Review comment by @charliermarsh on `docs/pip/compatibility.md`:151 on 2024-10-01 00:49_

I could go either way. I don't mind having dedicated content here on how it differs from pip.

---

_@charliermarsh reviewed on 2024-10-01 00:51_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:3871 on 2024-10-01 00:51_

@zanieb -- Should we... hide these?

---

_@zanieb reviewed on 2024-10-01 02:16_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3871 on 2024-10-01 02:16_

In a future release, I think.

---

_@charliermarsh reviewed on 2024-10-01 04:31_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:70 on 2024-10-01 04:31_

I'm working on this, but it will be a separate PR.

---

_@charliermarsh reviewed on 2024-10-01 04:44_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:70 on 2024-10-01 04:44_

I'm considering tracking the "source" of every index (CLI, configuration file, etc.), as field on the index type.

---

_@zanieb reviewed on 2024-10-01 04:57_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:34 on 2024-10-01 04:57_

I think it'll be fairly common to want to disable PyPI entirely?

---

_@zanieb approved on 2024-10-01 04:58_

---

_@charliermarsh reviewed on 2024-10-01 12:04_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:34 on 2024-10-01 12:04_

That’s the same as defining a different default index.

---

_@zanieb reviewed on 2024-10-01 12:13_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:34 on 2024-10-01 12:13_

Yes, but it seems weird to need to do that. I might not want to mark my other index as a default for some reason? I don't fully understand the trade-offs. We can wait and see if it comes up, it doesn't block this pull request.

---

_@charliermarsh reviewed on 2024-10-01 13:00_

---

_Review comment by @charliermarsh on `docs/configuration/indexes.md`:34 on 2024-10-01 13:00_

Yeah, I think what you're getting at here is that `default = true` does two things: (1) replaces PyPI, (2) changes priority.

---

_Comment by @szobov on 2024-10-07 15:24_

Hey @charliermarsh
Thanks a lot for the work you're doing to improve python's ecosystem.
I'm very much looking forward for this feature since I'm currently blocked by it.

I wanted to ask what are the steps to get it merged and released and if I can somehow assist you?

I tested it locally by installing `uv` from the branch and it worked smoothly :tada:
<details>
<summary>details on the setup</summary>

    ```
    $ uv version
    uv 0.4.18 (f9ee2a9e2 2024-10-03)
    
    $ cat pyproject.toml
    [project]
    name = "inference"
    version = "0.1.0"
    description = "Add your description here"
    readme = "README.md"
    requires-python = ">=3.12"
    dependencies = [
        "numpy>=2.1.2",
        "torch==2.4.0+cu118",
    ]

    [build-system]
    requires = ["hatchling"]
    build-backend = "hatchling.build"

    [tool.uv.sources]
    torch = { index = "pytorch" }

    [[tool.uv.index]]
    name = "pytorch"
    url = "https://download.pytorch.org/whl/cu118"
    explicit = true
    
    $ cat uv.lock
    ....
    [[package]]
    name = "torch"
    version = "2.4.0+cu118"
    source = { registry = "https://download.pytorch.org/whl/cu118" }
    dependencies = [
        { name = "filelock" },
        { name = "fsspec" },
        { name = "jinja2" },
        { name = "networkx" },
        { name = "nvidia-cublas-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-cuda-cupti-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-cuda-nvrtc-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-cuda-runtime-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-cudnn-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-cufft-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-curand-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-cusolver-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-cusparse-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-nccl-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "nvidia-nvtx-cu11", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "setuptools" },
        { name = "sympy" },
        { name = "triton", marker = "python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'" },
        { name = "typing-extensions" },
    ]
    wheels = [
        { url = "https://download.pytorch.org/whl/cu118/torch-2.4.0%2Bcu118-cp312-cp312-linux_x86_64.whl", hash = "sha256:d38bd98e5faf9565f04d2b59a481cf576cdf4078444cdf24868fbf5ad685dc4d" },
        { url = "https://download.pytorch.org/whl/cu118/torch-2.4.0%2Bcu118-cp312-cp312-win_amd64.whl", hash = "sha256:bd16a12a003fe58276d7b7394a3760d576eb8dbfb4bacea025eb52a1eaf5172b" },
    ]
    ```
</details>

Thanks in advance :pray:


---

_Comment by @charliermarsh on 2024-10-07 15:55_

I’m planning to release it early next week. Sorry for the delay — I’m on vacation today, then at EuroRust later in the week.

---

_Comment by @pythonbyte on 2024-10-10 12:53_

Hey @charliermarsh 

Amazing work, would be amazing if in this PR we could have the 
`uv add should automatically write --index entries to the pyproject.toml file.`

That would help my team and me not to add a library to write/dump the data.toml file 🙏🏽  

---

_Comment by @odie5533 on 2024-10-10 14:05_

> Hey @charliermarsh
> 
> Amazing work, would be amazing if in this PR we could have the `uv add should automatically write --index entries to the pyproject.toml file.`
> 
> That would help my team and me not to add a library to write/dump the data.toml file 🙏🏽

See PRs #7746 and #7747

---

_Comment by @pythonbyte on 2024-10-10 15:42_

> > Hey @charliermarsh
> > Amazing work, would be amazing if in this PR we could have the `uv add should automatically write --index entries to the pyproject.toml file.`
> > That would help my team and me not to add a library to write/dump the data.toml file 🙏🏽
> 
> See PRs #7746 and #7747

That's awesome, thank you so much, really excited about these features 😄 

---

_Merged by @charliermarsh on 2024-10-15 22:24_

---

_Closed by @charliermarsh on 2024-10-15 22:24_

---

_Branch deleted on 2024-10-15 22:24_

---

_Comment by @danieleades on 2024-10-22 07:05_

does this support, or is support planned for, using API keys for authentication against private registries (ie artifactory) rather than using username/password?

---

_Comment by @zanieb on 2024-10-22 11:33_

You can use an API key in the PASSWORD variable — it works for anything that supports HTTP Basic Authentication.

---

_Comment by @danieleades on 2024-10-22 12:30_

> You can use an API key in the PASSWORD variable — it works for anything that supports HTTP Basic Authentication.

thanks for the support!

i'm trying this now and seeing issues. For context, i have a project which has one dependency from a private artifactory registry.

with or without credentials exported to the environment i see the following output:

```
uv sync
  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-package was not found in the package registry and your project depends on my-private-package>=2.0.0,<2.1.dev0, we can conclude that your project's requirements are unsatisfiable.

      hint: my-private-package was requested with a pre-release marker (e.g., sb-qag-sphinx>=2.0.0,<2.1.dev0), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

the dependencies are specified as follows:

```toml
dependencies = [
    "my-private-package~=2.0.0",
    # ...
]

[tool.uv.sources]
my-private-package = {index = "mycompany-py-release"}

[[tool.uv.index]]
name = "artifactory-pypi"
url = "https://artifactory.mycompany.com/artifactory/api/pypi/pypi/simple"
default = true

[[tool.uv.index]]
# Requires:
name = "mycompany-py-release"
url = "https://artifactory.mycompany.com/artifactory/api/pypi/my-private-package/simple"
explicit = true
```

with

```sh
export UV_INDEX_MYCOMPANY_PY_RELEASE_USERNAME={username}
export UV_INDEX_MYCOMPANY_PY_RELEASE_PASSWORD={artifactory token}
```

---

_Comment by @zanieb on 2024-10-22 12:38_

You should open a new issue for this and we can discuss there

---

_Comment by @dariocurr on 2024-11-15 03:52_

> * Users should be able to specify a proxy URL for a given index, to avoid writing user-specific URLs to a lockfile:
> 
> ```toml
> [[tool.uv.index]]
> name = "test"
> url = "https://private.org/simple"
> proxy = "http://<omitted>/pypi/simple"
> ```

Any news about this feature? This is critical for some users



---
