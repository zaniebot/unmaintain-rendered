```yaml
number: 12280
title: Provide feedback and improve doc on keyring index authentication
type: issue
state: closed
author: LSerranoPEReN
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2025-03-18T14:02:01Z
updated_at: 2025-09-30T16:16:21Z
url: https://github.com/astral-sh/uv/issues/12280
synced_at: 2026-01-10T03:23:53Z
```

# Provide feedback and improve doc on keyring index authentication

---

_Issue opened by @LSerranoPEReN on 2025-03-18 14:02_

### Summary

Hello,
I tried using `uv` with custom package indexes and found the way to configure the authentication with `keyring` really very counter-intuitive.

Foremost, the services names that `uv` tries to access with `keyring` is, to the best of my knowledge, not documented.
By looking at the traces it seems that `uv` tries at least two services names:
- `<package_index_url>/<package_name>`
- `<package_index_fqdn>`

Moreover, invalid credentials or missing credentials gives the same error:  
`An index URL (<package_index_url>) could not be queried due to a lack of valid authentication credentials (401 Unauthorized)`. This become more confusing as [the documentation clearly states that:](https://docs.astral.sh/uv/configuration/indexes/#using-credential-providers)
> When authenticate is set to always, uv will eagerly search for credentials and error if credentials cannot be found.

But I personally could not get `uv` to return a different error in this case. 


I think that using custom package indexes would be a lot less frustrating if :
1. Services names looked up by `uv` were documented in the [authentication section](https://docs.astral.sh/uv/configuration/indexes/#authentication)
2. Failures due to missing credentials printed a different error from failures due to invalid credentials.
3. Failures due to missing credentials listed the service name tried.
4. Failures due to invalid credentials printed the service name used for the index authentication.

### Example

_No response_

---

_Label `enhancement` added by @LSerranoPEReN on 2025-03-18 14:02_

---

_Comment by @zanieb on 2025-03-18 16:01_

Regarding (1), we do link to the providers in https://docs.astral.sh/uv/configuration/indexes/#using-credential-providers — I'm hesitant to repeat them all there because they are relevant for other requests than those for indexes. We're interested in broadly improving the credential provider experience though, I expect the documentation to be expanded.

The rest sound like nice improvements to the error messages.

---

_Label `error messages` added by @zanieb on 2025-03-18 16:01_

---

_Comment by @jtfmumm on 2025-03-18 16:17_

Would you mind sharing more details about your environment (including uv version) and what you ran using `authenticate = "always"`? With the latest uv, I would have expected you to see different errors if there are missing vs. incorrect credentials. 

With the following `pyproject.toml`:
```
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[[tool.uv.index]]
name = "example-index"
url = "https://pypi-proxy.fly.dev/basic-auth/simple"
authenticate = "always"
default = true
```

if I run `uv add anyio` with uv 0.6.7, it produces the following error for me on both macOS and ubuntu:

```
error: Failed to fetch: `https://pypi.org/simple/anyio/`
  Caused by: Missing credentials for https://pypi.org/simple/anyio/
```

Whereas running with incorrect credentials produces the `lack of valid authentication credentials` error.

---

_Comment by @LSerranoPEReN on 2025-03-19 09:22_

@zanieb & @jtfmumm thank you for taking the time to look at my issue.

I think a bit more context of my use case will be useful.

My team and I are using GitLab package registries to distribute our internal packages.
If you don't already know, a GitLab package registry can hold many python packages. If the repository is private then the package registry can be accessed with a personal (or project) assess token. This token should be provided as the password for authentication and in this case the username is completely ignored by GitLab (but is by convention usually set to `__token__` in the documentation).

For instance GitLab provides the following example to use package registry with `pip`:
```
pip install <package_name> --index-url https://__token__:<your_personal_token>@<gitlab_domain>/api/v4/projects/<project_id>/packages/pypi/simple
```

At the moment, we are using `poetry` as a package and project manager, but I'm looking for alternatives. In `poetry`, credentials are managed using `poetry` own subcommand and are stored using python `keyring` (or plaintext if no keyring is available). My understanding that `uv` does not manage credentials for you, and [that this feature is not in the work pipeline at the moment](https://github.com/astral-sh/uv/pull/9920#issuecomment-2715144498).

Still, `uv` can retrieve credentials from `keyring` using the `--keyring-provider subprocess`, which call the following command `keyring get <service_name> <username>` internally, which is great. So as a user I can simply `keyring set <service_name> <username>` to store my credentials, and everything should work.
But as a user, I have no idea what to set `<service_name>` to. By comparison, it's not a problem in `poetry` because since it can manage credentials for you, it sets its own `<service_name>`.

Using enough verbosity when calling `uv` I can see that if `"https://pypi-proxy.fly.dev/basic-auth/simple"` is configured as the index URL, the following services names are tried:
- `https://pypi-proxy.fly.dev/basic-auth/simple/<package_name>/`
- `pypi-proxy.fly.dev`

I don't think I saw this behavior in the documentation, bringing point (1).  
(Weirdly `https://pypi-proxy.fly.dev/basic-auth/simple/` is not tested as a service name)


Concerning @jtfmumm remark, I can indeed reproduce this behavior.
I had made the mistake of providing a username in the URL `https://__token__@pypi-proxy.fly.dev/basic-auth/simple` to match my GitLab use case. In this situation, if no credentials are found with `keyring get`, I think `uv` still tries to connect using only a username and no password which brings the `401 Unauthorized` error.

So I guess it's technically not a bug, but it is still frustrating to not be able to distinguish between credentials being invalid and credentials not being found. Keep in mind that, for `uv` to use `keyring` you need to provide a `username`, otherwise you get a `Skipping keyring lookup for https://pypi-proxy.fly.dev/basic-auth/simple/anyio/ with no username`.

_Edit:_ all experiments were done `uv 0.6.7+1 (e0f81f0d4 2025-03-17)` on Debian 12 - amd64

---

_Comment by @zanieb on 2025-03-20 22:06_

We added keyring lookups for usernames in #12316 

I think your other note is a case of https://github.com/astral-sh/uv/issues/4056

---

_Comment by @LSerranoPEReN on 2025-03-24 12:51_

Hi, thank you a lot for your fixes.

I think #12316 fixes @jtfmumm case, but it seems not to work with `explicit = true`.

```ini
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[[tool.uv.index]]
name = "example-index"
url = "https://pypi-proxy.fly.dev/basic-auth/simple"
authenticate = "always"
explicit = true

[tool.uv.sources]
anyio = { index = "example-index" }
```

The following command fails with `401 Unauthorized`
```
uv add anyio --keyring-provider subprocess -vvv
```

In this case, it seems that `authenticate = "always"` is not verified. Here is a relevant log excerpt:
```
TRACE Request for https://pypi-proxy.fly.dev/basic-auth/simple/anyio/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://pypi-proxy.fly.dev
DEBUG No netrc file found
DEBUG Skipping keyring fetch for https://pypi-proxy.fly.dev/basic-auth/simple/anyio/ without username; use `authenticate = always` to force
```


This was done with `uv 0.6.9+31 (7c57cefaa 2025-03-24)`

---

_Comment by @TheFriendlyCoder on 2025-03-31 19:01_

NOTE: keyring support on MacOS looks to be broken right now. Details are [here](https://github.com/astral-sh/uv/issues/12591). As I mentioned in my bug report, it would be helpful to include some debugging steps in the `uv` users guides explaining to people how to ensure `keyring` is installed and set up properly to support `uv` to save people time trying to debug it by hand.

---

_Comment by @jtfmumm on 2025-04-04 09:30_

@LSerranoPEReN Some updates:
* The issue you noticed regarding `explicit = true` should be addressed by #12631. 
* #12651 will ensure that uv retrieves credentials for the index URL rather than the package URL. 
* #12667 updates the error messages for indexes not configured as `authenticate = "always"`.

---

_Comment by @kmkolasinski on 2025-04-07 13:49_

Hi, I think I have a similar problem and I cannot figure out how to make uv work for may case when I have some private repositories stored in the google artifact registry. I check two commands. This one works:
```bash
GOOGLE_APPLICATION_CREDENTIALS=${HOME}/.config/gcloud/application_default_credentials.json UV_EXTRA_INDEX_URL=https://us-east4-python.pkg.dev/path/path/simple/  pip install private-lib --no-cache-dir
```
but this one fails, I just added `uv`:
```bash
GOOGLE_APPLICATION_CREDENTIALS=${HOME}/.config/gcloud/application_default_credentials.json UV_EXTRA_INDEX_URL=https://lel-python.pkg.dev/path/path/simple/ uv pip install private-lib --no-cache-dir
```
with error
```
Using Python 3.11.9 environment:
  × No solution found when resolving dependencies:
  ╰─▶ Because private-lib was not found in the package registry and you require private-lib, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://lel-python.pkg.dev/path/path/simple/) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
```

Interestingly, in our company we have also secondary index which is running on jfrog artifactory and the same approach works for both uv and pip. 
What is the reason for this and how can I fix it ? 



---

_Comment by @jtfmumm on 2025-04-07 14:14_

> Hi, I think I have a similar problem and I cannot figure out how to make uv work for may case when I have some private repositories stored in the google artifact registry. 

Hi @kmkolasinski. Would you mind opening a separate issue for this? At first glance, I assume your pip command won't use `UV_EXTRA_INDEX_URL` for anything (but maybe you've passed it in as `--extra-index-url` when you tested?). And you've provided a different URL for that env var in the pip and uv scenarios. 



---

_Comment by @kmkolasinski on 2025-04-07 14:20_

hey, yes I changed the commands arguments I will create an issue for this if you prefere and correct the commands as indeed this can be confusing.

---

_Comment by @kmkolasinski on 2025-04-07 14:32_

@jtfmumm  here is the ticket I created https://github.com/astral-sh/uv/issues/12716 
thank you for the help !

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-11 07:51_

---

_Comment by @LSerranoPEReN on 2025-09-30 16:16_

Thank you a lot for all these great changes.

I believe that all the points raised in this issue have been resolved through your commits, and that the recent introduction of `uv auth` greatly simplifies the configuration of credentials without the need to document how to use the python keyring cli.

I consider this issue to be fully resolved and am therefore closing it.

---

_Closed by @LSerranoPEReN on 2025-09-30 16:16_

---
