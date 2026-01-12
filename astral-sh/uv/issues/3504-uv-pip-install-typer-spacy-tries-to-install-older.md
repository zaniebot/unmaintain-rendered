```yaml
number: 3504
title: "`uv pip install typer spacy` tries to install older versions of spacy and fails on Python 3.12"
type: issue
state: closed
author: skshetry
labels:
  - question
assignees: []
created_at: 2024-05-10T12:59:31Z
updated_at: 2024-10-21T21:30:22Z
url: https://github.com/astral-sh/uv/issues/3504
synced_at: 2026-01-12T15:58:44Z
```

# `uv pip install typer spacy` tries to install older versions of spacy and fails on Python 3.12

---

_@skshetry_

```bash
cd "$(mktemp -d)"
uv venv -p 3.12 --seed
uv pip install typer spacy --no-cache --verbose
```

There are wheels available for spacy for Python 3.12, but `uv` chooses to install older versions for some reason.

Please check [uv-pip-install.txt](https://github.com/astral-sh/uv/files/15275296/uv-pip-install.txt).

`pip` can install it without any issues:
```bash
cd "$(mktemp -d)"
uv venv -p 3.12 --seed
.venv/bin/pip install typer spacy
```

<details><summary>Details</summary>
<p>

```console
Using Python 3.12.3 interpreter at: /usr/bin/python3.12
Creating virtualenv at: .venv
 + pip==24.0
Activate with: source .venv/bin/activate
Collecting typer
  Using cached typer-0.12.3-py3-none-any.whl.metadata (15 kB)
Collecting spacy
  Using cached spacy-3.7.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (27 kB)
Collecting click>=8.0.0 (from typer)
  Using cached click-8.1.7-py3-none-any.whl.metadata (3.0 kB)
Collecting typing-extensions>=3.7.4.3 (from typer)
  Using cached typing_extensions-4.11.0-py3-none-any.whl.metadata (3.0 kB)
Collecting shellingham>=1.3.0 (from typer)
  Using cached shellingham-1.5.4-py2.py3-none-any.whl.metadata (3.5 kB)
Collecting rich>=10.11.0 (from typer)
  Using cached rich-13.7.1-py3-none-any.whl.metadata (18 kB)
Collecting spacy-legacy<3.1.0,>=3.0.11 (from spacy)
  Using cached spacy_legacy-3.0.12-py2.py3-none-any.whl.metadata (2.8 kB)
Collecting spacy-loggers<2.0.0,>=1.0.0 (from spacy)
  Using cached spacy_loggers-1.0.5-py3-none-any.whl.metadata (23 kB)
Collecting murmurhash<1.1.0,>=0.28.0 (from spacy)
  Using cached murmurhash-1.0.10-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (2.0 kB)
Collecting cymem<2.1.0,>=2.0.2 (from spacy)
  Using cached cymem-2.0.8-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (8.4 kB)
Collecting preshed<3.1.0,>=3.0.2 (from spacy)
  Using cached preshed-3.0.9-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (2.2 kB)
Collecting thinc<8.3.0,>=8.2.2 (from spacy)
  Using cached thinc-8.2.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (15 kB)
Collecting wasabi<1.2.0,>=0.9.1 (from spacy)
  Using cached wasabi-1.1.2-py3-none-any.whl.metadata (28 kB)
Collecting srsly<3.0.0,>=2.4.3 (from spacy)
  Using cached srsly-2.4.8-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (20 kB)
Collecting catalogue<2.1.0,>=2.0.6 (from spacy)
  Using cached catalogue-2.0.10-py3-none-any.whl.metadata (14 kB)
Collecting weasel<0.4.0,>=0.1.0 (from spacy)
  Using cached weasel-0.3.4-py3-none-any.whl.metadata (4.7 kB)
Collecting typer
  Using cached typer-0.9.4-py3-none-any.whl.metadata (14 kB)
Collecting smart-open<7.0.0,>=5.2.1 (from spacy)
  Using cached smart_open-6.4.0-py3-none-any.whl.metadata (21 kB)
Collecting tqdm<5.0.0,>=4.38.0 (from spacy)
  Using cached tqdm-4.66.4-py3-none-any.whl.metadata (57 kB)
Collecting requests<3.0.0,>=2.13.0 (from spacy)
  Using cached requests-2.31.0-py3-none-any.whl.metadata (4.6 kB)
Collecting pydantic!=1.8,!=1.8.1,<3.0.0,>=1.7.4 (from spacy)
  Using cached pydantic-2.7.1-py3-none-any.whl.metadata (107 kB)
Collecting jinja2 (from spacy)
  Using cached jinja2-3.1.4-py3-none-any.whl.metadata (2.6 kB)
Collecting setuptools (from spacy)
  Using cached setuptools-69.5.1-py3-none-any.whl.metadata (6.2 kB)
Collecting packaging>=20.0 (from spacy)
  Using cached packaging-24.0-py3-none-any.whl.metadata (3.2 kB)
Collecting langcodes<4.0.0,>=3.2.0 (from spacy)
  Using cached langcodes-3.4.0-py3-none-any.whl.metadata (29 kB)
Collecting numpy>=1.19.0 (from spacy)
  Using cached numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (61 kB)
Collecting language-data>=1.2 (from langcodes<4.0.0,>=3.2.0->spacy)
  Using cached language_data-1.2.0-py3-none-any.whl.metadata (4.3 kB)
Collecting annotated-types>=0.4.0 (from pydantic!=1.8,!=1.8.1,<3.0.0,>=1.7.4->spacy)
  Using cached annotated_types-0.6.0-py3-none-any.whl.metadata (12 kB)
Collecting pydantic-core==2.18.2 (from pydantic!=1.8,!=1.8.1,<3.0.0,>=1.7.4->spacy)
  Using cached pydantic_core-2.18.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (6.5 kB)
Collecting charset-normalizer<4,>=2 (from requests<3.0.0,>=2.13.0->spacy)
  Using cached charset_normalizer-3.3.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (33 kB)
Collecting idna<4,>=2.5 (from requests<3.0.0,>=2.13.0->spacy)
  Using cached idna-3.7-py3-none-any.whl.metadata (9.9 kB)
Collecting urllib3<3,>=1.21.1 (from requests<3.0.0,>=2.13.0->spacy)
  Using cached urllib3-2.2.1-py3-none-any.whl.metadata (6.4 kB)
Collecting certifi>=2017.4.17 (from requests<3.0.0,>=2.13.0->spacy)
  Using cached certifi-2024.2.2-py3-none-any.whl.metadata (2.2 kB)
Collecting blis<0.8.0,>=0.7.8 (from thinc<8.3.0,>=8.2.2->spacy)
  Using cached blis-0.7.11-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (7.4 kB)
Collecting confection<1.0.0,>=0.0.1 (from thinc<8.3.0,>=8.2.2->spacy)
  Using cached confection-0.1.4-py3-none-any.whl.metadata (19 kB)
Collecting cloudpathlib<0.17.0,>=0.7.0 (from weasel<0.4.0,>=0.1.0->spacy)
  Using cached cloudpathlib-0.16.0-py3-none-any.whl.metadata (14 kB)
Collecting MarkupSafe>=2.0 (from jinja2->spacy)
  Using cached MarkupSafe-2.1.5-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (3.0 kB)
Collecting marisa-trie>=0.7.7 (from language-data>=1.2->langcodes<4.0.0,>=3.2.0->spacy)
  Using cached marisa_trie-1.1.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (8.6 kB)
Collecting markdown-it-py>=2.2.0 (from rich>=10.11.0->typer)
  Using cached markdown_it_py-3.0.0-py3-none-any.whl.metadata (6.9 kB)
Collecting pygments<3.0.0,>=2.13.0 (from rich>=10.11.0->typer)
  Using cached pygments-2.18.0-py3-none-any.whl.metadata (2.5 kB)
Collecting mdurl~=0.1 (from markdown-it-py>=2.2.0->rich>=10.11.0->typer)
  Using cached mdurl-0.1.2-py3-none-any.whl.metadata (1.6 kB)
Using cached spacy-3.7.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (6.5 MB)
Using cached typer-0.9.4-py3-none-any.whl (45 kB)
Using cached catalogue-2.0.10-py3-none-any.whl (17 kB)
Using cached click-8.1.7-py3-none-any.whl (97 kB)
Using cached cymem-2.0.8-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (46 kB)
Using cached langcodes-3.4.0-py3-none-any.whl (182 kB)
Using cached murmurhash-1.0.10-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (29 kB)
Using cached numpy-1.26.4-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.0 MB)
Using cached packaging-24.0-py3-none-any.whl (53 kB)
Using cached preshed-3.0.9-cp312-cp312-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl (156 kB)
Using cached pydantic-2.7.1-py3-none-any.whl (409 kB)
Using cached pydantic_core-2.18.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.1 MB)
Using cached requests-2.31.0-py3-none-any.whl (62 kB)
Using cached smart_open-6.4.0-py3-none-any.whl (57 kB)
Using cached spacy_legacy-3.0.12-py2.py3-none-any.whl (29 kB)
Using cached spacy_loggers-1.0.5-py3-none-any.whl (22 kB)
Using cached srsly-2.4.8-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (491 kB)
Using cached thinc-8.2.3-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (865 kB)
Using cached tqdm-4.66.4-py3-none-any.whl (78 kB)
Using cached typing_extensions-4.11.0-py3-none-any.whl (34 kB)
Using cached wasabi-1.1.2-py3-none-any.whl (27 kB)
Using cached weasel-0.3.4-py3-none-any.whl (50 kB)
Using cached jinja2-3.1.4-py3-none-any.whl (133 kB)
Using cached setuptools-69.5.1-py3-none-any.whl (894 kB)
Using cached annotated_types-0.6.0-py3-none-any.whl (12 kB)
Using cached blis-0.7.11-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (10.2 MB)
Using cached certifi-2024.2.2-py3-none-any.whl (163 kB)
Using cached charset_normalizer-3.3.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (141 kB)
Using cached cloudpathlib-0.16.0-py3-none-any.whl (45 kB)
Using cached confection-0.1.4-py3-none-any.whl (35 kB)
Using cached idna-3.7-py3-none-any.whl (66 kB)
Using cached language_data-1.2.0-py3-none-any.whl (5.4 MB)
Using cached MarkupSafe-2.1.5-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (28 kB)
Using cached urllib3-2.2.1-py3-none-any.whl (121 kB)
Using cached marisa_trie-1.1.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.4 MB)
Installing collected packages: cymem, wasabi, urllib3, typing-extensions, tqdm, spacy-loggers, spacy-legacy, smart-open, setuptools, packaging, numpy, murmurhash, MarkupSafe, idna, cloudpathlib, click, charset-normalizer, certifi, catalogue, annotated-types, typer, srsly, requests, pydantic-core, preshed, marisa-trie, jinja2, blis, pydantic, language-data, langcodes, confection, weasel, thinc, spacy
Successfully installed MarkupSafe-2.1.5 annotated-types-0.6.0 blis-0.7.11 catalogue-2.0.10 certifi-2024.2.2 charset-normalizer-3.3.2 click-8.1.7 cloudpathlib-0.16.0 confection-0.1.4 cymem-2.0.8 idna-3.7 jinja2-3.1.4 langcodes-3.4.0 language-data-1.2.0 marisa-trie-1.1.1 murmurhash-1.0.10 numpy-1.26.4 packaging-24.0 preshed-3.0.9 pydantic-2.7.1 pydantic-core-2.18.2 requests-2.31.0 setuptools-69.5.1 smart-open-6.4.0 spacy-3.7.4 spacy-legacy-3.0.12 spacy-loggers-1.0.5 srsly-2.4.8 thinc-8.2.3 tqdm-4.66.4 typer-0.9.4 typing-extensions-4.11.0 urllib3-2.2.1 wasabi-1.1.2 weasel-0.3.4
```

</p>
</details> 

The above logs are from Archlinux, but I have same issues on macOS as well.

```console
$ uv --version
uv 0.1.42 (367958e6b 2024-05-08)
```

---

_Comment by @charliermarsh on 2024-05-10 13:15_

Spacy puts an upper-bound on Typer. So we try to pin `typer==0.12.3`, then end up backtracking to older and older versions of Spacy, and ultimately try to build some package that isn't building on your machine. I think you should either invert the order of the dependencies to hint to uv that we should try Spacy first, or add clearer bounds to the specifiers.

---

_Label `question` added by @charliermarsh on 2024-05-10 13:15_

---

_Comment by @skshetry on 2024-05-10 13:29_

This was just a minimal example that I could reproduce the issue with. In reality, I have a project that has indirect dependencies with both `typer` and `spacy`, and I don't have control over the ordering.

One of my _indirect_ dependencies requires `typer>=0.4.1`, and I have an extra dependency that indirectly requires `spacy<4`. So I guess, `typer` is always going to come before spacy(?).


---

_Comment by @charliermarsh on 2024-05-10 13:30_

Can you provide a lower bound on `spacy`?

---

_Comment by @skshetry on 2024-05-10 13:41_

Unfortunately, `spacy` is not a direct dependency of the project. Given the dependency (`fastai`) has left it unpinned, we'd have to do the same for maximum compatibility with the Python ecosystem.
Otherwise, we may end up getting conflicts at some point when it changes the requirements.

---

_Comment by @charliermarsh on 2024-05-10 13:45_

I don't see an issue with adding a _lower_ bound. Upper bounds are far more likely to cause problems (as you're seeing here), but is your project really compatible with all versions of `spacy`?

---

_Comment by @skshetry on 2024-05-10 15:16_

> but is your project really compatible with all versions of `spacy`?

To be honest, we are not even using it (it comes with the package 😄).
But as a library maintainer, I don't want to set lower bounds for transitive dependencies. It's too much of an effort, and for maximum compatibility, ideally I'd have to choose multiple versions of `spacy` for different Python versions.

Although this is a valid resolution, I'd consider this a bug. Python packages don't set upper bounds for Python versions so the older packages may be incompatible with newer Python versions. Similarly, it's not uncommon for packages to overlook version pinning, that are fixed in subsequent releases.

Overall, newer versions tend to work better than the older ones.
It'd be great if it could prefer newer or latest releases rather than the older ones.






---

_Comment by @notatallshaw on 2024-05-10 17:09_

> Overall, newer versions tend to work better than the older ones.
> It'd be great if it could prefer newer or latest releases rather than the older ones.

But when you have two packages A and B, and the newest version of A isn't compatible with the newest version of B, you *must* backtrack on one of them. There is no universal way of telling whether it is going to be better for A or B to be backtracked on, and your resolution algorithm and it's current state may determine for you which one will be backtracked on.

Pip does the same and runs into similiar problems, although in different cases as it's resolution algorithm picks different paths to uv, and tends to be a little bit more flexible about not having to exhaust all older versions due to some implementation details. But it definetly still happens with pip.

---

_Comment by @notatallshaw on 2024-05-10 17:11_

> But as a library maintainer, I don't want to set lower bounds for transitive dependencies. It's too much of an effort, and for maximum compatibility, ideally I'd have to choose multiple versions of `spacy` for different Python versions.

One solution is to use and recommend a constraint file then, this way you specify bounds on certain packages without forcing consumers to use those bounds.

Apache-airflow does this to specify all the dependencies and transitive dependencies that were tested against when a particular version was released.

---

_Comment by @nathanjmcdougall on 2024-06-11 03:34_

> But when you have two packages A and B, and the newest version of A isn't compatible with the newest version of B, you _must_ backtrack on one of them. There is no universal way of telling whether it is going to be better for A or B to be backtracked on, and your resolution algorithm and it's current state may determine for you which one will be backtracked on.

If A is a direct dependency and B is a transitive dependency then I wonder whether it will nearly always be better to backtrack on B. Just a thought.

---

_Comment by @zanieb on 2024-10-21 21:30_

Going to close this in favor of our goal to have more robust heuristics in the resolver.

---

_Closed by @zanieb on 2024-10-21 21:30_

---
