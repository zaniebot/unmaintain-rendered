---
number: 14264
title: Issue with Authentication in uv pip compile Command Using Private Repository
type: issue
state: open
author: jmwachtel
labels:
  - question
assignees: []
created_at: 2025-06-25T18:00:53Z
updated_at: 2025-07-02T13:39:30Z
url: https://github.com/astral-sh/uv/issues/14264
synced_at: 2026-01-07T13:12:18-06:00
---

# Issue with Authentication in uv pip compile Command Using Private Repository

---

_Issue opened by @jmwachtel on 2025-06-25 18:00_

### Summary

When using the `uv pip compile` command with `authenticate = always`, on a private Artifactory repository, the initial request sent is unauthenticated, resulting in a 401 Unauthorized error. This behavior is contrary to expectations where I believed all requests should be authenticated upfront due to the authentication policy. Our credentials are stored in a .netrc file. Am I misunderstanding the documentation around this flag?

**Steps to Reproduce:**

1. Ensure uv is set up with the latest version, configured for the target environment and Python version.
1. Set authentication settings to authenticate = always in the uv configuration.
2. Store username/password in a .netrc file.
1. Use the --no-cache flag with the uv pip compile command:
  ```Bash
  uv pip compile requirements.in --no-cache --override=<OVERRIDE_FILE> --config-file <UV_TOML> --allow-insecure-host=<YOUR_PRIVATE_ARTIFACTORY> --index-strategy=unsafe-any-match --emit-index-url --output-file=requirements.txt -vv
  ```
4. Attempt to compile requirements using a private Artifactory that requires authentication.
5. Review the logs to observe unauthorized requests with a 401 status preceding credential retries.

**uv.toml**
```toml
[[index]]
name = "artifactory"
url = "https://REDACTED/artifactory/api/pypi/pypi/simple"
authenticate = "always"
default= true
```

**Expected Result:**
All pip compile-associated requests should utilize proper authentication based on the specified policy to avoid 401 errors.

**Actual Result:**
Initial requests are executed without authentication, leading to 401 responses and require subsequent retries with credentials.

**Trace Logs:**
```
TRACE Request https://REDACTED/artifactory/api/pypi/pypi/simple/urllib3/ does not have a fresh cache because it has a 'no-cache' cache-control directive
DEBUG Found stale response for: https://REDACTED/artifactory/api/pypi/pypi/simple/urllib3/
DEBUG Sending revalidation request for: https://REDACTED/artifactory/api/pypi/pypi/simple/urllib3/
TRACE Handling request for https://REDACTED/artifactory/api/pypi/pypi/simple/urllib3/ with authentication policy always
TRACE Request for https://REDACTED/artifactory/api/pypi/pypi/simple/urllib3/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://REDACTED/artifactory/api/pypi/pypi/simple/urllib3/
TRACE Checking for credentials for https://REDACTED/artifactory/api/pypi/pypi/simple/urllib3/
TRACE No credentials in cache for URL https://REDACTED/artifactory/api/pypi/pypi/simple
TRACE Found cached credentials for realm https://REDACTED
TRACE Retrying request for https://REDACTED/artifactory/api/pypi/pypi/simple/urllib3/ with credentials from cache Basic { username: Username(Some("REDACTED")), password: Some(****) }
```

**nginx Logs**
```
2025-06-25 12:50:09.455 | Method: GET \| Status: 200 \| Request Time: 0.153s \| Endpoint: /artifactory/api/pypi/pypi/simple/urllib3/ \| User: REDACTED \| |  
-- | -- | --
2025-06-25 12:50:08.056 | Method: GET \| Status: 401 \| Request Time: 0.002s \| Endpoint: /artifactory/api/pypi/pypi/packages/packages/a7/c2/fe1e52489ae3122415c51f387e221dd0773709bad6c6cdaa599e8a2c5185/urllib3-2.5.0-py3-none-any.whl \| User: - \|
```

I have also seen similar behavior with HEAD requests.

### Platform

Linux 5.15.0-142-generic x86_64 GNU/Linux

### Version

uv 0.7.14

### Python version

Python 3.10

---

_Label `bug` added by @jmwachtel on 2025-06-25 18:00_

---

_Comment by @jtfmumm on 2025-06-25 18:45_

I am unable to replicate this with a similar setup and commands. It finds the credentials in `.netrc`, authenticates with Artifactory, and successfully compiles requirements.

Have you verified that your credentials are still valid? Do they work with pip or another package manager?

---

_Assigned to @jtfmumm by @jtfmumm on 2025-06-25 18:45_

---

_Label `needs-mre` added by @jtfmumm on 2025-06-25 18:45_

---

_Comment by @jmwachtel on 2025-06-25 19:07_

Hi @jtfmumm. It does find the credentials as you cam see by the last trace and the nginx logs show the next request is a 200 with the credentials. I added my `uv.toml` config to the description as well as the `--index-strategy=unsafe-any-match` flag. In the end everything works, I am confused why an unauthenticated request is ever sent given the authenticate policy I set.

---

_Comment by @jtfmumm on 2025-06-25 19:26_

Ah, I see. I was using the index configured as `authenticate = always` in `pyproject.toml`. But your command is specifying the URL in `--index-url`. When you do it that way, it's prioritizing that version of the URL, which is not treated as authenticate always.

Can you try removing `--index-url` from your command to verify that you no longer get the 401s? 

---

_Comment by @jmwachtel on 2025-06-25 20:34_

I did try that and confirmed again, even with that removed, I see the same behavior. As stated above, I also see it in HEAD requests like this:

```
2025-06-25 15:29:55.669 | Method: HEAD \| Status: 200 \| Request Time: 0.080s \| Endpoint: /artifactory/pypi/deeplake/3.9.31/deeplake-3.9.31-py3-none-any.whl \| User: REDACTED \| |
2025-06-25 15:29:55.588 | Method: HEAD \| Status: 401 \| Request Time: 0.001s \| Endpoint: /artifactory/pypi/deeplake/3.9.31/deeplake-3.9.31-py3-none-any.whl \| User: - \|
```

---

_Comment by @jtfmumm on 2025-06-25 21:05_

Interesting, I'm not able to replicate that. To be clear, here is what I'm seeing.

#### With `--index-url` 

```
TRACE Handling request for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/ with authentication policy auto
TRACE Request for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/
TRACE Attempting unauthenticated request for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/
TRACE Request for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/ failed with 401 Unauthorized, checking for credentials
```

* The logs indicate it's using "authentication policy auto".
* The logs say "Attempting unauthenticated request"
* It fails with a 401 ("failed with 401 Unauthorized") and then tries again.

#### Without `--index-url` (using index defined in `pyproject.toml`)

```
TRACE Handling request for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/ with authentication policy always
TRACE Request for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/
TRACE Checking for credentials for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/
TRACE No credentials in cache for URL https://<DOMAIN>/artifactory/api/pypi/<..>/simple
TRACE No credentials in cache for realm https://<DOMAIN>
DEBUG Checking netrc for credentials for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/
DEBUG Found credentials in netrc file for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/
TRACE Retrying request for https://<DOMAIN>/artifactory/api/pypi/<...>/simple/<PKG>/ with Basic { username: Username(Some(<...>)), password: Some(****) }
```

* The logs indicate it's using "authentication policy always".
* The logs never say "Attempting unauthenticated request". 
  * "Request for \<URL\> is unauthenticated" just means that uv doesn't initially find credentials. But it does find them before sending the request, which should be guaranteed by `authenticate = always`.
* It doesn't encounter any 401s

The logs you provide in the initial description look like this case, but you say you're noticing 401s. Do you see "failed with 401" or "Attempting unauthenticated request" in the logs when you run without `--index-url`? Or is it only in the nginx logs?

---

_Comment by @jmwachtel on 2025-06-25 21:53_

Upon investigating the authentication issue further, we believe we have identified the root cause. When using the `--output-file=requirements.txt` and `--emit-index-url` options, the `--index-url` becomes embedded within the requirements.txt file. This URL is subsequently read by UV during processing. After removing the --index-url from the requirements.txt file, the 401 Unauthorized errors ceased to occur.

Initially, these details appeared insignificant, but they evidently influence authentication behavior. Is there a strategy or configuration within UV to bypass or ignore the --index-url when processing the requirements.txt file?

---

_Comment by @jtfmumm on 2025-06-26 08:13_

Thank you for investigating further and updating the command in the issue description. I don't believe there is any way for uv to ignore `--index-url` if included in a `requirements.txt` it's using.

---

_Label `bug` removed by @jtfmumm on 2025-06-26 08:14_

---

_Label `needs-mre` removed by @jtfmumm on 2025-06-26 08:14_

---

_Label `question` added by @jtfmumm on 2025-06-26 08:14_

---

_Comment by @jmwachtel on 2025-06-26 15:45_

Thank you for the clarification. I’m glad we could pinpoint the issue together. We can proceed with removing the `--index-url` from the `requirements.txt` to address the problem.

I recommend updating the documentation to include this crucial detail regarding `authenticate = always`. It’s currently unclear that specifying an `--index-url` on the command line can override settings in the `uv.toml` file, and even less clear that having it within the `requirements.txt` would do the same. Ideally, any index stipulated in the toml should adhere to the configurations set therein, irrespective of where else the index might be specified.

---

_Comment by @jmwachtel on 2025-07-02 13:39_

We have encountered similar issues with pinned packages where the Artifactory URL does not align with the PyPI simple API. To resolve this, there is a need to authenticate requests consistently. It would be beneficial to specify a URL that mandates authentication (such as a base url, e.g. https://artifactory.company.com/artifactory) or introduce a command-line parameter that globally sets the authentication method to “always.” This would ensure smooth access as we exclusively utilize a private index.

---
