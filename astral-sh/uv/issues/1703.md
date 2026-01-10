```yaml
number: 1703
title: Add support for managing Conda environments and packages
type: issue
state: open
author: matterhorn103
labels:
  - wish
assignees: []
created_at: 2024-02-19T16:20:06Z
updated_at: 2025-07-23T14:26:50Z
url: https://github.com/astral-sh/uv/issues/1703
synced_at: 2026-01-10T03:32:43Z
```

# Add support for managing Conda environments and packages

---

_Issue opened by @matterhorn103 on 2024-02-19 16:20_

I'm very excited by uv and the possibilities it hints at for the future of one-stop packaging and environment management.

As for a vast number of scientists, though, conda is pretty much essential to many of my projects. A package manager limited to PyPI may replace pip and venv for pure Python projects for me but will always have to sit alongside conda with conda-forge.

I get that it's still very early days, but are there any ambitions/plans to truly turn Python packaging into a single effective ecosystem and unite the science and programming camps? Might uv in future also manage conda environments or use packages from conda-forge/other repos?

Or should I reign in my excitement a little?

---

_Label `question` added by @zanieb on 2024-02-19 16:54_

---

_Comment by @zanieb on 2024-02-19 16:55_

This might be out of scope for us. I'm not sure about the long-term future, but definitely in the short term. [Prefix.dev](https://prefix.dev/) is more focused on conda integration.

---

_Comment by @matterhorn103 on 2024-02-19 19:02_

Ok, thanks. I understand, but still a pity. A single Python management tool for both camps would be nice one day.

Hopefully the collaboration between you and the pixi team continues and they become complementary tools following the same standards :)

---

_Comment by @forrestfwilliams on 2024-02-21 13:30_

Wanted to echo my support for this question as well. Many AI/ML and scientific workflows rely on Python packages that have complex C/C++ dependencies. [GDAL](https://pypi.org/project/GDAL/) is a good example of a package that is essential for many Python applications, but installing it via Pip requires that you have already installed the GDAL C library your machine.

Conda-based tools generally do a good job of managing both the Python and C/C++ dependencies, which is why many scientists use this ecosystem. Unfortunately, using a package manager that cannot manage these lower-level dependencies is a non-starter for many of us.

Would love to see the `uv` team try to come up with their own solution for this problem sometime in the future!

---

_Comment by @sam-goodwin on 2024-04-10 23:37_

Just ran into this with a requirement to use `mambda` to use a specific version of `tensorflow` on a Mac. It would be great if UV could support conda environments.

---

_Comment by @den-run-ai on 2024-05-07 18:58_

just had an issue that uv does not work in a conda environment, but installation is ok. uv binary is missing.

---

_Comment by @marcelroed on 2024-08-23 00:03_

With the latest stabilizations `uv` has covered a majority of my needs for a Python packaging solution. However, the needs of Python users in the scientific space go a little further than the stated goal of being to Python what Cargo is to Rust. In scientific Python, environments are expected to include system libraries and other important packages that don't mesh well with PyPI.

Currently my environments require something like `micromamba` or `pixi` for system packages and binaries, and then `uv` to install PyPI packages that either can't be sourced from Conda repos, or are easier to manage in PyPI. For example, I might need to install a modern C++ compiler and some system libraries with conda before using `uv` to install the rest of my requirements. A key issue with this is that solving is not possible across this boundary, leading to environments that deteriorate over time, and the requirements for an install script that orders installs instead of just a single `requirements.txt` (or `environment.yml`) that can be solved.

In short, supplanting `pip` has been a brilliant beginning for `uv` to take off, but I can't see a world where scientific Python has its needs met by a solution that can only solve using packages on PyPI. Python is not Rust, and users expect an all-encompassing Python package manager to provide more than what `pip` currently does.

@zanieb, have there been any developments in thinking about expanding capabilities in this direction, or is this still out of scope? I'm partially served by `pixi` for my use-cases, but I still find just using `uv` from the terminal to be superior when dealing with the PyPI side of package management.

Thanks for the great work in this space! `uv` has been a huge boost to my morale and productivity.

---

_Comment by @zanieb on 2024-08-23 12:42_

Thanks for the well written comment.

We care about scientific Python (several of us worked in that domain before Astral), but right now we're focused on polishing the project management experience and working on correctness. Hopefully we'll be able to figure out a good story for Conda integration, but it's not in the immediate scope.

> Thanks for the great work in this space! uv has been a huge boost to my morale and productivity.

That's great to hear <3

---

_Label `wish` added by @zanieb on 2024-08-23 12:42_

---

_Label `question` removed by @zanieb on 2024-08-23 12:42_

---

_Comment by @shuowpro on 2024-09-04 00:09_

Is it possible to export a Conda environment YAML file? It already supports exporting a `requirements.txt` file, so adding support for exporting a Conda environment file shouldn't be difficult. As a temporary workaround, we can still export the environment file while using pip as the package manager.

---

_Comment by @matterhorn103 on 2024-09-10 13:38_

@zanieb related to your response in [#6989](https://github.com/astral-sh/uv/issues/6989#issuecomment-2328978747):

> I think it's okay to support if it works, but I'm not sure how well it will translate since Conda has, for example, different names for some packages. We can't actually perform a resolution with Conda.

It has felt to me for a while that the unknown correspondence between PyPI and Conda packages is the biggest thing keeping the ecosystems separate. When looking into the topic many months ago I came across Grayskull, [this thread discussing the exact problem](https://github.com/conda/grayskull/issues/168), and [this bot-generated mapping effort](https://github.com/regro/cf-graph-countyfair/blob/master/mappings/pypi/grayskull_pypi_mapping.yaml). By no means exhaustive, but I imagine some mapping like that would be a necessary starting point for Conda/PyPI interop, so figured I'd leave the links here.

---

_Comment by @rosmur on 2024-11-18 07:27_

@matterhorn103 alternatively, is there any (technical) reason to continue using anything in the conda ecosystem? I've made the decision to never install anaconda or anything related on my new MacBook and im all in on uv - its the future. 

---

_Comment by @matterhorn103 on 2024-11-18 08:56_

Like you, I don't use Conda personally at all any more. But I know that colleagues do and are unlikely to be budged. Part of that is that although PyPI can now, thanks to wheels, match Conda in the historically difficult things like `numpy`, in my field at least programs are often distributed via `conda-forge` that have no Python content whatsoever and will therefore never be on PyPI.

---

_Comment by @tpanum on 2024-11-18 09:10_

I try to avoid `conda` in favor of `uv` whenever possible. However, certain packages have so complex external dependencies its is not always feasible.

---

_Comment by @bart-maykin on 2024-11-22 15:55_

There's https://github.com/nextgis/pygdal which installs without problems with pip, but uv pip fails.
`pip install  pygdal==3.4.1.10`  install fine.
`uv pip install  pygdal==3.4.1.10` gives this output: (the version is needed because of the C library version I have installed)
```console
Resolved 2 packages in 5ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: pygdal==3.4.1.10


[stdout]
running bdist_wheel
running build
running build_py
copying ./osgeo/gnm.py -> build/lib.linux-x86_64-cpython-311/osgeo
copying ./osgeo/gdalnumeric.py -> build/lib.linux-x86_64-cpython-311/osgeo
copying ./osgeo/gdalconst.py -> build/lib.linux-x86_64-cpython-311/osgeo
copying ./osgeo/gdal_array.py -> build/lib.linux-x86_64-cpython-311/osgeo
copying ./osgeo/utils.py -> build/lib.linux-x86_64-cpython-311/osgeo
copying ./osgeo/gdal.py -> build/lib.linux-x86_64-cpython-311/osgeo
copying ./osgeo/osr.py -> build/lib.linux-x86_64-cpython-311/osgeo
copying ./osgeo/__init__.py -> build/lib.linux-x86_64-cpython-311/osgeo
copying ./osgeo/ogr.py -> build/lib.linux-x86_64-cpython-311/osgeo
running egg_info
writing pygdal.egg-info/PKG-INFO
writing dependency_links to pygdal.egg-info/dependency_links.txt
writing requirements to pygdal.egg-info/requires.txt
writing top-level names to pygdal.egg-info/top_level.txt

[stderr]
Traceback (most recent call last):
  File "<string>", line 11, in <module>
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/build_meta.py", line 435, in build_wheel
    return _build(['bdist_wheel'])
           ^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/build_meta.py", line 426, in _build
    return self._build_with_temp_dir(
           ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/build_meta.py", line 407, in _build_with_temp_dir
    self.run_setup()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/build_meta.py", line 522, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/build_meta.py", line 320, in run_setup
    exec(code, locals())
  File "<string>", line 225, in <module>
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/__init__.py", line 117, in setup
    return distutils.core.setup(**attrs)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 183, in setup
    return run_commands(dist)
           ^^^^^^^^^^^^^^^^^^
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 199, in run_commands
    dist.run_commands()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 954, in run_commands
    self.run_command(cmd)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/dist.py", line 995, in run_command
    super().run_command(command)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 973, in run_command
    cmd_obj.run()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/bdist_wheel.py", line 381, in run
    self.run_command("build")
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 316, in run_command
    self.distribution.run_command(command)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/dist.py", line 995, in run_command
    super().run_command(command)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 973, in run_command
    cmd_obj.run()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/command/build.py", line 135, in run
    self.run_command(cmd_name)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 316, in run_command
    self.distribution.run_command(command)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/dist.py", line 995, in run_command
    super().run_command(command)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 973, in run_command
    cmd_obj.run()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/build_py.py", line 78, in run
    self.build_package_data()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/build_py.py", line 169, in build_package_data
    for target, srcfile in self._get_package_data_output_mapping():
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/build_py.py", line 161, in _get_package_data_output_mapping
    for package, src_dir, build_dir, filenames in self.data_files:
                                                  ^^^^^^^^^^^^^^^
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/build_py.py", line 87, in __getattr__
    self.data_files = self._get_data_files()
                      ^^^^^^^^^^^^^^^^^^^^^^
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/build_py.py", line 93, in _get_data_files
    self.analyze_manifest()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/build_py.py", line 191, in analyze_manifest
    self.run_command('egg_info')
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 316, in run_command
    self.distribution.run_command(command)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/dist.py", line 995, in run_command
    super().run_command(command)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 973, in run_command
    cmd_obj.run()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/egg_info.py", line 313, in run
    self.find_sources()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/egg_info.py", line 321, in find_sources
    mm.run()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/egg_info.py", line 544, in run
    self.add_defaults()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/egg_info.py", line 582, in add_defaults
    sdist.add_defaults(self)
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/command/sdist.py", line 109, in add_defaults
    super().add_defaults()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/command/sdist.py", line 238, in add_defaults
    self._add_defaults_ext()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/command/sdist.py", line 322, in _add_defaults_ext
    build_ext = self.get_finalized_command('build_ext')
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 303, in get_finalized_command
    cmd_obj.ensure_finalized()
  File "/home/user/.cache/uv/builds-v0/.tmpkEGGLO/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 111, in ensure_finalized
    self.finalize_options()
  File "<string>", line 129, in finalize_options
  File "<string>", line 30, in get_numpy_include
AttributeError: 'dict' object has no attribute '__NUMPY_SETUP__'. Did you mean: 'get_data_files'?
```

---

_Comment by @zanieb on 2024-11-22 15:57_

@bart-maykin please open a new issue, this does not seem like the right place for that problem.

---

_Renamed from "conda environment management" to "Add support for managing Conda environments and packages" by @zanieb on 2024-11-26 23:58_

---

_Comment by @tobiasdiez on 2024-11-30 06:02_

The following is a concrete proposal how initial conda support may look like in `uv`:
- Add support for [PEP 725 External dependencies](https://peps.python.org/pep-0725/). This allows to specify non-python dependencies in `pyproject.toml`. For example, scipy may declare
  ```toml
  [external]
  build-requires = [
    "virtual:compiler/c",
    "virtual:compiler/cpp",
    "virtual:compiler/fortran",
    "pkg:generic/ninja",
    "pkg:generic/pkg-config",
  ]
  host-requires = [
    "virtual:interface/blas",
    "virtual:interface/lapack",
  ]
  ```
  "Support" here would mean at least that one could add external dependencies via `uv add` and that `uv` can read these information from `pyproject.toml`. This should actually be straight-forward and not too involved.

  At this point, the `pyproject.toml` file contains all the information to create a conda environment, including python and non-python dependencies.

- Render these information as a conda environment file. For this, the python dependencies and the external dependencies need to be translated into the correct conda packages. As @matterhorn103 mentioned above, grayskull maintains such a mapping that could be reused for this purpose. Then the user can use the generated conda environments files via mamba/conda.

- Bonus: Streamline the process into one command. For example, `uv sync` in an activated conda environment should also update the non-python dependencies. 
- End goal: Use https://github.com/conda/rattler to directly and transparently solve the conda environment, bypassing the export step completely.

In https://github.com/sagemath/sage/pull/37447, I've implemented this strategy in a custom python script (not using uv at all). It works quite well.

---

_Comment by @xinyu-dev on 2024-12-02 16:35_

I also support this. There are some biology packages that are only conda installable. 

---

_Comment by @hoaihuongbk on 2024-12-11 04:04_

+1000 for this support. This simplifies the operational of managing the python dependencies on a large scale.

---

_Comment by @fepegar on 2024-12-15 23:05_

> Is it possible to export a Conda environment YAML file? It already supports exporting a `requirements.txt` file, so adding support for exporting a Conda environment file shouldn't be difficult. As a temporary workaround, we can still export the environment file while using pip as the package manager.

@shuowpro, I've written [`uv2conda`](https://pypi.org/project/uv2conda/) for that. You might find it useful.

---

_Comment by @tupui on 2024-12-16 18:35_

Adding my upvote here. We are adding some recommendations to uv in SciPy's doc but we also need to still recommend things like pixi until we have a solution for Conda.

@charliermarsh I believe support for non Python deps is around the corner?

---

_Comment by @zanieb on 2024-12-16 18:52_

@tupui Happy to review those SciPy changes, if you want to ping me.

I don't think support for non-Python dependencies is around the corner though :)

---

_Comment by @binbjz on 2025-01-02 02:12_

I've been using pyenv to manage python versions and miniconda and anaconda versions. I really need support for anaconda and miniconda, this is so critical.

---

_Comment by @notatallshaw on 2025-01-02 10:17_

I think Astral are aware people would really like this feature, I don't think there's need for any more "me too" comments, add a thumbs up to the original post. Unless you have some novel use case not already described here.

Also, for those who really need conda packages and/or environments, and are not already aware, pixi provides project management, like uv, and does both conda and standard Python packages: https://github.com/prefix-dev/pixi

---

_Comment by @matterhorn103 on 2025-01-02 15:00_

Thought I'd just drop a link here to raise awareness of a [PR](https://github.com/conda/conda/pull/14067) I have open over at the conda repo which would add support for creating a conda environment from a specification in a `pyproject.toml` file.

I like to think that it would be a good first step towards better PyPI-conda interop and would help a little in enabling projects like `uv` to achieve this feature in future.

---

_Comment by @tupui on 2025-01-02 16:59_

> Thought I'd just drop a link here to raise awareness of a [PR](https://github.com/conda/conda/pull/14067) I have open over at the conda repo which would add support for creating a conda environment from a specification in a `pyproject.toml` file.
> 
> I like to think that it would be a good first step towards better PyPI-conda interop and would help a little in enabling projects like `uv` to achieve this feature in future.

Yeah that might be the most sensible way forward tbh. I hate to have a specific file for the conda env while the whole ecosystem of tools transitioned from having their custom file to using the pyproject. Everything conda could very well be defined there.

---

_Comment by @MilkClouds on 2025-01-23 06:49_

I'm the whom wants to manage virtual environment in cross-project manner, and `conda` is the best solution fits to this. And I want to use this feature along with `uv`'s powerful other features.
I would be very happy if `uv` supports feature to manage virtual environment in cross-project manner.

Installing/uninstalling, or managing `conda` package with `uv` is not a instant requirement for me.


Sidenotes:
`poetry` supports integration with `conda`, which installs package in `conda` virtual environment if non-base environment is activated. This feature is very helpful to me.

---

_Comment by @MilkClouds on 2025-01-23 06:54_

Even it's not a seems-good workaround, `pixi`'s virtual environment even can be registered as `conda` environment.
https://stackoverflow.com/questions/70851048/does-it-make-sense-to-use-conda-poetry

---

_Comment by @chrisrodrigue on 2025-01-25 16:51_

It would be awesome if uv could install conda packages into venvs.

On Windows, and likely other platforms, Anaconda bundles hundreds of `*.conda` packages in the `anaconda3/pkgs` directory and stores the interpreter at `anaconda3/python.exe`. 

uv makes environments way faster than conda, so if one could use a directory of conda packages as a package index for uv, it would enable powerful offline workflows.

---

_Comment by @chrisrodrigue on 2025-01-25 17:13_

Side note… `conda init` modifies the user PowerShell/pwsh profiles to activate the base environment on every new shell, which is *extremely* slow. uv is already on the PATH and knows where the interpreter is, so it can fix this completely.

---

_Comment by @trungleduc on 2025-01-26 23:13_

why do people keep using `conda` while `mamba` or `pixi` can do the same job but is way faster?

---

_Comment by @notatallshaw on 2025-01-27 00:15_

> why do people keep using `conda` while `mamba` or `pixi` can do the same job but is way faster?

This is off topic, but conda now defaults to the C++ mamba resolver engine. So the speed differences aren't as nearly big as they used to be.

That, and some users rely on the Anaconda distribution, for, convince, licensing (uv obviously can't solve that), or other enterprise service guarantees.

---

_Comment by @charliermarsh on 2025-01-27 01:06_

> why do people keep using `conda` while `mamba` or `pixi` can do the same job but is way faster?

Let's try to keep the conversation on-topic -- this doesn't seem relevant to uv.


---

_Comment by @Yc-Chen on 2025-02-25 17:04_

I recently changed our conda/mamba based project to using uv and it works. I already like uv more because it resolves pytorch in a more considerate way: uv distinguishes mac vs. windows/linux whereas conda doesn't.

So I understand the wish, but what exactly? `pyproject.toml` is already more complete than `environment.yml`, so supporting `environment.yml` is not a good idea.

---

_Comment by @trungleduc on 2025-02-25 17:33_

> So I understand the wish, but what exactly?

Lots of packages are available on `conda-forge` but not `pypi`

---

_Comment by @matterhorn103 on 2025-02-25 18:50_

> So I understand the wish, but what exactly? `pyproject.toml` is already more complete than `environment.yml`, so supporting `environment.yml` is not a good idea.

It's about several things, but speaking primarily from a sciences perspective I can off the top of my head come up with:

1. **Package availability.** As mentioned by @trungleduc, the repositories simply contain different packages and particularly scientific ones often necessitate conda. This was historically about big ones like `numpy` and `pytorch`, these days it's more about small ones with a purely scientific audience – tools, programs, and libraries created and released as part of academic research, for example. In computational chemistry nothing ever seems to get put on PyPI.
2. **Necessity for collaboration.** Many research projects and groups of researchers use conda, many researchers have a strong preference for/investment in conda and are not going to be persuaded to switch, I imagine many places have an institute-wide policy or need for conda. At the moment collaborating in such environments is impossible with `uv`.
3. **Backwards compatibility.** Scientific Python projects are often not updated long-term, especially if they were produced in an academic environment, or are research projects full of scripts. Many have their environment specified exclusively in an `environment.yml`. So to use or replicate that work you are obliged to use a tool that understands conda, and that need is unlikely to go away.
4. **Continued recommendation.** Tutorials or package READMEs aimed at scientists will, in my experience, exclusively recommend the use of conda and installation from `conda-forge`, which means the above are not likely to change any time soon.

---

_Comment by @mfisher87 on 2025-02-25 19:24_

To expand on point (1), some of those packages available on conda are general purpose shared libraries that can't be installed with pypi, for example libgdal which underpins most libraries that work with raster and vector geospatial data. You can only install the GDAL python bindings with PyPI. The alternative to using conda for this is using the system package manager or Nix. Ignoring the latter, the former will only offer one version out-of-the-box, and so you would need to manually constrain your python environment to be compatible with that likely-old version. This would create a dependency between the python environment and the user's operating system choice, making it difficult to collaborate with users across operating systems. For example Windows users might use OSGeo4W, and likely get a different version of the shared library than someone installing gdal with `apt` on Ubuntu 24.

---

_Comment by @fperez on 2025-02-25 19:24_

And to expand on #1 above by @matterhorn103 and @trungleduc - the key point is that many key scientific dependencies will _never_ be on PyPI b/c they have zero Python in them, yet they are used as computational engines wrapped by a Python layer. VTK or ITK are classic examples of such packages, that were a royal, spectacular pain to install prior to conda-forge offering them, but there's a lot more like it.

All of the other points @matterhorn103 makes above are also critical, and #3 is a big one. We've made big strides in convincing the scientific community to share their code and make it reproducible, but for now that's anchored in offering reasonable `environment.yml` files. For most scientists that's already a big ask and _it works_.  Supporting that community and thousands of legacy uses (often tied to published research) is extremely important, IMHO.

---

_Comment by @fperez on 2025-02-25 19:25_

Oops, basically repeated what @mfisher87 said 😆 


---

_Comment by @mfisher87 on 2025-02-25 19:26_

I saw your post come through literally the same second I submitted mine 😆 

---

_Comment by @Yc-Chen on 2025-02-25 20:11_

Thanks to @fperez @mfisher87 @trungleduc for the explanation!

Just add one point for uv - and also one of the reasons I did the switch - is because the latest version of pytorch [stopped official conda support](https://pytorch.org/get-started/locally/#start-locally) (and also image below). And I was very happy to see uv has already [documented](https://docs.astral.sh/uv/guides/integration/pytorch/) how to install pytorch. I wonder how this will change point (2) and (4) in the future.

<img width="809" alt="Image" src="https://github.com/user-attachments/assets/eeca49d0-a39b-4a4e-9a13-d5377ae14418" />


For point (1) that many packages do not exist on pypi, that's a broader problem than a package manager. It is also about a place to publish and host packages, and a `.conda` format instead of whl format. Since uv works with `pyproject.toml`, I searched a bit whether conda can be more compatible with `pyproject.toml`. And I found this `conda-lock` most relevant. Maybe that could be an idea to make a step towards:

https://conda.github.io/conda-lock/

---

_Comment by @tupui on 2025-02-25 20:47_

There is a proposal of a PEP to be able to express system and other dependencies in pyproject.toml. Until that happens, conda can still decide to have a specific section in the pyproject.toml to define these extra bits. It's yaml to toml 🤷 there is no reason to not do that now.

---

_Comment by @mfisher87 on 2025-02-25 23:56_

https://peps.python.org/pep-0725/

---

_Comment by @Yc-Chen on 2025-02-26 08:10_

https://github.com/conda/conda/issues/12462
It has been there for >1 year....

---

_Comment by @tupui on 2025-02-26 15:49_

> https://github.com/conda/conda/issues/12462
> It has been there for >1 year....

Yes. Still something they have to do. They are the one not in-line with the rest of the ecosystem 😉

---

_Comment by @mfisher87 on 2025-03-17 15:46_

I had a discussion about this at some point with someone very familiar with the ecosystem (I think a conda forge developer?) and they raised that `pyproject.toml` is Python-specific, and `conda` is not. Supporting `pyproject.toml` would mean they would have to treat it as an optional format and continue support for the non-python-specific format.

Pixi has decided that for them, supporting two formats is worth it. I'm glad they did :) But `pixi` is at a different level of abstraction (project management) than conda (environment management), so that likely made this decision easier. I don't know if `conda` developers necessarily see this as something they "have to do", and I would agree with that. Right now, `pixi` is leading the charge on bringing excellent ergonomics to this space, and I'm so excited about that :)

---

_Comment by @DOSull on 2025-04-17 11:28_

Definitely would like to see this - both support for the not-pure-python packages of conda (which sounds hard), but more straightforwardly (I would think) the option to put virtual environments in some central location, like `/opt/uv/lib/...` or `~/.uv/envs` or somesuch with an associated ability to run `uv activate <environment-name>` from anywhere.

Sometimes I want to quickly try out an idea and I know that the package I need exists in some environment. In `conda` I just drop into the command line and no matter where I am in the file system `conda activate <environment-name>` let's me fire up python and explore. With `uv` as currently configured I have to also remember where that environment is on the file system. That's not a huge burden, but an option to have a centralised location for environments doesn't seem like a big ask.

Similarly a centralised location gives an IDE like vscode or whatever the ability to offer all the virtual environments installed on a machine as options if your exploring some ideas in a notebook say, not working on a particular project.

Finally, I know we all have giant hard drives now, but seriously... virtual environments take up a lot of space and scattering them all over every project folder when many projects will have common requirements seems like a bit of a waste of storage space!

---

_Comment by @zanieb on 2025-04-21 03:00_

> the option to put virtual environments in some central location

You're looking for #1495 

> Sometimes I want to quickly try out an idea and I know that the package I need exists in some environment. In conda I just drop into the command line and no matter where I am in the file system

The non-environment centric way to do this is just (for example) `uvx --with requests,httpx python` — no need to remember the name or path of an environment.

> virtual environments take up a lot of space and scattering them all over every project folder when many projects will have common requirements seems like a bit of a waste of storage space!

To avoid confusion here: we use hard-links and copy-on-write with a shared cache for packages, each virtual environment does not consume additional space.

---

_Comment by @DOSull on 2025-04-21 23:46_

> The non-environment centric way to do this is just (for example) uvx --with requests,httpx python — no need to remember the name or path of an environment.

This is really useful to know. I think I'd still like the option for some more permanent configurations accessible from a central location, but this is really nice for one of my use-cases.

---

_Comment by @zanieb on 2025-04-21 23:50_

@DOSull in that case, I'd recommend just creating a project in your central location then doing `uv run --project ~/my-central-thing python` (or `uvx --with ~/my-central-thing python`). Anyway, we'll add support for centralized environments eventually — it's just a matter of finding the time to design and implement it.

---

_Comment by @ubaldot on 2025-05-06 17:47_

FWIW: conda is an agnostic package manager (you can e.g. install gcc, pandoc, etc.) not only for python packages. And you can install them in separate environments. Plus it really checks dependencies conflicts. Plus it aggressively check SSL stuff. I like to think it as a "light docker". 

---

_Comment by @matterhorn103 on 2025-07-23 14:26_

> [@zanieb](https://github.com/zanieb) related to your response in [#6989](https://github.com/astral-sh/uv/issues/6989#issuecomment-2328978747):
> 
> > I think it's okay to support if it works, but I'm not sure how well it will translate since Conda has, for example, different names for some packages. We can't actually perform a resolution with Conda.
> 
> It has felt to me for a while that the unknown correspondence between PyPI and Conda packages is the biggest thing keeping the ecosystems separate. When looking into the topic many months ago I came across Grayskull, [this thread discussing the exact problem](https://github.com/conda/grayskull/issues/168), and [this bot-generated mapping effort](https://github.com/regro/cf-graph-countyfair/blob/master/mappings/pypi/grayskull_pypi_mapping.yaml). By no means exhaustive, but I imagine some mapping like that would be a necessary starting point for Conda/PyPI interop, so figured I'd leave the links here.

[This thread](https://github.com/conda/grayskull/issues/564) on the grayskull repo is interesting with regards to this aspect of things.

Somehow I missed at the time of that comment that the Pixi team have been maintaining their own PyPI => `conda-forge` mapping using their [`parselmouth`](https://github.com/prefix-dev/parselmouth) tool. At the moment it seems to be only in that direction, with [work on the inverse](https://github.com/prefix-dev/parselmouth/pull/33) in progress but apparently stalled. It seems that [they'd like `parselmouth` to have it](https://github.com/conda/grayskull/issues/564#issuecomment-2436849218), though, and for it to be a library/reference useable by the whole ecosystem.

---
