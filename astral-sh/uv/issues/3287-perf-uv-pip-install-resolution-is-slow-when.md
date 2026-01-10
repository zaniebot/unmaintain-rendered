---
number: 3287
title: "[perf] uv pip install resolution is slow when installing from VCS"
type: issue
state: open
author: baggiponte
labels:
  - performance
assignees: []
created_at: 2024-04-27T09:11:33Z
updated_at: 2025-06-06T19:11:19Z
url: https://github.com/astral-sh/uv/issues/3287
synced_at: 2026-01-10T01:23:26Z
---

# [perf] uv pip install resolution is slow when installing from VCS

---

_Issue opened by @baggiponte on 2024-04-27 09:11_

Ciao! I am installing a library from VCS and I _feel_ it should be faster.

The library in question is functime, a forecasting library I maintain. I installed first a version from a [PR I am about to merge](https://github.com/functime-org/functime/pull/181), but then I realised it's just as slow if I just install from VSC.

I took the install command from the official [`pip` docs](https://pip.pypa.io/en/stable/topics/vcs-support/#git).

```bash
uv pip install --no-cache  -- "functime[plot,lgb] @ git+https://github.com/functime-org/functime.git@refs/pull/181/head"

 Updated https://github.com/functime-org/functime.git (7c699c2)                             
Resolved 21 packages in 37.10s
   Built functime @ git+https://github.com/functime-org/functime.git@7c699c2118c96a7799b9e2bbb077b25149beeac2
   Built lightgbm==4.3.0                                                                                                                                                                Downloaded 21 packages in 1m 49s
Installed 21 packages in 295ms
 + cloudpickle==3.0.0
 + flaml==2.1.2
 + functime==0.9.5 (from git+https://github.com/functime-org/functime.git@7c699c2118c96a7799b9e2bbb077b25149beeac2)
 + holidays==0.47
 + joblib==1.4.0
 + kaleido==0.2.1
 + lightgbm==4.3.0
 + numpy==1.26.4
 + packaging==24.0
 + pandas==2.2.2
 + plotly==5.21.0
 + polars==0.20.22
 + python-dateutil==2.9.0.post0
 + pytz==2024.1
 + scikit-learn==1.4.2
 + scipy==1.13.0
 + six==1.16.0
 + tenacity==8.2.3
 + threadpoolctl==3.4.0
 + tqdm==4.66.2
 + tzdata==2024.1
```

And how much it takes from regular git repo:

```bash
uv pip install --no-cache -- "functime[plot,lgb] @ git+https://github.com/functime-org/functime.git"
 Updated https://github.com/functime-org/functime.git (0608c78)
Resolved 21 packages in 37.95s
   Built functime @ git+https://github.com/functime-org/functime.git@0608c78118b5defd42df73b812
   Built lightgbm==4.3.0
Downloaded 21 packages in 1m 48s
Installed 21 packages in 282ms
 + cloudpickle==3.0.0
 + flaml==2.1.2
 + functime==0.9.5 (from git+https://github.com/functime-org/functime.git@0608c78118b5defd42df73b81288e0e7b32fdb59)
 + holidays==0.47
 + joblib==1.4.0
 + kaleido==0.2.1
 + lightgbm==4.3.0
 + numpy==1.26.4
 + packaging==24.0
 + pandas==2.2.2
 + plotly==5.21.0
 + polars==0.20.22
 + python-dateutil==2.9.0.post0
 + pytz==2024.1
 + scikit-learn==1.4.2
 + scipy==1.13.0
 + six==1.16.0
 + tenacity==8.2.3
 + threadpoolctl==3.4.0
 + tqdm==4.66.2
 + tzdata==2024.1
```

As a comparison, here is much it takes form PyPI, no cache:

```bash
uv pip install --no-cache -- 'functime[plot,lgb]'
Resolved 21 packages in 417ms
   Built lightgbm==4.3.0
Downloaded 21 packages in 30.98s
Installed 21 packages in 284ms
 + cloudpickle==3.0.0
 + flaml==2.1.2
 + functime==0.9.5
 + holidays==0.47
 + joblib==1.4.0
 + kaleido==0.2.1
 + lightgbm==4.3.0
 + numpy==1.26.4
 + packaging==24.0
 + pandas==2.2.2
 + plotly==5.21.0
 + polars==0.20.22
 + python-dateutil==2.9.0.post0
 + pytz==2024.1
 + scikit-learn==1.4.2
 + scipy==1.13.0
 + six==1.16.0
 + tenacity==8.2.3
 + threadpoolctl==3.4.0
 + tqdm==4.66.2
 + tzdata==2024.1
```

---

_Renamed from "[perf] uv pip install resolution is slow when installing from VCS (and branch) (?)" to "[perf] uv pip install resolution is slow when installing from VCS" by @baggiponte on 2024-04-27 09:11_

---

_Comment by @charliermarsh on 2024-04-27 11:25_

I think the problem might be that when you install from PyPI, they can serve you a wheel, which is a built artifact (since the uploader of the package uploaded wheels for it, for a bunch of platforms). But if you install from VCS, you're required to build the package from source. And building from source can be really long and expensive -- it completely depends on the package, we basically have to call out to their build method, which could involve compiling native code.

---

_Comment by @charliermarsh on 2024-04-27 11:25_

And [`functime`](https://pypi.org/project/functime/#files) does ship per-platform wheels which suggests they're compiling some native code.

---

_Comment by @baggiponte on 2024-04-27 12:09_

Ciao Charlie, thank you very much for the prompt reply.

> And [functime](https://pypi.org/project/functime/#files) does ship per-platform wheels which suggests they're compiling some native code.

Yes, we have some Rust plugins for Polars!

> I think the problem might be that when you install from PyPI, they can serve you a wheel, which is a built artifact (since the uploader of the package uploaded wheels for it, for a bunch of platforms). But if you install from VCS, you're required to build the package from source. And building from source can be really long and expensive -- it completely depends on the package, we basically have to call out to their build method, which could involve compiling native code.

Indeed! I should've been more precise, sorry. What bugged me was the resolution time:

From VCS:

```diff
+Resolved 21 packages in 37.95s
   Built functime @ git+https://github.com/functime-org/functime.git@0608c78118b5defd42df73b812
   Built lightgbm==4.3.0
Downloaded 21 packages in 1m 48s
Installed 21 packages in 282ms
```

From PyPI:

```diff
+Resolved 21 packages in 417ms
   Built lightgbm==4.3.0
Downloaded 21 packages in 30.98s
Installed 21 packages in 284ms
```

You say that in the first case it's 38s because it has to download and build the binary? Couldn't `uv` try to fetch `pyproject.toml` to perform resolution first? I guess the overall time would not change, since build would need to happen anyway.

Feel free to close the issue.

---

_Comment by @charliermarsh on 2024-04-27 12:13_

Ahh I see! Let me take a look -- we should be able to clone the repo and read the metadata without building the wheel in this case. (But we do need to clone it, we don't do selective reads (e.g., _just_ checkout the `pyproject.toml`) from Git.)

---

_Comment by @charliermarsh on 2024-04-27 12:15_

Mmm I think for me basically the entire time is spent cloning the repo. That's a bummer. Maybe a datapoint for @ibraheemdev when it comes to seeing if we can make clones any faster.

---

_Label `performance` added by @charliermarsh on 2024-04-27 12:15_

---

_Comment by @charliermarsh on 2024-04-27 12:16_

(But I confirmed that we _do_ read the metadata from `pyproject.toml`, and we _don't_ build the wheel, which is good.)

---

_Comment by @baggiponte on 2024-04-27 15:45_

Very thorough, thanks!

> (But we do need to clone it, we don't do selective reads (e.g., just checkout the pyproject.toml) from Git.)

Why this? I am incredibly naive on the parallelisation side of things, but if you managed to collect the list of requirements from pyproject.toml then you could parallelise the build and download/installation.

---

_Comment by @charliermarsh on 2024-04-27 15:49_

If we have a Git dependency, the first step is that we need to clone the repo. Then we read the `pyproject.toml` if it exists. Perhaps in theory we could try to _only_ clone `pyproject.toml` (we can't know whether it exists in advance, but we could try), I don't know if it's even possible fetch a single file from Git though. Maybe if we know it's on GitHub, we could try to add a fast path for it by downloading the file directly rather than using a Git client.


---

_Comment by @notatallshaw on 2024-04-27 20:26_

> Mmm I think for me basically the entire time is spent cloning the repo. That's a bummer. Maybe a datapoint for @ibraheemdev when it comes to seeing if we can make clones any faster.

FYI pip uses blobless clones with git to make performance faster: https://github.com/pypa/pip/pull/9086. Maybe uv is already doing this, but thought I'd mention just in case.

---

_Comment by @hmc-cs-mdrissi on 2024-04-27 21:15_

Shallow clones(—depth=1) can also give nice speed up. However they have caveats as some libraries (setuptools_scm) relies on git metadata that shallow clones will not have. https://github.com/pypa/pip/issues/2432 this discusses issue more in depth. Shallow cloning if done likely needs a flag to control it to handle situations where full clone is necessary.

---

_Comment by @bluss on 2024-04-27 22:30_

For that particular repo (functime.git @ main), it seems to be downloading 285 MB for a regular clone and 231 MB for filter=blob:none. That's a surprisingly small difference.

---

_Comment by @notatallshaw on 2024-04-29 14:25_

> For that particular repo (functime.git @ main), it seems to be downloading 285 MB for a regular clone and 231 MB for filter=blob:none. That's a surprisingly small difference.

Yeah, not sure how much performance impact this will have in general, but one advantage of enabling it is that it is well tested in pip as it has been enabled in for ~2.5 years now.

---

_Comment by @charliermarsh on 2024-04-29 14:26_

(We might already be doing that, I haven’t investigated deeply and can’t quite remember.)

---

_Comment by @zanieb on 2024-04-29 14:36_

I sort of think we do shallow clones already, but a bunch of our git code is vendored and adapted from elsewhere so it's a little unclear — I'd need to investigate too... regardless it sounds like that's not likely to be the root problem here.

---

_Comment by @charliermarsh on 2024-04-29 14:36_

Yeah there are separate issues tracking general Git clone performance.

---

_Comment by @charliermarsh on 2024-04-29 14:37_

I only left this open because I think there’s a possibly-interesting thing to try here where we fetch just the pyproject.toml to extract the metadata.

---

_Comment by @bschoenmaeckers on 2024-04-29 14:43_

I don't know if it is useful but there is something called [sparse checkout](https://git-scm.com/docs/git-sparse-checkout).

---

_Comment by @samypr100 on 2024-05-01 23:02_

> I don't know if it is useful but there is something called [sparse checkout](https://git-scm.com/docs/git-sparse-checkout).

Yes, you could use sparse checkout here effectively to speed things up quite substantially. Normally you'd do in the CLI in the following way:

1. Clone the repo: `git clone --filter=blob:none --depth 1 --sparse git@github.com:functime-org/functime.git`
2. Reapply a sparse filter to keep all the `pyproject.toml`'s inside the cloned repo: `git sparse-checkout set '**/pyproject.toml'`

Note the first `--sparse` is needed to keep the repo with only top-level content.
`--filter=blob:none` is still needed to keep blobs out (this is what pip does) and saves a decent amount of space.
`--depth=1` or shallow is still nice to keep as it saves a more space depending on the history size.

After that, you do `git sparse-checkout` to filter to keep only all the `pyproject.toml`'s in the repo (if any).

This recudes the clone size of functime down to
`Receiving objects: 100% (11/11), 15.56 KiB | 1.56 MiB/s, done.`
Then with the sparse checkout reduces the functime repo to virtually 3.4k since there's a single pyproject.toml.

I've been using a similar technique personally on very large repos at work in CI/CD for quite some time.

---

_Comment by @charliermarsh on 2024-05-19 02:41_

I looked into this a bit and I think libgit2 doesn't support it (https://github.com/libgit2/libgit2/issues/5564).

I'm tempted to rethink our Git strategy a bit more holistically... Right now, we create a single copy of each repo, and then checkout commits by performing a sort of "local" clone into the build directory. I'm wondering if, instead, we should just have a separate clone for each commit, where that clone is a partial clone? It could in theory be less efficient if you're building from the same repo at multiple commits with overlapping blobs, but significantly faster when you're building from a single commit.

---

_Referenced in [astral-sh/uv#3952](../../astral-sh/uv/issues/3952.md) on 2024-06-01 22:12_

---

_Referenced in [nf-core/modules#8191](../../nf-core/modules/pulls/8191.md) on 2025-04-02 08:25_

---

_Comment by @flying-sheep on 2025-06-06 12:14_

You no longer use libgit2 anymore, right?

---

_Comment by @charliermarsh on 2025-06-06 12:19_

Correct.

---

_Comment by @flying-sheep on 2025-06-06 12:22_

I also don’t think that uv downloads a repo from VCS as part of resolution. If I’m right and every project downloaded from VCS is also built really all we need is probably #1737.

---

_Referenced in [astral-sh/uv#1737](../../astral-sh/uv/issues/1737.md) on 2025-06-06 12:25_

---

_Comment by @charliermarsh on 2025-06-06 12:43_

In brief, the way it works is that we have a single clone of each repo, and when we need to use a commit, we fetch it then create a _local_ clone of that ref to build from. (But I don't believe it's blobless.)

---

_Comment by @flying-sheep on 2025-06-06 12:53_

As said, this issue is about resolution, we should talk about building things in #1737.

---

_Comment by @baggiponte on 2025-06-06 19:11_

Feel free to close in case you need it’s solved! :)

---
