```yaml
number: 7349
title: "Unclear `uvx --with` help text / how to include a bunch of requirements?"
type: issue
state: open
author: dimaqq
labels:
  - documentation
assignees: []
created_at: 2024-09-13T00:32:45Z
updated_at: 2024-09-18T14:01:00Z
url: https://github.com/astral-sh/uv/issues/7349
synced_at: 2026-01-12T15:59:12Z
```

# Unclear `uvx --with` help text / how to include a bunch of requirements?

---

_@dimaqq_

Here's the help snippet:

```
      --with <WITH>                            Run with the given packages installed
```

It seems to suggest that multiple deps can be specified with a single `--with`, but that doesn't seem to be the case.

(Or maybe I didn't figure out the syntax yet)

```
# non of these work
uvx --with "requests tomli" pytest
uvx --with "requests,tomli" pytest
uvx --with "requests;tomli" pytest
```

```
# this works
uvx --with requests --with tomli pytest --help
```

---

_Comment by @zanieb on 2024-09-13 01:29_

Yes, it's one at a time or `--with-requirements` to provide a file.

We can try to tweak the docs here, but I haven't seen anyone else run into this.

---

_Label `documentation` added by @zanieb on 2024-09-13 01:29_

---

_Assigned to @zanieb by @zanieb on 2024-09-13 01:29_

---

_Comment by @dimaqq on 2024-09-13 02:42_

I guess I keep expecting just a tad too much from a relatively fresh tool 🙈 

I wanted to reproduce a CI failure where the list of installed pacakges was dumped, so I tried to just add all of those with a single `--with`...

My work-around was this:
```command
> set packages "annotated-types==0.7.0,asttokens==2.4.1,attrs==24.2.0,bcrypt==4.2.0,cachetools==5.5.0,certifi==2024.8.30,cffi==1.17.1,charset-normalizer==3.3.2,cryptography==43.0.1,decorator==5.1.1,exceptiongroup==1.2.2,executing==2.1.0,google-auth==2.34.0,hvac==2.3.0,idna==3.8,iniconfig==2.0.0,ipdb==0.13.9,ipython==8.27.0,jedi==0.19.1,Jinja2==3.1.4,juju==3.5.2.0,kubernetes==30.1.0,macaroonbakery==1.3.4,MarkupSafe==2.1.5,matplotlib-inline==0.1.7,mypy-extensions==1.0.0,oauthlib==3.2.2,ops==2.16.1,packaging==24.1,paramiko==3.4.1,parso==0.8.4,pexpect==4.9.0,pip==24.2,pluggy==1.5.0,prompt_toolkit==3.0.47,protobuf==5.28.0,ptyprocess==0.7.0,pure_eval==0.2.3,py==1.11.0,pyasn1==0.6.1,pyasn1_modules==0.4.1,pycparser==2.22,pydantic==2.9.1,pydantic_core==2.23.3,Pygments==2.18.0,pymacaroons==0.13.0,PyNaCl==1.5.0,pyRFC3339==1.1,pytest==7.1.3,pytest-asyncio==0.21.0,pytest-operator==0.35.0,python-dateutil==2.9.0.post0,pytz==2024.2,PyYAML==6.0.2,requests==2.32.3,requests-oauthlib==2.0.0,rsa==4.9,setuptools==74.1.2,six==1.16.0,stack-data==0.6.3,temporalio==1.6.0,toml==0.10.2,tomli==2.0.1,toposort==1.10,traitlets==5.14.3,types-protobuf==5.27.0.20240907,typing-inspect==0.9.0,typing_extensions==4.12.2,urllib3==2.2.2,wcwidth==0.2.13,websocket-client==1.8.0,websockets==13.0.1,wheel==0.44.0"
 set args (echo $packages | sed -e 's/,/ --with /g')
 eval uvx --with $args pytest -v -s --model testing --other-flags
```

🦄🦄🦄 The upside of doing this, is that `uvx` is blazing fast 🚀🚀🚀

I've certainly saved time compared to manually recreating a venv using standard tools!

---

_Comment by @Aditya-PS-05 on 2024-09-18 13:49_

This should illustrate the actual case and avoid the confusion.

<dt><code>--with</code> <i>with</i></dt>
<dd>
<p>Run with the given package installed with a single dependency. To include multiple dependencies, use the <code>--with</code> flag for each dependency individually:</p>
<p>Using <code>--with <i>with1</i> --with <i>with2</i> --with <i>with3</i></code>...</p>

<p>will install all the dependencies of the package.</p>
</dd>

Is this description fine or something else to be incorporate?

---

_Comment by @zanieb on 2024-09-18 13:51_

That's too verbose for the short-help menu, unfortunately. I'm planning on taking a look at this soon.

---

_Comment by @Aditya-PS-05 on 2024-09-18 14:00_

<dt><code>--with</code> <i>with</i></dt>
<dd>
<p>Run with the specified package and its dependencies. To include multiple dependencies, use the --with flag for each one:</p>
<code>--with <i>dep1</i> --with <i>dep2</i> --with <i>dep3</i></code>...</p>

<p>This installs all specified dependencies.</p>
</dd>

Is this good. I think this is less verbose.

---
