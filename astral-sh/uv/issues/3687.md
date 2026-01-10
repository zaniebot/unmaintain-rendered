```yaml
number: 3687
title: Failed to download whl due to the missing of credentials in URL
type: issue
state: closed
author: iyanging
labels:
  - bug
assignees: []
created_at: 2024-05-21T08:02:45Z
updated_at: 2025-03-20T22:12:13Z
url: https://github.com/astral-sh/uv/issues/3687
synced_at: 2026-01-10T03:50:30Z
```

# Failed to download whl due to the missing of credentials in URL

---

_Issue opened by @iyanging on 2024-05-21 08:02_

cmd:
```
export UV_INDEX_URL=https://username:password@pypi.whatever.cn/whatever/pypi/+simple
uv pip install -U whatever
```

output:
```
error: Failed to download `whatever==2.1.1`
  Caused by: Failed to unzip wheel: whatever-2.1.1-py3-none-any.whl
  Caused by: Encountered an unexpected header (actual: 0x6f64213c, expected: 0x4034b50).
```

version:
```
uv 0.1.44
```

The private pypi server is `devpi`, and I am sure that the link `https://username:password@pypi.whatever.cn/teletraan/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22` is download-able

<details>

```
INFO Found a virtualenv through VIRTUAL_ENV at: /Users/iyang/Projects/.venv
DEBUG Cached interpreter info for Python 3.11.9, skipping probing: .venv/bin/python
DEBUG Using Python 3.11.9 environment at [36m.venv/bin/python[39m
DEBUG Trying to lock if free: .venv/.lock
TRACE Caching credentials for https://username:password@pypi.whatever.cn/whatever/pypi/+simple
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.11.9
DEBUG Adding direct dependency: whatever*
INFO add_decision: root @ 0a0.dev0
TRACE Fetching metadata for whatever from https://username:password@pypi.whatever.cn/whatever/pypi/+simple/whatever/
TRACE cached request https://pypi.whatever.cn/whatever/pypi/+simple/whatever/ is storable because its response has a heuristically cacheable status code 200
TRACE cached request https://pypi.whatever.cn/whatever/pypi/+simple/whatever/ has a cached response that does not allow staleness
TRACE request https://pypi.whatever.cn/whatever/pypi/+simple/whatever/ does not have a fresh cache because its age is 14 seconds, it is greater than the freshness lifetime of 0 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.whatever.cn/whatever/pypi/+simple/whatever/
DEBUG Sending revalidation request for: https://pypi.whatever.cn/whatever/pypi/+simple/whatever/
TRACE Handling request for https://username:****@pypi.whatever.cn/whatever/pypi/+simple/whatever/
TRACE Request for https://username:****@pypi.whatever.cn/whatever/pypi/+simple/whatever/ is already fully authenticated
TRACE checkout waiting for idle connection: ("https", pypi.whatever.cn)
DEBUG starting new connection: https://pypi.whatever.cn/
TRACE Http::connect; scheme=Some("https"), host=Some("pypi.whatever.cn"), port=None
DEBUG resolving host="pypi.whatever.cn"
DEBUG connecting to 1.2.3.4:443
DEBUG connected to 1.2.3.4:443
DEBUG No cached session for DnsName("pypi.whatever.cn")
DEBUG Not resuming any session
TRACE Sending ClientHello Message {
}
TRACE We got ServerHello ServerHelloPayload {
}
DEBUG Using ciphersuite TLS13_AES_128_GCM_SHA256
DEBUG Not resuming
TRACE EarlyData rejected
TRACE Dropping CCS
DEBUG TLS1.3 encrypted extensions: [Protocols([ProtocolName(687474702f312e31)])]
DEBUG ALPN protocol is Some(b"http/1.1")
TRACE Server cert is CertificateChain([CertificateDer(), CertificateDer()])
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", pypi.whatever.cn)
TRACE Updating cached credentials for https://pypi.whatever.cn/whatever/pypi/+simple/whatever/ to Credentials { username: Username(Some("username")), password: Some("password") }
TRACE is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.whatever.cn/whatever/pypi/+simple/whatever/
TRACE cached request https://pypi.whatever.cn/whatever/pypi/+simple/whatever/ is storable because its response has a heuristically cacheable status code 200
TRACE put; add idle connection for ("https", pypi.whatever.cn)
DEBUG pooling idle connection for ("https", pypi.whatever.cn)
TRACE Received package metadata for: whatever
TRACE selecting candidate for package whatever with range Range { segments: [(Unbounded, Unbounded)] } with 74 remote versions
TRACE found candidate for package PackageName("whatever") with range Range { segments: [(Unbounded, Unbounded)] } after 10 steps: "2.1.1" version
DEBUG Searching for a compatible version of whatever (*)
TRACE selecting candidate for package whatever with range Range { segments: [(Unbounded, Unbounded)] } with 74 remote versions
TRACE found candidate for package PackageName("whatever") with range Range { segments: [(Unbounded, Unbounded)] } after 10 steps: "2.1.1" version
DEBUG Selecting: whatever==2.1.1 (whatever-2.1.1-py3-none-any.whl)
TRACE No cache entry exists for /Users/iyang/Library/Caches/uv/wheels-v1/index/7ecbe29c763f7522/whatever/whatever-2.1.1-py3-none-any.msgpack
DEBUG No cache entry for: https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE Sending fresh HEAD request for https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE Handling request for https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE Request for https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE Attempting unauthenticated request for https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE take? ("https", pypi.whatever.cn): expiration = Some(90s)
DEBUG reuse idle connection for ("https", pypi.whatever.cn)
TRACE put; add idle connection for ("https", pypi.whatever.cn)
DEBUG pooling idle connection for ("https", pypi.whatever.cn)
DEBUG redirecting 'https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22' to 'http://pypi.whatever.cn/+login'
TRACE checkout waiting for idle connection: ("http", pypi.whatever.cn)
DEBUG starting new connection: http://pypi.whatever.cn/
TRACE Http::connect; scheme=Some("http"), host=Some("pypi.whatever.cn"), port=None
DEBUG resolving host="pypi.whatever.cn"
DEBUG connecting to 1.2.3.4:80
DEBUG connected to 1.2.3.4:80
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("http", pypi.whatever.cn)
TRACE put; add idle connection for ("http", pypi.whatever.cn)
DEBUG pooling idle connection for ("http", pypi.whatever.cn)
DEBUG redirecting 'http://pypi.whatever.cn/+login' to 'https://pypi.whatever.cn/+login'
TRACE take? ("https", pypi.whatever.cn): expiration = Some(90s)
DEBUG reuse idle connection for ("https", pypi.whatever.cn)
TRACE put; add idle connection for ("https", pypi.whatever.cn)
DEBUG pooling idle connection for ("https", pypi.whatever.cn)
TRACE cached request https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22 is storable because its response has a heuristically cacheable status code 200
WARN Range requests not supported for whatever-2.1.1-py3-none-any.whl; streaming wheel
TRACE No cache entry exists for /Users/iyang/Library/Caches/uv/wheels-v1/index/7ecbe29c763f7522/whatever/whatever-2.1.1-py3-none-any.msgpack
DEBUG No cache entry for: https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE Sending fresh GET request for https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE Handling request for https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE Request for https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE Attempting unauthenticated request for https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22
TRACE take? ("https", pypi.whatever.cn): expiration = Some(90s)
DEBUG reuse idle connection for ("https", pypi.whatever.cn)
TRACE put; add idle connection for ("https", pypi.whatever.cn)
DEBUG pooling idle connection for ("https", pypi.whatever.cn)
DEBUG redirecting 'https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22' to 'http://pypi.whatever.cn/+login'
TRACE take? ("http", pypi.whatever.cn): expiration = Some(90s)
DEBUG reuse idle connection for ("http", pypi.whatever.cn)
TRACE put; add idle connection for ("http", pypi.whatever.cn)
DEBUG pooling idle connection for ("http", pypi.whatever.cn)
DEBUG redirecting 'http://pypi.whatever.cn/+login' to 'https://pypi.whatever.cn/+login'
TRACE take? ("https", pypi.whatever.cn): expiration = Some(90s)
DEBUG reuse idle connection for ("https", pypi.whatever.cn)
TRACE cached request https://pypi.whatever.cn/whatever/pypi/+f/eb2/82c1e607a1a41/whatever-2.1.1-py3-none-any.whl#sha256=eb282c1e607a1a41ea65f24261245ecafc599edd259c3203ed5d78a835446e22 is storable because its response has a heuristically cacheable status code 200
DEBUG Sending warning alert CloseNotify
error: Failed to download `whatever==2.1.1`
  Caused by: Failed to unzip wheel: whatever-2.1.1-py3-none-any.whl
  Caused by: Encountered an unexpected header (actual: 0x6f64213c, expected: 0x4034b50).

```
</details>

My discover:

* It may relate to #3130
* `devpi` will redirect to `/+login` if the request cannot be authenticated 

---

_Comment by @zanieb on 2024-05-21 14:33_

~Can you share logs with `RUST_LOG=uv=trace`?~ Oops, in the toggle.

---

_Comment by @zanieb on 2024-05-21 14:36_

Yeah it looks like the problem here is that devpi is _not_ returning an unauthenticated response instead it's redirecting us to a login page. If it _said_ it was unauthenticated we would attempt the request again with the authentication from the original URL. Unfortunately we can't know if a URL requires authentication until we try and receive a bad response (as discussed in #3130). Is there a way to tell devpi that we aren't an interactive user?

---

_Label `bug` added by @zanieb on 2024-05-21 14:36_

---

_Comment by @iyanging on 2024-05-22 16:51_

Is it possible for uv to invert the logic of `AuthMiddleware.handle()`? It will let uv send an authorized request first.

---

_Comment by @zanieb on 2024-05-22 16:57_

We can't do that because e.g. if you have two GItHub repositories with separate authentication we should not use the credentials from one for the other.

Our logic should be the same as pip's here.

---

_Comment by @iyanging on 2024-05-23 02:35_

FWIW, the logic of `pip`, which handles auth, is located at `MultiDomainBasicAuth._get_new_credentials()`

Code:
<details>

```Python
def _get_new_credentials(
    self,
    original_url: str,
    *,
    allow_netrc: bool = True,
    allow_keyring: bool = False,
) -> AuthInfo:
    """Find and return credentials for the specified URL."""
    # Split the credentials and netloc from the url.
    url, netloc, url_user_password = split_auth_netloc_from_url(
        original_url,
    )

    # Start with the credentials embedded in the url
    username, password = url_user_password
    if username is not None and password is not None:
        logger.debug("Found credentials in url for %s", netloc)
        return url_user_password

    # Find a matching index url for this request
    index_url = self._get_index_url(url)
    if index_url:
        # Split the credentials from the url.
        index_info = split_auth_netloc_from_url(index_url)
        if index_info:
            index_url, _, index_url_user_password = index_info
            logger.debug("Found index url %s", index_url)

    # If an index URL was found, try its embedded credentials
    if index_url and index_url_user_password[0] is not None:
        username, password = index_url_user_password
        if username is not None and password is not None:
            logger.debug("Found credentials in index url for %s", netloc)
            return index_url_user_password

    # Get creds from netrc if we still don't have them
    if allow_netrc:
        netrc_auth = get_netrc_auth(original_url)
        if netrc_auth:
            logger.debug("Found credentials in netrc for %s", netloc)
            return netrc_auth

    # If we don't have a password and keyring is available, use it.
    if allow_keyring:
        # The index url is more specific than the netloc, so try it first
        # fmt: off
        kr_auth = (
            self._get_keyring_auth(index_url, username) or
            self._get_keyring_auth(netloc, username)
        )
        # fmt: on
        if kr_auth:
            logger.debug("Found credentials in keyring for %s", netloc)
            return kr_auth

    return username, password


def _get_index_url(self, url: str) -> Optional[str]:
    """Return the original index URL matching the requested URL.

    Cached or dynamically generated credentials may work against
    the original index URL rather than just the netloc.

    The provided url should have had its username and password
    removed already. If the original index url had credentials then
    they will be included in the return value.

    Returns None if no matching index was found, or if --no-index
    was specified by the user.
    """
    if not url or not self.index_urls:
        return None

    url = remove_auth_from_url(url).rstrip("/") + "/"
    parsed_url = urllib.parse.urlsplit(url)

    candidates = []

    for index in self.index_urls:
        index = index.rstrip("/") + "/"
        parsed_index = urllib.parse.urlsplit(remove_auth_from_url(index))
        if parsed_url == parsed_index:
            return index

        if parsed_url.netloc != parsed_index.netloc:
            continue

        candidate = urllib.parse.urlsplit(index)
        candidates.append(candidate)

    if not candidates:
        return None

    candidates.sort(
        reverse=True,
        key=lambda candidate: commonprefix(
            [
                parsed_url.path,
                candidate.path,
            ]
        ).rfind("/"),
    )

    return urllib.parse.urlunsplit(candidates[0])
```

</details>

I think the biggest difference in this problem between `uv` and `pip`, is that `pip` will try to use the auth from `index_url`.

---

_Comment by @charliermarsh on 2024-05-23 02:42_

We also use authentication from an index URL, but perhaps I'm misunderstanding your comment.

---

_Comment by @zanieb on 2025-03-20 22:12_

We now support setting `authenticate = "always"` #11896 to force authentication on the first request.

---

_Closed by @zanieb on 2025-03-20 22:12_

---
