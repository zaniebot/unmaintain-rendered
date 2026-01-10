```yaml
number: 9778
title: Unable to publish package to Google Artifact Registry
type: issue
state: closed
author: joaodaher
labels:
  - bug
assignees: []
created_at: 2024-12-10T15:59:19Z
updated_at: 2025-01-20T09:02:55Z
url: https://github.com/astral-sh/uv/issues/9778
synced_at: 2026-01-10T04:27:58Z
```

# Unable to publish package to Google Artifact Registry

---

_Issue opened by @joaodaher on 2024-12-10 15:59_

We're currently migrating from Poetry to uv.
We use Google Artifact Registry as our private PyPI server.

I was able to **download** private packages using `uv` with the `pyproject.toml` config:
```
[[tool.uv.index]]
name = "private-pypi"
url = "https://oauth2accesstoken@us-east1-python.pkg.dev/nilo-prd/python-nilo/simple/"

[tool.uv.sources]
our-private-lib = { index = "private-pypi" }
```

But I **cannot publish/upload** any private package.

I'm using:
`uv publish --keyring-provider subprocess --username __token__ --publish-url https://us-east1-python.pkg.dev/nilo-prd/python-libs/`

And I get the output (when verbose) followed by a stuck execution:
```
DEBUG Checking keyring for credentials for __token__@https://us-east1-python.pkg.dev/nilo-prd/python-libs/
DEBUG Found credentials in keyring for https://us-east1-python.pkg.dev/nilo-prd/python-libs/
DEBUG Response code for https://us-east1-python.pkg.dev/nilo-prd/python-libs/: 401 Unauthorized
```

With **twine** I can execute:
```
twine upload --repository-url https://us-east1-python.pkg.dev/nilo-dev/python-libs/ dist/*
```
And successfully publish the package. So the keyring seems to be working fine.

Any idea of what I'm missing here?

---

_Comment by @thejosephstevens on 2024-12-20 05:43_

Unfortunately I'm in the same boat, but I'd like to see a solution here (super excited to see #7839 !) . I've tried both of the documented solutions [here](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#google-artifact-registry) and have not been able to get either of them to work. The same twine upload command works just fine for us (that's what we've been using with `rye`). I have never been able to get anything other than a 401 or 404 with `uv publish`. What I've run into:
* If I set keyring provider to subprocess I get a 404 and the command never returns (same as above)
* If I have `oauth2accesstoken@` in the publish url I get a failure due to redirect on publishing. The error instructs me to use the canonical url
* If I pass `oauth2accesstoken` and the token in basic auth in the publish url, I get prompted for username/password
* If I use the canonical url (with `/simple`) and I explicitly pass in the token, I get a 404 and the command never returns.

---

_Label `bug` added by @zanieb on 2025-01-07 19:14_

---

_Comment by @zanieb on 2025-01-07 19:14_

cc @konstin 

---

_Comment by @mmisiewicz-yext on 2025-01-07 22:19_

I hit the same issue attempting to publish to a Google Artifact Repository in a Github Action. I was able to work around by running:

```
          ARTIFACT_REGISTRY_TOKEN=$(gcloud auth application-default print-access-token)
          uv publish --index my-registry-here -u oauth2accesstoken -p $ARTIFACT_REGISTRY_TOKEN dist/*

```

in my GitHub action. 

---

_Comment by @konstin on 2025-01-10 12:48_

There's two interleaving things: The google cloud artifact registry uses `oauth2accesstoken` as username, not `__token__` as pypi does, so you need set `--username oauth2accesstoken`. We also read the username from the URL if you want to set it there, but there was a bug in uv due to how redirects with POST requests are handled, which should be fixed with https://github.com/astral-sh/uv/pull/10469. With that PR, you should be able to specify `oauth2accesstoken` as username both in the CLI and the URL and publish with e.g.

```
uv publish --index gcp-index --username oauth2accesstoken --keyring-provider subprocess
```

where the index is defined as:

```toml
[tool.uv.index]
name = "gcp-index"
index-url = "https://<REGION>-python.pkg.dev/<PROJECT>/<REPOSITORY>/simple/"
publish-url = "https://<REGION>-python.pkg.dev/<PROJECT>/<REPOSITORY>/"
```

---

_Comment by @tlienart on 2025-01-17 22:36_

I wasted a bit of time on this so in case it helps someone @konstin's answer should be adjusted for 

```
uv publish --index $INDEX_NAME --username oauth2accesstoken --password $ACCESS_TOKEN --keyring-provider subprocess
```

where ACCESS_TOKEN locally can be `$(gcloud auth application-default print-access-token)` as in @mmisiewicz-yext 's answer or, on github action, using `google-github-actions/auth@v2` with token format access token.

and in the pyproject.toml

```
[[tool.uv.index]]
name = "<INDEX_NAME>"
url = "https://<REGION>-python.pkg.dev/<PROJECT>/<REPOSITORY>/simple/"
publish-url = "https://<REGION>-python.pkg.dev/<PROJECT>/<REPOSITORY>/"
```

at least that's what I needed to get it to work

---

_Comment by @konstin on 2025-01-20 09:02_

#10469 is released, if there are still problems with the google artifact registry, don't hesitate to open a new issue.

---

_Closed by @konstin on 2025-01-20 09:02_

---
