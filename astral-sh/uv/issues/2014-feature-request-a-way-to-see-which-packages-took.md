```yaml
number: 2014
title: "[Feature request] A way to see which packages took long to resolve"
type: issue
state: open
author: alexandervaneck
labels:
  - enhancement
  - resolver
assignees: []
created_at: 2024-02-27T13:53:23Z
updated_at: 2024-03-07T11:37:42Z
url: https://github.com/astral-sh/uv/issues/2014
synced_at: 2026-01-12T15:58:34Z
```

# [Feature request] A way to see which packages took long to resolve

---

_@alexandervaneck_

Dear team 🙇‍♂️ !

Thank you for this amazing development and congratulations with the launch of `uv`. I'm following your work closely and am eager to bake it into many projects!

I've stumbled upon a small feature request that I hope you could implement/muse about.

**The situation**: I live in dependency hell 🔥 

A typical project that I maintain has ~250 (transient) python dependencies it uses. These are larger AI projects that unfortunately are all currently monolith with no real designed way to change that. Try installing `lightning` (from `torch`/`pytorch_lightning`) and you'll see what I mean.

`pip` has a hard time downloading all these dependencies due to the endless configurations that all need to be tried. f.e. `fastapi` is depended upon several times and this creates many download requests.

```
$ time pip install -r requirements.txt -r requirements-test.txt --no-cache-dir
...
real	4m43.482s
user	1m28.243s
sys	0m7.930s

$ pip freeze | wc -l
230
```

Due to this scenario `uv` isn't much faster because it will have to do the same loops as `pip` to figure it out.

```
$ time uv pip install -r requirements.txt -r requirements-test.txt --no-cache-dir
...
Resolved 228 packages in 2m 24s
...
Downloaded 227 packages in 2m 08s
Installed 227 packages in 367ms
...
real	4m33.699s
user	1m25.720s
sys	0m54.672s

$ uv pip freeze | wc -l
231 (uv also installed)
```

**The feature request**: Print out which packages took longest to resolve.

If `uv` could print out a summary (upon request) of which dependencies took longest to figure out and settle that would be extremely helpful! I could then go in and hard-pin those dependencies to an exact version to shorten the dependency calculation time.

I suppose I am in part describing creation of a lock file 🤔 but I have little to no experience with that, your guidance would be appreciated. 🙇 

Thank you for your consideration.

P.S. I unfortunately cannot show the actual requirements.txt files since they contain many private packages that we maintain ourselves.



---

_Comment by @konstin on 2024-02-27 14:32_

I think we could log packages during resolution where we tried more than let's say 20 versions and where we had to build more than let's say 5 source dists. The bonus is you get to see as we know it and if you want, you can abort and try again with tighter constraints.

For your specific case, do urllib and boto3 without strict constraints show up in your dependency tree? This is the most common bad resolver case we see.

---

_Comment by @alexandervaneck on 2024-02-27 15:00_

Thank you @konstin for responding so quickly 🙇‍♂️ 

>I think we could log packages during resolution where we tried more than let's say 20 versions and where we had to build more than let's say 5 source dists. The bonus is you get to see as we know it and if you want, you can abort and try again with tighter constraints.

Yes, that would be handy! and solve my request. If those were configurable it would be even more flexible. 🙏 

>For your specific case, do urllib and boto3 without strict constraints show up in your dependency tree? This is the most common bad resolver case we see.

I ran `pipdeptree` to see very wide ranges for these packages.

```
boto3 [required: >=1.21.38,<2, installed: 1.34.50]
urllib3 [required: >=1,<3, installed: 2.0.7]
```

I cannot be sure which packages are the worst offenders (hence the request).


---

_Comment by @notatallshaw on 2024-02-27 15:06_

Do you have a sense for whether it's download time or resolution time that's taking up the bulk of either uv or pip's resolution?

Also, I know this is a uv issue, but it would be helpful for me, with an open PR I have on pip side, to understand if this pip branch significantly helps things or not: https://github.com/notatallshaw/pip/tree/prefer-conflicting-causes (you can install like so: `pip install git+https://github.com/notatallshaw/pip.git@prefer-conflicting-causes`

Also also, I have a very experimental branch of pip that tests an resolution idea that I'm suggesting for uv to try (preferring upper bounded requirements) but I make no promises about that branch e.g. it could break pip, but if you're willing I'd be curious of the results: https://github.com/notatallshaw/pip/tree/backjump-to-upper-bounded

---

_Label `enhancement` added by @konstin on 2024-02-27 15:51_

---

_Label `resolver` added by @zanieb on 2024-02-27 15:53_

---

_Comment by @konstin on 2024-02-27 15:58_

It can be both, for the slow cases i have investigated (i can't repro this one, this is a guess from other requirements): For the first (uncached) run we're bound by network times since we make a request for each version. We'd like to improve this with prefetching when we see that we already fetched a lot of versions of the same package. For the cached case, we currently run into a bottleneck with pubgrub-rs where its range implementation is slow. pubgrub-rs stores ranges as exclusions of versions that , so we perform all operations including backtracking on a large list of `!=`s, which gets slow. 

I'd be cool if we could fix boto by just changing the selection strategy; I'm fine with whatever order we do as long as it's deterministic (e.g. we can't decide based on raced network requests).

---

_Comment by @alexandervaneck on 2024-02-27 16:11_

>Do you have a sense for whether it's download time or resolution time that's taking up the bulk of either uv or pip's resolution?

I suppose it's the resolution time (given the output from above). The download time is linked to the resolution time as we are downloading all those versions that we think may work but then find out actually don't. Right?

That being said, downloading 230 packages can be a lot.

I managed `du -sh <path-to-site-packages>* | sort -hr`, for a total of ~6.1GB of dependencies. The `torch` package is cached in the docker image I'm using, so it should download ~4.6GB. My connection is ~250Mb/s  so I'd expect that to download in about (4600MB/(250Mbit/8))=~150 seconds or around 2.5 minutes. and that's exactly what `uv` shows above as download time!

<details>
2.8G	nvidia
1.5G	torch
419M	triton
124M	pyarrow
90M	opencv_python.libs
83M	scipy
79M	polars
79M	plotly
77M	cv2
63M	opencv_python_headless.libs
51M	rasterio.libs
46M	pandas
42M	nbclassic
40M	sklearn
38M	matplotlib
37M	numpy.libs
37M	babel
35M	scipy.libs
33M	skimage
29M	6c7190bc8b55ffe67f57__mypyc.cpython-310-x86_64-linux-gnu.so
28M	numpy
27M	sympy
27M	jupyter_contrib_nbextensions
24M	fontTools
23M	botocore
21M	lxml
20M	jupyterlab
18M	debugpy
17M	ddtrace
15M	rasterio
14M	pip
12M	faker
11M	torchvision
11M	jedi
9.7M	wandb
9.1M	mypy
8.9M	pillow.libs
8.4M	torchvision.libs
8.1M	pygments
7.7M	timm
7.2M	jupyterlab_plotly
7.1M	networkx
6.5M	lightning
6.4M	kiwisolver
6.1M	asdf
5.9M	IPython
5.4M	pydantic_core
5.0M	lightning_cloud
5.0M	PIL
4.9M	safetensors
4.7M	mypyc
4.5M	setuptools
4.5M	aiohttp
4.4M	shapely.libs
4.2M	rpds
4.2M	30fcd23745efe32ce681__mypyc.cpython-310-x86_64-linux-gnu.so
4.1M	shapely
3.7M	typeshed_client
3.7M	notebook
3.6M	imgaug
3.5M	torchmetrics
3.3M	tornado
3.2M	prompt_toolkit
2.8M	jupyter_server
2.7M	tzdata
2.7M	pytz
2.6M	zmq
2.6M	yaml
2.3M	torchgen
2.2M	pyzmq.libs
2.1M	mpmath
1.8M	psutil
1.7M	pydantic
1.5M	torch-2.2.1.dist-info
1.5M	huggingface_hub
1.4M	pkg_resources
1.4M	bleach
1.3M	plotly-5.19.0.dist-info
1.3M	mpl_toolkits
1.3M	joblib
1.3M	imageio
1.3M	_pytest
1.2M	seaborn
1.2M	google
1.2M	bs4
1.2M	boto3
1.2M	asdf_transform_schemas
1.1M	sentry_sdk
1.1M	rich
1.1M	pycparser
1.1M	jupyter_client
1.1M	hydra
1.1M	contourpy
976K	_jpype.cpython-310-x86_64-linux-gnu.so
968K	yarl
</details>


> Also, I know this is a uv issue, but it would be helpful for me, with an open PR I have on pip side, to understand if this pip branch significantly helps things or not: https://github.com/notatallshaw/pip/tree/prefer-conflicting-causes

```
$ pip install git+https://github.com/notatallshaw/pip.git@prefer-conflicting-causes
$ time pip install -r requirements.txt -r requirements-test.txt --no-cache-dir
...
real	4m47.995s
user	1m23.331s
sys	0m8.356s
```

No significant change.

> Also also, I have a very experimental branch of pip that tests an resolution idea

How does this relate to `uv pip install --resolution`?

```
$ pip install git+https://github.com/notatallshaw/pip.git@backjump-to-upper-bounded
$ time pip install -r requirements.txt -r requirements-test.txt --no-cache-dir
...
real	4m47.682s
user	1m22.664s
sys	0m7.578s
```

No significant change.


---

_Comment by @notatallshaw on 2024-02-27 16:23_

> The download time is linked to the resolution time as we are downloading all those versions that we think may work but then find out actually don't. Right?

Well there's a recent innovation where there can be a metadata file separate from the wheel itself, so in the case of lots of backtracking only the metadata needs to be downloaded until the solution has been found. But the index needs to support it (PyPI does but it hasn't applied it to all older files yet, I haven't seen other indexes support it yet).

Thanks for checking my branches, shame they doesn't help your resolution times.

---

_Comment by @alexandervaneck on 2024-02-27 16:32_

>Well there's a recent innovation where there can be a metadata file separate from the wheel itself, so in the case of lots of backtracking only the metadata needs to be downloaded until the solution has been found. But the index needs to support it (PyPI does but it hasn't applied it to all older files yet, I haven't seen other indexes support it yet).

Oh! That would be amazing :) We're using Artifactory as a private pypi, with read-through. Do you know if it supports it?

---

_Comment by @konstin on 2024-02-27 20:42_

If there isn't supported by the index for metadata files, we try to read only the metadata from a remote wheel using range requests (pip does the same thing iirc). This is standard HTTP but depends on the server supporting it ([mdn has info how to check](https://developer.mozilla.org/en-US/docs/Web/HTTP/Range_requests#checking_if_a_server_supports_partial_requests)). I unfortunately don't know if artifactory supports this.

---

_Comment by @notatallshaw on 2024-02-27 21:49_

> We're using Artifactory as a private pypi, with read-through. Do you know if it supports it?

I've not used Artifactory in a few years, but as I understand it there would be two different parts to could support it:

1. Allowing the metadata files from PyPI (and other indexes in the future) read through 
2. Having the Artifactory generate metadata files for internal packages

For 1 this will be a configuration issue, you might have to allow "\*.metadata" files to read through, the same as "\*.whl" files. I don't know if this is set up by default for a PyPI configured repo or not.

For 2 this is something Artifactory would need to impelement.

> we try to read only the metadata from a remote wheel using range requests (pip does the same thing iirc). This is standard HTTP but depends on the server supporting it

Artifactory might have implemented this, but in most configurations it acts a caching proxy, so it would have to handle both cases of where it has to go out and get the file and where it has already cached the file permanently. I'm sure at some point the Artifactory team will post any comaptability issues they have with uv, they have worked with pip in the past.

---

_Comment by @kdebrab on 2024-02-28 07:49_

@AlexanderVanEck Do you happen to have also s3fs as a dependency? If so, including it as s3fs[boto3], while removing the other boto3 entry from requirements.txt may relieve you from dependency hell. At least, it did so for us about a year ago. See also https://github.com/aio-libs/aiobotocore/issues/983.

---

_Comment by @alexandervaneck on 2024-02-28 08:02_

I don't see `s3fs` as a dependency, so that's not the problem @kdebrab . Thank you for the suggestion!

---

_Comment by @konstin on 2024-02-28 10:29_

I've put up #2041 to inform the user when we try a lot of versions.

@AlexanderVanEck Could you try running with `-v` and check for "Range requests not supported" in the output? It would be helpful for me to know if that's the cause for the slow resolution.

---

_Comment by @alexandervaneck on 2024-02-28 14:25_

> Could you try running with -v and check for "Range requests not supported" in the output? It would be helpful for me to know if that's the cause for the slow resolution.

It's as you suspected. I see "Range requests not supported" showing up in the logs a lot. I suppose this means that Artifactory does not support it.

For fun I decided to ran `uv pip compile -o requirements.compiled requirements.txt requirements-test.txt` so I could check which dependencies are mentioned a lot. Then I wrote a small script that shows how many other dependencies depend on them.

<details>
1: yarl==1.9.4
1: zipp==3.17.0
3: aiohttp==3.9.3
3: antlr4-python3-runtime==4.9.3
3: botocore==1.34.51
3: deepdiff==6.7.1
3: deprecated==1.2.14
3: flake8==7.0.0
3: frozenlist==1.4.1
3: hydra-core==1.3.2
3: imageio==2.34.0
3: lightning-utilities==0.10.1
3: lightning==1.8.6
3: matplotlib==3.8.3
3: multidict==6.0.5
3: networkx==3.2.1
3: nvidia-cusparse-cu12==12.1.0.106
3: nvidia-nvjitlink-cu12==12.3.101
3: omegaconf==2.3.0
3: polars-lts-cpu==0.20.11
3: polars==0.20.11
3: pydantic==2.6.3
3: pyjwt==2.8.0
3: pyparsing==3.1.1
3: referencing==0.33.0
3: rpds-py==0.18.0
3: semantic-version==2.10.0
3: shapely==2.0.3
3: starlette==0.36.3
3: tenacity==8.2.3
3: tifffile==2022.10.10
3: torchmetrics==1.2.1
3: wandb==0.16.2
3: xmod==1.8.1
4: asdf-standard==1.0.3
4: certifi==2024.2.2
4: dacite==1.8.1
4: exceptiongroup==1.2.0
4: filelock==3.13.1
4: fsspec==2024.2.0
4: idna==3.6
4: jmespath==1.0.1
4: mypy-extensions==1.0.0
4: nvidia-cublas-cu12==12.1.3.1
4: stringcase==1.2.0
5: opencv-python==4.7.0.72
5: pandas==2.2.1
5: protobuf==4.25.3
5: pytest==7.4.4
5: scikit-learn==1.4.1.post1
5: tomli==2.0.1
5: torchvision==0.17.1
6: asdf==2.15.2
6: boto3==1.34.51
6: environs==9.5.0
6: psutil==5.9.8
6: scikit-image==0.22.0
7: opencv-python-headless==4.8.1.78
7: requests==2.31.0
7: scipy==1.12.0
7: setuptools==69.1.1
7: tqdm==4.66.2
7: urllib3==2.0.7
8: six==1.16.0
9: attrs==23.2.0
9: python-dateutil==2.8.2
10: click==8.1.7
10: torch==2.2.1
11: pillow==10.2.0
11: pyyaml==6.0.1
17: packaging==23.2
20: typing-extensions==4.10.0
31: numpy==1.26.4
</details>

Numpy is mentioned by 31(!) other dependencies. Looking at the graph that pipdeptree outputs I can understand why `uv` would have a difficult time...

<details>
├── numpy [required: >=1.21, installed: 1.26.4]
│   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.2,<3.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >1.20.0, installed: 1.26.4]
│   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: >=1.22.4,<2, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │       └── numpy [required: >=1.19.2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.19.5,<2.0, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: >1.20.0, installed: 1.26.4]
│   │   ├── numpy [required: Any, installed: 1.26.4]
│   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   ├── numpy [required: Any, installed: 1.26.4]
│   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: >=1.11.1, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   ├── numpy [required: >=0.18.0, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.19.5,<2.0, installed: 1.26.4]
│   │   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │       └── numpy [required: >=1.19.2, installed: 1.26.4]
│   │       └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.20,<2.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21,<2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.15, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │       └── numpy [required: >=1.19.2, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.14,<2, installed: 1.26.4]
│   ├── numpy [required: >=1.22,<2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22.4,<2, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.19.5,<2.0, installed: 1.26.4]
│   │   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.14,<2, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   │   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: >=1.22.4,<2, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │       └── numpy [required: >=1.19.2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.19.5,<2.0, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.20,<2.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21,<2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.20,!=1.24.0, installed: 1.26.4]
│   │       ├── numpy [required: >=1.22.4,<2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22.4,<2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.19.5,<2.0, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   └── numpy [required: >=1.14,<2, installed: 1.26.4]
│   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.2,<3.0, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >1.20.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22.4,<2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.19.5,<2.0, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │       │   ├── numpy [required: Any, installed: 1.26.4]
│   │       ├── numpy [required: >=1.22, installed: 1.26.4]
│   │       │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │           └── numpy [required: >=1.19.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22.4,<2, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.19.5,<2.0, installed: 1.26.4]
│   │   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.14,<2, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.2,<3.0, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >1.20.0, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22.4,<2, installed: 1.26.4]
│   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │       └── numpy [required: >=1.19.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.19.5,<2.0, installed: 1.26.4]
│   │   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.22.4,<1.29.0, installed: 1.26.4]
│   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   │   ├── numpy [required: >1.20.0, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   ├── numpy [required: ~=1.21, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.21.2, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.0, installed: 1.26.4]
│   │   │   ├── numpy [required: >=1.17.3, installed: 1.26.4]
│   │   │   └── numpy [required: >=1.19.3, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   │   │   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   │   │   ├── numpy [required: Any, installed: 1.26.4]
│   │   │   │       ├── numpy [required: Any, installed: 1.26.4]
│   │   ├── numpy [required: >=1.22, installed: 1.26.4]
│   ├── numpy [required: ~=1.22, installed: 1.26.4]
│   │   ├── numpy [required: Any, installed: 1.26.4]
│   │       ├── numpy [required: Any, installed: 1.26.4]
    └── numpy [required: >=1.16.6,<2, installed: 1.26.4]
</details>

I hope this illustrates why it would be useful to understand which dependencies were particularly hard to find out 🙇 I cannot tell you if numpy actually is an issue 🤔 

---

_Comment by @konstin on 2024-02-28 14:34_

I suspect the problem is less that the dependency graph is too complex (making decisions in the resolver is generally very fast), but that for every version you try, uv has to download the entire wheel just to know which dependencies it has. So when we want to know what deps numpy has, we have a ~15MB web request, between 60MB and 755MB for checking a torch version, you get the gist (at least for scientific packages, pure python packages, a 80KB tqdm is still fast). With PEP 658 (pypi gives us metadata for some packages directly in the api) or range requests, it's a few kilobytes at most.

---

_Comment by @alexandervaneck on 2024-02-28 15:00_

Ah yes. That makes sense. Thank you @konstin 🙇 

---

_Comment by @alexandervaneck on 2024-02-28 15:05_

I believe it is tracked [here](https://jfrog.atlassian.net/browse/RTFACT-26891) for Artifactory to implement PEP-658. (For those who come onto this thread and also use Artifactory, not important for `uv`).

---

_Comment by @notatallshaw on 2024-02-28 17:31_

> I believe it is tracked [here](https://jfrog.atlassian.net/browse/RTFACT-26891) for Artifactory to implement PEP-658. (For those who come onto this thread and also use Artifactory, not important for `uv`).

I beleive thats to support Artifactory adding metadata files for interally uploaded packages. When proxying PyPI you should be able to configure it to pass through *.metadata files.

But its been years since I had to touch Artifactory configuration, so I cant directly advise.

---

_Comment by @alexandervaneck on 2024-02-28 18:02_

That makes sense 🙌 .

@notatallshaw Is there somewhere for me to track which packages do and do not (yet) have metadata available on pypi?


---

_Comment by @notatallshaw on 2024-02-28 21:19_

> Is there somewhere for me to track which packages do and do not (yet) have metadata available on pypi?

This issue: https://github.com/pypi/warehouse/issues/8254

---

_Comment by @konstin on 2024-03-07 11:37_

As a less intrusive version, #2270 will show some stats in the end when running with `-v`.

---
