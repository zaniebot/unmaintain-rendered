```yaml
number: 9583
title: Add documentation on using dependencies from private GitLab package indexes
type: pull_request
state: closed
author: rafalkrupinski
labels: []
assignees: []
base: main
head: docs/gitlab
created_at: 2024-12-02T19:52:00Z
updated_at: 2025-04-16T14:59:00Z
url: https://github.com/astral-sh/uv/pull/9583
synced_at: 2026-01-12T16:08:52Z
```

# Add documentation on using dependencies from private GitLab package indexes

---

_@rafalkrupinski_

Doc change: adding a section on using dependncies from a GitLab package index.

Relates to #9331

Could mention using explicit source configuration as GitLab may be configured as a proxy to the official PyPI, but I see other sections don't include anything more than strictly necessary.

---

_Assigned to @zanieb by @zanieb on 2024-12-02 20:10_

---

_@samypr100 reviewed on 2024-12-03 02:22_

---

_Review comment by @samypr100 on `docs/guides/integration/alternative-indexes.md`:181 on 2024-12-03 02:22_

It might be preferred to suggest using [UV_INDEX_{name}_USERNAME](https://docs.astral.sh/uv/configuration/environment/#uv_index_name_username) and [UV_INDEX_{name}_PASSWORD](https://docs.astral.sh/uv/configuration/environment/#uv_index_name_password) for the credentialed pieces.

---

_@rafalkrupinski reviewed on 2024-12-03 07:52_

---

_Review comment by @rafalkrupinski on `docs/guides/integration/alternative-indexes.md`:181 on 2024-12-03 07:52_

Yeah, that too. With credentials but without token name in URL, uv will first try unauthroized request, receive 404 and cry uncle. I'll reword it for better clarity. Any suggestions?

---

_Review comment by @Raymi306 on `docs/guides/integration/alternative-indexes.md`:181 on 2024-12-03 14:13_

https://github.com/astral-sh/uv/blob/81569c47bfa91b24ff0712baf1c001ef9604676e/crates/uv-auth/src/middleware.rs#L148-L160

Are you sure the 404 is the problem?  It is explicitly one of the status codes that UV will retry with auth for.

---

_@Raymi306 reviewed on 2024-12-03 14:13_

---

_@rafalkrupinski reviewed on 2024-12-03 14:24_

---

_Review comment by @rafalkrupinski on `docs/guides/integration/alternative-indexes.md`:181 on 2024-12-03 14:24_

What I'm sure is that adding token name fixes the issue described in the linked report. Take a look at the logs I've linked there. @zanieb seems to know the underlying cause.

---

_@Raymi306 reviewed on 2024-12-03 14:36_

---

_Review comment by @Raymi306 on `docs/guides/integration/alternative-indexes.md`:181 on 2024-12-03 14:36_

For what it is worth I ran into a very similar issue to the one that you had in terms of uv output, and determined that a strangely configured python package index was returning 200 with no auth, and not listing any versions unless auth was provided.  I may try to implement one of the configuration options discussed in your issue and raise a PR, but I am hoping that I can get the creator of the index I need to use at work to simply fix the probable permissions issue.

---

_@zanieb reviewed on 2024-12-03 14:39_

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:181 on 2024-12-03 14:39_

Adding the token name fixes it because we skip trying an unauthenticated request.

My earlier comments were incorrect regarding the 404, thanks @Raymi306 for pointing that out — we will search for credentails on a 404. It looks like GitLab is actually returning an okay response? The relevant logs being:

```
TRACE No credentials in cache for URL https://gitlab.com/api/v4/groups/${ID}/-/packages/pypi/simple/livity-airtable/
TRACE Attempting unauthenticated request for https://gitlab.com/api/v4/groups/${ID}/-/packages/pypi/simple/livity-airtable/
TRACE cached request https://gitlab.com/api/v4/groups/${ID}/-/packages/pypi/simple/livity-airtable/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: livity-airtable
TRACE Selecting candidate for livity-airtable with range * with 0 remote versions
```

Sorry for misleading you there, I'm not sure why I thought we only retried on 401..

---

_@rafalkrupinski reviewed on 2024-12-03 15:23_

---

_Review comment by @rafalkrupinski on `docs/guides/integration/alternative-indexes.md`:181 on 2024-12-03 15:23_

Cool, thanks for clearing it out.

> strangely configured python package index was returning 200

Hmm, the free gitlab.com offering has only one config option in the package registry - forwarding requests to PyPI. It actually might be the issue. I'll investigate it further.


---

_@zanieb reviewed on 2024-12-03 15:32_

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:181 on 2024-12-03 15:32_

Thanks @rafalkrupinski!

---

_Comment by @timotk on 2025-01-09 13:59_

@rafalkrupinski I would prefer the use of a `.netrc` file. Using the following command, `uv` will modify the `pyproject.toml` for you:
```shell
uv add <INTERNAL-PACKAGE> --index gitlab=https://{GITLAB_HOST}/api/v4/projects/{PROJECT_ID}/packages/pypi/simple
```

The `~/.netrc` will look like this:
```
machine myorganization.gitlab.host
login __token__
password <your_token>
```

or with `gitlab.com`:
```
machine gitlab.com
login __token__
password <your_token>
```

Each user only needs to set up `~/.netrc` once. And you don't have to use environment variables.

I explain more [here](https://xebia.com/blog/how-to-install-python-packages-from-an-internal-package-registry-with-uv/)

---

_Comment by @rafalkrupinski on 2025-01-09 15:45_

I do need to use environment variables in CI tho

---

_Comment by @rafalkrupinski on 2025-01-09 16:24_

@timotk I don't see any documentation on the "gitlab=" part of URL. is it something new?

---

_Comment by @zanieb on 2025-01-09 17:45_

@rafalkrupinski that's part of the index API (https://docs.astral.sh/uv/configuration/indexes/)

---

_Comment by @rafalkrupinski on 2025-01-09 18:30_

Oh, OK, it's the name of index, not kind. So does UV now know how to handle GitLab idiosyncrasies? For me it didn't work because of request forwarding in gitlab index.

---

_Comment by @rafalkrupinski on 2025-01-09 18:46_

@zanieb You just wanted to link to your barely related article, didn't you. This is off-topic.

---

_Comment by @zanieb on 2025-04-16 14:58_

I think this is stale now

---

_Closed by @zanieb on 2025-04-16 14:58_

---
