---
number: 7660
title: "Allow command for `tool.uv.index-url` and `tool.uv.pip.index-url`"
type: issue
state: open
author: jpedrick
labels:
  - needs-design
  - needs-decision
assignees: []
created_at: 2024-09-24T14:34:28Z
updated_at: 2024-12-04T02:54:32Z
url: https://github.com/astral-sh/uv/issues/7660
synced_at: 2026-01-07T13:12:17-06:00
---

# Allow command for `tool.uv.index-url` and `tool.uv.pip.index-url`

---

_Issue opened by @jpedrick on 2024-09-24 14:34_

UV currently supports using a keyring for the credential provider. However, in my case it's not always practical. I would like to have a simple command line script: `get_pip_credentials.sh` that could invoke something like:

`scripts/get-pip-credentials.sh`
```
aws codeartifact login --tool pip --domain my_domain --repository python --dry-run --profile pip-read-access | cut -d " " -f 5
```

So, in my pyproject.toml:
```
[tool.uv]
index-url = { 'command' : [ './scripts/get-pip-credentials.sh' ] }

[tool.uv.pip]
index-url = { 'command' : [ './scripts/get-pip-credentials.sh' ] }
```

Likewise, if `--index-url` is specified on the command line, I would prefer that to override `tool.uv.index-url.command` and `tool.uv.pip.index-url.command`


---

_Comment by @zanieb on 2024-09-24 19:42_

How would this work with writing index URLs to the lockfile?

---

_Comment by @jpedrick on 2024-09-24 19:48_

> How would this work with writing index URLs to the lockfile?

Currently, when I put CodeArtifact credentials in the index-url it strips the credentials and just puts the repository address. I would expect the same behavior.

---

_Comment by @woutervh on 2024-09-24 21:37_

For many usecases, the private pypi-credentials  are set as environment variables.

Would there be a reason to not support variable substitution?

For example:
```
[tool.uv]
index-url = "https://__token__:${PERSONAL_ACCESS_TOKEN}@gitlab.com/..."
```

---

_Comment by @zanieb on 2024-09-24 21:40_

@woutervh that is tracked in https://github.com/astral-sh/uv/issues/5734

---

_Comment by @jpedrick on 2024-09-25 00:15_

@zanieb as I think about this more, it could be more general to have the configuration look like the following:

```
[tool.uv]
index-url = {
'url' = "https://aws:${ACCESS_TOKEN}@${DOMAIN}-${ACCOUNT_ID}.d.codeartifact.${REGION}.amazonaws.com/pypi/python/simple/"
'substitition_command' :  './scripts/get-codeartifact-url-with-credentials.sh'
}
```

`./scripts/get-codeartifact-url-with-credentials.sh` would return json like:
```
{
"ACCESS_TOKEN" : "ABCDEFG",
"REGION" : "eu-west-1",
"DOMAIN": "my_domain",
"ACCOUNT_ID" : "1234567890"
}
```

Ideally, the system call would use the location of the `pyproject.toml` as CWD, but absolute paths could be provided by users. However, I don't want to over specify. 

---

_Comment by @chrisrodrigue on 2024-10-02 00:44_

> @zanieb as I think about this more, it could be more general to have the configuration look like the following:
> 
> ```
> [tool.uv]
> index-url = {
> 'url' = "https://aws:${ACCESS_TOKEN}@${DOMAIN}-${ACCOUNT_ID}.d.codeartifact.${REGION}.amazonaws.com/pypi/python/simple/"
> 'substitition_command' :  './scripts/get-codeartifact-url-with-credentials.sh'
> }
> ```
> 
> `./scripts/get-codeartifact-url-with-credentials.sh` would return json like:
> 
> ```
> {
> "ACCESS_TOKEN" : "ABCDEFG",
> "REGION" : "eu-west-1",
> "DOMAIN": "my_domain",
> "ACCOUNT_ID" : "1234567890"
> }
> ```
> 
> Ideally, the system call would use the location of the `pyproject.toml` as CWD, but absolute paths could be provided by users. However, I don't want to over specify.

Could you just call that script and set the required environment variables prior to using `uv`, rather than specifying it in `pyproject.toml`?

---

_Comment by @jpedrick on 2024-10-02 18:26_


> Could you just call that script and set the required environment variables prior to using `uv`, rather than specifying it in `pyproject.toml`?

Sure, everything except the access token.

---

_Comment by @zanieb on 2024-10-02 18:38_

Why can't the access token be in the environment variable?

---

_Label `needs-design` added by @zanieb on 2024-10-02 18:38_

---

_Label `needs-decision` added by @zanieb on 2024-10-02 18:38_

---

_Comment by @jpedrick on 2024-10-02 22:42_

> Why can't the access token be in the environment variable?

In that case I can just put the entire url in the UV_INDEX_URL environment variable, but that doesn't allow dynamic keychain-like credentials 

---

_Comment by @zanieb on 2024-10-02 22:54_

I'm responding to

> > Could you just call that script and set the required environment variables prior to using uv, rather than specifying it in pyproject.toml?
>
> Sure, everything except the access token.

In which you wrap uv with a script that sets the relevant variable with authentication.

---

_Comment by @jpedrick on 2024-10-03 14:43_

 
> In which you wrap uv with a script that sets the relevant variable with authentication.

Hi @zanieb, perhaps we have gotten away from the central idea of the feature request. Currently, I do pre-set the index-url/extra-index-url. I'm basically requesting something like https://pypi.org/project/keyrings.codeartifact/, but without requiring all the setup for the keyring.

`credential_process` for the AWS cli config would be an example of the kind of solution I'm hoping for: https://docs.aws.amazon.com/sdkref/latest/guide/feature-process-credentials.html


---

_Comment by @dean-tate on 2024-12-04 02:54_

It'd be nice to be able to use keyring for AWS CodeArtifact. It'd make switching my team over to uv a much easier proposition. This approach for pixi using pipx is the approach that'd make sense - I'm trying to replicate this with uv now. 

https://ericmjl.github.io/blog/2024/9/19/how-to-set-up-pixi-with-codeartifacts/

---
