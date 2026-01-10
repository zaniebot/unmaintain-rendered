```yaml
number: 11097
title: uv fails on HTTP redirects with authentication
type: issue
state: closed
author: bra-fsn
labels:
  - bug
  - network
assignees: []
created_at: 2025-01-30T14:30:59Z
updated_at: 2025-06-30T10:05:33Z
url: https://github.com/astral-sh/uv/issues/11097
synced_at: 2026-01-10T03:32:45Z
```

# uv fails on HTTP redirects with authentication

---

_Issue opened by @bra-fsn on 2025-01-30 14:30_

### Summary

I'm trying to install a package from a local repo via a redirector proxy, which authenticates to AWS Code Artifact (a local Python repository, which can only be used with authentication).
Here's how the proxy works (only the relevant parts):
1. if it gets a `GET` request with the `{path}` ending in `/`, it does a `HEAD` to https://pypi.org/simple/{path}
1.1. if pypi returns 2xx, the proxy redirects uv to https://pypi.org/simple/{path}, so uv will get the package from pypi
1.2. otherwise it redirects uv to https://aws:{TOKEN}@{DOMAIN}-{ACCOUNT_ID}.d.codeartifact.{REGION}.amazonaws.com/pypi/python/simple/{path}, so uv should get the package from Code Artifact

uv is instructed to use the proxy with `--extra-index-url {proxy_url`.

**Installing local packages through this proxy and pip works fine, but fails with uv.**
Installing local packages with the authenticated Code Artifact URL ( https://aws:{TOKEN}@{DOMAIN}-{ACCOUNT_ID}.d.codeartifact.{REGION}.amazonaws.com/pypi/python/simple/) in `--extra-index-url` with uv works.

So it seems when I return a HTTP 302 redirect to an authenticated URL, uv fails to fetch that. I've tried http:// and https:// as the proxy URL (the redirect URL is always https), both fail, so it doesn't seem to be a "https redirect is not followed from a http site" issue.
The domains are different though, but I can't see any errors related to that.

Running uv with `RUST_LOG=uv=trace uv pip install --extra-index-url http://{proxy_url}` gives:
```
DEBUG uv 0.5.25
DEBUG Searching for default Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.11.10, skipping probing: /datasci/bin/python3
DEBUG Found `cpython-3.11.10-linux-x86_64-gnu` at `/datasci/bin/python3` (active virtual environment)
Using Python 3.11.10 environment at: /datasci
TRACE Checking lock for `/datasci` at `/datasci/.lock`
DEBUG Acquired lock for `/datasci`
DEBUG At least one requirement is not satisfied: {INTERNAL_PACKAGE_NAME}
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.10
DEBUG Solving with target Python version: >=3.11.10
TRACE assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: {INTERNAL_PACKAGE_NAME}*
TRACE Fetching metadata for {INTERNAL_PACKAGE_NAME} from http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/
TRACE assigned packages: root==0a0.dev0
TRACE Chose package for decision: {INTERNAL_PACKAGE_NAME}. remaining choices: 
TRACE cached request http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/ is storable because its response has a heuristically cacheable status code 200
TRACE could not determine freshness lifetime, assuming none exists
TRACE cached request http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/ has a cached response that does not allow staleness
TRACE request http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/ does not have a fresh cache because its age is 1742 seconds, it is greater than the freshness lifetime of 0 seconds and stale cached responses are not allowed
DEBUG Found stale response for: http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/
DEBUG Sending revalidation request for: http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/
TRACE Handling request for http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/
TRACE Request for http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/ is unauthenticated, checking cache
TRACE No credentials in cache for URL http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/
TRACE Attempting unauthenticated request for http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/
TRACE Request for http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm http://localhost
DEBUG No netrc file found
TRACE Attempting to retry error: Error { kind: WrappedReqwestError(Url { scheme: "http", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("localhost")), port: None, path: "/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://aws:{TOKEN}@{DOMAIN}-{AWS_ACCOUNT_ID}.d.codeartifact.{AWS_REGION}.amazonaws.com/pypi/python/simple/{INTERNAL_PACKAGE_NAME}/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Fetching metadata for {INTERNAL_PACKAGE_NAME} from https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/
TRACE No cache entry exists for /root/.cache/uv/simple-v15/pypi/{INTERNAL_PACKAGE_NAME}.rkyv
DEBUG No cache entry for: https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/
TRACE Sending fresh GET request for https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/
TRACE Handling request for https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/
TRACE Request for https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/
TRACE Attempting unauthenticated request for https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/
TRACE Request for https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm https://pypi.org
TRACE Attempting to retry error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple/{INTERNAL_PACKAGE_NAME}/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://pypi.org/simple/{INTERNAL_PACKAGE_NAME}/" }))) }
TRACE Cannot retry error: not an IO error
TRACE Received package metadata for: {INTERNAL_PACKAGE_NAME}
DEBUG Searching for a compatible version of {INTERNAL_PACKAGE_NAME} (*)
TRACE Selecting candidate for {INTERNAL_PACKAGE_NAME} with range * with 0 remote versions
DEBUG No compatible version found for: {INTERNAL_PACKAGE_NAME}
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on {INTERNAL_PACKAGE_NAME}*
  {INTERNAL_PACKAGE_NAME} not found in the package registry
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on {INTERNAL_PACKAGE_NAME}*
  {INTERNAL_PACKAGE_NAME} not found in the package registry
  × No solution found when resolving dependencies:
  ╰─▶ Because {INTERNAL_PACKAGE_NAME} was not found in the package registry and you require {INTERNAL_PACKAGE_NAME}, we can conclude that your requirements are unsatisfiable.
DEBUG Released lock at `/datasci/.lock`
```

Between `{}` the values are redacted and replaced with a sensible name.

The problem seems to be this line:
```
TRACE Attempting to retry error: Error { kind: WrappedReqwestError(Url { scheme: "http", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("localhost")), port: None, path: "/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/", query: None, fragment: None }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(404), url: "https://aws:{TOKEN}@{DOMAIN}-{AWS_ACCOUNT_ID}.d.codeartifact.{AWS_REGION}.amazonaws.com/pypi/python/simple/{INTERNAL_PACKAGE_NAME}/" }))) }
```
It says the URL https://aws:{TOKEN}@{DOMAIN}-{AWS_ACCOUNT_ID}.d.codeartifact.{AWS_REGION}.amazonaws.com/pypi/python/simple/{INTERNAL_PACKAGE_NAME}/ is 404, although if I do a `curl` to it, it loads fine.
As it's a 404 and not a 403, basic auth seems to be passed, but the URL might be mangled internally (it must not be the same as the printed one, as it exists).

If I return the package's page directly (proxying it, instead of a redirect to the backend URL), uv gets through it, then fails when trying to download packages (with redirects).
This actually logs an error and the 404 is not just in the TRACE logs:
```
error: Failed to fetch: `http://localhost/{AWS_ACCOUNT_ID}/{AWS_REGION}/{DOMAIN}/python/{INTERNAL_PACKAGE_NAME}/0.0.10/{INTERNAL_PACKAGE_NAME}-0.0.10-py3-none-any.whl#sha256=d30cafa1864a752939ec5ad6a56245ed8f78f3f87827e3aa78e09ddeb9023ca4`
  Caused by: HTTP status client error (404 Not Found) for url (https://aws:{TOKEN}@{DOMAIN}-{AWS_ACCOUNT_ID}.d.codeartifact.{AWS_REGION}.amazonaws.com/pypi/python/simple/{INTERNAL_PACKAGE_NAME}//0.0.10/{INTERNAL_PACKAGE_NAME}-0.0.10-py3-none-any.whl)
```
Again, if I try this URL with `curl`, it just loads fine.

### Platform

Ubuntu 22.04 amd64

### Version

uv 0.5.25

### Python version

Python 3.11.10

---

_Label `bug` added by @bra-fsn on 2025-01-30 14:30_

---

_Label `network` added by @zanieb on 2025-01-30 15:04_

---

_Comment by @zanieb on 2025-01-30 15:07_

Is there a way I could run this proxy locally as a demo?

You could try `RUST_LOG=trace` to get logs from the underlying network stack too, it'll be really verbose but it could be helpful.

---

_Comment by @bra-fsn on 2025-01-30 15:12_

> Is there a way I could run this proxy locally as a demo?
> 
> You could try `RUST_LOG=trace` to get logs from the underlying network stack too, it'll be really verbose but it could be helpful.

Sure!
https://gist.github.com/bra-fsn/9363bfb68c0694c545dc283a7e4e0042


---

_Comment by @bra-fsn on 2025-02-13 10:16_

Did you possibly have a chance to look at it?

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-15 07:58_

---

_Closed by @jtfmumm on 2025-04-18 12:56_

---

_Closed by @jtfmumm on 2025-04-18 12:56_

---

_Comment by @zanieb on 2025-05-01 18:28_

We've reverted the fix here, and will now track this in https://github.com/astral-sh/uv/issues/13255

---

_Comment by @jtfmumm on 2025-06-18 08:24_

@bra-fsn Thanks for testing this case! It looks like the redirect fix doesn't handle this because the credentials are encoded in the redirect request URL and our auth middleware currently assumes these have been moved to a header. I'm going to re-open this issue and will work on a fix for this separately from #13754.

---

_Reopened by @jtfmumm on 2025-06-18 08:24_

---

_Comment by @jtfmumm on 2025-06-20 08:32_

@bra-fsn Looking into this further, I'm not sure this is something uv should support. Returning URL-encoded credentials in the redirect location isn't standard and is a potential security risk. Is there a reason the proxy behaves that way?

Relatedly, have you considered replacing your proxy with [this approach](https://docs.aws.amazon.com/codeartifact/latest/ug/external-connection.html) recommended by AWS? Your current setup is vulnerable to dependency confusion attacks since you check PyPI before checking your private registry. 

---

_Comment by @bra-fsn on 2025-06-20 09:15_

> [@bra-fsn](https://github.com/bra-fsn) Looking into this further, I'm not sure this is something uv should support. Returning URL-encoded credentials in the redirect location isn't standard and is a potential security risk. Is there a reason the proxy behaves that way?
> 
> Relatedly, have you considered replacing your proxy with [this approach](https://docs.aws.amazon.com/codeartifact/latest/ug/external-connection.html) recommended by AWS? Your current setup is vulnerable to dependency confusion attacks since you check PyPI before checking your private registry.

We use a product, which creates build environments with the pip command embedded. Because AWS Code Artifact (CA from now on) supports only authenticated access, with a short lived token, we would have to embed this token into the build environment.
While this works, the problem is that once the build environment is created, it sticks to its settings, so we can't change pip's extra-index-url parameter and upon an environment rebuild, it will fail as the token has already expired.
There are more limitations, but this is the main one.

The proxy provides unauthenticated access to CA by dynamically getting a token for it and redirecting the client to the proper place, so we can use the same, static extra-index-url, without embedding the auth token.

The approach you linked adds a public repo to the CA, so it doesn't solve our problem. We're not using CA to proxy public repos, we use that for storing our internal packages, so the auth problem is still there.
BTW, using CA for proxying is much-much slower than going out to the internet and fetch the package, so that's why we prefer to fetch publicly available packages from pypi.

The real proxy prefers CA for the internal packages and goes out to the internet only if it couldn't find one.

pip is working just fine, but we prefer to use uv, so it would be great if you could support this. (and in fact, you already did it for a single release if I recall correctly :))

---

_Comment by @jtfmumm on 2025-06-20 10:03_

Thanks for the context, that's helpful. We'll need to discuss this case internally to decide if we want to support it.

---

_Label `needs-design` added by @jtfmumm on 2025-06-20 10:04_

---

_Comment by @bra-fsn on 2025-06-20 11:43_

> Thanks for the context, that's helpful. We'll need to discuss this case internally to decide if we want to support it.

Please do if you can. I admit that this might be considered as an edge case, but supporting it helps to achieve feature parity with pip, which is a good thing IMO.
Thanks for considering it!


---

_Label `needs-design` removed by @jtfmumm on 2025-06-27 13:56_

---

_Comment by @jtfmumm on 2025-06-27 14:01_

We decided it makes sense to support it. I've opened a PR

---

_Closed by @jtfmumm on 2025-06-27 14:41_

---

_Comment by @bra-fsn on 2025-06-30 10:05_

> We decided it makes sense to support it. I've opened a PR

I've just verified: 0.7.17 works! Thank you very much!

---
