```yaml
number: 5073
title: uv downloads are slow on fallback to streamed wheel downloads
type: issue
state: closed
author: morotti
labels:
  - question
assignees: []
created_at: 2024-07-15T14:13:33Z
updated_at: 2025-07-29T15:37:48Z
url: https://github.com/astral-sh/uv/issues/5073
synced_at: 2026-01-10T03:32:44Z
```

# uv downloads are slow on fallback to streamed wheel downloads

---

_Issue opened by @morotti on 2024-07-15 14:13_

Hello,

I'm trying the latest version of uv and I see that you added progress bars. :) 

uv is downloading tensorflow at less than 1 MB/s.
when tensorflow is the last remaining package, the speed starts increasing to a more reasonable speed of double digits MB/s.
for reference, I have pip downloading at 400+ MB/s for tensorflow/torch (with one patch pending to merge to fix the progress bar :D ).

There is a chance it's not the progress bars themselves, but the addition of progress bars made the issue visible.

<img width="528" alt="image" src="https://github.com/user-attachments/assets/e1e66915-c904-43df-a849-2c66f63c5e6e">


Looking at the output:

1) I see that uv is doing 50 concurrent downloads in parallel. It's too many to the point it's counterproductive for performance. 
It will also cause issues with proxy/firewall and internal pypi servers that can't take the load and drop connections.
Can I suggest to reduce that to 20 at most? It will be faster and more stable.
As a nice side effect, it will allow the output to fit in the screen (default console is around 80 x 24 lines). The default output of 50 lines never fits in the screen ^^

I do note that both the resolver and the download seem to use the same concurrency settings (50 by default). It might make sense for the settings to be separate, the resolver is doing small requests and is more affected by latency, the downloading is more about bandwidth.

2) Can you review what is the chunk size for reading from network? writing to disk? 

3) The screen flashes a lot, like it's trying to rerender all the progress bars every few kB. Do you have some sort of limits on how often the bar should be refreshed?

4) Do you have some throttling or priority where it tries to download packages at the top of the list first by any chance?
The list is sorted with bigger packages to the bottom. 


DEBUG RUN
the download time is part of the "prepare 394 packages"

```
# export UV_CONCURRENT_DOWNLOADS=50 && rm -rf ./deleteme && time uv pip install --no-cache --prefix ./deleteme mypackage
...
Resolved 394 packages in 20.82s
Prepared 394 packages in 30.54s
Installed 394 packages in 4.64s
...
real    1m3.689s
user    0m38.068s
sys     0m53.199s
```


```
# export UV_CONCURRENT_DOWNLOADS=32 && rm -rf ./deleteme && time uv pip install --no-cache --prefix ./deleteme mypackage
...
Resolved 394 packages in 20.92s
Prepared 394 packages in 28.69s
Installed 394 packages in 5.03s
...
real    1m2.636s
user    0m36.721s
sys     0m51.546s

```


```
# export UV_CONCURRENT_DOWNLOADS=16 && rm -rf ./deleteme && time uv pip install --no-cache --prefix ./deleteme mypackage
...
Resolved 394 packages in 22.40s
Prepared 394 packages in 27.94s
Installed 394 packages in 4.84s
...
real    1m2.172s
user    0m36.054s
sys     0m49.306s
```


```
#export UV_CONCURRENT_DOWNLOADS=8 && rm -rf ./deleteme && time uv pip install --no-cache --prefix ./deleteme mypackage
...
Resolved 394 packages in 26.51s
Prepared 394 packages in 28.72s
Installed 394 packages in 4.59s
...
real    1m6.988s
user    0m36.651s
sys     0m50.748s
```

---

_Comment by @charliermarsh on 2024-07-15 14:16_

My guess is that you're being rate-limited or throttled somehow.

---

_Comment by @morotti on 2024-07-15 14:25_

> My guess is that you're being rate-limited or throttled somehow.

I am not rate limited. I can download torch at ~450 MB/s with `time pip download --dest /tmp/deleteme --no-cache torch ` to an internal mirror of pypi.

uv is downloading at 1 MB/s for the first 10 MB or so, then it's increasing to double digits MB/s, it's better toward the end but still below expectations. there is something wrong with uv.



---

_Comment by @charliermarsh on 2024-07-15 14:27_

What do you see if you download _just_ torch though? Your examples above all feature concurrent downloads.

---

_Comment by @morotti on 2024-07-15 14:37_

installing torch alone (it requires typing-extensions too)

I see the progress bar appearing with a few MB done, the download is progressing very slowly (I'd say around 1 MB/s) from 5 to 10 MB completed, then the download speed is increasing to double digits MB/s (hard to estimate on sight) until the end.


```
$ rm -rf /tmp/deleteme/ && time uv pip install --prefix /tmp/deleteme  --no-cache torch

 Preparing packages... (1/2)
torch      ------------------------------ 5.22 MB/1.80 GB
...
Resolved 2 packages in 19.07s
Prepared 2 packages in 20.52s
Installed 2 packages in 3.92s
 + torch==1.13.1+cu117
 + typing-extensions==4.2.0

real    0m45.237s
user    0m34.342s
sys     0m16.957s

```

---

_Comment by @zanieb on 2024-07-15 14:40_

That invocation gives me the following:

```
❯ time uv pip install --prefix /tmp/deleteme  --no-cache torch
Resolved 9 packages in 287ms
Prepared 9 packages in 2.55s
Installed 9 packages in 143ms
 + filelock==3.15.4
 + fsspec==2024.6.1
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.3
 + sympy==1.13.0
 + torch==2.3.1
 + typing-extensions==4.12.2
uv pip install --prefix /tmp/deleteme --no-cache torch  0.88s user 1.52s system 69% cpu 3.445 total
```

---

_Comment by @morotti on 2024-07-15 14:53_

Interesting...

From the logs, I get both "resolved" and "prepared" logs at 19 and 20 seconds.
You get 0.2 and 2 seconds.

I think it's unexpected to say the least? why does the resolution takes this long for me? and it's so much faster for you?

With `--verbose` flag and timestamping I get a warning `WARN Range requests not supported for torch-1.13.1+cu117-cp38-cp38-linux_x86_64.whl; streaming wheel` during the resolution phase then it takes 20 seconds to download it.
Repeat the same logs and duration during the preparation phase (I think the prepare is the download phase).

web server is artifactory behind some caching server. maybe it doesn't support range requests. is it possible `uv` hits a completely different codepath when the web server does not support range requests?

```
Jul 15 15:48:59 WARN Range requests not supported for torch-1.13.1+cu117-cp38-cp38-linux_x86_64.whl; streaming wheel
Jul 15 15:48:59 DEBUG No cache entry for: https://mycompany.example.com/artifactory/api/pypi/pypi-mirror/torch/1.13.1+cu117/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.whl#md5=f9aa82f33cd00d9d244b1fd1dd930a95
Jul 15 15:49:19 DEBUG Adding transitive dependency for torch==1.13.1+cu117: typing-extensions*
```


---

_Comment by @zanieb on 2024-07-15 15:00_

If you don't support range requests, we can't just download the part of the wheel we need to extract metadata so we need to download the entire thing to perform resolution. Servers that do not support range requests are expected to have significant degradation of resolver performance.

---

_Comment by @charliermarsh on 2024-07-15 15:00_

That shouldn't be the problem though, since we should be caching that wheel (and we need to download it in the next step anyway).

---

_Comment by @charliermarsh on 2024-07-15 15:01_

(As an aside, I feel like your tone assumes we will disagree with you. I'm not here to tell you that you're wrong, and that uv is perfect! I'm just trying to get more information so we can diagnose the problem.)

---

_Comment by @zanieb on 2024-07-15 15:01_

It does sound weird that we don't cache and use it in the subsequent step. Maybe there's a bug there if you're seeing that log twice?

---

_Comment by @charliermarsh on 2024-07-15 15:02_

Even still, that doesn't explain why the download is slow.

---

_Comment by @morotti on 2024-07-15 15:05_

apologies if the tone came wrong, I am not a native speaker :sorry:

note that we are testing with `--no-cache`, that might prevent the resolution phase from caching the package, for the prepare phase. 

---

_Comment by @zanieb on 2024-07-15 15:10_

Thanks! No problem we're happy to help out.

With `--no-cache` we should use a temporary cache that is reused across the entire invocation.

As Charlie noted, it is still confusing that the download is slow separately from the other problems. Maybe we don't chunk the download well when streaming the whole file? Maybe we need to try to reproduce with a registry that does not allow range requests.

---

_Comment by @zanieb on 2024-07-15 15:10_

Do you see this problem using the standard PyPI registry?

---

_Comment by @morotti on 2024-07-15 15:28_

> Do you see this problem using the standard PyPI registry?

sorry, I am not able to test that for comparison, connection to pypi.org is blocked by firewall at my workplace.


Trying in verbose debug with `UV_CONCURRENT_DOWNLOADS=1 ... --verbose --verbose`, still on the internal mirror.

logs in the resolution phase below.
Notice the part with no logs for 20 seconds, that must be the part where it's downloading the wheel. 

code in or around uv_client::registry_client::read_metadata_stream? 
best guess, is it possible the code is downloading in very small chunks? could be small chunks from the network or to the disk or both?

```
Jul 15 16:21:16  uv_client::cached_client::from_path_sync path="/tmp/.tmpNJBBiq/wheels-v1/index/456aceeb8fbaf909/torch/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.msgpack"
Jul 15 16:21:16             0.230980s   0ms DEBUG uv_client::cached_client No cache entry for: https://mycompany.example.com/artifactory/api/pypi/pypi-mirror/torch/1.13.1+cu117/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.whl#md5=f9aa82f33cd00d9d244b1fd1dd930a95
Jul 15 16:21:16            uv_client::cached_client::fresh_request url="https://mycompany.example.com/artifactory/api/pypi/pypi-mirror/torch/1.13.1+cu117/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.whl#md5=f9aa82f33cd00d9d244b1fd1dd930a95"
Jul 15 16:21:16            uv_client::cached_client::new_cache file=/tmp/.tmpNJBBiq/wheels-v1/index/456aceeb8fbaf909/torch/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.msgpack
Jul 15 16:21:16            uv_client::registry_client::read_metadata_stream wheel=torch-1.13.1+cu117-cp38-cp38-linux_x86_64.whl
Jul 15 16:21:34      18.589721s  18s  DEBUG uv_resolver::resolver Adding transitive dependency for torch==1.13.1+cu117: typing-extensions*
```

similar logs in the prepare phase below.
notice the 20 seconds gap as well.

```
Jul 15 16:21:34  uv_client::cached_client::from_path_sync path="/tmp/.tmpNJBBiq/wheels-v1/index/456aceeb8fbaf909/torch/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.http"
Jul 15 16:21:34            18.723822s   0ms DEBUG uv_client::cached_client No cache entry for: https://mycompany.example.com/artifactory/api/pypi/pypi-mirror/torch/1.13.1+cu117/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.whl#md5=f9aa82f33cd00d9d244b1fd1dd930a95
Jul 15 16:21:34            uv_client::cached_client::fresh_request url="https://mycompany.example.com/artifactory/api/pypi/pypi-mirror/torch/1.13.1+cu117/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.whl#md5=f9aa82f33cd00d9d244b1fd1dd930a95"
Jul 15 16:21:34    uv_installer::preparer::get_wheel name=typing-extensions==4.2.0, size=None, url="../../typing-extensions/4.2.0/typing_extensions-4.2.0-py3-none-any.whl#md5=95fc87a08006c5249ae13b8a1c3770b9"
Jul 15 16:21:34      uv_distribution::distribution_database::get_or_build_wheel dist=typing-extensions==4.2.0
Jul 15 16:21:34            uv_client::cached_client::new_cache file=/tmp/.tmpNJBBiq/wheels-v1/index/456aceeb8fbaf909/torch/torch-1.13.1+cu117-cp38-cp38-linux_x86_64.http
Jul 15 16:21:34            uv_distribution::distribution_database::wheel wheel=torch==1.13.1+cu117
Jul 15 16:21:55        uv_client::cached_client::get_serde

```



---

_Comment by @charliermarsh on 2024-07-15 15:38_

> apologies if the tone came wrong, I am not a native speaker :sorry:

No worries, I just want to make clear that we're on your side and want to help :)

Let me take a look at the streaming code to verify (1) that the chunk size is what we expect, and (2) that we're caching the wheel in that case.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-15 15:39_

---

_Comment by @morotti on 2024-07-15 16:23_

I see 3 calls to `tokio::io::copy` in the codebase.

1 call in https://github.com/astral-sh/uv/blob/e34ab96e807764fb0bfdfa0ca9c96d258d0d22de/crates/uv-extract/src/stream.rs#L53C13-L53C28
2 call in https://github.com/astral-sh/uv/blob/main/crates/uv-distribution/src/distribution_database.rs#L601C25-L601C40

It seems to me the copy() is defaulting to 2k chunks?
source code https://dtantsur.github.io/rust-openstack/src/tokio/io/util/copy.rs.html#61-75

(Just a thought after browsing through the code, I could be completely wrong, I have no experience in rust)

---

_Comment by @morotti on 2024-07-15 17:28_

similarly, i see 2 usages of tokio::io::BufWriter::new and one usage of tokio::io::BufWriter::with_capacity passing the filesize.
I think there are different codepath that can be hit.

the default write buffer seems to be 8k.
https://dtantsur.github.io/rust-openstack/src/tokio/io/util/buf_writer.rs.html#45
https://stdrs.dev/nightly/x86_64-unknown-linux-gnu/src/std/sys_common/io.rs.html#3

that could explain some of the performance., the torch wheel is 1800 MB, that could be 200k+ I/O calls to write(), the overhead would be quite sizable.
I am curious if you testing on a NVMe SSD with ultra fast CPU?


---

_Comment by @charliermarsh on 2024-07-15 17:38_

(I will trace through these, it's a little tricky because sometimes we already wrap in a buffer elsewhere.)

---

_Renamed from "performance regression, uv downloads is horribly slow with progress bars" to "uv downloads are slow on fallback to streamed wheel downloads" by @zanieb on 2024-07-15 22:07_

---

_Comment by @charliermarsh on 2024-07-16 01:33_

Ok so for one, I believe we _are_ downloading the wheel twice if your server doesn't support range requests, because we stream the wheel and stop as soon as we see a `METADATA` file, which is sometimes an optimization, but means we don't fully download (and cache) the wheel: https://github.com/astral-sh/uv/issues/5088.

---

_Unassigned @charliermarsh by @charliermarsh on 2024-08-10 01:01_

---

_Label `question` added by @charliermarsh on 2024-08-10 01:01_

---

_Comment by @fersarr on 2024-08-22 16:52_

Hi! Just wanted to check whether there has been any progress on this one? We initially tried `uv` in one of the earlier versions and it was really fast but then it got worse as @morotti explained. We had big plans to integrate this into our infra and made multiple changes but it had to be paused when the performance got so much worse. Is there info we could provide to help with this?

---

_Comment by @zanieb on 2024-08-22 17:07_

@fersarr It's possible this regressed with https://github.com/astral-sh/uv/pull/5089 as described in https://github.com/astral-sh/uv/issues/6104 — is that your situation?

---

_Comment by @charliermarsh on 2024-08-22 17:09_

@zanieb -- I think we should probably just revert that change and revisit. It looks like it did more harm than good. Any objections?

---

_Comment by @charliermarsh on 2024-08-22 17:09_

If anyone has an example of a wheel that was slow to download, that's mirrored from PyPI (so I can inspect it), it would be helpful.

---

_Comment by @morotti on 2024-08-23 13:39_

quick update on this ticket:

1) the main issue, packages are downloading extremely slow, around 1 MB/s, until around 10 MB mark then the download gets faster.

we have no clue why and not closer to a resolution or an understanding of the root cause.
the only reason uv is not extremely slow due this bug, is because it's downloading 50 packages in parallel by default.

2) secondary issue we found while debugging. all packages are downloaded twice.

packages are downloaded a first time to identify dependencies then a second time to do the installation.
the first download is a streamlit download which stops when it finds dependency information and does not cache. the METADATA file is always at the end of the wheel so it's counter productive.
this issue was fixed the week after this ticket was opened.
the fix was just reverted yesterday so it's back to square one https://github.com/astral-sh/uv/pull/6470


---

_Comment by @charliermarsh on 2024-08-23 19:08_

Empirically it proved to be significantly worse in practice to “always download the wheel”. We measured it with users.

---

_Comment by @jpedrick-numeus on 2024-09-06 14:45_

I'm running into an issue with this as well. A package, which is around 160Mb, is hosted in a private CodeArtifact repository. 

I get the following warnings for _every single version_ of the package in code artifact, which means multiple gigabytes of data is downloaded, seemingly just to determine the package version?

Additionally, I have the package version locked in the pyproject.toml.

```
WARN Range requests not supported for mypackage-1.7.34-py3-none-linux_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://some-server.d.codeartifact.eu-west-1.amazonaws.com/pypi/python/simple/mypackage/1.7.19/mypackage-1.7.19-py3-none-linux_x86_64.whl#sha256=aaaaaa47deb6b27795f516e13431acffc1f0197cb9274a1ef7d2b6ae40zzzzz9
```

---

_Comment by @zanieb on 2024-09-06 14:51_

@jpedrick-numeus unfortunately this is a problem with CodeArtifact — they should support the modern metadata API so we don't need to download wheels to inspect their requirements and if they don't support that they should at least support range requests so we can effectively extract the requirements.

---

_Comment by @jpedrick-numeus on 2024-09-09 12:03_

> @jpedrick-numeus unfortunately this is a problem with CodeArtifact — they should support the modern metadata API so we don't need to download wheels to inspect their requirements and if they don't support that they should at least support range requests so we can effectively extract the requirements.

@zanieb does UV have to download every single version? I tried setting a version constraint `mypackage > 1.7.34` or `== 1.7.34` and it still downloads all prior versions.

---

_Comment by @charliermarsh on 2024-09-09 13:19_

Can you share the `--verbose` logs?

---

_Comment by @ewianda on 2024-09-09 13:35_

> That shouldn't be the problem though, since we should be caching that wheel (and we need to download it in the next step anyway).

@charliermarsh Based on the logs and without understanding how the code works, I have observed that the full wheels for multiple versions are being downloaded during the resolution phase and, I guess, just picking one version in the next step. 

---

_Comment by @charliermarsh on 2024-09-09 13:37_

@ewianda -- That part is totally normal. If you're using a registry that doesn't implement either the latest standards _or_ range requests, then we have to download a wheel in order to determine its dependencies.


---

_Comment by @jpedrick-numeus on 2024-09-09 13:40_

> @ewianda -- That part is totally normal. If you're using a registry that doesn't implement either the latest standards _or_ range requests, then we have to download a wheel in order to determine its dependencies.

@charliermarsh does UV have to download every version though? It's a bit unexpected since the version is right in the path:
```https://some-server.d.codeartifact.eu-west-1.amazonaws.com/pypi/python/simple/mypackage/1.7.19/mypackage-1.7.19-py3-none-linux_x86_64.whl#sha256=aaaaaa47deb6b27795f516e13431acffc1f0197cb9274a1ef7d2b6ae40zzzzz9```

It makes sense to download full packages for all valid versions, but filtering the versions should happen before downloading(I think).

---

_Comment by @charliermarsh on 2024-09-09 13:41_

We're not requesting the _version_. We need the _dependencies_ in order to compute an accurate resolution.

---

_Comment by @charliermarsh on 2024-09-09 13:42_

(For example, if you run with `--no-deps` or use `uv pip sync`, we'll only download the necessary wheels.)

---

_Comment by @notatallshaw on 2024-09-09 13:44_

Does pip perform any differently here?

Without access to logs it's not possible to figure out if uv is making an "efficient" resolution, and if not, why not. There are weaknesses in both the way uv and pip perform resolution, but they are different.

---

_Comment by @jpedrick-numeus on 2024-09-09 13:46_

> (For example, if you run with `--no-deps` or use `uv pip sync`, we'll only download the necessary wheels.)

Unfortunately, this goes a bit too far... I want `uv pip install` to look into the deps, but please check versions first. In my case this causes 4-5 Gb of downloads for a single package when uv could easily know the versions it's downloading will be filtered later.

---

_Comment by @charliermarsh on 2024-09-09 13:47_

Candidly I think your diagnosis is not quite correct. We _already_ know the versions when we request those URLs. So whatever you're seeing is something else. I can help if you share the `--verbose` logs, but otherwise I can only speculate, sorry.

---

_Comment by @charliermarsh on 2024-09-09 13:50_

I'd love to make this better I just need a little more data.

---

_Comment by @jpedrick-numeus on 2024-09-09 13:52_

> I'd love to make this better I just need a little more data.

Thanks! I'll respond with the verbose logs when I get a chance today

---

_Comment by @ewianda on 2024-09-09 14:00_

This is [a log](https://github.com/user-attachments/files/16625226/uv.log) I generated in https://github.com/astral-sh/uv/issues/6104, Not sure if that is usefull

I see this 

```
 28.505574s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for pydantic-core==2.20.1; downloading wheel directly (range requests are not supported)
     31.928460s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.65.2; downloading wheel directly (range requests are not supported)
     35.333905s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.65.1; downloading wheel directly (range requests are not supported)
     38.559191s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.65.0; downloading wheel directly (range requests are not supported)
     42.058716s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.63.2; downloading wheel directly (range requests are not supported)
     42.104684s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.64.1; downloading wheel directly (range requests are not supported)
     42.186745s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.64.0; downloading wheel directly (range requests are not supported)
     42.226790s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.64.3; downloading wheel directly (range requests are not supported)
     42.232728s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.63.0; downloading wheel directly (range requests are not supported)
     45.606925s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.62.0; downloading wheel directly (range requests are not supported)
     45.607242s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.62.2; downloading wheel directly (range requests are not supported)
     45.607368s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.60.2; downloading wheel directly (range requests are not supported)
     45.680231s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.59.3; downloading wheel directly (range requests are not supported)
     45.704318s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.61.3; downloading wheel directly (range requests are not supported)
     45.706239s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.62.3; downloading wheel directly (range requests are not supported)
     45.764227s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.60.1; downloading wheel directly (range requests are not supported)
     45.774340s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.59.5; downloading wheel directly (range requests are not supported)
     45.788117s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.62.1; downloading wheel directly (range requests are not supported)
     45.868474s   1s  WARN uv_distribution::distribution_database Range requests unsupported when fetching metadata for grpcio-status==1.60.0; downloading wheel directly (range requests are not supported)

```

---

_Comment by @charliermarsh on 2024-09-09 14:09_

So what's happening there is that we're finding that we have to try a _lot_ of `grpcio-status` versions during the resolution. When that happens, we often prefetch them in bulk to speed things up. This probably _isn't_ worth doing when a registry doesn't support range requests _or_ `.metadata` endpoints, I bet it's more harmful than helpful, so we could try to turn it off in those cases.

---

_Comment by @charliermarsh on 2024-09-09 14:09_

(The batch prefetching is _extremely_ helpful in most cases because fetching metadata is usually cheap.)

---

_Comment by @notatallshaw on 2024-09-09 14:35_

I've seen reported cases, on the pip side, where packages are multiple GB big and they are the cause of backtracking, so you should probably avoid prefetching whole wheels.

Unless there's some way to reliably detect what size it is before downloading, as on the other size end I do notice with some wheels the the majority of the size is the metadata, so if it's only small, like 50 KiB or less, you could probably make an exception there.

---

_Comment by @notatallshaw on 2024-09-09 23:42_

In case others missed it, @charliermarsh already made this change https://github.com/astral-sh/uv/pull/7226 and a new uv was released https://github.com/astral-sh/uv/releases/tag/0.4.8

---

_Comment by @jpedrick-numeus on 2024-09-10 14:47_

@notatallshaw @charliermarsh this fixes the issue for me! Thanks!

---

_Closed by @zanieb on 2024-10-21 21:22_

---

_Comment by @cameron-git on 2025-07-24 17:21_

I am having a similar issue.
On my 1000mbit home internet uv takes ages to install torch seemingly capped at 1MiB/s while pip takes seconds.
On my 100mbit office network uv was as fast as pip.

I also noticed that installing new python versions is much slower on my home network.

---

_Comment by @notatallshaw on 2025-07-29 15:37_

@cameron-git please open a new issue with your specific details if you want it to be looked at, this is an old issue that was resolved along time ago. 

---
