```yaml
number: 7860
title: "FR: Ability to publish to Azure Artifacts"
type: issue
state: closed
author: FauxPrada
labels:
  - question
assignees: []
created_at: 2024-10-02T08:21:53Z
updated_at: 2024-11-14T15:51:02Z
url: https://github.com/astral-sh/uv/issues/7860
synced_at: 2026-01-12T15:59:17Z
```

# FR: Ability to publish to Azure Artifacts

---

_@FauxPrada_

Hello,

My apologies if this is already a feature, and I'm misunderstanding how to publish.

The ability to publish a wheel package to Azure artifacts would be greatly welcomed. I hate using GitHub Issue's to ask for help so I'm potentially writing this under the guise of a Feature Request.

At the moment `uv publish` can attempt to publish to Artifacts:

**Using:**
`uv publish --publish-url https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/ --verbose --username VssSessionToken`
**Gets us:**
```
DEBUG uv 0.4.17
warning: `uv publish` is experimental and may change without warning
Publishing 4 files https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/
DEBUG Using request timeout of 30s
Uploading package-2.1.0-py3-none-any.whl (27.2KiB)
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/
[Information] [CredentialProvider]VstsCredentialProvider - Acquired bearer token using 'MSAL Silent'
[Information] [CredentialProvider]VstsCredentialProvider - Attempting to exchange the bearer token for an Azure DevOps session token.
DEBUG Found credentials in keyring for https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/
DEBUG Response code for https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/: 405 Method Not Allowed
DEBUG Upload error response: {"count":1,"value":{"Message":"The requested resource does not support http method 'POST'."}}
error: Failed to publish `dist\package-2.1.0-py3-none-any.whl` to https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/
  Caused by: Upload failed with status code 405 Method Not Allowed: {"count":1,"value":{"Message":"The requested resource does not support http method 'POST'."}}
```

**While alternativly:**
`uv publish --publish-url https://VssSessionToken@pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/ --verbose`
**Gets us**
```
DEBUG uv 0.4.17
warning: 'uv publish' is experimental and may change without warning
Publishing 4 files https://VssSessionToken@pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/
DEBUG Using request timeout of 30s
Uploading package-2.1.0-py3-none-any.whl (27.2KiB)
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/
DEBUG Found credentials in keyring for https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/
DEBUG Response code for https://VssSessionToken@pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/: 405 Method Not Allowed
error: Failed to publish 'dist\package-2.1.0-py3-none-any.whl' to https://VssSessionToken@pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/
  Caused by: The request was redirected, but redirects are not allowed when publishing, please use the canonical URL: 'https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/'
```

This is with using `pyproject.toml` to configure "keyring-provider" and "extra-index-url" under `[tool.uv]` which appears to be working nicely.

```
[tool.uv]
keyring-provider = "subprocess"
extra-index-url = ["https://VssSessionToken@pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/simple/"]
```

Alternativly, if anyone has any ideas on how to bodge my config into letting me upload a package to Artifacts, that would be very welcomed!

If this is just user-error then I'll pretend this Issue was _actually_ a Documentation idiotification Feature Request :)

Thanks!

---

_Label `question` added by @zanieb on 2024-10-02 14:40_

---

_Assigned to @konstin by @zanieb on 2024-10-02 14:40_

---

_Comment by @konstin on 2024-10-02 15:48_

Hi! the `/simple/` url is the index url, the upload url ends with `/upload/` instead. Could you try again with that other url?

---

_Comment by @FauxPrada on 2024-10-03 07:29_

> Hi! the `/simple/` url is the index url, the upload url ends with `/upload/` instead. Could you try again with that other url?

OMG you're RIGHT 👍👍👍

That's on me for blindly copying the Twine setup link from the Azure devops docs page - without checking it was correct. Bless u

It works like a charm now

```
uv publish --publish-url https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/upload --verbose --username VssSessionToken
```

```
DEBUG uv 0.4.18
warning: `uv publish` is experimental and may change without warning
Publishing 4 files https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/upload
DEBUG Using request timeout of 30s
Uploading package-2.1.0-py3-none-any.whl (27.2KiB)
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/upload
[Information] [CredentialProvider]VstsCredentialProvider - Acquired bearer token using 'MSAL Silent'
[Information] [CredentialProvider]VstsCredentialProvider - Attempting to exchange the bearer token for an Azure DevOps session token.
DEBUG Found credentials in keyring for https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/upload
DEBUG Response code for https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/upload: 409 Conflict
DEBUG Upload error response: {"$id":"1","innerException":null,"message":"The feed 'Feed' already contains file 'package-2.1.0-py3-none-any.whl' in package 'package 2.1.0'.","typeName":"Microsoft.VisualStudio.Services.Packaging.Shared.WebApi.Exceptions.PackageAlreadyExistsException, Microsoft.VisualStudio.Services.Packaging.Shared.WebApi","typeKey":"PackageAlreadyExistsException","errorCode":0,"eventId":3000}
INFO Upload succeeded
File already exists, skipping
Uploading package-2.1.0.tar.gz (27.1KiB)
DEBUG Response code for https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/upload: 200 OK
INFO Upload succeeded
Uploading package-2.1.1-py3-none-any.whl (34.2KiB)
DEBUG Response code for https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/upload: 200 OK
INFO Upload succeeded
Uploading package-2.1.1.tar.gz (36.3KiB)
DEBUG Response code for https://pkgs.dev.azure.com/Company/Package/_packaging/Feed/pypi/upload: 200 OK
INFO Upload succeeded
```

You're a gem, good PR too it'll stop people like me coming here in future lol. I'll let that pull close the issue. Cheers

---

_Closed by @zanieb on 2024-10-08 19:16_

---

_Comment by @benedikt-mue on 2024-11-14 15:50_

I ended up using 
```
uv publish --publish-url https://pkgs.dev.azure.com/{organisation}/{project}/_packaging/{feedName}/pypi/upload/ --token PAT
``` 
and avoiding keyring as it gave me a nasty error.

---
