```yaml
number: 8531
title: "Skip existing, second iteration: Check the index before uploading"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/implement-skip-existing
created_at: 2024-10-24T15:59:23Z
updated_at: 2024-10-31T15:23:14Z
url: https://github.com/astral-sh/uv/pull/8531
synced_at: 2026-01-12T16:08:21Z
```

# Skip existing, second iteration: Check the index before uploading

---

_@konstin_

Implementation of https://github.com/astral-sh/uv/issues/7917#issuecomment-2408636016:

The user can pass `--check-url <index url>`. For each file given to upload, uv checks whether the file is already on the index. If the filename does not exist, uv will upload the file. If the filename does exist and the file on the index is the exact same as the local file (hash match), we skip the upload as there's nothing to do. If the file exists and mismatches with different content, we error: There is an inconsistency, and uv does not upload in this case.

The upload itself may then error or succeed. If it errors, we check the index URL again: Maybe there was a parallel upload of the same file, and the other upload was quicker (avoid TOCTOU errors). We then apply the same logic: If the filename does exist and the file on the index is the same (hash match), it's ok, there was an upload race. If the file exists and mismatches, we error: There is an inconsistency, and uv does not upload in this case. I'm not sure yet if we should do the second check only if the error is one of the twine-recognized file-already-existed conditions; i tend to think we can just do the second check on any error.

This enables re-trying publish, but it still requires the author of a package to ensure that they only try to publish each version from a specific commit. We impose this requirement to avoid situations where different files in a version were built from different sources, which breaks a fundamental assumption in uv.

The name `--check-url` is up for bike shedding. I'm hesitant to call it `--index` because it's different from `--index` in resolve or install commands (only one index, caching turned off).

The test script previously only did step 1, now it also checks skip existing behavior. 
1. An upload with a fresh version succeeds.
2. If we're using PyPI, uploading the same files again succeeds.
3. Skip existing works and reports the files as skipped.

Follow-up: Test that skip existing error when the hashes mismatch.

Fixes #7917

---

_Label `enhancement` added by @konstin on 2024-10-24 15:59_

---

_Label `preview` added by @konstin on 2024-10-24 15:59_

---

_@konstin reviewed on 2024-10-24 16:01_

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:291 on 2024-10-24 16:01_

This is the minimally invasive change. An alternative would be making the `Cache` an `Arc<Mutex<Cache>>`, acknowledging that we're editing externally mutable state.

---

_@konstin reviewed on 2024-10-24 16:01_

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:148 on 2024-10-24 16:01_

Avoid the cost for creating another client.

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:448 on 2024-10-24 16:02_

Do we need the index url here or will it do the right thing without it?

---

_@konstin reviewed on 2024-10-24 16:02_

---

_@konstin reviewed on 2024-10-24 16:03_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:653 on 2024-10-24 16:03_

We scrap the index-specific twine code entirely and do the skip existing checks for any bad status

---

_Review requested from @charliermarsh by @konstin on 2024-10-28 09:27_

---

_@charliermarsh reviewed on 2024-10-28 12:32_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:448 on 2024-10-28 12:32_

It depends. Was the client created with _only_ this index as the set of locations? It seems right to pass it in though.

---

_@charliermarsh reviewed on 2024-10-28 12:33_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:4815 on 2024-10-28 12:33_

Why does this take an index, rather than a bool?

---

_@charliermarsh reviewed on 2024-10-28 12:36_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:291 on 2024-10-28 12:36_

This seems off... Can we not create an entirely new client with a fresh cache?

---

_@konstin reviewed on 2024-10-28 12:47_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4815 on 2024-10-28 12:47_

It's the story documented in https://github.com/astral-sh/uv/issues/7917#issuecomment-2402208639 and https://github.com/astral-sh/uv/issues/7917#issuecomment-2408636016: An index may react arbitrarily to a file with a filename that does already exist: PyPI for example checks if the hash matches and then return a plain OK as for a fresh upload, while other registries error even when re-uploading the same file. By proactively checking the simple index over relying on unspecified, inconsistent behaviour, `uv publish` behaves consistently across registries, while also preventing one case of uploading inconsistent wheel/source dist combinations. In cases where we skip an upload, it is also faster than a twine-style skip existing bool.

---

_@charliermarsh reviewed on 2024-10-28 13:10_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:4815 on 2024-10-28 13:10_

But why do we need to provide the index here? Because you need to be able to query for metadata, and the publish URL isn't sufficient?

---

_@konstin reviewed on 2024-10-28 13:16_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4815 on 2024-10-28 13:16_

There could e.g. be multiple index URLs, and we can't compute the index URL from the publish URL, so you need to manually provide the index URL that matches the publish URL. Making it explicit here seemed like a clear interface.

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4815 on 2024-10-28 13:59_

I'm not overly attached to doing it this way, i mainly picked it because it seemed like a clear, explainable design.

---

_@konstin reviewed on 2024-10-28 13:59_

---

_@zanieb reviewed on 2024-10-28 14:13_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4808 on 2024-10-28 14:13_

This last sentence is a bit unclear.

---

_@zanieb reviewed on 2024-10-28 14:13_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4810 on 2024-10-28 14:13_

I'd say something like "The exact behavior will vary based on implementation details of the index. When uploading to PyPI, ..."

---

_@zanieb reviewed on 2024-10-28 14:14_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4815 on 2024-10-28 14:14_

Won't this be surprising when coming from another tool like `twine` where this is a bool? Should we call it something else?

---

_@konstin reviewed on 2024-10-28 14:36_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4815 on 2024-10-28 14:36_

As I've said above, the name is open for bike shedding. I like it for being self explaining (`uv publish --skip-existing https://example.org/simple` means to skip any files existing on https://example.org/simple when publishing) and for not being confused with the regular index API.

---

_Comment by @zanieb on 2024-10-28 21:49_

It'd be nice if we could re-use the new index settings to improve the publishing experience for uv-managed projects (which I think is the key use-case we should be focusing on). For example:

```toml
[[index]]
name = "example"
url = "https://pypi.org/simple"
publish-url = "https://upload.pypi.org/legacy"
```

with `uv publish --index example` we'd have all the information we need for `--skip-existing` to remain a boolean flag. I think a change like that would require quite a bit of discussion though, and I don't want to derail improvements here, so my goal is to question if there's a way to do this that's consistent with other tooling and provides a path to a better experience in the long run?

Could we have `--skip-existing` work as as a boolean flag and provide an optional flag for an index URL (e.g., `--query-url`) to improve its behavior? It's not clear to me if `--skip-existing` should fail when used with non-PyPI indexes without an additional `--query-url` or just operate on a best-effort basis and provide a hint on failure.

As a closing note, I worry about optimizing the `--skip-existing` behavior for non-PyPI servers. I think if someone is using a non-standard server than it's reasonable to provide an additional flag.

---

_Comment by @konstin on 2024-10-29 09:09_

> with `uv publish --index example` we'd have all the information we need for `--skip-existing` to remain a boolean flag. I think a change like that would require quite a bit of discussion though, and I don't want to derail improvements here, so my goal is to question if there's a way to do this that's consistent with other tooling and provides a path to a better experience in the long run?

I like the `uv publish --index example`: It's concise, it has toml configuration and it pairs the publish URL with the index URL directly. We should provide both a named index option and individual options, so I'm fine with adding `--index` in a follow-up.
 
> Could we have `--skip-existing` work as as a boolean flag and provide an optional flag for an index URL (e.g., `--query-url`) to improve its behavior? It's not clear to me if `--skip-existing` should fail when used with non-PyPI indexes without an additional `--query-url` or just operate on a best-effort basis and provide a hint on failure.
>
> As a closing note, I worry about optimizing the `--skip-existing` behavior for non-PyPI servers. I think if someone is using a non-standard server than it's reasonable to provide an additional flag.

We should provide consistent publish experience no matter the server behavior. There is no guarantee a server behaves a certain way (or changes that way), and only an index URL gives us confidence when checking.

We can't consider non-PyPI servers non-standard, there is no standard (that would be the long-awaited Upload API 2.0), and I believe we shouldn't consider internal index users second class. All implementations exist in a back-and-forth between twine as client and the various servers: Twine itself contains special casing for their skip existing implementation for various servers (https://github.com/pypa/twine/blob/c512bbf166ac38239e58545a39155285f8747a7b/twine/commands/upload.py#L58-L72). An advantage of the skip existing with URL in that it means no more special casing heuristics.

The boolean skip existing option can't differentiate between matching and mismatching re-uploads, which is crucial to catch mismatching uploads, and it also doesn't allow skipping uploads of files that are already uploaded. Since users will need to know their index URL anyway for using the uploaded package, we can make this `--query-url` mandatory, which here is called `--skip-existing`.

---

_Comment by @zanieb on 2024-10-29 17:55_

> We can't consider non-PyPI servers non-standard, there is no standard (that would be the long-awaited Upload API 2.0), and I believe we shouldn't consider internal index users second class. 

I think you're picking at the language here. I think we can consider servers that are not _the official Python package index_ as unofficial. I don't think they need to be second-class, but the they're already secondary, e.g., `uv publish` uses PyPI by default. 

Can you clarify the existing behaviors for me, i.e., for twine? A repeated upload with the same hash to PyPI results in an OK so `--skip-existing` isn't needed, that's just the default behavior? Other indexes do inconsistent things, i.e., erroring so `--skip-existing` needs to ignore special-cased error messages?

---

_Comment by @konstin on 2024-10-29 19:06_

Skip existing makes twine ignore all error responses matching https://github.com/pypa/twine/blob/c512bbf166ac38239e58545a39155285f8747a7b/twine/commands/upload.py#L58-L72. See also https://github.com/astral-sh/uv/issues/7917#issuecomment-2402208639 for more cross index testing.

## GitLab

**Uploading the same file for the same name**

```
twine upload --repository-url https://gitlab.com/api/v4/projects/61853105/packages/pypi scripts/publish/astral-test-token/dist/astral_test_token-0.1.730-py3-none-any.whl
```

Error 400 Bad Request

```
twine upload --repository-url --skip-existing https://gitlab.com/api/v4/projects/61853105/packages/pypi scripts/publish/astral-test-token/dist/astral_test_token-0.1.730-py3-none-any.whl
```

Ok

**Uploading a different file for the same name**

```
twine upload --repository-url https://gitlab.com/api/v4/projects/61853105/packages/pypi scripts/publish/astral-test-token-modified/dist/astral_test_token-0.1.730-py3-none-any.whl
```

Error 400 Bad Request

```
twine upload --repository-url --skip-existing https://gitlab.com/api/v4/projects/61853105/packages/pypi scripts/publish/astral-test-token-modified/dist/astral_test_token-0.1.730-py3-none-any.whl
```

Ok

## PyPI

Using PyPI instead of GitLab gives a slightly different pattern:

**Uploading the same file for the same name**

```
twine upload -r testpypi scripts/publish/astral-test-token/dist/astral_test_token-0.1.730-py3-none-any.whl
```

OK

```
twine upload -r testpypi --skip-existing scripts/publish/astral-test-token/dist/astral_test_token-0.1.730-py3-none-any.whl
```

Ok

**Uploading a different file for the same name**

```
twine upload -r testpypi scripts/publish/astral-test-token-modified/dist/astral_test_token-0.1.730-py3-none-any.whl
```

Error 400 Bad Request

```
twine upload -r testpypi --skip-existing scripts/publish/astral-test-token-modified/dist/astral_test_token-0.1.730-py3-none-any.whl
```

Ok


---

_@konstin reviewed on 2024-10-29 23:10_

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:291 on 2024-10-29 23:10_

My thought was avoiding that, so we only have to do a revalidation request. Is there an advantage in using a temp cache here?

---

_Comment by @konstin on 2024-10-29 23:18_

The main design constraint here is that i feel strongly that the behavior should be consistent across all indexes, and I've picked the design with a mandatory index URL since it was the only one that could provide this consistency.

---

_Comment by @zanieb on 2024-10-29 23:29_

So.. in a future world where we use `--index` instead of `--publish-url`, does `--skip-existing` become the default behavior? Is there a world in which you would not want to skip existing, identical objects? (Trying to inform a decision on the flag name)

---

_Comment by @konstin on 2024-10-29 23:41_

Yes, i'm planning to add `--index` and make skip existing the default behavior with this; I couldn't come up with any case where we didn't want to skip existing, identical objects if we can.

---

_Comment by @zanieb on 2024-10-30 19:23_

I think if `--skip-existing` will be the default future behavior, I think we we want to call this something else. Concretely, I think `--check-url <URL>` is the clearest corollary to `--publish-url`. Additionally, we may want to add a `--skip-existing` boolean flag which errors and suggests the correct usage like we do for unimplemented behaviors in `uv pip`. Once we add support for `--index`, `--skip-existing` can just warn that it has no effect when we have an implicit `--check-url`.

---

_@zanieb reviewed on 2024-10-31 13:33_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4823 on 2024-10-31 13:33_

Maybe rephrase now?

```suggestion
    /// Check an index URL for existing files to skip duplicate uploads.
 ```

---

_@zanieb approved on 2024-10-31 13:35_

Thanks for your patience. I'm not sure re. @charliermarsh's comments about the client.

---

_@zanieb reviewed on 2024-10-31 13:36_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:704 on 2024-10-31 13:36_

This isn't quite descriptive

---

_@charliermarsh reviewed on 2024-10-31 13:40_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:291 on 2024-10-31 13:40_

I don't mean a temp cache. I mean a new `Cache` object that points to the same storage, but with a replaced `Refresh`. The advantage is we avoid this strange API.

---

_@charliermarsh reviewed on 2024-10-31 13:48_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:421 on 2024-10-31 13:48_

I think I'd prefer that this method _not_ take an `Option`, and we just don't call it if `index_client` is `None`.

---

_@charliermarsh reviewed on 2024-10-31 13:56_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:291 on 2024-10-31 13:56_

I can demonstrate what I mean with a refactor if it's not clear.

---

_Merged by @konstin on 2024-10-31 15:23_

---

_Closed by @konstin on 2024-10-31 15:23_

---

_Branch deleted on 2024-10-31 15:23_

---
