---
number: 8728
title: "When tool.uv.index.url contains a username, it can't be overridden in UV_INDEX"
type: issue
state: closed
author: torarvid
labels:
  - question
assignees: []
created_at: 2024-10-31T15:29:13Z
updated_at: 2024-10-31T20:08:24Z
url: https://github.com/astral-sh/uv/issues/8728
synced_at: 2026-01-07T13:12:18-06:00
---

# When tool.uv.index.url contains a username, it can't be overridden in UV_INDEX

---

_Issue opened by @torarvid on 2024-10-31 15:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hello 👋🏻 

`uv 0.4.28`

We have an issue where our pyproject.toml has the equivalent of:

```toml
[project]
name = "foo"
version = "0.0.1"
description = "desc"
authors = [{ name = "Me", email = "me@example.com" }]
license = { text = "UNLICENSED" }
requires-python = ">=3.12.6, <3.13"
dependencies = [
    "priv_package==0.3.8",
]

[tool.uv]
dev-dependencies = [
    "mypy>=1.12.0,<1.13.0",
]
package = true

[tool.uv.sources]
priv_package = { index = "priv" }

[[tool.uv.index]]
name = "priv"
url = "https://oauth2accesstoken@europe-python.pkg.dev/foo/python/simple"
explicit = true
```
So a private package on GCP artifact registry and a public package (the public package might not matter for the bug).

By having the `oauth2accesstoken` as a username in the private package url, all human users in the company are able to export `UV_KEYRING_PROVIDER=subprocess` and use `keyring` (with `keyring-gcloud`) to hav `uv` fetch-and-refresh their gcloud access tokens automatically use it for authentication. Perfect so far 👍🏼

However, in Github we use [Renovate](https://www.mend.io/renovate/) to bump our dependencies, and for that to work, we have this in our renovate config:

```json5
{
  description: "Preset to fetch deps from Google Artifact Registry",
  hostRules: [
    {
      description: "Authenticate to GCP Artifact Registry",
      matchHost: "europe-python.pkg.dev",
      username: "_json_key_base64",
      password: "{{ secrets.OUR_ARTIFACT_REGISTRY_JSON_KEY }}",
    }
}
```

So Renovate needs a long-lived secret for authentication to be practical, but the company prefers that employees use a less-important secret (a google access token in our case).

This used to work with Renovate+uv a while ago, but it stopped working some time in the past 5–7 days. I'm not really sure how the public Renovate bot chooses the version of `uv` that they run, so I can't say on what version this started failing for us.

I can repro locally by ensuring the only `UV_<...>` env var is `UV_INDEX=https://_json_key_base64:$PASSWORD@europe-python.pkg.dev/foo/python/simple`, and then do `rm -rf .venv && uv sync --no-cache` (not sure if the `--no-cache` is needed). I can also work around the issue by changing `oauth2accesstoken` to `_json_key_base64` in pyproject.toml.

I'm not sure what I want the solution to be — not even sure this is a bug (maybe I can improve my configuration to avoid this being a problem?). But perhaps it could be an idea that if I have a url in pyproject.toml of the `https://alice@domain.tld` format and there is a `UV_INDEX=https://bob:pwd@domain.tld`, then the `bob` user should "step in" for `alice` since env vars usually are prioritized over config files (?)

Happy to hear your thoughts. Thanks for an otherwise excellent tool!

---

_Comment by @charliermarsh on 2024-10-31 17:08_

Does anything change if you instead do: `UV_INDEX=priv=https://_json_key_base64:$PASSWORD@europe-python.pkg.dev/foo/python/simple`? I.e., add `priv=` before the URL

---

_Label `question` added by @charliermarsh on 2024-10-31 17:08_

---

_Comment by @torarvid on 2024-10-31 19:47_

@charliermarsh Just tried now, ~but sadly, it does not change anything.~

☝🏻 Whoops, it turns out I confused myself when trying this out, since I actually exported `UV_INDEX_PRIV_PACKAGE_USERNAME` and `_PASSWORD` (so I fell into the danger of substituting my "real" index+package names with "priv" and "priv-package").

So when trying it again, this tip **also** works.

---

_Comment by @charliermarsh on 2024-10-31 19:50_

Oh sorry -- what about this?

```
export UV_INDEX_PRIV_USERNAME= _json_key_base64
export UV_INDEX_PRIV_PASSWORD={{ PASSWORD }}
```

---

_Comment by @torarvid on 2024-10-31 20:03_

Yes! Thank you, that worked 🎉 

Ok, so I think that means that our easiest path forward is to **not** put the `oauth2accesstoken@` prefix into our pyproject.toml (that would make Renovate start working again, I now presume), and then I guess employees would just export `UV_INDEX_PRIV_USERNAME=oauth2accesstoken`.

Thanks, feel free to close this issue if that feels right to you 👍🏼 

(PS: I updated my previous comment since your previous tip also did work on my second try 🙈)

---

_Comment by @charliermarsh on 2024-10-31 20:08_

Oh perfect, ok! I'm gonna close out then. Glad I could help.

---

_Closed by @charliermarsh on 2024-10-31 20:08_

---
