```yaml
number: 4063
title: "Feature Request: Allow `--no-binary <package>` to override `--only-binary :all:`"
type: issue
state: closed
author: notatallshaw-gts
labels: []
assignees: []
created_at: 2024-06-05T19:14:28Z
updated_at: 2024-06-12T20:47:47Z
url: https://github.com/astral-sh/uv/issues/4063
synced_at: 2026-01-10T05:31:37Z
```

# Feature Request: Allow `--no-binary <package>` to override `--only-binary :all:`

---

_Issue opened by @notatallshaw-gts on 2024-06-05 19:14_

My use case is *in general* I do not want to use sdists, but there are a small number of exceptions to this rule, either because sdists are required, or because I am referencing local sources.

So I either want to be able to say "only use an sdist if no wheel is available at all", or I want to say "only use an sdist for this list of explicit packages".

Pip allows both of these behaviors, the first can be achieved with `--prefer-binary` which has been discussed here: https://github.com/astral-sh/uv/issues/1794, and it appears it would be difficult for uv to support given it's design choices.

The second can be achieved by using `--no-binary <package>` to override: `--only-binary :all:` (though I have not checked if this is intentional or not, for pip I generally rely on `--prefer-binary`), e.g.

```
$ pip install --dry-run --only-binary :all: --no-binary yt yt
Collecting yt
  Using cached yt-4.3.1.tar.gz (13.8 MB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Installing backend dependencies ... done
  Preparing metadata (pyproject.toml) ... done
Collecting cmyt>=1.1.2 (from yt)
  Using cached cmyt-2.0.0-py3-none-any.whl.metadata (4.4 kB)
Collecting ewah-bool-utils>=1.2.0 (from yt)
  Using cached ewah_bool_utils-1.2.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (1.4 kB)
Collecting ipywidgets>=8.0.0 (from yt)
  Using cached ipywidgets-8.1.3-py3-none-any.whl.metadata (2.4 kB)
Collecting matplotlib>=3.5 (from yt)
  Using cached matplotlib-3.9.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (11 kB)
Collecting more-itertools>=8.4 (from yt)
  Using cached more_itertools-10.2.0-py3-none-any.whl.metadata (34 kB)
Collecting numpy<3,>=1.19.3 (from yt)
  Using cached numpy-1.26.4-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (61 kB)
Collecting packaging>=20.9 (from yt)
  Using cached packaging-24.0-py3-none-any.whl.metadata (3.2 kB)
Collecting pillow>=8.0.0 (from yt)
  Using cached pillow-10.3.0-cp311-cp311-manylinux_2_28_x86_64.whl.metadata (9.2 kB)
Collecting tomli-w>=0.4.0 (from yt)
  Using cached tomli_w-1.0.0-py3-none-any.whl.metadata (4.9 kB)
Collecting tqdm>=3.4.0 (from yt)
  Using cached tqdm-4.66.4-py3-none-any.whl.metadata (57 kB)
Collecting unyt>=2.9.2 (from yt)
  Using cached unyt-3.0.2-py3-none-any.whl.metadata (4.7 kB)
Collecting typing-extensions>=4.4.0 (from yt)
  Using cached typing_extensions-4.12.1-py3-none-any.whl.metadata (3.0 kB)
Collecting comm>=0.1.3 (from ipywidgets>=8.0.0->yt)
  Using cached comm-0.2.2-py3-none-any.whl.metadata (3.7 kB)
Collecting ipython>=6.1.0 (from ipywidgets>=8.0.0->yt)
  Using cached ipython-8.25.0-py3-none-any.whl.metadata (4.9 kB)
Collecting traitlets>=4.3.1 (from ipywidgets>=8.0.0->yt)
  Using cached traitlets-5.14.3-py3-none-any.whl.metadata (10 kB)
Collecting widgetsnbextension~=4.0.11 (from ipywidgets>=8.0.0->yt)
  Using cached widgetsnbextension-4.0.11-py3-none-any.whl.metadata (1.6 kB)
Collecting jupyterlab-widgets~=3.0.11 (from ipywidgets>=8.0.0->yt)
  Using cached jupyterlab_widgets-3.0.11-py3-none-any.whl.metadata (4.1 kB)
Collecting contourpy>=1.0.1 (from matplotlib>=3.5->yt)
  Using cached contourpy-1.2.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (5.8 kB)
Collecting cycler>=0.10 (from matplotlib>=3.5->yt)
  Using cached cycler-0.12.1-py3-none-any.whl.metadata (3.8 kB)
Collecting fonttools>=4.22.0 (from matplotlib>=3.5->yt)
  Using cached fonttools-4.53.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (162 kB)
Collecting kiwisolver>=1.3.1 (from matplotlib>=3.5->yt)
  Using cached kiwisolver-1.4.5-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (6.4 kB)
Collecting pyparsing>=2.3.1 (from matplotlib>=3.5->yt)
  Using cached pyparsing-3.1.2-py3-none-any.whl.metadata (5.1 kB)
Collecting python-dateutil>=2.7 (from matplotlib>=3.5->yt)
  Using cached python_dateutil-2.9.0.post0-py2.py3-none-any.whl.metadata (8.4 kB)
Collecting sympy>=1.7 (from unyt>=2.9.2->yt)
  Using cached sympy-1.12.1-py3-none-any.whl.metadata (12 kB)
Collecting decorator (from ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached decorator-5.1.1-py3-none-any.whl.metadata (4.0 kB)
Collecting jedi>=0.16 (from ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached jedi-0.19.1-py2.py3-none-any.whl.metadata (22 kB)
Collecting matplotlib-inline (from ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached matplotlib_inline-0.1.7-py3-none-any.whl.metadata (3.9 kB)
Collecting prompt-toolkit<3.1.0,>=3.0.41 (from ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached prompt_toolkit-3.0.46-py3-none-any.whl.metadata (6.4 kB)
Collecting pygments>=2.4.0 (from ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached pygments-2.18.0-py3-none-any.whl.metadata (2.5 kB)
Collecting stack-data (from ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached stack_data-0.6.3-py3-none-any.whl.metadata (18 kB)
Collecting pexpect>4.3 (from ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached pexpect-4.9.0-py2.py3-none-any.whl.metadata (2.5 kB)
Collecting six>=1.5 (from python-dateutil>=2.7->matplotlib>=3.5->yt)
  Using cached six-1.16.0-py2.py3-none-any.whl.metadata (1.8 kB)
Collecting mpmath<1.4.0,>=1.1.0 (from sympy>=1.7->unyt>=2.9.2->yt)
  Using cached mpmath-1.3.0-py3-none-any.whl.metadata (8.6 kB)
Collecting parso<0.9.0,>=0.8.3 (from jedi>=0.16->ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached parso-0.8.4-py2.py3-none-any.whl.metadata (7.7 kB)
Collecting ptyprocess>=0.5 (from pexpect>4.3->ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached ptyprocess-0.7.0-py2.py3-none-any.whl.metadata (1.3 kB)
Collecting wcwidth (from prompt-toolkit<3.1.0,>=3.0.41->ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached wcwidth-0.2.13-py2.py3-none-any.whl.metadata (14 kB)
Collecting executing>=1.2.0 (from stack-data->ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached executing-2.0.1-py2.py3-none-any.whl.metadata (9.0 kB)
Collecting asttokens>=2.1.0 (from stack-data->ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached asttokens-2.4.1-py2.py3-none-any.whl.metadata (5.2 kB)
Collecting pure-eval (from stack-data->ipython>=6.1.0->ipywidgets>=8.0.0->yt)
  Using cached pure_eval-0.2.2-py3-none-any.whl.metadata (6.2 kB)
Using cached cmyt-2.0.0-py3-none-any.whl (31 kB)
Using cached ewah_bool_utils-1.2.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.7 MB)
Using cached ipywidgets-8.1.3-py3-none-any.whl (139 kB)
Using cached matplotlib-3.9.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (8.3 MB)
Using cached more_itertools-10.2.0-py3-none-any.whl (57 kB)
Using cached numpy-1.26.4-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.3 MB)
Using cached packaging-24.0-py3-none-any.whl (53 kB)
Using cached pillow-10.3.0-cp311-cp311-manylinux_2_28_x86_64.whl (4.5 MB)
Using cached tomli_w-1.0.0-py3-none-any.whl (6.0 kB)
Using cached tqdm-4.66.4-py3-none-any.whl (78 kB)
Using cached typing_extensions-4.12.1-py3-none-any.whl (37 kB)
Using cached unyt-3.0.2-py3-none-any.whl (138 kB)
Using cached comm-0.2.2-py3-none-any.whl (7.2 kB)
Using cached contourpy-1.2.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (306 kB)
Using cached cycler-0.12.1-py3-none-any.whl (8.3 kB)
Using cached fonttools-4.53.0-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (4.9 MB)
Using cached ipython-8.25.0-py3-none-any.whl (817 kB)
Using cached jupyterlab_widgets-3.0.11-py3-none-any.whl (214 kB)
Using cached kiwisolver-1.4.5-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (1.4 MB)
Using cached pyparsing-3.1.2-py3-none-any.whl (103 kB)
Using cached python_dateutil-2.9.0.post0-py2.py3-none-any.whl (229 kB)
Using cached sympy-1.12.1-py3-none-any.whl (5.7 MB)
Using cached traitlets-5.14.3-py3-none-any.whl (85 kB)
Using cached widgetsnbextension-4.0.11-py3-none-any.whl (2.3 MB)
Using cached jedi-0.19.1-py2.py3-none-any.whl (1.6 MB)
Using cached mpmath-1.3.0-py3-none-any.whl (536 kB)
Using cached pexpect-4.9.0-py2.py3-none-any.whl (63 kB)
Using cached prompt_toolkit-3.0.46-py3-none-any.whl (386 kB)
Using cached pygments-2.18.0-py3-none-any.whl (1.2 MB)
Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
Using cached decorator-5.1.1-py3-none-any.whl (9.1 kB)
Using cached matplotlib_inline-0.1.7-py3-none-any.whl (9.9 kB)
Using cached stack_data-0.6.3-py3-none-any.whl (24 kB)
Using cached asttokens-2.4.1-py2.py3-none-any.whl (27 kB)
Using cached executing-2.0.1-py2.py3-none-any.whl (24 kB)
Using cached parso-0.8.4-py2.py3-none-any.whl (103 kB)
Using cached ptyprocess-0.7.0-py2.py3-none-any.whl (13 kB)
Using cached pure_eval-0.2.2-py3-none-any.whl (11 kB)
Using cached wcwidth-0.2.13-py2.py3-none-any.whl (34 kB)
Would install Pygments-2.18.0 asttokens-2.4.1 cmyt-2.0.0 comm-0.2.2 contourpy-1.2.1 cycler-0.12.1 decorator-5.1.1 ewah_bool_utils-1.2.1 executing-2.0.1 fonttools-4.53.0 ipython-8.25.0 ipywidgets-8.1.3 jedi-0.19.1 jupyterlab_widgets-3.0.11 kiwisolver-1.4.5 matplotlib-3.9.0 matplotlib-inline-0.1.7 more-itertools-10.2.0 mpmath-1.3.0 numpy-1.26.4 packaging-24.0 parso-0.8.4 pexpect-4.9.0 pillow-10.3.0 prompt_toolkit-3.0.46 ptyprocess-0.7.0 pure-eval-0.2.2 pyparsing-3.1.2 python-dateutil-2.9.0.post0 six-1.16.0 stack-data-0.6.3 sympy-1.12.1 tomli_w-1.0.0 tqdm-4.66.4 traitlets-5.14.3 typing_extensions-4.12.1 unyt-3.0.2 wcwidth-0.2.13 widgetsnbextension-4.0.11 yt-4.3.1
```

But for uv this appears to fail:

```
$ uv pip install --dry-run --only-binary :all: --no-binary yt yt
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of yt are available:
          yt<=3.2.3
          yt>=3.3.0
      and yt==1.0.1 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<1.5
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==1.5 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<1.6
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==1.6 has no usable wheels and building from source is disabled and yt==1.6.1 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<1.7
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==1.7 has no usable wheels and building from source is disabled and yt==2.0 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<2.0.1
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==2.0.1 has no usable wheels and building from source is disabled and yt==2.1 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<2.2
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==2.2 has no usable wheels and building from source is disabled and yt==2.3 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<2.4
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==2.4 has no usable wheels and building from source is disabled and yt==2.5 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<2.5.1
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==2.5.1 has no usable wheels and building from source is disabled and yt==2.5.2 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<2.5.3
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==2.5.3 has no usable wheels and building from source is disabled and yt==2.5.4 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<2.5.5
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==2.5.5 has no usable wheels and building from source is disabled and yt==2.6 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<2.6.1
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==2.6.1 has no usable wheels and building from source is disabled and yt==2.6.2 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<2.6.3
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==2.6.3 has no usable wheels and building from source is disabled and yt==3.0 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<3.0.1
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==3.0.1 has no usable wheels and building from source is disabled and yt==3.0.2 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<3.1
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==3.1 has no usable wheels and building from source is disabled and yt==3.2 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<3.2.1
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==3.2.1 has no usable wheels and building from source is disabled and yt==3.2.2 has no usable wheels and building from source is disabled, we can conclude that any of:
          yt<3.2.2.post1
          yt>3.2.3,<3.3.0
       cannot be used.
      And because yt==3.2.2.post1 has no available source distribution and using wheels is disabled and yt==3.2.3 has no usable wheels and building from source is disabled, we can conclude that yt<3.3.0 cannot be used.
      And because yt==3.3.0 has no usable wheels and building from source is disabled and yt==3.3.1 has no usable wheels and building from source is disabled, we can conclude that yt<3.3.2 cannot be used.
      And because yt==3.3.2 has no usable wheels and building from source is disabled and yt==3.3.3 has no usable wheels and building from source is disabled, we can conclude that yt<3.3.3.post1 cannot be used.
      And because yt==3.3.3.post1 has no available source distribution and using wheels is disabled and yt==3.3.4 has no usable wheels and building from source is disabled, we can conclude that yt<3.3.5 cannot be used.
      And because yt==3.3.5 has no usable wheels and building from source is disabled and yt==3.4.0 has no usable wheels and building from source is disabled, we can conclude that yt<3.4.1 cannot be used.
      And because yt==3.4.1 has no usable wheels and building from source is disabled and yt==3.5.0 has no usable wheels and building from source is disabled, we can conclude that yt<3.5.1 cannot be used.
      And because yt==3.5.1 has no usable wheels and building from source is disabled and yt==3.6.0 has no usable wheels and building from source is disabled, we can conclude that yt<3.6.1 cannot be used.
      And because yt==3.6.1 has no usable wheels and building from source is disabled and yt==4.0.0 has no usable wheels and building from source is disabled, we can conclude that yt<4.0.1 cannot be used.
      And because yt==4.0.1 has no usable wheels and building from source is disabled and yt==4.0.2 has no usable wheels and building from source is disabled, we can conclude that yt<4.0.3 cannot be used.
      And because yt==4.0.3 has no usable wheels and building from source is disabled and yt==4.0.4 has no usable wheels and building from source is disabled, we can conclude that yt<4.0.5 cannot be used.
      And because yt==4.0.5 has no usable wheels and building from source is disabled and yt==4.1.0 has no usable wheels and building from source is disabled, we can conclude that yt<4.1.1 cannot be used.
      And because yt==4.1.1 has no usable wheels and building from source is disabled and yt==4.1.2 has no usable wheels and building from source is disabled, we can conclude that yt<4.1.3 cannot be used.
      And because yt==4.1.3 has no usable wheels and building from source is disabled and yt==4.1.4 has no usable wheels and building from source is disabled, we can conclude that yt<4.2.0 cannot be used.
      And because yt==4.2.0 has no usable wheels and building from source is disabled and yt==4.2.1 has no usable wheels and building from source is disabled, we can conclude that yt<4.2.2 cannot be used.
      And because yt==4.2.2 has no usable wheels and building from source is disabled and yt==4.3.0 has no usable wheels and building from source is disabled, we can conclude that yt<4.3.1 cannot be used.
      And because yt==4.3.1 has no usable wheels and building from source is disabled and you require yt, we can conclude that the requirements are unsatisfiable.

      hint: Pre-releases are available for yt in the requested range (e.g., 3.3.dev0), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

But let me know if there's a better solution for my use case.

---

_Comment by @zanieb on 2024-06-05 19:43_

Looking into this...

---

_Comment by @zanieb on 2024-06-05 19:58_

I put up a draft at https://github.com/astral-sh/uv/pull/4067

Implementation wise this isn't too difficult. I'm unsure of the ramifications though.

---

_Comment by @notatallshaw-gts on 2024-06-05 20:18_

> I put up a draft at https://github.com/astral-sh/uv/pull/4067

I swear I only glanced away from my emails for a moment between "Looking into this..." and "I put up a draft" 😆 

> I'm unsure of the ramifications though.

I agree it's not clear that this is what the user would want 100% of the time, but I think it does makes sense that the more specific requirement would override the less specific requirement.

If it's not agreed that uv should allow this, I think it would make sense to throw an immediate error on specifying `--no-binary <anything>` with `--only-binary :all:`.

---

_Comment by @zanieb on 2024-06-05 20:25_

:)

Yeah we should definitely improve the error instead of attempting to resolve. I'll open some issues around that.

---

_Closed by @zanieb on 2024-06-12 20:47_

---
