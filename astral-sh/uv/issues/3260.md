```yaml
number: 3260
title: Azure artifacts auth fails on first attempt but works on second.
type: issue
state: closed
author: jenshnielsen
labels:
  - external
assignees: []
created_at: 2024-04-25T08:14:31Z
updated_at: 2024-06-18T13:54:50Z
url: https://github.com/astral-sh/uv/issues/3260
synced_at: 2026-01-10T05:31:37Z
```

# Azure artifacts auth fails on first attempt but works on second.

---

_Issue opened by @jenshnielsen on 2024-04-25 08:14_

For example running 
```
uv pip install numpy --verbose
```

with the following env variables set
```
UV_INDEX_URL=https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/
UV_KEYRING_PROVIDER=subprocess
```
the first time fails but rerunning it again works correctly.

UV version 0.1.38 but has been an issue since support for this type of auth landed in #2976

# Redacted log of non working execution

```
❯ uv pip install numpy --verbose
INFO Found a virtualenv through CONDA_PREFIX at: C:\Users\username\Miniconda3\envs\testuv
DEBUG Probing interpreter info for: \\?\C:\Users\username\Miniconda3\envs\testuv\python.exe
DEBUG Found Python 3.12.3 for: \\?\C:\Users\username\Miniconda3\envs\testuv\python.exe
DEBUG Using Python 3.12.3 environment at C:\Users\username\Miniconda3\envs\testuv\python.exe
DEBUG Trying to lock if free: C:\Users\username\AppData\Local\Temp\uv-53f2e5d017b1b435.lock
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: numpy*
INFO add_decision: root @ 0a0.dev0
TRACE Fetching metadata for numpy from https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE No cache entry exists for \\?\C:\Users\username\AppData\Local\uv\cache\simple-v7\cf3a2cb28c077ae5\numpy.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE Checking keyring for URL https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
DEBUG Found credentials in keyring for https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE checkout waiting for idle connection: ("https", pkgs.dev.azure.com)
DEBUG starting new connection: https://pkgs.dev.azure.com/
TRACE Http::connect; scheme=Some("https"), host=Some("pkgs.dev.azure.com"), port=None
DEBUG resolving host="pkgs.dev.azure.com"
DEBUG connecting to [2620:1ec:21::20]:443
DEBUG connected to [2620:1ec:21::20]:443
DEBUG No cached session for DnsName("pkgs.dev.azure.com")
DEBUG Not resuming any session
TRACE Sending ClientHello Message {
    ...
}
TRACE We got ServerHello ServerHelloPayload {
    ...
}
DEBUG ALPN protocol is Some(b"http/1.1")
DEBUG Using ciphersuite TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
DEBUG Server supports tickets
DEBUG Server may staple OCSP response
TRACE Server stapled OCSP response is [...]
DEBUG ECDHE curve is EcParameters { curve_type: NamedCurve, named_group: secp384r1 }
TRACE Server cert is CertificateChain([CertificateDer(...)])
DEBUG Server DNS name is DnsName("pkgs.dev.azure.com")
TRACE Unvalidated OCSP response: [...]
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", pkgs.dev.azure.com)
TRACE put; add idle connection for ("https", pkgs.dev.azure.com)
DEBUG pooling idle connection for ("https", pkgs.dev.azure.com)
DEBUG Sending warning alert CloseNotify
error: HTTP status client error (401 Unauthorized) for url (https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/)
```

# Redacted log of working execution

```
❯ uv pip install numpy --verbose
INFO Found a virtualenv through CONDA_PREFIX at: C:\Users\username\Miniconda3\envs\testuv
DEBUG Cached interpreter info for Python 3.12.3, skipping probing: C:\Users\username\Miniconda3\envs\testuv\python.exe
DEBUG Using Python 3.12.3 environment at C:\Users\username\Miniconda3\envs\testuv\python.exe

DEBUG Trying to lock if free: C:\Users\username\AppData\Local\Temp\uv-53f2e5d017b1b435.lock
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: numpy*
INFO add_decision: root @ 0a0.dev0
TRACE Fetching metadata for numpy from https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE No cache entry exists for \\?\C:\Users\username\AppData\Local\uv\cache\simple-v7\cf3a2cb28c077ae5\numpy.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE Checking keyring for URL https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
DEBUG Found credentials in keyring for https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/
TRACE checkout waiting for idle connection: ("https", pkgs.dev.azure.com)
DEBUG starting new connection: https://pkgs.dev.azure.com/
TRACE Http::connect; scheme=Some("https"), host=Some("pkgs.dev.azure.com"), port=None
DEBUG resolving host="pkgs.dev.azure.com"
DEBUG connecting to [2620:1ec:21::20]:443
DEBUG connected to [2620:1ec:21::20]:443
DEBUG No cached session for DnsName("pkgs.dev.azure.com")
DEBUG Not resuming any session
TRACE Sending ClientHello Message {
    ...
}
TRACE We got ServerHello ServerHelloPayload {
    ...
}
DEBUG ALPN protocol is Some(b"http/1.1")
DEBUG Using ciphersuite TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
DEBUG Server supports tickets
DEBUG Server may staple OCSP response
TRACE Server stapled OCSP response is [...]
DEBUG ECDHE curve is EcParameters { curve_type: NamedCurve, named_group: secp384r1 }
TRACE Server cert is CertificateChain([CertificateDer(...)])
DEBUG Server DNS name is DnsName("pkgs.dev.azure.com")
TRACE Unvalidated OCSP response: [...]
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", pkgs.dev.azure.com)
TRACE Updating cached credentials for https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/ to Credentials { username: Username(Some("VssSessionToken")), password: Some("...") }
TRACE cached request https://pkgs.dev.azure.com/project-name/_packaging/feed-name/pypi/simple/numpy/ is not storable because its response has a 'no-store' cache-control directive
TRACE attempting to decode a frame
TRACE frame decoded from buffer
...
```



---

_Comment by @zanieb on 2024-04-25 14:27_

Thanks for the logs that's super helpful for telling what's going on.

So what changed in https://github.com/astral-sh/uv/pull/2976 is we use the URL as the service name for keyring before using just the host. Is it possible your keyring backend is returning different credentials in this case?

I don't see anything that could be going wrong on our end in those logs. In both cases, we find credentials in the keyring and use them for the request.

---

_Comment by @jenshnielsen on 2024-04-25 15:11_

Thanks @zanieb I don't necessarily think this broke with #2976 but before that PR cannot easily test this since there was no way for me to inject the username. If useful I can manually recompile version 0.1.33 with the hardcoded username changed to match what Azure Artifacts expects. 

I would not expect the keyring responsens to be different between the 2 requests but perhaps its possible. 

---

_Comment by @zanieb on 2024-04-25 15:31_

You could manually call `keyring <url> <user>` and see if it is idempotent.

> I don't necessarily think this broke with https://github.com/astral-sh/uv/pull/2976 but before that PR cannot easily test this since there was no way for me to inject the username.

Ah thanks for clarifying. I wouldn't bother trying an older version in that case.

---

_Comment by @jenshnielsen on 2024-04-25 19:20_

@zanieb Found the problem by adding the credentials to the trace message when the credentials are found. 
e.g something like 
```
debug!("Found credentials in keyring for {url} as {credentials:?}");
```

The issue is that artifacts-keyring (the keyring extension that azure artifacts uses) may print information like:
```
[Information] [CredentialProvider]VstsCredentialProvider - Acquired bearer token using 'MSAL Silent'\r\r\n[Information] [CredentialProvider]VstsCredentialProvider - Attempting to exchange the bearer token for an Azure DevOps session token.\r\r\n
```
On the first fetch from keyring. This currently leaks into the credentials which naturally are not valid then. 




---

_Comment by @charliermarsh on 2024-04-25 19:23_

Oh wow, like that goes to stdout and so we consider them "part of" the credentials?

---

_Comment by @jenshnielsen on 2024-04-25 19:29_

@charliermarsh Yes that seems to be the case. 

---

_Comment by @charliermarsh on 2024-04-25 19:33_

@zanieb - We may want to instead take the last line rather than the full output? \cc @jaraco in case you have advice on the contract that we should respect from the keyring CLI.

---

_Comment by @charliermarsh on 2024-04-25 19:42_

Err, wait, it looks like we might be inspecting both `stdout` and `stderr`? I'll fix that.

---

_Comment by @charliermarsh on 2024-04-25 19:43_

Nevermind, we're only collecting `stdout`, I misread.

---

_Comment by @jenshnielsen on 2024-04-25 19:51_

It does not look like the implementation in pip does anything special https://github.com/pypa/pip/blob/main/src/pip/_internal/network/auth.py#L145 It reads stdout and strips newline. 

However, for some reason that I do not understand calling keyring from uv seems to result in a token exchange much more frequently than from pip making this much easier to observe. 

---

_Comment by @charliermarsh on 2024-04-25 19:53_

Very strange that there's any difference in behavior here then.

---

_Comment by @jenshnielsen on 2024-04-25 19:59_

@charliermarsh I am guessing the main reason that this goes relatively unnoticed with pip is that the keyring CLI is only a fallback for the in-process keyring implementation which does not have this issue.  

---

_Comment by @jenshnielsen on 2024-04-25 20:34_

For reference the issue is likely this code https://github.com/microsoft/artifacts-keyring/blob/master/src/artifacts_keyring/plugin.py#L116-L119 which converts stderr from the .net tool that artifacts-keyring wraps to stdout. 

---

_Comment by @zanieb on 2024-04-25 22:55_

Nice investigation! Thank you!

I'm not sure we can just read the last line of output, there's nothing guaranteeing the password does not include new lines. We recently discussed this at https://github.com/jaraco/keyring/pull/678#discussion_r1567891980. Frankly this sounds like bad behavior by the Azure Artifacts keyring plugin — we should open an issue requesting that they change it to stream the logs to stderr as appropriate.

---

_Label `upstream` added by @zanieb on 2024-04-25 22:55_

---

_Comment by @charliermarsh on 2024-04-25 23:04_

It seems really hard to guarantee that nothing will print to `stdout` when you're loading arbitrary Python plugins and packages :/

---

_Comment by @charliermarsh on 2024-04-25 23:11_

Could it write its output to a file instead?

---

_Comment by @zanieb on 2024-04-25 23:16_

That's true but like.. we're not differing from pip here and this is the contract that keyring provides. We're adding a JSON output format to keyring which might sort of help... but yeah writing to an output file seems like the only way to avoid confusion. Even if we get that added to keyring though, only the people on the latest keyring version will benefit from it and it's expensive for us to check what keyring version is being used.

We should probably just provide an option to invoke a Python interpreter to call into keyring.

---

_Comment by @charliermarsh on 2024-04-25 23:18_

Yeah that's a bummer. I don't think JSON output helps either since we won't know where to delimit the JSON.


---

_Comment by @charliermarsh on 2024-04-25 23:23_

No great suggestions here.

---

_Comment by @jenshnielsen on 2024-04-26 07:25_

> Very strange that there's any difference in behavior here then.

So I think the reason there is a difference goes like the following. 

* Artifacts-keyring (and Azure Artifacts Credential Provider) maintains a pr. url based cache of session tokens locally.
* If there is a hit to this cache nothing is printed to stdout otherwise the above is printed and cli based auth works
* If there is a cache miss the two above lines are printed to stdout.
* Pip authenticates against the feed url which is constant. This means that you will only see this failure once pr lifetime of the sessiontoken.
* UV authenticates against the url of the first request and reuses this for other requests making a cache miss much more likely. 





---

_Comment by @jenshnielsen on 2024-04-26 07:46_

To redeem the immediate problem, I have submitted https://github.com/microsoft/artifacts-keyring/pull/73 which does resolve the issue for me but of cause does nothing to fix the general problem. 




---

_Comment by @jaraco on 2024-04-26 14:34_

> @zanieb - We may want to instead take the last line rather than the full output? \cc @jaraco in case you have advice on the contract that we should respect from the keyring CLI.

The keyring CLI hasn't been designed around having a stable interface. Moreover, it cannot in general, as the backends are pluggable (the VSTS backend and the output it emits come from the plugin). The most reliable way to use keyring as an API would be to consume its API in Python (e.g. `import keyring; keyring.get_credential(...)`), but of course, that would require to reach into a Python environment, which may not be as discoverable as an executable on the PATH.

Perhaps `keyring` could offer a separate CLI API that would disable stdout and emit structured output. Or maybe it should do that by default if `--format json` is indicated. Lots of options here, probably worthy of a design discussion, given there's no obvious bug to fix.

---

_Comment by @jaraco on 2024-04-26 14:40_

> We should probably just provide an option to invoke a Python interpreter to call into keyring.

Sounds like you may be on the same page. Do be careful here, as the behavior of keyring behavior is highly dependent on the backends installed, so you'll need to be sure to have the same backends installed as the `keyring` command has.

---

_Comment by @jaraco on 2024-04-26 14:41_

If it helps, the `keyring` command could emit its `PYTHONPATH` and `sys.executable` for an API caller to call into (not sure I love that idea 😬 ). If only our operating systems had a framework for making RPC calls into installed applications.

---

_Comment by @mitsuhiko on 2024-05-08 22:06_

One rather absurd but quite doable thing would be to allocate a pty and invoke `keyring` with `PYTHONINSPECT=1` and then send the python code in to use the keyring API internally.

Example with [teetty](https://github.com/mitsuhiko/teetty):

```
PYTHONINSPECT=1 teetty -i ./stdin -- ./keyring
```

```
echo 'import keyring; open("/tmp/out.txt", "w").write(keyring.get_password("system", "username") or ""); quit()' > ./stdin
cat /tmp/out.txt
```

---

_Comment by @jenshnielsen on 2024-06-18 13:22_

@charliermarsh @zanieb This fix to artifacts-keyring has been merged and included in version 0.3.6. Should we close this issue or do you prefer to keep it around to track more fundamental improvements. e.g. not reading from stdout  

---

_Comment by @zanieb on 2024-06-18 13:54_

Thanks for following up! We can track something like that in a more specific issue.

---

_Closed by @zanieb on 2024-06-18 13:54_

---
