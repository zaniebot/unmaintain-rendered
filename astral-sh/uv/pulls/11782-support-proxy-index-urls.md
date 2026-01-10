```yaml
number: 11782
title: Support proxy index urls
type: pull_request
state: closed
author: jtfmumm
labels:
  - enhancement
assignees: []
draft: true
base: main
head: jtfm/url-proxy
created_at: 2025-02-25T20:38:15Z
updated_at: 2025-08-15T13:58:25Z
url: https://github.com/astral-sh/uv/pull/11782
synced_at: 2026-01-10T06:44:32Z
```

# Support proxy index urls

---

_Pull request opened by @jtfmumm on 2025-02-25 20:38_

[This draft PR still has a couple of minor FIXMEs and a number of FIXMEs for documenting code. I also plan to add more tests] 

These changes enable users to supply a proxy index url for an index/registry via an `--index-proxy-url` argument or `UV_INDEX_PROXY_URL` env var (both take values of the form `<index-name>=<proxy-url>`). The "canonical" URL for that index is defined via the normal `url` field in `[[tool.uv.index]]`. 

This canonical URL is written to the lockfile. But when a proxy url is passed in for that index, it will actually be used in place of the canonical URL (though never written to the lockfile). This way, if different users or machines have private proxy urls, these will never be checked into source control. Proxy URLs are supported for `uv add`, `uv sync`, `uv lock`, and `uv run`.

There is currently a test for adding a package with a proxy index and then removing `.venv` and syncing. 

Closes #6349


---

_Review requested from @zanieb by @zanieb on 2025-02-25 21:03_

---

_@charliermarsh reviewed on 2025-02-26 02:50_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:722 on 2025-02-26 02:50_

I haven't had a chance to review this yet, but as a minor note, I'd expect this to take `Option<&[ProxyWithCanonicalUrl]>` (and similarly for all public APIs). A slice is strictly more flexible than a reference to a `Vec`: if you have a vector, you can always pass it as a slice, but the opposite is not true.

---

_@charliermarsh reviewed on 2025-02-26 02:52_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock_target.rs`:274 on 2025-02-26 02:52_

It seems like this should be composable -- in other words, should we do this outside of `read`, i.e., expect callers to call `with_proxy_urls` rather than passing it in as an argument and calling this method as the last step? It seems more flexible to me. Same for `commit` -- could we expect callers to apply the proxies before calling `commit`?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:200 on 2025-02-26 02:52_

We would stylize this as "URLs".

---

_@charliermarsh reviewed on 2025-02-26 02:52_

---

_@charliermarsh reviewed on 2025-02-26 02:55_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:269 on 2025-02-26 02:55_

Do these need to be stored on the struct? Can't they always be recovered via `url.as_str()`? (Also, does this mean they're part of the schema, serialized, deserialized, etc.? If so, is that intentional?)

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:318 on 2025-02-26 02:56_

(Nit: dead code)

---

_@charliermarsh reviewed on 2025-02-26 02:56_

---

_@charliermarsh reviewed on 2025-02-26 02:56_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:269 on 2025-02-26 02:56_

It might help to add rustdoc-style comments to each field here to illustrate the expected values with an example.

---

_@charliermarsh reviewed on 2025-02-26 02:58_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:977 on 2025-02-26 02:58_

If we had a method like `with_canonical_urls` that took `mut self` on the `Lock`, could we avoid having to re-clone the entire distribution here?

---

_Comment by @charliermarsh on 2025-02-26 03:00_

Nice job figuring out all the plumbing here. As a higher-level comment, we should definitely get feedback from the users that participated in https://github.com/astral-sh/uv/issues/6349 to understand whether the proposed API solves their problem (or not). I'd probably suggesting commenting there with a clear outline on the proposed design and a link to this PR sooner rather than later, so that we can make sure we're on the right track. In that comment, you could also ask if users are willing / able to test this branch out on their own projects.


---

_@zanieb reviewed on 2025-02-26 03:01_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 03:01_

I worry about `UV_INDEX_PROXY_URL` not matching the `UV_INDEX_<name>_USERNAME` and `UV_INDEX_<name>_PASSWORD` options. Presumably, the reason the environment variables include the index name is it makes it easier to set variables for multiple indexes? We also may have just copied Poetry here. But, otherwise you have to pass them as separated values (with spaces here, commas in some other interfaces) — which is fine in the general case, but probably problematic for credentials where separators could be valid characters.

I'm not entirely sure this should change, but consistency is important. You could argue we should support `UV_INDEX_USERNAME=<name>=<username>` instead, but I don't know how we'd deal with the aforementioned case where the separator is a valid character in the value.

Another argument is that we don't have CLI options for usernames and passwords — they're part of the `--index <...>` value. It could be sufficiently distinct for that reason. We could weigh the usage of `--index-proxy-url` directly matching `UV_INDEX_PROXY_URL` over consistency with the other environment variables.

Another option is to support both, but I don't think there's strong justification for that.

---

_@zanieb reviewed on 2025-02-26 03:11_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10216 on 2025-02-26 03:11_

Might be preferable not to use our proxy here, it's a bit flaky and I'd only use it if you're trying to test for proper credential management. You should just be able to use the normal PyPI?

---

_@zanieb reviewed on 2025-02-26 03:13_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10259 on 2025-02-26 03:13_

Why remove the virtual environment? (Generally `--reinstall` will suffice). Are you just testing `sync` here? Couldn't we be pulling the package fro the cache?

I'd probably just write a dedicated sync test.

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10232 on 2025-02-26 03:14_

We should also snapshot the lockfile, right? Maybe that'd be a test case in `crates/uv/tests/it/lock.rs`?

---

_@zanieb reviewed on 2025-02-26 03:14_

---

_@zanieb reviewed on 2025-02-26 03:17_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10216 on 2025-02-26 03:17_

Speaking of testing credentials, it may be worth having a separate test that uses the `UV_INDEX...` variables to set credentials and ensure they're propagated to the proxy URL correctly. 

---

_@jtfmumm reviewed on 2025-02-26 09:57_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/project/lock_target.rs`:274 on 2025-02-26 09:57_

Updated

---

_@jtfmumm reviewed on 2025-02-26 10:53_

---

_Review comment by @jtfmumm on `crates/uv-resolver/src/lock/mod.rs`:977 on 2025-02-26 10:53_

I've changed this and related methods to mutate directly without cloning

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:10216 on 2025-02-26 13:32_

This is an interesting question. The proxy replacement only works if the url bases for sdists/wheels are the same as the canonical url. As @charliermarsh [mentioned](https://github.com/astral-sh/uv/issues/6349#issuecomment-2393409286) in the original issue, this isn't true for PyPi, which uses urls like `https://files.pythonhosted.org/packages/94/49/26a7b0f3f35da4b5a65f081943b7bcd22d7002f5f0fb8098ec1ff21cb6ef/black-25.1.0.tar.gz`.  

Testing locally, I've been using devpi, which on its own writes your localhost URL as the base for everything. The PyPi proxy I'm using in this test has the same problem as PyPi. Do we have anything like devpi we've used for other tests?

---

_@jtfmumm reviewed on 2025-02-26 13:32_

---

_@jtfmumm reviewed on 2025-02-26 13:33_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/edit.rs`:10259 on 2025-02-26 13:33_

Yeah good point. I'll update

---

_@jtfmumm reviewed on 2025-02-26 13:39_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 13:39_

Can index names contain a combination of dashes and underscores (e.g., `alpha-1_b`)? If there were a name like this, you couldn't specify it following the `UV_INDEX_PROXY_URL_<name>` approach.

My thinking was to keep the values of the command line arg and env var the same, but I'm open to alternatives.

---

_@jtfmumm reviewed on 2025-02-26 13:51_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:269 on 2025-02-26 13:51_

I've removed `raw_canonical_url` as suggested. `url_base` is `url` without its path, so I'd rather do that work once on construction. 

I can add the comments with examples.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 14:38_

We already support index names in environment variables, so there should be no "new" problems here.

The supported characters are at 

https://github.com/astral-sh/uv/blob/c37af945b31da6275770c6ba032fc73e8e09cb1d/crates/uv-distribution-types/src/index_name.rs#L19

and here's the environment variable key conversion

https://github.com/astral-sh/uv/blob/c37af945b31da6275770c6ba032fc73e8e09cb1d/crates/uv-distribution-types/src/index_name.rs#L31-L45

---

_@zanieb reviewed on 2025-02-26 14:38_

---

_@zanieb reviewed on 2025-02-26 14:41_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:10216 on 2025-02-26 14:41_

Oh, interesting. That's.. tricky. I worry this will be a problem for users, but we can discuss that separately — be sure to call this out in the pull request summary and documentation.

We don't have anything like that for other tests. You can use the un-authenticated proxy at `https://pypi-proxy.fly.dev` instead. See https://github.com/astral-sh/pypi-proxy


---

_@zanieb reviewed on 2025-02-26 14:42_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 14:42_

(I'm still undecided what's best for users here)

---

_@jtfmumm reviewed on 2025-02-26 14:56_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 14:56_

This is an edge case, but what happens if a user names one index `alpha-b` and another `alpha_b`? Do we treat those as the same name?

---

_@zanieb reviewed on 2025-02-26 15:01_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 15:01_

It's a good question — I think we should require unique normalized names and error if they do that. I'm not sure if we do today.

---

_@jtfmumm reviewed on 2025-02-26 15:09_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 15:09_

Agree that's the right behavior, which means it's not a reason against your env var proposal. Though I'm still not sure whether it's better to have the values be the same as for the cli arg.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 15:17_

On that note, can you check if we error and if not open a ticket to do so?

I'm leaning towards the `<name>` in the variable. I think consistency across our environment variables probably makes more sense than consistency between the CLI and environment; so:

```
UV_INDEX_<name>_USERNAME
UV_INDEX_<name>_PASSWORD
UV_INDEX_<name>_PROXY_URL
```



---

_@zanieb reviewed on 2025-02-26 15:17_

---

_@zanieb reviewed on 2025-02-26 15:19_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:313 on 2025-02-26 15:19_

Does it makes sense to allow `--proxy-url <url>` without a name? Like we do for `--index <url>`?

If so, I think it'd override the "default" index URL?

---

_@jtfmumm reviewed on 2025-02-26 15:31_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/index.rs`:313 on 2025-02-26 15:31_

That could make sense, but my concern is that since PyPi is the default out of the box, it will increase the chances of users hitting confusing behavior.

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:4762 on 2025-02-26 16:08_

Will do. And yeah, I agree with you. Let's change it to your proposal.

---

_@jtfmumm reviewed on 2025-02-26 16:08_

---

_@zanieb reviewed on 2025-02-26 17:22_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:313 on 2025-02-26 17:22_

It seems indicative that we'll need to find a way to fix the confusing behavior or provide a dedicated error message — but we can consider that separately. It's fine to not support in the initial version, as long as we've considered how it would work if we supported it in the future.

---

_Label `enhancement` added by @jtfmumm on 2025-03-12 16:33_

---

_Comment by @abhiaagarwal on 2025-03-26 03:58_

Hey @jtfmumm, are you still working on this? This has become necessary for me, I'd be happy to take it over if you're too busy :D 

---

_Comment by @zanieb on 2025-04-01 19:02_

@abhiaagarwal this is blocked by a need for more feedback on the feature and use-cases.

---

_Closed by @jtfmumm on 2025-08-15 13:58_

---
