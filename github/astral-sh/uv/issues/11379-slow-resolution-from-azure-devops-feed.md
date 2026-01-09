---
number: 11379
title: Slow resolution from Azure DevOps feed
type: issue
state: open
author: thomasaarholt
labels:
  - question
  - external
assignees: []
created_at: 2025-02-10T08:50:56Z
updated_at: 2025-10-22T16:54:49Z
url: https://github.com/astral-sh/uv/issues/11379
synced_at: 2026-01-07T13:12:18-06:00
---

# Slow resolution from Azure DevOps feed

---

_Issue opened by @thomasaarholt on 2025-02-10 08:50_

### Summary

Hi!
I'm trying to find the best way to debug why installing from my work's package index is so slow. The dependency resolution is the slow part, at **25 sec** for our index vs [0.3 sec](https://gist.github.com/thomasaarholt/d8a39136769a864e34d4be1f64856b31) for pypi.

I have followed the uv instructions for [alternative package indexes](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#using-a-pat). We mostly mirror pypi, and for these packages I don't expect there to be any real difference, except that it should be a bit faster since we probably host fewer packages.

There is no authentication window that I need to click on, everything happens automatically. We've added keyring to uv tool with `uv tool install keyring --with artifacts-keyring`.

# Summary logs
```bash
# From pypi
❯ uv pip install pandas numpy scikit-learn --no-cache
Resolved 10 packages in 379ms
Prepared 10 packages in 2.44s
Installed 10 packages in 617ms
# (package versions printed here)
``` 

```bash
# From our index
uv pip install pandas numpy scikit-learn --no-cache --keyring-provider=subprocess --index-url=<redacted>
⠴ Resolving dependencies...                                                                                                                                                                                  [Information] [CredentialProvider]VstsCredentialProvider - Acquired bearer token using 'REDACTED'
[Information] [CredentialProvider]VstsCredentialProvider - Attempting to exchange the bearer token for an Azure DevOps session token.
Resolved 10 packages in 25.31s # <-------------------- 25 sec vs 0.3 sec!
Prepared 10 packages in 3.70s
Installed 10 packages in 985ms
# (package versions printed here)
``` 

# Verbose logs

I include full logs when appending --verbose to the calls for the [pypi ](https://gist.github.com/thomasaarholt/d8a39136769a864e34d4be1f64856b31)and other [index-url case](https://gist.github.com/thomasaarholt/9341f80c27de93ae3c6c9d0d3b016163).


### Platform

Windows 11 x86_64

### Version

uv 0.5.25 (9c07c3fc5 2025-01-28)

### Python version

3.12.8

---

_Label `bug` added by @thomasaarholt on 2025-02-10 08:50_

---

_Comment by @konstin on 2025-02-10 10:32_

When resolving, uv needs the metadata of every version used in the resolution. This information we get from a file called `METADATA` inside the `<name>-<version>.dist-info` directory of the wheel. PyPI exposes this file directly in the API (PEP 658). For most other server we can use HTTP Range requests to read only the METADATA file from the zip without downloading and unpacking the entire file. The index you're using does neither support PEP 658 nor HTTP Range requests, so we have to download each wheel file to extract its metadata.

The hint is in this log message:

> WARN Range requests not supported for numpy-2.2.2-cp311-cp311-win_amd64.whl; streaming wheel

---

_Label `bug` removed by @konstin on 2025-02-10 10:32_

---

_Label `question` added by @konstin on 2025-02-10 10:32_

---

_Label `upstream` added by @konstin on 2025-02-10 10:32_

---

_Comment by @thomasaarholt on 2025-02-10 16:35_

Thank you so much for this quick response, this makes perfect sense. I’m contacting those responsible for adding support for this. 

I’m closing this, but any pointers or advice for adding support for this sort of thing to a tracker is very much appreciated! I’m very pro _not spending unnecessary energy boiling the oceans_, especially in a CI!

---

_Closed by @thomasaarholt on 2025-02-10 16:35_

---

_Reopened by @thomasaarholt on 2025-02-12 12:21_

---

_Comment by @thomasaarholt on 2025-02-12 12:41_

Reopening this because I have some new information and am trying to find a minimum repro.

Full disclosure: I actually work at Microsoft, but as a Data Scientist and not affiliated with the Azure teams. My opinions are my own.

I raised this internally within Microsoft, and learnt that we do not support PEP 658 yet, but we _do_ support HTTP range requests. However, we think that `uv` might not be handling the fact that we redirect from Azure Storage to [Azure Front Door](https://learn.microsoft.com/en-us/azure/frontdoor/integrate-storage-account), a caching service.

My intention is now to see if we can work out why uv doesn't handle the redirect for HTTP range requests.

I have created a [public tracker](https://dev.azure.com/thomasaarholt/thomasaarholt-python/_artifacts/feed/thomas-python-feed) on Azure DevOps (ADO) where I can add specific python packages from PyPI to make them available to the public on that tracker. I have added PyPI as an "upstream provider", which lets an authenticated user to the ADO project (me) add any PyPI package version to the feed for public consumption. I do that via `pip install <package>==<version>` while [authenticating via artifacts-keyring](https://docs.astral.sh/uv/guides/integration/alternative-indexes/#using-keyring) (I install it directly in the venv, pip didn't pick up the `uv tool`). There's actually a (possibly redirect-related?) bug with uv here, as I can't get uv to authenticate correctly for packages that aren't already in the index. Will make a new issue on that, but that requires authentication to the server to debug.

```
export INDEX='https://pkgs.dev.azure.com/thomasaarholt/thomasaarholt-python/_packaging/thomas-python-feed/pypi/simple/'
uv venv --seed --quiet
source .venv/bin/activate.fish # activate however you normally do
uv pip install numpy pandas scikit-learn --index-url=$INDEX --no-cache -v  &| grep stream # I pipe stdout and stderr into grep
```
I've run the above a few times now, and always get at least one package warning, sometimes two or three:

```fish
❯ export INDEX='https://pkgs.dev.azure.com/thomasaarholt/thomasaarholt-python/_packaging/thomas-python-feed/pypi/simple/'
  uv venv --seed --quiet
  source .venv/bin/activate.fish
  uv pip install numpy pandas scikit-learn --index-url=$INDEX --no-cache -v  &| grep stream # I pipe stdout and stderr into grep
# WARN Range requests not supported for scikit_learn-1.6.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl; streaming wheel

# rerun
❯ export INDEX='https://pkgs.dev.azure.com/thomasaarholt/thomasaarholt-python/_packaging/thomas-python-feed/pypi/simple/'
  uv venv --seed --quiet
  source .venv/bin/activate.fish
  uv pip install numpy pandas scikit-learn --index-url=$INDEX --no-cache -v  &| grep stream # I pipe stdout and stderr into grep
# WARN Range requests not supported for pandas-2.2.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl; streaming wheel
# WARN Range requests not supported for scikit_learn-1.6.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl; streaming wheel
# WARN Range requests not supported for numpy-2.2.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl; streaming wheel
```

Is there any chance I could get some help debugging this from the uv team?

---

_Comment by @zanieb on 2025-02-12 14:01_

> There's actually a (possibly redirect-related?) bug with uv here, as I can't get uv to authenticate correctly for packages that aren't already in the index. Will make a new issue on that, but that requires authentication to the server to debug.

I'm not seeing the opt-in to using the keyring and a username in the index URL.

On the range requests...

Is it feasible to create a simple nginx container or something that replicates the setup?

Can you reproduce with [async_http_range_reader](https://github.com/prefix-dev/async_http_range_reader) directly?

cc @konstin I think you have some familiarity with `async_http_range_reader`?

---

_Comment by @thomasaarholt on 2025-02-12 16:33_

> I'm not seeing the opt-in to using the keyring and a username in the index URL.

Yes, I'm showing the regular index url here since the user shouldn't need authentication when installing packages that are on the feed. When trying to get uv to authenticate to add packages from PyPI to my public feed, I was using:
```
export UV_INDEX='https://VssSessionToken@pkgs.dev.azure.com/thomasaarholt/thomasaarholt-python/_packaging/thomas-python-feed/pypi/simple/'

uv venv --seed --quiet
source .venv/bin/activate.fish
uv pip install numpy==2.1.1 --index-url=$UV_INDEX --keyring-provider='subprocess' -v
```
[Here's the log for that in a gist](https://gist.github.com/thomasaarholt/78e141efd79188a362c8b1b09258adfe).

To clear one possible point of confusion: One must be an authenticated user in order to install an upstream (PyPI) package through an ADO feed. [This is by design](https://learn.microsoft.com/en-us/azure/devops/artifacts/how-to/public-feeds-upstream-sources?view=azure-devops&tabs=python#restore-packages) (purple box).

> On the range requests...

I might be able to. Never worked with nginx before, and only a bit with rust. I will try if I can find the time.

---

_Comment by @zanieb on 2025-02-12 16:40_

So a range request does work with `curl` _if_ you provide the `-L` flag to follow the redirects

```
❯ curl -H "Range: bytes=0-1048575" "https://pkgs.dev.azure.com/thomasaarholt/5e230929-47ac-48ad-97db-15ab1d2aa0c5/_packaging/56fc1e3a-f592-420b-bd14-a8c967a65c28/pypi/download/pandas/2.2.3/pandas-2.2.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=c124333816c3a9b03fbeef3a9f230ba9a737e9e5bb4060aa2107a86cc0a497fc" -f -L > chunk
```

---

_Comment by @zanieb on 2025-02-12 16:45_

But...  if you make a HEAD request, the index returns a HTTP 405 method not allowed

```
❯ curl --head -L "https://pkgs.dev.azure.com/thomasaarholt/5e230929-47ac-48ad-97db-15ab1d2aa0c5/_packaging/56fc1e3a-f592-420b-bd14-a8c967a65c28/pypi/download/pandas/2.2.3/pandas-2.2.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl"
HTTP/2 405 
```

which is what we use to determine if range requests are supported

https://github.com/astral-sh/uv/blob/748582ee6f3ff1007692f131fa4b59d7c100ec66/crates/uv-client/src/registry_client.rs#L711-L753
https://github.com/prefix-dev/async_http_range_reader/blob/c2a1b5a63712a34bca661cc94389fb03fb0dcddd/src/lib.rs#L310-L318

---

_Comment by @zanieb on 2025-02-12 16:48_

I would be curious to hear why they don't support HEAD requests — as that seems to be the crux of the problem here.

You could set `RUST_LOG=trace` to get verbose logs from the network layer and confirm the HTTP 405 is occurring.

---

_Comment by @thomasaarholt on 2025-02-12 16:52_

That was quick! I've brought it up internally now. Thank you!

---

_Comment by @konstin on 2025-02-12 16:53_

For additional context, we should be following redirects in the HEAD request, but we need the initial HEAD requests to determine whether the server supports range requests (https://github.com/prefix-dev/async_http_range_reader/blob/9c1e2d3082745f3a75d8065a3dd145cd51b2715a/src/lib.rs#L310-L318).

---

_Comment by @thomasaarholt on 2025-02-12 17:05_

> You could set `RUST_LOG=trace` to get verbose logs from the network layer and confirm the HTTP 405 is occurring.

✅
```
export INDEX='https://pkgs.dev.azure.com/thomasaarholt/thomasaarholt-python/_packaging/thomas-python-feed/pypi/simple/'
export RUST_LOG=trace
uv venv --seed --quiet
source .venv/bin/activate.fish # activate however you normally do
uv pip install numpy --index-url=$INDEX --no-cache -v  &> log.txt
```
```	
# Ctrl+F 405
TRACE Attempting to retry error: Error { kind: WrappedReqwestError(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pkgs.dev.azure.com")), port: None, path: "/thomasaarholt/5e230929-47ac-48ad-97db-15ab1d2aa0c5/_packaging/56fc1e3a-f592-420b-bd14-a8c967a65c28/pypi/download/numpy/2.2.2/numpy-2.2.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", query: None, fragment: Some("sha256=02935e2c3c0c6cbe9c7955a8efa8908dd4221d7755644c59d1bba28b94fd334f") }, WrappedReqwestError(Reqwest(reqwest::Error { kind: Status(405), url: "https://pkgs.dev.azure.com/thomasaarholt/5e230929-47ac-48ad-97db-15ab1d2aa0c5/_packaging/56fc1e3a-f592-420b-bd14-a8c967a65c28/pypi/download/numpy/2.2.2/numpy-2.2.2-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl#sha256=02935e2c3c0c6cbe9c7955a8efa8908dd4221d7755644c59d1bba28b94fd334f" }))) }
```

---

_Renamed from "Slow package resolution from authenticated package index" to "Slow resolution from Azure DevOps feed" by @thomasaarholt on 2025-02-12 17:34_

---

_Comment by @thomasaarholt on 2025-02-14 11:49_

We're taking a look internally at fixing this. No promises at timelines. 😅 Thanks for your help!

---

_Referenced in [pypa/pip#12991](../../pypa/pip/pulls/12991.md) on 2025-03-09 22:30_

---

_Comment by @thomasaarholt on 2025-06-24 19:30_

Okay, so we have some progress from our side - we are testing support for HEAD requests, but still struggling with range requests.
I just wanted to ask if you have any perspective on these logs ([old](https://gist.github.com/thomasaarholt/d6ac29cb811d8fa1dd1ecb402670d277) vs [new beta](https://gist.github.com/thomasaarholt/f5172244b1ee07b87c4e66f4e0671404)) where I've extracted out the most relevant bits. I would really appreciate any help here. (Just a reminder that I'm 'just a (really keen) data scientist' acting as a messenger here, and not a representative of the ADO team)

Here's the most relevant bits:
```log
# Old version
TRACE Considering retry of response HTTP 405 Method Not Allowed for https://o365exchange.pkgs.visualstudio.com/_packaging/4e1b791c-19a4-4f51-af8c-cfb5f7ab41ff/pypi/download/numpy/2.3.1/numpy-2.3.1-cp313-cp313-manylinux_2_28_x86_64.whl
TRACE Cannot retry error: not an IO error
WARN Range requests not supported for numpy-2.3.1-cp313-cp313-manylinux_2_28_x86_64.whl; streaming wheel
```

```log
# New beta version
TRACE Considering retry of error: Error { kind: Zip(WheelFilename { name: PackageName("numpy"), version: "2.3.1", tags: Small { small: WheelTagSmall { python_tag: CPython { python_version: (3, 13) }, abi_tag: CPython { gil_disabled: false, python_version: (3, 13) }, platform_tag: Manylinux { major: 2, minor: 28, arch: X86_64 } } } }, UpstreamReadError(Custom { kind: Other, error: HttpRangeRequestUnsupported })) }
TRACE Cannot retry IO error: not one of `ConnectionReset` or `UnexpectedEof`
TRACE Cannot retry error: not an IO error
WARN Range requests not supported for numpy-2.3.1-cp313-cp313-manylinux_2_28_x86_64.whl; streaming wheel
```

---

_Comment by @konstin on 2025-06-24 19:42_

It seems we're a different error path now, with the second one being inside the async-http-range-reader crate.

---

_Comment by @thomasaarholt on 2025-06-24 19:43_

I also see this line a few lines above what I quoted in the previous comment:

```
DEBUG redirect policy disallowed redirection to 'https://vsblobprodscussu0shard22.vsblob.vsassets.io/REDACTED?sv=2019-07-07&sr=b&sig=6nWQ8vjzgdYFB1CVD1DF1ifKJJIxBuBOH6hx%2F2nP648%3D&skoid=b735c262-fb1c-41c9-b9ac-cdb426a80296&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skt=2025-06-24T16%3A21%3A50Z&ske=2025-06-26T17%3A21%3A50Z&sks=b&skv=2019-07-07&se=2025-06-24T20%3A33%3A33Z&sp=r&rscl=x-e2eid-6b7350b3-9623492d-9d49d5c0-a360ba58-session-6b7350b3-9623492d-9d49d5c0-a360ba58&rscd=attachment%3B%20filename%3D%22numpy-2.3.1-cp313-cp313-manylinux_2_28_x86_64.whl%22&P1=1750808014&P2=1&P3=2&P4=QwPC5E0pj0GF%2bw2ODhIpBy5bNfGHbpSh2pDGkaNwFI8%3d'    
```

When I curl that url, I get the following response:
```
curl -I https://vsblobprodscussu0shard22.vsblob.vsassets.io/REDACTED=2019-07-07&sr=b&sig=6nWQ8vjzgdYFB1CVD1DF1ifKJJIxBuBOH6hx%2F2nP648%3D&skoid=b735c262-fb1c-41c9-b9ac-cdb426a80296&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skt=2025-06-24T16%3A21%3A50Z&ske=2025-06-26T17%3A21%3A50Z&sks=b&skv=2019-07-07&se=2025-06-24T20%3A33%3A33Z&sp=r&rscl=x-e2eid-6b7350b3-9623492d-9d49d5c0-a360ba58-session-6b7350b3-9623492d-9d49d5c0-a360ba58&rscd=attachment%3B%20filename%3D%22numpy-2.3.1-cp313-cp313-manylinux_2_28_x86_64.whl%22&P1=1750808014&P2=1&P3=2&P4=QwPC5E0pj0GF%2bw2ODhIpBy5bNfGHbpSh2pDGkaNwFI8%3d
HTTP/2 200
content-length: 16627546
content-type: application/octet-stream
content-language: x-e2eid-0fef6d97-5ec04d58-b5f8a8db-c95cc161-session-0fef6d97-5ec04d58-b5f8a8db-c95cc161
last-modified: Tue, 24 Jun 2025 18:34:36 GMT
accept-ranges: bytes
etag: "0x8DDB34DC5DB0604"
x-cache: TCP_HIT
x-ms-request-id: 3cfeb847-f01e-0001-4e36-e5ed6f000000
x-ms-version: 2019-07-07
x-ms-creation-time: Tue, 24 Jun 2025 18:34:36 GMT
x-ms-lease-status: unlocked
x-ms-lease-state: available
x-ms-blob-type: BlockBlob
content-disposition: attachment; filename="numpy-2.3.1-cp313-cp313-manylinux_2_28_x86_64.whl"
x-ms-server-encrypted: true
access-control-expose-headers: Content-Length
access-control-allow-origin: *
x-cid: 7
x-ccc: NO
x-msedge-ref: Ref A: 06E20667CA8B4B0E8D0F248E5976F541 Ref B: SVG20EDGE0108 Ref C: 2025-06-24T19:45:26Z
date: Tue, 24 Jun 2025 19:45:25 GMT
```
And that **does** contain `accept-ranges: bytes` indicating support of range requests!

Where does the redirect policy come from, and is there any way either you can adjust it, or we can support it?

---

_Comment by @thomasaarholt on 2025-06-24 19:50_

@konstin agreed. I just edited the above comment with new info, didn't see that you responded.

---

_Comment by @thomasaarholt on 2025-06-24 20:59_

EDIT: This was not correct. Here I compared the latest release with the effect of me applying the patch in this post, when I should have compared master branch alone with me applying the patch. The following is already happening on latest master branch:
<details><summary>Incorrect comment</summary>
<p>
@konstin I believe I am able to change the uv reqwest policy to allow the redirect. Here installing `numpy scipy scikit-learn`.

When I compile uv with the following patch, I go from 

```
9726 TRACE Getting metadata for scikit_learn-1.7.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl by range request
...
10428 DEBUG redirect policy disallowed redirection to 'https://vsblobprodscussu0shard6.vsblob.vsassets.io/b-6cb12e9fc4334ae59c34553955d1a530/C264C1D7F05B28047E27011CBA39318F1C931C8D610E4695326FCF8217D8077300.blob?sv=2019-07-07&sr=b&sig=sKZzOIaeSAwNhCKNdc5UPl71VxEwMHb0cHlRlk4jZa0%3D&skoid=b735c262-fb1c-41c9-b9ac-cdb426a80296&sktid=33e01921-4d64-4f8c-a055-5bdaffd5e33d&skt=2025-06-24T16%3A21%3A56Z&ske=2025-06-26T17%3A21%3A56Z&sks=b&skv=2019-07-07&se=2025-06-24T21%3A18%3A17Z&sp=r&rscl=x-e2eid-6b73e904-9623492d-9d49d5c0-a360ba58-session-6b73e904-9623492d-9d49d5c0-a360ba58&rscd=attachment%3B%20filename%3D%22scikit_learn-1.7.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl%22&P1=1750812358&P2=1&P3=2&P4=yHZBtGsF5WEdz89dWI6sdjGIRBZ7ph5ntzgaFblphJY%3d'
...
10428 TRACE Considering retry of error: Error { kind: Zip(WheelFilename { name: PackageName("scikit-learn"), version: "1.7.0", tags: Large { large: WheelTagLarge { build_tag: None, python_tag: [CPython { python_version: (3, 13) }], abi_tag: [CPython { gil_disabled: false, python_version: (3, 13) }], platform_tag: [Manylinux { major: 2, minor: 17, arch: X86_64 }, Manylinux2014 { arch: X86_64 }], repr: "cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64" } } }, UpstreamReadError(Custom { kind: Other, error: HttpRangeRequestUnsupported })) }
10429 TRACE Cannot retry IO error: not one of `ConnectionReset` or `UnexpectedEof`
10430 TRACE Cannot retry error: not an IO error
10431 WARN Range requests not supported for scikit_learn-1.7.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl; streaming wheel
```
to
```log
9705 TRACE Getting metadata for scikit_learn-1.7.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl by range request
# With no further error
```
Here is the patch. It isn't necessarily the correct change to make, but it shows that it is possible. 

```
git diff
diff --git a/crates/uv-client/src/base_client.rs b/crates/uv-client/src/base_client.rs
index 85c384b0d..9ce3b2797 100644
--- a/crates/uv-client/src/base_client.rs
+++ b/crates/uv-client/src/base_client.rs
@@ -96,7 +96,7 @@ impl RedirectPolicy {
     pub fn reqwest_policy(self) -> reqwest::redirect::Policy {
         match self {
             RedirectPolicy::BypassMiddleware => reqwest::redirect::Policy::default(),
-            RedirectPolicy::RetriggerMiddleware => reqwest::redirect::Policy::none(),
+            RedirectPolicy::RetriggerMiddleware => reqwest::redirect::Policy::default(),
         }
     }
 }
```

Resolution is still slow - they both resolve in about 5 sec. I can send you the logs if you want, but its getting late here (CET) and I should go to bed now.
</p>
</details> 


---

_Comment by @thomasaarholt on 2025-06-30 07:21_

The latest ADO update will be live in a few weeks. I will update here then. It'll be much easier to debug using ADO feeds that don't require authentication. From what I can tell, we definitely support range requests, but there might be some tweaking on either our or uv's end to support faster package resolution.

---

_Comment by @laenan8466 on 2025-10-22 16:54_

Revisiting this, do you have an update for us @thomasaarholt ?

---
