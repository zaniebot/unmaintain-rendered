```yaml
number: 9429
title: "uv suffers dependency confusion if user forgets to put username:password on internal index url"
type: issue
state: closed
author: dhandy2013
labels:
  - error messages
assignees: []
created_at: 2024-11-25T21:42:12Z
updated_at: 2025-04-29T21:37:08Z
url: https://github.com/astral-sh/uv/issues/9429
synced_at: 2026-01-12T15:59:50Z
```

# uv suffers dependency confusion if user forgets to put username:password on internal index url

---

_@dhandy2013_

I accidentally triggered a dependency confusion vulnerability in uv just by forgetting to add credentials to our company's internal package index URL.

Steps to reproduce:
- Run an internal Python package index that requires basic HTTP auth to connect
- Put a package on this internal package index named `example-component` (seriously, this is what I named it, I was testing!)
- Run these commands:

```bash
$ uv venv scratchenv
Using CPython 3.11.10
Creating virtual environment at: scratchenv
Activate with: source scratchenv/bin/activate
$ uv pip install --python=scratchenv --index=https://internal.example.com/repository/production/simple example-component
Using Python 3.11.10 environment at scratchenv
Resolved 1 package in 870ms
Installed 1 package in 1ms
 + example-component==0.0.1
$ uv pip list --python=scratchenv
Using Python 3.11.10 environment at scratchenv
Package           Version
----------------- -------
example-component 0.0.1
```

Expected result:

Either `example-component` is installed from my internal Python package index, or else I get some kind of error message if something was wrong with the package index URL.

Actual result:

As you can see from the command output above, `uv` silently and happily ignored my internal package index URL, gave no warning, and installs [example-component from pypi](https://pypi.org/project/example-component/)! The only reason I knew something was wrong is that `uv pip list` reported an unexpected version number.

Something was very wrong with my internal package index URL: It was missing the required credentials. So `uv` just silently ignored the error and went on to the default index URL, causing dependency confusion.

(Fortunately the example-component from pypi.org does not seem to be any kind of malware.)

It seems from the uv docs about [Packages that exist on multiple indexes](https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes) that uv prides itself on being better than pip at avoiding dependency confusion. So I thought you would want to hear about this case where uv is easily vulnerable to it.

Operating system: Ubuntu 24.04
```bash
$ uv version
uv 0.5.4
```

---

_Comment by @charliermarsh on 2024-11-25 21:46_

Thanks. I'm not certain what we should change here if anything. Are you for `--default-index=` rather than `--index=`? `--index=` allows fallback to PyPI, _if_ a package isn't found on a preceding index (in other words, it prevents dependency confusion attacks by disallowing packages to shadow packages on other indexes).

I don't know if there's actually anything we can do about erroring if we fail to connect to your index, because indexes in general are so bad and so inconsistent in how they handle these cases. Some indexes return 403 when you provide valid credentials but request a non-existent package; some indexes return 404 when you provide invalid credentials. No matter what we do, we'll break something. We do show a dedicated hint if we fail to resolve and see (e.g.) a 403. I guess we could consider showing that warning even if the installation completes.


---

_Label `error messages` added by @charliermarsh on 2024-11-25 21:47_

---

_Comment by @charliermarsh on 2024-11-25 21:51_

We should probably _at least_ warn when we fail to connect to an index. We may want to special-case the indexes that we _know_ return 403 consistently.

---

_Comment by @dhandy2013 on 2024-11-25 21:56_

A warning would be helpful. Something in the docs to warn users would also be helpful.

Speaking of the docs, I really hate putting credentials in the package index URL. I would much rather put them in environment variables. I saw the docs for [UV_INDEX_{name}_PASSWORD](https://docs.astral.sh/uv/configuration/environment/#uv_index_name_password) and was wondering how on earth to set the name of an index given on the command line, given that configuring the index in a pyproject.toml file wasn't going to work for me.

So I searched the source code (Thanks for writing `uv` in Rust!) and found in crates/uv-distribution-types/src/index.rs the implementation for `Index::from_str` that apparently lets you put `name=` in front of an index URL on the command line! I'm going to try this out right now. If this works, then I humbly request that you please document this feature!

---

_Comment by @charliermarsh on 2024-11-25 21:57_

I documented it here recently: https://docs.astral.sh/uv/configuration/indexes/#defining-an-index. Take a look at "When providing an index on the command line..."

---

_Comment by @dhandy2013 on 2024-11-25 22:00_

Thank you. The docs are good, I don't know why I missed that. :)

---

_Comment by @chrisrodrigue on 2024-11-26 19:58_

What happens when a user configures 
`UV_DEFAULT_INDEX` to a PyPI mirror URL that does not contain credentials? 

`UV_INDEX_INTERNAL_PROXY_USERNAME` and `UV_INDEX_INTERNAL_PROXY_PASSWORD` do not appear to work with the default index.

Should the variables `UV_INDEX_DEFAULT_USERNAME` and `UV_INDEX_DEFAULT_PASSWORD` apply to the default index? Or should they be `UV_DEFAULT_INDEX_USERNAME` and `UV_DEFAULT_INDEX_PASSWORD`?

---

_Comment by @dhandy2013 on 2024-11-26 23:00_

Answering this previous question from @charliermarsh :
> Are you for --default-index= rather than --index=? --index= allows fallback to PyPI, if a package isn't found on a preceding index

In my case I wanted `example-component` to come from my internal package index, but its dependencies to come from PyPI. That's why I used `--index` rather than `--default-index` to point to the internal package index.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-11 08:39_

---

_Closed by @zanieb on 2025-04-29 21:37_

---
