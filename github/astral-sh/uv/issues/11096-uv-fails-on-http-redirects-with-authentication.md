---
number: 11096
title: uv fails on HTTP redirects with authentication
type: issue
state: closed
author: bra-fsn
labels:
  - bug
assignees: []
created_at: 2025-01-30T14:30:23Z
updated_at: 2025-01-30T15:01:51Z
url: https://github.com/astral-sh/uv/issues/11096
synced_at: 2026-01-07T13:12:18-06:00
---

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

_Closed by @zanieb on 2025-01-30 14:56_

---

_Comment by @bra-fsn on 2025-01-30 14:58_

Sorry, github was dying during the submission

---

_Comment by @zanieb on 2025-01-30 15:01_

I understand haha

---
