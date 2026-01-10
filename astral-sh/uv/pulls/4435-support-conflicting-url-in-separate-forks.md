```yaml
number: 4435
title: Support conflicting URL in separate forks
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/branching-url-support
created_at: 2024-06-21T16:10:03Z
updated_at: 2024-06-26T11:58:24Z
url: https://github.com/astral-sh/uv/pull/4435
synced_at: 2026-01-10T13:48:28Z
```

# Support conflicting URL in separate forks

---

_Pull request opened by @konstin on 2024-06-21 16:10_

Downstack PR: #4481

## Introduction

We support forking the dependency resolution to support conflicting registry requirements for different platforms, say on package range is required for an older python version while a newer is required for newer python versions, or dependencies that are different per platform. We need to extend this support to direct URL requirements.

```toml
dependencies = [
  "iniconfig @ https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl ; python_version >= '3.12'",
  "iniconfig @ https://files.pythonhosted.org/packages/9b/dd/b3c12c6d707058fa947864b67f0c4e0c39ef8610988d7baea9578f3c48f3/iniconfig-1.1.1-py2.py3-none-any.whl ; python_version < '3.12'"
]
```

This did not work because `Urls` was built on the assumption that there is a single allowed URL per package. We collect all allowed URL ahead of resolution by following direct URL dependencies (including path dependencies) transitively, i.e. a registry distribution can't require a URL.

## The same package can have Registry and URL requirements

Consider the following two cases:

requirements.in:
```text
werkzeug==2.0.0
werkzeug @ https://files.pythonhosted.org/packages/ff/1d/960bb4017c68674a1cb099534840f18d3def3ce44aed12b5ed8b78e0153e/Werkzeug-2.0.0-py3-none-any.whl
```
pyproject.toml:
```toml
dependencies = [
  "iniconfig == 1.1.1 ; python_version < '3.12'",
  "iniconfig @ git+https://github.com/pytest-dev/iniconfig@93f5930e668c0d1ddf4597e38dd0dea4e2665e7a ; python_version >= '3.12'",
]
```

In the first case, we want the URL to override the registry dependency, in the second case we want to fork and have one branch use the registry and the other the URL. We have to know about this in `PubGrubRequirement::from_registry_requirement`, but we only fork after the current method.

Consider the following case too:

a:
```
c==1.0.0
b @ https://b.zip
```
b:
```
c @ https://c_new.zip ; python_version >= '3.12'",
c @ https://c_old.zip ; python_version < '3.12'",
```

When we convert the requirements of `a`, we can't know the url of `c` yet. The solution is to remove the `Url` from `PubGrubPackage`: The `Url` is redundant with `PackageName`, there can be only one url per package name per fork. We now do the following: We track the urls from requirements in `PubGrubDependency`. After forking, we call `add_package_version_dependencies` where we apply override URLs, check if the URL is allowed and check if the url is unique in this fork. When we request a distribution, we ask the fork urls for the real URL. Since we prioritize url dependencies over registry dependencies and skip packages with `Urls` entries in pre-visiting, we know that when fetching a package, we know if it has a url or not.

## URL conflicts

pyproject.toml (invalid):
```toml
dependencies = [
  "iniconfig @ https://files.pythonhosted.org/packages/44/39/e96292c7f7068e58877f476908c5974dc76c37c623f1fa332fe4ed6dfbec/iniconfig-1.1.0.tar.gz",
  "iniconfig @ https://files.pythonhosted.org/packages/9b/dd/b3c12c6d707058fa947864b67f0c4e0c39ef8610988d7baea9578f3c48f3/iniconfig-1.1.1-py2.py3-none-any.whl ; python_version < '3.12'",
  "iniconfig @ https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl ; python_version >= '3.12'",
]
```

On the fork state, we keep `ForkUrls` that check for conflicts after forking, rejecting the third case because we added two packages of the same name with different URLs.

We need to flatten out the requirements before transformation into pubgrub requirements to get the full list of other requirements which may contain a URL, which was changed in a previous PR: #4430.

## Complex Example

a:
```toml
dependencies = [
  # Force a split
  "anyio==4.3.0 ; python_version >= '3.12'",
  "anyio==4.2.0 ; python_version < '3.12'",
  # Include URLs transitively
  "b"
]
```
b:
```toml
dependencies = [
  # Only one is used in each split.
  "b1 ; python_version < '3.12'",
  "b2 ; python_version >= '3.12'",
  "b3 ; python_version >= '3.12'",
]
```
b1:
```toml
dependencies = [
  "iniconfig @ https://files.pythonhosted.org/packages/9b/dd/b3c12c6d707058fa947864b67f0c4e0c39ef8610988d7baea9578f3c48f3/iniconfig-1.1.1-py2.py3-none-any.whl",
]
```
b2:
```toml
dependencies = [
  "iniconfig @ https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl",
]
```
b3:
```toml
dependencies = [
  "iniconfig @ https://files.pythonhosted.org/packages/44/39/e96292c7f7068e58877f476908c5974dc76c37c623f1fa332fe4ed6dfbec/iniconfig-1.1.0.tar.gz",
]
```

In this example, all packages are url requirements (directory requirements) and the root package is `a`. We first split on `a`, `b` being in each split. In the first fork, we reach `b1`, the fork URLs are empty, we insert the iniconfig 1.1.1 URL, and then we skip over `b2` and `b3` since the mark is disjoint with the fork markers. In the second fork, we skip over `b1`, visit `b2`, insert the iniconfig 2.0.0 URL into the again empty fork URLs, then visit `b3` and try to insert the iniconfig 1.1.0 URL. At this point we find a conflict for the iniconfig URL and error.

## Closing

The git tests are slow, but they make the best example for different URL types i could find.

Part of #3927. This PR does not handle `Locals` or pre-releases yet.


---

_Label `preview` added by @konstin on 2024-06-21 16:10_

---

_Review requested from @BurntSushi by @konstin on 2024-06-21 16:10_

---

_@charliermarsh reviewed on 2024-06-23 17:59_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:259 on 2024-06-23 17:59_

Unnecessary `return`

---

_@charliermarsh reviewed on 2024-06-23 18:00_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/urls.rs`:47 on 2024-06-23 18:00_

Why no overrides?

---

_@charliermarsh reviewed on 2024-06-23 18:01_

---

_Review comment by @charliermarsh on `scripts/branching-urls/a1-root-allowed/pyproject.toml`:14 on 2024-06-23 18:01_

Can we inline these into the tests (by writing out the `pyproject.toml` to the temporary directory at the start of the test)? I would find it easier to write and maintain, and it matches the pattern we use elsewhere.

---

_@charliermarsh reviewed on 2024-06-23 18:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/priority.rs`:115 on 2024-06-23 18:07_

Why does this work for cases in which the URL dependency is deeper in the tree than the registry version?

---

_@charliermarsh reviewed on 2024-06-23 18:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/urls.rs`:120 on 2024-06-23 18:08_

Why does this not have to check overrides?

---

_@charliermarsh reviewed on 2024-06-23 18:13_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/priority.rs`:115 on 2024-06-23 18:13_

E.g., what if we fork, and add `flask == 3.0.0`, but then some other package in the forked branch depends on `flask @ https://...`. I might've imagined that we instead populate `ForkUrls` from `Urls` by filtering with the fork markers, but it looks like we instead add them by looking at the added dependencies.

---

_@konstin reviewed on 2024-06-23 18:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/priority.rs`:115 on 2024-06-23 18:35_

Correct me if i'm wrong, but my understanding was that we only allow url dependencies to come from urls dependencies, and we do any url dependency before any registry dependency, so if we have:
```
flask==3.0.0
foo @ https://foo.zip
```
foo:
```
flask @ https://flask.zip
```
then the order we go is `foo @ https://foo.zip`, `flask @ https://flask.zip` and `flask==3.0.0`

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/urls.rs`:47 on 2024-06-23 18:35_

They have different semantics and are handled below

---

_@konstin reviewed on 2024-06-23 18:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/urls.rs`:120 on 2024-06-23 18:36_

We try `urls.get_override` before calling this method

---

_@konstin reviewed on 2024-06-23 18:36_

---

_@charliermarsh reviewed on 2024-06-23 18:55_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/urls.rs`:47 on 2024-06-23 18:55_

Why?

---

_@charliermarsh reviewed on 2024-06-23 20:23_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/priority.rs`:115 on 2024-06-23 20:23_

But in that case, when we add the dependencies for whatever package contains `flask` and `foo` (the first package in your example), wouldn't we add a dependency on `flask` _without_ the URL component, since we haven't visited it yet?

---

_Converted to draft by @konstin on 2024-06-24 12:03_

---

_Comment by @codspeed-hq[bot] on 2024-06-24 12:07_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/branching-url-support)

### Merging #4435 will **improve performances by 23.65%**

<sub>Comparing <code>konsti/branching-url-support</code> (7647172) with <code>main</code> (ca92b55)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/branching-url-support` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_airflow` | 1.3 s | 1 s | +23.65% |


---

_@konstin reviewed on 2024-06-24 14:53_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/urls.rs`:47 on 2024-06-24 14:53_

I added better inline docs about that at `Urls`

---

_@konstin reviewed on 2024-06-24 19:28_

---

_Review comment by @konstin on `scripts/branching-urls/a1-root-allowed/pyproject.toml`:14 on 2024-06-24 19:28_

I'll inline these last when everything else is done; inline test make debugging harder than it needs to be.

---

_@konstin reviewed on 2024-06-24 20:08_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/priority.rs`:115 on 2024-06-24 20:08_

I removed the url field from `PubGrubPackage` to support this.

---

_@konstin reviewed on 2024-06-24 20:59_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/urls.rs`:120 on 2024-06-24 20:59_

This code also changed entirely

---

_@charliermarsh reviewed on 2024-06-25 11:32_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/fork_urls.rs`:37 on 2024-06-25 11:32_

Do we need some kind of `is_empty`?

---

_@charliermarsh reviewed on 2024-06-25 11:33_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/fork_urls.rs`:34 on 2024-06-25 11:33_

Does `url` have a `.verbatim`?

---

_@charliermarsh reviewed on 2024-06-25 11:33_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:23 on 2024-06-25 11:33_

Perhaps give an example here?

---

_@charliermarsh reviewed on 2024-06-25 11:34_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/package.rs`:91 on 2024-06-25 11:34_

"and, optionally, a URL." should be removed now, I think.

---

_@charliermarsh reviewed on 2024-06-25 11:35_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:420 on 2024-06-25 11:35_

I personally might do this with `fork_urls.contains_key(name)` rather than `if let None` but subjective :)

---

_@charliermarsh reviewed on 2024-06-25 11:36_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:61 on 2024-06-25 11:36_

Does this file need to be changed too, to avoid batch pre-fetching for URL packages?

---

_@charliermarsh reviewed on 2024-06-25 11:37_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:61 on 2024-06-25 11:37_

Oh I see, you enforce this _outside_ of `prefetch_batches`, at the call site.

---

_@charliermarsh reviewed on 2024-06-25 11:38_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:596 on 2024-06-25 11:38_

Will we potentially make the `Request::Package` prefetch call in `visit_package` for a package that might have a URL but doesn't yet? Similar to the batch prefetch case, but just at the package level? This could be okay... but if the package doesn't exist (and only exists at the URL), we might error?

---

_@charliermarsh reviewed on 2024-06-25 11:39_

---

_Review comment by @charliermarsh on `scripts/branching-urls/a1-root-allowed/pyproject.toml`:14 on 2024-06-25 11:39_

That's fine, thanks.

---

_@charliermarsh reviewed on 2024-06-25 11:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:596 on 2024-06-25 11:40_

Like the case from the summary...

a:
```
c==1.0.0
b @ https://b.zip
```
b:
```
c @ https://c_new.zip ; python_version >= '3.12'",
c @ https://c_old.zip ; python_version < '3.12'",
```

When we visit `a`, wouldn't we then fetch the package-level metadata for `c` from the registry here?


---

_@konstin reviewed on 2024-06-25 14:46_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:596 on 2024-06-25 14:46_

Good catch! I changed pre-visiting and added an extra `self.request_package(&state.next, url, &request_sink)` call in the main loop for the following case:

> Consider:
> ```toml
> dependencies = [
>   "iniconfig == 1.1.1 ; python_version < '3.12'",
>   "iniconfig @ https://files.pythonhosted.org/packages/ef/a6/62565a6e1cf69e10f5727360368e451d4b7f58beeac6173dc9db836a5b46/iniconfig-2.0.0-py3-none-any.whl ; python_version >= '3.12'",
> ]
> ```
> In the `python_version < '3.12'` case, we haven't pre-visited `iniconfig` yet,
> since we weren't sure whether it might also be a URL requirement when
> transforming the requirements. For that case, we do another request here
> (idempotent due to caching).

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/fork_urls.rs`:37 on 2024-06-25 14:59_

I would vote for `is_always_true` and `is_always_false`.

---

_Marked ready for review by @konstin on 2024-06-25 15:26_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/dependencies.rs`:167 on 2024-06-25 16:06_

We should probably just turn this lint off. :)

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/priority.rs`:41 on 2024-06-25 16:10_

https://github.com/astral-sh/uv/blob/153484bc181795b367917523cabd53fbf711526a/crates/uv-resolver/src/pubgrub/package.rs#L205-L217 is _almost_ helpful here, except it returns a name when `Root` has a name, which would be slightly different than the behavior here.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1097 on 2024-06-25 16:21_

A turbofish practitioner I see.

---

_@BurntSushi approved on 2024-06-25 16:26_

Awesome work! :tada: I feel like this ended up simplifying aspects of the code, especially `PubGrubPackage`. I like.

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:1591 on 2024-06-25 16:36_

We should probably check `Or` too -- it's equally "empty".

---

_@charliermarsh reviewed on 2024-06-25 16:36_

---

_@charliermarsh reviewed on 2024-06-25 16:36_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/requirement.rs`:257 on 2024-06-25 16:36_

Basic Rustdoc please.

---

_@charliermarsh approved on 2024-06-25 16:37_

Ok, I think this is sound. Nice job figuring it out. I'm a little nervous because it's a huge change to the core of the resolver, but not sure there's more to do about it :)

The part I understand least is the whole override URLs thing.

How do you think this will extend to locals and/or pre-releases?

---

_@charliermarsh reviewed on 2024-06-25 16:38_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:384 on 2024-06-25 16:38_

Is this only necessary if `url` is `None`?

---

_@charliermarsh reviewed on 2024-06-25 16:38_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:384 on 2024-06-25 16:38_

I guess that's the majority of the time anyway.

---

_@konstin reviewed on 2024-06-25 19:03_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/dependencies.rs`:167 on 2024-06-25 19:03_

Good eye https://github.com/astral-sh/uv/pull/4529

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/priority.rs`:41 on 2024-06-25 19:09_

What do you think about? We would also use this in `request_package`

```
    /// Returns the name of this PubGrub package, if it has one and is not the root package.
    pub(crate) fn name_no_root(&self) -> Option<&PackageName> {
        match &**self {
            PubGrubPackageInner::Root(_) | PubGrubPackageInner::Python(_) => None,
            PubGrubPackageInner::Package { name, .. }
            | PubGrubPackageInner::Extra { name, .. }
            | PubGrubPackageInner::Dev { name, .. }
            | PubGrubPackageInner::Marker { name, .. } => Some(name),
        }
    }
```

---

_@konstin reviewed on 2024-06-25 19:09_

---

_@konstin reviewed on 2024-06-25 19:09_

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/priority.rs`:41 on 2024-06-25 19:09_

(If yes, i'll make a follow-up PR, the stack is large enough)

---

_@konstin reviewed on 2024-06-25 19:19_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:384 on 2024-06-25 19:19_

We need it if `fork_urls` return `None` and `Urls` return something, that is the case when it's ambiguous. I didn't add that check specifically because `request_package` already checks if the request was previously registered

---

_Comment by @konstin on 2024-06-25 19:21_

> The part I understand least is the whole override URLs thing.

Whenever we encounter an override URL, that takes precedence over everything else. We also only allow only a single URL per package from overrides, not supporting splitting on overrides. Does that make it clearer?

> How do you think this will extend to locals and/or pre-releases?

Yes, i think we have to make the same changes for them too.

---

_@konstin reviewed on 2024-06-25 20:24_

---

_Review comment by @konstin on `crates/pep508-rs/src/marker.rs`:1591 on 2024-06-25 20:24_

I'd define `or` semantics as it is true if there exists any value that is true, so an empty `or` would be false

---

_@BurntSushi reviewed on 2024-06-26 11:56_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/priority.rs`:41 on 2024-06-26 11:56_

Yeah I think that's fine.

---

_Merged by @konstin on 2024-06-26 11:58_

---

_Closed by @konstin on 2024-06-26 11:58_

---

_Branch deleted on 2024-06-26 11:58_

---
