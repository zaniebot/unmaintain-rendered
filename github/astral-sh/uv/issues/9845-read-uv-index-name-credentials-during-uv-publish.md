---
number: 9845
title: "Read `UV_INDEX_<NAME>_` credentials during `uv publish`"
type: issue
state: open
author: sfriedowitz
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2024-12-12T16:16:51Z
updated_at: 2025-05-27T08:03:06Z
url: https://github.com/astral-sh/uv/issues/9845
synced_at: 2026-01-07T13:12:18-06:00
---

# Read `UV_INDEX_<NAME>_` credentials during `uv publish`

---

_Issue opened by @sfriedowitz on 2024-12-12 16:16_

I was very excited to see the functionality with `uv publish --index <index>` come out in version `0.5.8` with this PR https://github.com/astral-sh/uv/pull/9694, but I don't think it behaves quite as I expected. It does not appear that publishing reads the `UV_INDEX_MYINDEX_USERNAME` and `UV_INDEX_MYINDEX_PASSWORD` for credentials when publishing.

Is this expected behavior? And if so, would it be possible to add support for reading credentials from those existing environment variables?

---

_Label `needs-decision` added by @charliermarsh on 2024-12-13 22:26_

---

_Comment by @charliermarsh on 2024-12-13 22:26_

I think it seems reasonable to pass the configuration to the publish URL. What do you think, @konstin @zanieb?

---

_Comment by @zanieb on 2024-12-13 22:48_

Oh interesting. Do you expect the credentials for publishing to be the same as the ones for reading?

I'd expect them to be used, but I worry a different credential will be required?

---

_Comment by @charliermarsh on 2024-12-14 01:27_

Is the implication that you might want a different environment variable?

---

_Comment by @sfriedowitz on 2024-12-14 02:29_

At least for the private PyPI repository we run in my organization, the publishing and reading credentials are the same.
So it seemed intuitive that the UV_INDEX_<NAME> variables would be used.

But perhaps the UV_INDEX variables could be used in the absence of the UV_PUBLISH variables?



---

_Comment by @konstin on 2024-12-17 10:32_

We can add `UV_INDEX_` as lowest priority credential source, behind CLI,`UV_PUBLISH_` and (if used) the keyring.

---

_Comment by @sfriedowitz on 2024-12-18 02:23_

> We can add `UV_INDEX_` as lowest priority credential source, behind CLI,`UV_PUBLISH_` and (if used) the keyring.

That would be perfect, thank you !

---

_Referenced in [astral-sh/uv#10193](../../astral-sh/uv/issues/10193.md) on 2024-12-30 16:41_

---

_Comment by @menzenski on 2024-12-30 18:22_

We publish Python libraries to a private Artifactory registry and use separate credentials for read-only operations and for publishing. Those are easy enough to map to the same environment variable names if needed but IMO it'd be nicer to be able to use different names when reading (installing dependencies) vs writing (publishing).

> We can add UV_INDEX_ as lowest priority credential source, behind CLI,UV_PUBLISH_ and (if used) the keyring.

This option sounds great - this would let us use `UV_PUBLISH_` for publishing operations and `UV_INDEX_` for read-only operations - having the two "look" different is less confusing IMO.

---

_Renamed from "UV publish with index name does not read environment credentials" to "Read `UV_INDEX_<NAME>_` credentials during `uv publish`" by @zanieb on 2024-12-30 20:18_

---

_Label `needs-decision` removed by @zanieb on 2024-12-30 20:18_

---

_Label `enhancement` added by @zanieb on 2024-12-30 20:18_

---

_Referenced in [astral-sh/uv#10386](../../astral-sh/uv/issues/10386.md) on 2025-01-08 14:34_

---

_Comment by @SalttownUser on 2025-01-08 15:05_

We publish to private GitLab projects with individual credentials for every project.  In GitLab all the packages from one group are available accessing the group package registry(https://docs.gitlab.com/ee/user/packages/package_registry/#view-packages).

In this scenario the publish token for pushing the package to the project is not the same as the read token for getting the packages from the group registry. So in this case it would be nice, when `UV_PUBLISH_`  would be used to authenticate with `publish-url` and `UV_INDEX_<name>` would be used to authenticate with index  `<name>` to read/check dependencies.

---

_Label `good first issue` added by @zanieb on 2025-01-08 21:12_

---

_Referenced in [astral-sh/uv#7839](../../astral-sh/uv/issues/7839.md) on 2025-01-28 14:56_

---

_Comment by @konstin on 2025-01-28 15:11_

Reviewing our authentication system, i'm less inclined to do this: We currently fall back to prompting the user for username and password if there isn't any set. When index credentials and upload credentials are not the same (they usually should be different, since regular `uv` operations should have less privileges than publish), we would then instead of prompting use the wrong credentials from `UV_INDEX_*` and fail.

---

_Label `good first issue` removed by @konstin on 2025-01-28 15:11_

---

_Label `needs-decision` added by @konstin on 2025-01-28 15:13_

---

_Referenced in [astral-sh/uv#11032](../../astral-sh/uv/pulls/11032.md) on 2025-01-28 18:41_

---

_Comment by @erny on 2025-03-04 13:26_

I couldn't find any comment about using .netrc or .pypirc to store credentials for `uv publish`. Although I have both, uv doesn't use any of them.

---

_Comment by @konstin on 2025-03-04 13:39_

uv does not read `.netrc` or `.pypirc` for publishing, we currently only support the keyring as persistent password storage.

---

_Comment by @charliermarsh on 2025-03-04 13:54_

@konstin -- It seems like `.netrc` could make sense, what do you think?

---

_Comment by @konstin on 2025-03-04 14:09_

Both files are standards that we can support (https://everything.curl.dev/usingcurl/netrc.html, https://packaging.python.org/en/latest/specifications/pypirc/), and we already support netrc for index authenticaton (i.e. we only have to wire this to publish, too), with the caveat that I'd generally nudge people towards the system keyring as a safer alternative to storing password in a plain text file.

---

_Comment by @BartekSzpak on 2025-03-11 20:31_

I went through this [page](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#publishing-packages-to-aws-codeartifact) in the documentation and got surprised that setting the `UV_PUBLISH_USERNAME` and `UV_PUBLISH_PASSWORD` allowed me to publish to a private repository (nexus and not the codeartifact) without being prompted for credentials. 

Not sure if this is intentional, but very useful from my perspective.

---

_Comment by @dzanaga on 2025-03-21 10:43_

in case a user wants to publish to different indexes, would it be possible to have different UV_PUBLISH_ env vars?
similarly to UV_INDEX_INDEXNAME_USERNAME, could we have UV_PUBLISH_INDEXNAME_USERNAME/PASSWORD

or what would be the preferred way to automate publishing to different indexes?

---

_Comment by @konstin on 2025-03-21 23:16_

I'd recommend running the two `uv publish` commands with different `UV_PUBLISH_` values. If you're publishing from a local machine, you can also use the keyring support for dispatching.

---

_Comment by @rainermensing on 2025-05-26 15:30_

A closely related issue to this that I could consider part of it: Can we add the --env-file flag to uv publish? To be fair, it is not available for uv sync either (only uv run), but since sync runs in the background of uv run, you can already use a .env file to set index credentials. Maybe this needs a new issue?

EDIT:
would there be anything problematic with this workaround? It does not seem to install uv again...
`uvx --env-file .env uv publish`

---

_Referenced in [astral-sh/uv#14253](../../astral-sh/uv/pulls/14253.md) on 2025-06-25 07:50_

---
