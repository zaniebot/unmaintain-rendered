```yaml
number: 4422
title: Allow dependency name overriding or elimination
type: issue
state: open
author: leorochael
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-06-20T11:27:07Z
updated_at: 2025-10-31T14:18:11Z
url: https://github.com/astral-sh/uv/issues/4422
synced_at: 2026-01-10T03:23:52Z
```

# Allow dependency name overriding or elimination

---

_Issue opened by @leorochael on 2024-06-20 11:27_

While `uv` now has overrides, and issue #2686 is suggesting being able to declare certain overrides as pertaining to a specific dependency, I have an additional suggestion/request.

I would like to be able to override a package dependency with a package that has a different name.

As a concrete case, the [FuelSDK](https://pypi.org/project/FuelSDK/) package [depends](https://github.com/salesforce-marketingcloud/FuelSDK-Python/blob/master/setup.py#L20) on `suds-jurko`, but has been tested to work with it's fork/successor project [`suds-community`](https://github.com/suds-community/suds), a.k.a. just [`suds`](https://pypi.org/project/suds/) these days.

(Yes, the project has come full circle, with the fork of the fork officially taking over the original name).

A possibility that comes to mind would be a special syntax for the `-o overrides.txt` file like:

```plain
suds-jurko=>suds>=1.1,<2.0
```

Or perhaps:

```plains
suds-jurko:suds>=1.1,<2.0
```

Considering the long history of python packages that fork and continue the work of orphaned packages (e.g. PIL/Pillow), this could be a nice complement to the overrides functionality of uv.

Perhaps it could even be used to remove a dependency completely, e.g.:

* `suds-jurko=>!`
* `suds-jurko=>`
* `suds-jurko:!`
* `suds-jurko:`


---

_Comment by @leorochael on 2024-06-20 11:54_

Thinking about this some more, just the functionality of blocking a dependency from being installed already offers a way to do this kind of override.

That is, I could add to my `requirements.in`:

```
Fuel-SDK
suds>=1.1,<2.0
```

Then to my `overrides` one of the options for deleting a removing a dependency I mentioned previously, or perhaps just a special version. e.g one of:

* `suds-jurko==skip`
* `suds-jurko==none`
* `suds-jurko==`


---

_Label `enhancement` added by @zanieb on 2024-06-20 17:07_

---

_Comment by @zanieb on 2024-06-20 17:08_

Interesting idea! Makes some sense, though I'm also not quite sure how it should be expressed and if it's feasible.

---

_Label `needs-design` added by @zanieb on 2024-06-20 18:51_

---

_Renamed from "Allow package name overriding" to "Allow dependency name overriding or elimination" by @leorochael on 2024-06-21 11:41_

---

_Comment by @leorochael on 2024-06-21 12:21_

Another option to express dependency elision is to use the URI version specifier to mean: install this package from nowhere.

* `suds-jurko@skip:`
* `suds-jurko@about:blank`

The last one was suggested to me by github copilot:

> To explicitly reference nothing or create a URI that signifies an absence of a resource, you can use the `about:blank` URI. This URI leads to an empty document in web browsers and is often used to represent a null or non-existent resource in contexts where a URI is required but there is no actual resource to point to.

---

_Comment by @leorochael on 2024-06-21 12:29_

And since URIs effectively allow namespacing on schemes, and the definition of the interpretation of schemes is up to whoever introduces them, we could perhaps use schemes to implement the package replacement, e.g:

* `suds-jurko@replacement:suds>=1.1,<2.0`


---

_Comment by @leorochael on 2024-06-21 14:00_

Or, if we want to follow examples of other tools which use prefixed schemes, like `git+`, we could namespace the scheme with `uv+`, like:

* `suds-jurko@uv+skip:`
* `suds-jurko@uv+replacement:suds>=1.1,<2.0`

---

_Comment by @leorochael on 2024-06-21 15:55_

Note that `--no-emit-package/--unsafe-package suds-jurko` doesn't help here, because `suds-jurko` is broken at package building, so resolution still fails.

---

_Comment by @leorochael on 2024-06-21 16:09_

An alternative that would help me would be to just ignore the sub-dependencies of a package on a per-package basis.

That is, instead of having an elision or renaming on my overrides file, I would like to add something like this in my `requirements.in`:

```
# FuelSDK has broken sub-dependencies:
--no-deps-for FuelSDK
FuelSDK
# Corrected FuelSDK dependencies manually added here:
pyjwt >= 1.5.3
requests >= 2.18.4
suds >= 1.1.2
```

For reference, such an option has been under discussion for a while in pypa/pip#9948

---

_Comment by @charliermarsh on 2024-06-23 15:44_

This seems like a reasonable use-case to me.

---

_Comment by @hauntsaninja on 2024-07-24 23:14_

I think this would be very useful! At work, we have systems like this for first party packages, but not third party packages. It would make some things less painful if this were the case and unblock some other future use cases.

A related lower priority feature request would be the ability to override edges, not just nodes. E.g., if I know one package is overpinning a dependency in a way that I know is safe to override, I might still want to resolve older or error if something else pins it.

I don't have strong opinions on syntax. => is a good suggestion, but could be typo-ed. Maybe using `@@` as a separator? lhs is the thing to be overriden, rhs is empty or PEP 508
```
node>=2
node @@ node>=2  # same as above
node @@ node_nextgen>=2  # replace node with node_nextgen
node @@  # eliminate node (and don't resolve its deps)
node -> dep @@ dep>=2  # override node's dependency on dep
```
If syntax is controversial, another option is to switch to TOML or something, especially if uv has plans to use such a format somewhere else.

---

_Comment by @charliermarsh on 2024-07-27 15:45_

I realized that this actually is possible today... You can use a never-truthy marker. For example, to remove `typing-extensions`, use an `overrides.txt` like:

```
typing-extensions ; sys_platform == 'never'
```


---

_Comment by @charliermarsh on 2024-07-27 15:45_

(This works because with overrides, we just replace _all_ requirements of `typing-extensions` with whatever is in the overrides file.)

---

_Comment by @leorochael on 2024-07-29 09:13_

> (This works because with overrides, we just replace _all_ requirements of `typing-extensions` with whatever is in the overrides file.)

I'm surprised this works, because I'd expect it would result in the resolution being unsolvable...

And I'm slightly suspicious of relying on it and it being just accidental behaviour that might be "fixed" in the future...

---

_Comment by @charliermarsh on 2024-07-29 14:41_

@leorochael -- Can you say more about why you would expect that to be unsolvable? I would actually consider it a bug if the resolver failed there, rather than the other way around.

---

_Comment by @leorochael on 2024-07-29 15:33_

I might be reading it wrong, but for me an overrides file containing:

* `typing-extensions ; sys_platform == 'never'`

Mean:

* Only consider this `typing-extensions` version: the one that cannot be installed on your platform, because your platform is not `never`.

And if there are packages depending on `typing-extensions`, in my mind that would mean their dependencies would be unsolvable because of that.

To me, declaring a constraints override on an uninstallable package is different than declaring a constraints override on a package we're pretending is already installed.

---

_Comment by @leorochael on 2024-07-29 15:37_

Ah, but I see where my logic is failing.

The constraints override doesn't say: "depend on this uninstallable package".

It's saying: "when depending on `typing-extensions`, only do that if the platform is `never`", which never happens.

So, yeah, as long as we add a test for that and document it, then yes, that would already be supported.

---

_Comment by @hauntsaninja on 2024-07-29 20:21_

Oh, neat trick!

---

_Comment by @Avasam on 2025-02-03 00:23_

> I realized that this actually is possible today... You can use a never-truthy marker. For example, to remove `typing-extensions`, use an `overrides.txt` like:
> 
> ```
> typing-extensions ; sys_platform == 'never'
> ```

Neat! However it's giving warnings in recent uv versions:
`warning: Missing version constraint (e.g., a lower bound) for typing-extensions`

I'd expect a "dependency elimination" to not warn about that.

---

_Comment by @zanieb on 2025-02-03 00:50_

Yeah that seems incorrect. I wouldn't expect overrides to throw that warning at all (even if they're not an "elimination") cc @konstin 

---

_Comment by @tmm1 on 2025-02-06 04:15_

Is there a way to override the version of a transitive dependency, instead of removing it?

For example, if I have a venv with pytorch-nightly installed, then want to add `vllm`, it fails because that project has a `torch == 2.5.1` requirement listed.

```
❯ uv add vllm==0.7.1 
  × No solution found when resolving dependencies:
  ╰─▶ Because vllm==0.7.1 depends on torchvision==0.20.1 and testproject depends on torchvision==0.22.0.dev20250124+cu126, we can conclude that testproject and vllm==0.7.1 are incompatible.
      And because testproject depends on vllm==0.7.1 and your project requires testproject, we can conclude that your project's requirements are unsatisfiable.
```

---

_Comment by @notatallshaw on 2025-02-06 04:23_

> Is there a way to override the version of a transitive dependency, instead of removing it?

That's the whole purpose of dependency overrides:

https://docs.astral.sh/uv/reference/settings/#override-dependencies
https://docs.astral.sh/uv/pip/compile/#overriding-dependency-versions
https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides

---

_Comment by @hauntsaninja on 2025-02-06 05:57_

Btw, these days I actually find using uv's `dependency-metadata` to lie about the dependencies of my dependencies works better for me than overrides in most cases: https://docs.astral.sh/uv/reference/settings/#dependency-metadata

This is because it's narrower (i.e. override edges vs nodes as discussed in https://github.com/astral-sh/uv/issues/4422#issuecomment-2249044193 ). It's another very nice uv trick that flies under the radar a little :-)

---

_Comment by @davidblom603 on 2025-02-06 15:30_

A use case where you need this functionality is when a package depends on opencv.
I guess to confuse everyone, there are 4 opencv packages: 
- opencv-python
- opencv-contrib-python
- opencv-python-headless
- opencv-contrib-python-headless

And they are conflicting, so you can only install one package without running into import errors at runtime.

---

_Comment by @t-kalinowski on 2025-03-31 13:35_

Is there a way to override a dependency with `uv run`? Setting `UV_OVERRIDE` or `UV_CONSTRAINT` doesn’t seem to work for me.

Here’s the issue I’m running into:

I’m using both PyTorch and TensorFlow (with tensorflow-text). I want PyTorch to be GPU-capable and TensorFlow to run on CPU only. To do that, I explicitly declare `tensorflow-cpu`. However, `tensorflow-text` pulls in `tensorflow`, so I end up with both `tensorflow` and `tensorflow-cpu` being installed.

When running with `uv run`, this leads to a race condition where it's unpredictable which version of TensorFlow gets used.

Ideally, I’d like to tell `uv`: please don’t install `tensorflow`; treat `tensorflow-cpu` as satisfying the requirement from `tensorflow-text`.

This solution would ideally be accessible via `uv run` command line arguments, because that’s the mechanism that [reticulate](https://posit.co/blog/reticulate-1-41/) uses to resolve ephemeral venvs.

E.g., something like
```bash
uv run --no-project --no-cache --python 3.11.11 \
  --with tensorflow-cpu \
  --with tensorflow-text \
  --with 'tensorflow; from=tensorflow-cpu' \
-- python -c 'import tensorflow as tf; tf.config.list_physical_devices()' 
```

---

_Comment by @carolynsoo on 2025-04-03 20:43_

> A use case where you need this functionality is when a package depends on opencv.

So this is exactly my current issue with uv.. I want only `opencv-python-headless` (on some machines that don't have `libGL.so`), but I depend on wheels that include `opencv-python`.

In root pyproject.toml I've set `sys_platform == 'never'"` for `opencv-python` in `override-dependencies`, but there are still files in various packages (each package has its own `pyproject.toml`, where I tried adding `opencv-python-headless`) that still complain `ModuleNotFoundError: No module named 'cv2'`. Am I missing a step?

---

_Comment by @collinanderson on 2025-05-15 16:11_

I'm finding that even with the `sys_platform == 'never'` trick, uv still downloads's the package metadata, and the package still ends up with a version number the `uv.lock` file, and therefore would still get dependabot notifications for the dependency. (In my case `nodeenv` when installing `pyright[nodejs]`) Would be nice to have some sort of dependency exclusion list.

---

_Comment by @charliermarsh on 2025-05-15 16:13_

Please provide a complete reproduction — hard to help without more details.

---

_Comment by @collinanderson on 2025-05-15 16:27_

Steps to reproduce (uv 0.7.3):
```
uv init tmp
cd tmp
echo -e "[tool.uv]\n override-dependencies = [ \"nodeenv ; sys_platform == 'never'\" ]" >> pyproject.toml
uv add 'pyright[nodejs]'
grep -1 nodeenv uv.lock
```
It includes a `[[package]]` entry in `uv.lock` with a version number that can go stale. Ideally it would be nice to not include the `[[package]]` entry at all, or somehow have null `version`, `source`, `sdist`, `wheels`, etc.
```
[[package]]
name = "nodeenv"
version = "1.9.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/43/16/fc88b08840de0e0a72a2f9d8c6bae36be573e475a6326ae854bcc549fc45/nodeenv-1.9.1.tar.gz", hash = "sha256:6ec12890a2dab7946721edbfbcd91f3319c6ccc9aec47be7c7e6b7011ee6645f", size = 47437, upload-time = "2024-06-04T18:44:11.171Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/d2/1d/1b658dbd2b9fa9c4c9f32accbfc0205d532c8c6194dc0f2a4c0428e7128a/nodeenv-1.9.1-py2.py3-none-any.whl", hash = "sha256:ba11c9782d29c27c70ffbdda2d7415098754709be8a7056d79a737cd901155c9", size = 22314, upload-time = "2024-06-04T18:44:08.352Z" },
]
```

Also, less important, but I noticed if you set the `nodeenv` to an old version, `uv` still downloads it and tries to build it (I assume to gather metadata), which also ideally should also be avoided. Would be nice to have a way to skip it completely without caring about version.
```
uv init tmp
cd tmp
echo -e "[tool.uv]\n override-dependencies = [ \"nodeenv==1 ; sys_platform == 'never'\" ]" >> pyproject.toml
uv add 'pyright[nodejs]'
```
<details>
<summary>Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)</summary>

```
  × Failed to build `nodeenv==1.0.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
          requires = get_requires_for_build({})
        File "/home/collin/.cache/uv/builds-v0/.tmppwsTyc/lib/python3.13/site-packages/setuptools/build_meta.py", line 331, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/collin/.cache/uv/builds-v0/.tmppwsTyc/lib/python3.13/site-packages/setuptools/build_meta.py", line 301, in _get_build_requires
          self.run_setup()
          ~~~~~~~~~~~~~~^^
        File "/home/collin/.cache/uv/builds-v0/.tmppwsTyc/lib/python3.13/site-packages/setuptools/build_meta.py", line 512, in run_setup
          super().run_setup(setup_script=setup_script)
          ~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/collin/.cache/uv/builds-v0/.tmppwsTyc/lib/python3.13/site-packages/setuptools/build_meta.py", line 317, in run_setup
          exec(code, locals())
          ~~~~^^^^^^^^^^^^^^^^
        File "<string>", line 11, in <module>
        File "/home/collin/.cache/uv/sdists-v9/pypi/nodeenv/1.0.0/HfllLLqg_MJfEoVZ1fq8n/src/nodeenv.py", line 24, in <module>
          import pipes
      ModuleNotFoundError: No module named 'pipes'

      hint: This usually indicates a problem with the package or the build environment.
 help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```
</details>

Update: If the version in `uv.lock` is stale, running `uv lock --upgrade --dry-run` reports that the lock file needs to be updated. `uv sync --upgrade --check` however says `Would make no changes`, because the update wouldn't be synced, though it does say in grey text: `Would update lockfile at: uv.lock`.

---

_Comment by @ncoghlan on 2025-08-22 14:12_

The "use overrides to prevent installation" use case came up again recently in https://github.com/astral-sh/uv/issues/2500#issuecomment-3200409272 (as a way to instruct `uv` to skip installing packages available elsewhere on `sys.path` into the environment currently being updated)

---

_Comment by @LTMeyer on 2025-09-24 15:07_

> Btw, these days I actually find using uv's `dependency-metadata` to lie about the dependencies of my dependencies works better for me than overrides in most cases: https://docs.astral.sh/uv/reference/settings/#dependency-metadata
> 
> This is because it's narrower (i.e. override edges vs nodes as discussed in [#4422 (comment)](https://github.com/astral-sh/uv/issues/4422#issuecomment-2249044193) ). It's another very nice uv trick that flies under the radar a little :-)

Would you have an example @hauntsaninja?

I was following [dependency-metadata documentation](https://docs.astral.sh/uv/reference/settings/#dependency-metadata). I added the following to my `pyproject.toml` assuming it will override dependencies for `<a-dependency-package>` (whereas `override-dependencies` overrides for all dependency packages as far as I understood).
```toml
[tool.uv]
dependency-metadata = [
    { name = "<a-dependency-package>", requires-dist = ["<b-package>>=2"]},
]
```
However, when installing through `uv sync --all-extras --all-groups --dev` I get an older `<b-package>` version installed, the one that is listed as dependency for the dependency package. I would expect to have `<b-package>` version 2 or more being installed. What did I miss?

Note that the dependency on `<a-dependency-package>` is defined as a development dependency (within a group https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-groups) instead of an optional dependency.


---

_Comment by @konstin on 2025-10-23 13:21_

A reference can be PDM (https://pdm-project.org/latest/usage/config/#exclude-specific-packages-and-their-dependencies-from-the-lock-file):

> Sometimes you don't even want to include certain packages in the 
locked file because you are sure they won't be used by any code. In this
 case, you can completely skip them and their dependencies during 
dependency resolution:
> ```toml
> [tool.pdm.resolution]
> excludes = ["requests"]
> ```
> 
> With this config, <code>requests</code> will not be locked in the lockfile, and its dependencies such as <code>urllib3</code> and <code>idna</code>
 will also not show up in the resolution result, if not depended on by 
other packages. The installer will not be able to pick them up either.

---

_Comment by @charliermarsh on 2025-10-31 14:18_

We just added support for excluding dependencies entirely by name: https://github.com/astral-sh/uv/pull/16528 (available in the next release)

---
