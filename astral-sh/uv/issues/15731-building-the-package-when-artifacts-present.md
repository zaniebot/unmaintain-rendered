```yaml
number: 15731
title: Building the package when artifacts present
type: issue
state: open
author: GOGKI
labels:
  - question
assignees: []
created_at: 2025-09-08T08:07:55Z
updated_at: 2025-09-26T07:25:19Z
url: https://github.com/astral-sh/uv/issues/15731
synced_at: 2026-01-12T16:02:16Z
```

# Building the package when artifacts present

---

_@GOGKI_

# Description
Currently, there is no way to install from GH releases, as pointing to the rev on the repository results in building the package from the commit at this tag, and pointing directly to the artifact cannot be resolved on private repositories as the necessary GitHub API headers are not used (`Accept: application/octet-stream`). 

Example:

```toml
[tool.uv.sources]
internal-package = { git = "https://github.com/org/internal-package.git", rev = "v0.1.12" }
another-not-working = { url = "https://github.com/org/another-not-working/releases/download/v0.1.12/another_not_working-0.1.12.tar.gz" }
```

The first example will install from the source code, instead of the released package available on that tag. The second option will fail on authorization, and will return a 404 (which look like the package would not exist, but in fact it is not accessible).

# Expected outcome
A way to install the package from a private repo from the released artifact on Github is available.

# Why important
We have one scenario which will affect all our internal distribution of packages"
On dynamic versioning the version of the package will be set to 0.0.0 on the source code, whereas only the artifact will have the correct version propagated throughout all the necessary files. Without being able to access the released artifact, we will need to upload to PyPI, etc.

### Platform

all

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

all

---

_Label `bug` added by @GOGKI on 2025-09-08 08:07_

---

_Comment by @konstin on 2025-09-08 12:47_

Is the problem with GitHub releases the `Accept` header, or the lack of authorization credentials? Are you setting any auth credentials with which you could authenticate against GitHub?

---

_Comment by @GOGKI on 2025-09-09 14:35_

@konstin that depends on which route we are talking. Is there a suggested way which would enable the users to us the artifact from the GH release?

Both ways that I have shown in there did not work. First one builds package from code ignoring the artifact, second one because of the lack of authorization of proper authorization fails. Currently GH_TOKEN is ignored, and setting credentials as an index would be set is not working either (through e.g. UV_INDEX_... env variables).

---

_Label `bug` removed by @konstin on 2025-09-09 18:59_

---

_Label `question` added by @konstin on 2025-09-09 18:59_

---

_Comment by @konstin on 2025-09-09 19:00_

GitHub Releases aren't well-suited as package index. GitHub doesn't implement a Python package registry, so we're limited in what we can do with GitHub alone. The options we can currently support are installing from GitHub from source, e.g. from a tag, or installing from a service that implements the index API (PEP 503). Another alternative is vendoring wheels in the repository.

---

_Comment by @GOGKI on 2025-09-10 07:27_

@konstin I fully agree that Github release is not a package index. I understand the difference, I was saying that I was trying to authorize uv to access the private GH repo in different ways. None of them are currently working. Of course I might be missing some of the details as uv is a great and broad tool, that's why I created this issue. 

Now, trying to be a bit more specific, if we are using an additional source, pointing explicitly to git repo as the source, specifying the tag as the revision, and for this tag an artifact is already present on the repository, I don't think that building that from the source is the correct order, and this might be confusing for the other users as well. While pointing to the source and rev/tag, I don't expect any of the functionalities from the PEP503 to be applied (indexing, version resolution etc.). Does this makes more sense?

P.S. Vendoring packages on the repo leads to a fast inflation of the source code, which none of the developers would like to go for.

---

_Comment by @GOGKI on 2025-09-23 21:51_

@konstin any comments?

---

_Comment by @zanieb on 2025-09-23 21:57_

We don't support reading artifacts from GitHub Releases associated with a tag. We could do so, but I'd say it's out of scope for uv at this time.

---

_Comment by @zanieb on 2025-09-23 21:57_

>  pointing directly to the artifact cannot be resolved on private repositories 

I'm fine with making this work, fwiw.

---

_Comment by @zanieb on 2025-09-23 21:59_

Did you try providing the HTTP credentials via one of the methods outlined at https://docs.astral.sh/uv/concepts/authentication/http/ ? 

---

_Comment by @GOGKI on 2025-09-26 07:23_

> We don't support reading artifacts from GitHub Releases associated with a tag. We could do so, but I'd say it's out of scope for uv at this time.

That's totally fair. We find ways to currently workaround the issue, but I would say it would be beneficial in terms of having the correct/intuitive resolution order.

---

_Comment by @GOGKI on 2025-09-26 07:24_

> > pointing directly to the artifact cannot be resolved on private repositories
> 
> I'm fine with making this work, fwiw.
That would be greatly appreciated.

---

_Comment by @GOGKI on 2025-09-26 07:25_

> Did you try providing the HTTP credentials via one of the methods outlined at https://docs.astral.sh/uv/concepts/authentication/http/ ?

Yes, and unluckily it did not work.

---
