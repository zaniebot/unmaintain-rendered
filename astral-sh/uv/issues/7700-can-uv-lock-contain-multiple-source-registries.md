---
number: 7700
title: "Can `uv.lock` contain multiple source registries for a single package?"
type: issue
state: open
author: zmeir
labels:
  - enhancement
assignees: []
created_at: 2024-09-26T08:50:44Z
updated_at: 2025-02-28T09:39:51Z
url: https://github.com/astral-sh/uv/issues/7700
synced_at: 2026-01-10T01:24:18Z
---

# Can `uv.lock` contain multiple source registries for a single package?

---

_Issue opened by @zmeir on 2024-09-26 08:50_

I've been experimenting a bit with `uv lock` and came across a use-case that I can't seem to resolve.

My project is developed on python 3.10/3.11/3.12, and I have a dependency on a package that only has wheel distributions in pypi.org for 3.10 and 3.11, but not for 3.12. So when installing the package on 3.12 it has to be built from source distribution, which takes a very long time (~10 minutes).  
Eventually the build succeeds and my code works fine with this version, but I want to speed up my CI, so I looked up in `uv cache dir`, found the wheel, and then `uv publish`ed it to my private pypi index.  

The problem is that now when I `uv lock` (with `--index-strategy=unsafe-best-match` and `--extra-index-url=<my_private_index_url>`), the generated `uv.lock` file only lists the wheels from my private index, without the rest of the wheels from pypi.org.

This is a an example for my `pyproject.toml` before adding my private index and wheel:
```toml
[project]
name = "test-uv-lock"
version = "0.0.dev0"
requires-python = ">=3.10"
dependencies = [
    "pandas>=1.3.4,<2; python_version <= '3.10'",
    "pandas>=1.5.2,<2; python_version == '3.11'",
    "pandas>=1.5.3,<2; python_version >= '3.12'",
]
```

And this is the `[[package]]` section for `pandas` in `uv.lock` after running `uv lock`:
```
[[package]]
name = "pandas"
version = "1.5.3"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "numpy" },
    { name = "python-dateutil" },
    { name = "pytz" },
]
sdist = { url = "https://files.pythonhosted.org/packages/74/ee/146cab1ff6d575b54ace8a6a5994048380dc94879b0125b25e62edcb9e52/pandas-1.5.3.tar.gz", hash = "sha256:74a3fd7e5a7ec052f183273dc7b0acd3a863edf7520f5d3a1765c04ffdb3b0b1", size = 5203060 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/a9/cd/34f6b0780301be81be804d7aa71d571457369e6131e2b330af2b0fed1aad/pandas-1.5.3-cp310-cp310-macosx_10_9_universal2.whl", hash = "sha256:3749077d86e3a2f0ed51367f30bf5b82e131cc0f14260c4d3e499186fccc4406", size = 18619230 },
    { url = "https://files.pythonhosted.org/packages/5f/34/b7858bb7d6d6bf4d9df1dde777a11fcf3ff370e1d1b3956e3d0fcca8322c/pandas-1.5.3-cp310-cp310-macosx_10_9_x86_64.whl", hash = "sha256:972d8a45395f2a2d26733eb8d0f629b2f90bebe8e8eddbb8829b180c09639572", size = 11982991 },
    { url = "https://files.pythonhosted.org/packages/b8/6c/005bd604994f7cbede4d7bf030614ef49a2213f76bc3d738ecf5b0dcc810/pandas-1.5.3-cp310-cp310-macosx_11_0_arm64.whl", hash = "sha256:50869a35cbb0f2e0cd5ec04b191e7b12ed688874bd05dd777c19b28cbea90996", size = 10927131 },
    { url = "https://files.pythonhosted.org/packages/27/c7/35b81ce5f680f2dac55eac14d103245cd8cf656ae4a2ff3be2e69fd1d330/pandas-1.5.3-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:c3ac844a0fe00bfaeb2c9b51ab1424e5c8744f89860b138434a363b1f620f354", size = 11368188 },
    { url = "https://files.pythonhosted.org/packages/49/e2/79e46612dc25ebc7603dc11c560baa7266c90f9e48537ecf1a02a0dd6bff/pandas-1.5.3-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:7a0a56cef15fd1586726dace5616db75ebcfec9179a3a55e78f72c5639fa2a23", size = 12062104 },
    { url = "https://files.pythonhosted.org/packages/d9/cd/f27c2992cbe05a3e39937f73a4be635a9ec149ec3ca4467d8cf039718994/pandas-1.5.3-cp310-cp310-win_amd64.whl", hash = "sha256:478ff646ca42b20376e4ed3fa2e8d7341e8a63105586efe54fa2508ee087f328", size = 10362473 },
    { url = "https://files.pythonhosted.org/packages/e2/24/a26af514113fd5eca2d8fe41ba4f22f70dfe6afefde4a6beb6a203570935/pandas-1.5.3-cp311-cp311-macosx_10_9_universal2.whl", hash = "sha256:6973549c01ca91ec96199e940495219c887ea815b2083722821f1d7abfa2b4dc", size = 18387750 },
    { url = "https://files.pythonhosted.org/packages/53/c9/d2f910dace7ef849b626980d0fd033b9cded36568949c8d560c9630ad2e0/pandas-1.5.3-cp311-cp311-macosx_10_9_x86_64.whl", hash = "sha256:c39a8da13cede5adcd3be1182883aea1c925476f4e84b2807a46e2775306305d", size = 11868668 },
    { url = "https://files.pythonhosted.org/packages/b0/be/1843b9aff84b98899663e7cad9f45513dfdd11d69cb5bd85c648aaf6a8d4/pandas-1.5.3-cp311-cp311-macosx_11_0_arm64.whl", hash = "sha256:f76d097d12c82a535fda9dfe5e8dd4127952b45fea9b0276cb30cca5ea313fbc", size = 10814036 },
    { url = "https://files.pythonhosted.org/packages/63/8d/c2bd356b9d4baf1c5cf8d7e251fb4540e87083072c905430da48c2bb31eb/pandas-1.5.3-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:e474390e60ed609cec869b0da796ad94f420bb057d86784191eefc62b65819ae", size = 11374218 },
    { url = "https://files.pythonhosted.org/packages/56/73/3351beeb807dca69fcc3c4966bcccc51552bd01549a9b13c04ab00a43f21/pandas-1.5.3-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:5f2b952406a1588ad4cad5b3f55f520e82e902388a6d5a4a91baa8d38d23c7f6", size = 12017319 },
    { url = "https://files.pythonhosted.org/packages/da/6d/1235da14daddaa6e47f74ba0c255358f0ce7a6ee05da8bf8eb49161aa6b5/pandas-1.5.3-cp311-cp311-win_amd64.whl", hash = "sha256:bc4c368f42b551bf72fac35c5128963a171b40dce866fb066540eeaf46faa003", size = 10303385 },
]
```

As you can see - there are no wheels listed for python 3.12.

Now this is my modified `pyproject.toml` after building a wheel for `pandas==1.5.3` on python 3.12 and publishing it to my private pypi index:
```toml
[project]
name = "test-uv-lock"
version = "0.0.dev0"
requires-python = ">=3.10"
dependencies = [
    "pandas>=1.3.4,<2; python_version <= '3.10'",
    "pandas>=1.5.2,<2; python_version == '3.11'",
    "pandas>=1.5.3,<2; python_version >= '3.12'",
]

[tool.uv]
index-strategy = "unsafe-best-match"
extra-index-url = [
    "https://my-private-pypi-repo.org/simple",
]
```

And this is the `[[package]]` section for `pandas` in `uv.lock` after running `uv lock`:
```
[[package]]
name = "pandas"
version = "1.5.3"
source = { registry = "https://my-private-pypi-repo.org/simple" }
dependencies = [
    { name = "numpy" },
    { name = "python-dateutil" },
    { name = "pytz" },
]
wheels = [
    { url = "https://my-private-pypi-repo.org/simple/pandas/1.5.3/pandas-1.5.3-cp312-cp312-linux_x86_64.whl", hash = "sha256:6081e82b662d5109a7ba779063f947794e67be96a0ceac0aaf76121db56f096c" },
]
```

As you can see:
1. There is only one `source` registry which is my private index.
2. There is no `sdist` because I didn't publish the source distribution to my private index.
3. There is only one `wheel` listed - the one from my private index.

Currently my workaround for this is to download all the other wheels and sdist from the original lock file and publish them to my private repo, but that seems like a very brittle solution:
1. What do I do when I need to change dependency versions or face this issue with another package? Download and publish all the wheels again?
2. What if a wheel is published to pypi.org for some platform I didn't yet have the wheels for?

TL;DR - Can the `source` entry in the `[[pacakge]]` part of the `uv.lock` file support more than one `registry`?

Ran all of these examples with `uv==0.4.16`
```
$ uv --version
uv 0.4.16 (e81ed8ec5 2024-09-24)
```


---

_Assigned to @charliermarsh by @zanieb on 2024-09-26 12:21_

---

_Label `enhancement` added by @charliermarsh on 2024-09-28 19:08_

---

_Comment by @charliermarsh on 2024-09-28 19:09_

I think we can support this in the near future but you'll need to indicate (in the `pyproject.toml`) which platforms should pull from your index vs. which should not.

---

_Comment by @zmeir on 2024-09-29 07:59_

Thanks @charliermarsh !

How will that work with internal packages that are only available through my private index?

In my real example, my project doesn't depend on `pandas` directly, but rather on some internal package `foo`, which in turn depends on `pandas`.  
So when I generate the `uv.lock` file I want to get:
1. All of `foo`'s ditributions from my private index
2. The custom wheels I built for `pandas` from my private index (specifically for a couple of platforms on python 3.12)
3. All the rest of of the distributions for `pandas` (and any other packages) from pypi.org

Does that make sense?

---

_Comment by @zmeir on 2024-10-17 18:23_

Hey @charliermarsh 

Just saw #7769 was released in `0.4.23` and it seems like it could really help with my issue.

There's one thing I'm having trouble understanding:  
In my scenario, I only want to pull `pandas` from my private index for a specific version (1.5.3) and specific platforms (macos and linux).  
From the documentation on `[tool.uv.sources]` it looks like only the platform restriction part of my scenario is possible, but there is no way to specify that my private index should only be used for `pandas==1.5.3`, while for the rest of the versions the default index should be used. Am I missing something or is this true?

Regardless, thanks for all the hard work!

---

_Comment by @charliermarsh on 2024-10-17 21:26_

Yeah you can do something like this to pull Pandas from your custom index only on macOS (Darwin):

```toml
[project]
name = "test-uv-lock"
version = "0.0.dev0"
requires-python = ">=3.10"
dependencies = [
    "pandas"
]

[[tool.uv.index]]
name = "private"
url = "https://my-private-pypi-repo.org/simple"
explicit = true

[tool.uv.sources]
pandas = { index = "private", markers = "sys_platform == 'darwin'" }
```


---

_Comment by @charliermarsh on 2024-10-17 21:28_

I think the thing I don't follow from your request is: the lockfile is going to lock to a single version for a given platform. It's either going to be 1.5.3 or not. So what would it mean to pull 1.5.3 from the index, but other versions from some other index (on the same platform)?

---

_Comment by @zmeir on 2024-10-18 07:35_

I generate 2 lockfiles- one with the default resolution (highest) and another with `--resolution lowest-direct`. With the lowest resolution I get `pandas==1.5.3`, for which I want to use my prebuilt wheel in my private index, but for the highest resolution I can potentially get a higher `pandas` version, for which I want to use pypi.org as the index. I'm guessing I can tweak the command I use for the lowest resolution to include the extra information I need to control the index, without affecting the highest resolution, but I was hoping to find a way to contain all the necessary information inside the `pyproject.toml` itself. This way I can have a generic CI process for all projects that uses the same lock commands, and the specific differences are contained within the `pyproject.toml` of each project.

---

_Comment by @zmeir on 2024-10-20 13:05_

I'll try to elaborate on my use-case with another example.

Let's look at the following `pyproject.toml`:
```toml
[project]
name = "test-uv-lock"
version = "0.0.dev0"
requires-python = ">=3.12"
dependencies = [
    "pandas==1.5.3; python_version == '3.12.*'",
    "pandas==2.2.3; python_version == '3.13.*'",
]


[[tool.uv.index]]
name = "private"
url = "https://my-private-pypi-repo.org/simple"
explicit = true

[tool.uv.sources]
pandas = { index = "private", marker = "sys_platform == 'darwin' and python_version == '3.12.*'" }
```

As it currently stands, this `pyproject.toml` works exactly the way I need:
1. For python version 3.12 on MacOS - I pull the wheel for `pandas==1.5.3` from my private index
2. For all other platforms or for python version 3.13 - I pull the wheel for `pandas` from pypi.org

But I fear this setup will cause me much pain as I continue to develop and upgrade my pacakge:  
The strict `pandas==` version restrictions are too limiting since I am developing a library. Now if I change `pandas==1.5.3` to `pandas>=1.5.3`, I have to make sure to build and publish a python 3.12 wheel for all future versions of `pandas` to my private index, otherwise if I add another dependency that will cause the resolver to select a version of pandas that is not `1.5.3`, it won't exist in my index and the resolution will likely fail.

I hope this helps to better illustrate the situation.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-02-21 09:24_

---

_Unassigned @jtfmumm by @jtfmumm on 2025-02-28 09:39_

---
