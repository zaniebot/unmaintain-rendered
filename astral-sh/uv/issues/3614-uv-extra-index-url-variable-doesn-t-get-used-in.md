```yaml
number: 3614
title: "`UV_EXTRA_INDEX_URL` variable doesn't get used in `uv 0.1.44 (d417daad7 2024-05-14)` "
type: issue
state: closed
author: ArshanKhanifar
labels:
  - needs-mre
assignees: []
created_at: 2024-05-15T17:21:18Z
updated_at: 2024-05-16T20:11:39Z
url: https://github.com/astral-sh/uv/issues/3614
synced_at: 2026-01-12T15:58:45Z
```

# `UV_EXTRA_INDEX_URL` variable doesn't get used in `uv 0.1.44 (d417daad7 2024-05-14)` 

---

_@ArshanKhanifar_

This must be a change recently introduced that broke things, but today my installations from my private pypi repository are failing. 

```
uv pip install --extra-index-url $MY_URL requirements.txt
```
works, however

```
export UV_EXTRA_INDEX_URL=MY_URL
uv pip install requirements.txt
```

doesn't work.

### UV Version
```
uv 0.1.44 (d417daad7 2024-05-14)
```

---

_Comment by @zanieb on 2024-05-15 17:24_

Hi! What version were you using before?

---

_Comment by @charliermarsh on 2024-05-15 17:25_

Unfortunately I can't reproduce this -- it works as expected for me on `main`. Can you please give a complete example, with a real extra index URL and requirements, along with the full error or trace that you're seeing?


---

_Comment by @zanieb on 2024-05-15 17:26_

I can't reproduce this either, e.g.

```
❯ UV_EXTRA_INDEX_URL=https://astral-sh.github.io/packse/0.3.15/simple-html/ uv pip install anyio --reinstall --no-cache -v
DEBUG Found a virtualenv named .venv at: /Users/zb/workspace/uv/.venv
DEBUG Probing interpreter info for: /Users/zb/workspace/uv/.venv/bin/python
DEBUG Found Python 3.12.3 for: /Users/zb/workspace/uv/.venv/bin/python
DEBUG Using Python 3.12.3 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: anyio*
DEBUG No cache entry for: https://astral-sh.github.io/packse/0.3.15/simple-html/anyio/
DEBUG No cache entry for: https://pypi.org/simple/anyio/
DEBUG Searching for a compatible version of anyio (*)
DEBUG Selecting: anyio==4.3.0 (anyio-4.3.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for anyio==4.3.0: idna>=2.8
DEBUG Adding transitive dependency for anyio==4.3.0: sniffio>=1.1
DEBUG No cache entry for: https://astral-sh.github.io/packse/0.3.15/simple-html/idna/
DEBUG No cache entry for: https://astral-sh.github.io/packse/0.3.15/simple-html/sniffio/
DEBUG No cache entry for: https://pypi.org/simple/idna/
DEBUG Searching for a compatible version of idna (>=2.8)
DEBUG Selecting: idna==3.7 (idna-3.7-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e5/3e/741d8c82801c347547f8a2a06aa57dbb1992be9e948df2ea0eda2c8b79e8/idna-3.7-py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/sniffio/
DEBUG Searching for a compatible version of sniffio (>=1.1)
DEBUG Selecting: sniffio==1.3.1 (sniffio-1.3.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
DEBUG Tried 4 versions: anyio 1, idna 1, root 1, sniffio 1
Resolved 3 packages in 223ms
DEBUG Identified uncached requirement: anyio==4.3.0
DEBUG Identified uncached requirement: idna==3.7
DEBUG Identified uncached requirement: sniffio==1.3.1
DEBUG No cache entry for: https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e5/3e/741d8c82801c347547f8a2a06aa57dbb1992be9e948df2ea0eda2c8b79e8/idna-3.7-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
Downloaded 3 packages in 43ms
DEBUG Uninstalled anyio (46 files, 6 directories)
DEBUG Uninstalled idna (15 files, 2 directories)
DEBUG Uninstalled sniffio (15 files, 3 directories)
Installed 3 packages in 2ms
 - anyio==4.3.0
 + anyio==4.3.0
 - idna==3.7
 + idna==3.7
 - sniffio==1.3.1
 + sniffio==1.3.1
```

---

_Label `needs-mre` added by @charliermarsh on 2024-05-15 17:27_

---

_Comment by @ArshanKhanifar on 2024-05-15 20:01_

For what it's worth, this fails on my macbook only. I tried it inside of an alpine docker image & it works just fine (even with latest `uv` version) 


---

_Comment by @zanieb on 2024-05-16 02:15_

We're both using macOS. Unfortunately we'll need more information to help here.

---

_Comment by @ArshanKhanifar on 2024-05-16 03:22_

ah, well fwiw my instance is a private instance on GCP, & I have been basically using the token that I get from `gcloud atuh print-access-token` 

Here's my Makefile command:

```
get_index_url:
	$(eval token := $(shell gcloud auth print-access-token))
	$(eval index_url := "https://_token:$(token)@$(artifact_location)-python.pkg.dev/$(gcp_project)/$(artifact_repo)/simple")
``` 

& then I generate a `uv.env` file like so:

```
generate-uv-env-file: get_index_url
	@echo "UV_EXTRA_INDEX_URL=$(index_url)" > uv.env
```

Unfortunately this might be a bit hard to reproduce but I'll try my best to see if I can dig any deeper, but my token has the following format (I've modified it & cut it short, actual token is much longer haha):
```
https://_token:ya29.c.c0AY_VpZjGo_qz6DY7Dp-3Ut-9JLbMck6pjc8lt2VezaAmewIwpbH0EN71kAGzodNlb7FQpbIGVDwYQ-wQ@my-region-python.pkg.dev/gcp-project/artifact-repository-name/simple
```


---

_Comment by @polarathene on 2024-05-16 05:04_

> `export UV_EXTRA_INDEX_URL=MY_URL`
> `--extra-index-url $MY_URL`

Just to confirm, there's a typo there right? You have one using a variable via `$`, while the exported ENV lacks the `$` prefix to reference the value of `MY_URL` variable.

Try this with the above context:

```bash
echo "${UV_EXTRA_INDEX_URL}"
echo "${MY_URL}"
```

Are both values the same? Since the content is not human friendly to parse, double check with a diff tool or similar to confirm.

---

Your follow-up post has more going on with the generation of the value to `uv.env`, so likewise double check that it's the same value you had work with `--extra-index-url` option but failed when implicitly relying on the ENV. Be sure to try the computed value directly with both approaches if you haven't yet to rule out any issue related to that process, it'll help better isolate if it's an issue related to how you compute and provide the value vs shell environment.

---

It's possible that you either have a typo issue like mentioned, or that the value is not preserved properly. You can prevent accidents for static string values by assigning to shell variables / ENV by wrapping them with single quotes like `MY_URL='$HOME<not-an-accidental-file-redirection'` (_single quote prevents interpolating `$HOME`, while single/double quotes would prevent `<` being mistaken as a redirection_).

There can be other caveats with how the shell treats your variable when you reference it or assign a value, but quote wrapping (single in this case) should ensure no hidden surprises... except for your use of `eval` (_I can't quite recall the gotcha there, other than it's often discouraged_).







---

_Comment by @ArshanKhanifar on 2024-05-16 14:02_

sorry the shell commands that I've pasted are simplified/pseudocoded but I can assure you that I'm running them correctly. 


---

_Comment by @hmc-cs-mdrissi on 2024-05-16 17:40_

I think equivalent to [this](https://github.com/pypa/pip/issues/12706#issuecomment-2114030013), `uv pip config debug` command to see all config settings/where they came from would help debug this issue.

---

_Comment by @charliermarsh on 2024-05-16 18:01_

I'm happy to help with this but it requires a minimal reproduction, i.e., a set of commands we can run that reliably reproduce the issue. As-is, there isn't much we can do.

---

_Comment by @ArshanKhanifar on 2024-05-16 18:51_

@charliermarsh Yes, thank you for all your help already. Once I find sometime I'll see if I can provide a plan for you to be able to reproduce this better. 

---

_Comment by @ArshanKhanifar on 2024-05-16 18:53_

@polarathene I've changed all my `uv pip install`s as follows to get around this issue in the meantime:
```
uv pip install my-private-library --extra-index-url $(echo $UV_EXTRA_INDEX_URL)
```


---

_Comment by @ArshanKhanifar on 2024-05-16 19:33_

Okay I found what the issue was here, though I'm still confused what exactly changed on my system that made the scripts break. 

I need to make sure that I'm using the `export` keyword in my `uv.env` file to ensure that the set variable gets passed to subprocesses as well. I found out this fix by trying @zanieb 's format (in-lining the `UV_EXTRA_INDEX_URL` variable in the `uv pip install` command).

**Solution:**
Basically, if you're not in-lining the environment variable in your `uv pip install` command, be sure to use `export`:
```
export UV_EXTRA_INDEX_URL=my-url
# then in another command
uv pip install my-package
```
Before I was doing this:
```
UV_EXTRA_INDEX_URL=my-url
# then in another command
uv pip install my-package
```
& it wouldn't work. 

---

_Comment by @ArshanKhanifar on 2024-05-16 19:34_

Thank you all for your support, closing this issue.

---

_Closed by @ArshanKhanifar on 2024-05-16 19:34_

---

_Comment by @charliermarsh on 2024-05-16 20:11_

Thank you for following up, I appreciate it!

---
