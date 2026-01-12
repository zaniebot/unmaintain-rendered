```yaml
number: 20016
title: "[ty] Add option for ty to find modules in paths defined in PYTHONPATH"
type: pull_request
state: closed
author: natelust
labels:
  - ty
assignees: []
base: main
head: main
created_at: 2025-08-21T01:47:33Z
updated_at: 2025-09-20T11:46:01Z
url: https://github.com/astral-sh/ruff/pull/20016
synced_at: 2026-01-12T15:56:52Z
```

# [ty] Add option for ty to find modules in paths defined in PYTHONPATH

---

_@natelust_

Fixes https://github.com/astral-sh/ty/issues/547. Add the option for looking up modules from paths in the PYTHONPATH environment variable.

## Summary

I work on the [rubin observatory data management team](https://www.lsst.org/about/dm). Due to a long legacy of python use and our nature of development and deployment of code we have a setup where much of our code is in different packages which are dynamically setup and loaded through use of the PYTHONPATH environment variable. Though many projects make use of conda envs or venvs, we are likely far from the only user of this python feature. However, at the moment this makes ty (and several other type checkers) unable to check a large majority of our cross package code.

I have implemented a patch that adds a configuration option which when set will cause ty to read the paths defined in PYTHONPATH. These paths get added to the internal site-packages Vec, as that is what they logically are from the perspective of python. The only accompanying change is when looking up modules on all the search paths, I deferred failure until after all paths were checked. This is to account for a situation like:
```
path1 : /some/path/top_module/path1_module.py
path2:/some/path/top_module/path2_module.py
```
So instead of the checker finding the stub `top_module` in path1 when looking up `path2_module.py` and failing to find the module, it will wait until all search paths are checked.

I am also opening an accompanying pr on the ty repo to update the documentation on the new configuration option.

## Test Plan

I built ty with these changes and tested it operationally, and it is really nice how fast it is able to check everything. If more extensive tests are needed at this point I may need a small pointer on where best this test should go.


---

_Review requested from @carljm by @natelust on 2025-08-21 01:47_

---

_Review requested from @AlexWaygood by @natelust on 2025-08-21 01:47_

---

_Review requested from @sharkdp by @natelust on 2025-08-21 01:47_

---

_Review requested from @dcreager by @natelust on 2025-08-21 01:47_

---

_Review requested from @MichaReiser by @natelust on 2025-08-21 01:47_

---

_Comment by @MichaReiser on 2025-08-21 06:16_

Thanks for your work on this and also creating a documentation PR. 

I'm not yet convinced that we want to read PYTHONPATH but I'm curious to hear what other thinks. But I also must admit that I don't fully understand the use case. 

I'd prefer if we could take a step back and start with an issue where we discuss the problem, what other type checkers do, and then decide what we want for ty. 

E.g. an alternative is that ty supports `TY_EXTRA_PATHS` to configure extra paths, in which case you could set `TY_EXTRA_PATHS` to `PYTHONPATH`, which would aovid adding a new configuration option. We could also decide to always read `PYTHONPATH` if set, etc...

---

_Comment by @AlexWaygood on 2025-08-21 07:46_

> I'd prefer if we could take a step back and start with an issue where we discuss the problem, what other type checkers do, and then decide what we want for ty.

This feature was previously requested at https://github.com/astral-sh/ty/issues/547, and the person who filed that issue was kind enough to do a writeup of what all other existing major type checkers do here 

---

_Label `ty` added by @AlexWaygood on 2025-08-21 08:08_

---

_Comment by @AlexWaygood on 2025-08-21 11:15_

I posted some questions in the issue: https://github.com/astral-sh/ty/issues/547#issuecomment-3210097768

---

_Renamed from "[ty] Add option for TY to find modules in paths defined in PYTHONPATH" to "[ty] Add option for ty to find modules in paths defined in PYTHONPATH" by @AlexWaygood on 2025-08-21 11:15_

---

_Comment by @carljm on 2025-08-21 14:23_

Thanks for your work on this! A couple quick notes specifically regarding this implementation, based only on the PR description (haven't had a chance to dig into the code changes):

1. I don't think PYTHONPATH paths should be added to site-packages paths. They aren't the same: they have a different search priority (at runtime PYTHONPATH prepends to `sys.path` so takes top priority, even over stdlib; this is more like our extra-paths) and pth files are not respected in PYTHONPATH paths.
2. The PR description seems to describe some namespace-package like behavior being unconditionally applied in this PR (continuing to search other paths for parts of a module.) That behavior doesn't sound right; we already handle namespace packages, and non namespace packages should not be allowed to span multiple search paths. If there are any issues here they should be handled in a separate issue report and PR, not lumped in with this feature. 

---

_Comment by @natelust on 2025-08-21 16:21_

That you for you attention on this. In retrospect, I should have written a bit more about our use case as a motivation, but I was rushing a bit to get this filled opened before going to bed and should have taken a bit more time.

I also apologize, there are comments here and over in the ty repo as well. I will try to answer each thread in the respective repo, but I imagine there will be some overlap.

Our software stack is divided into roughly 100 packages of mixed c++ and python code. When we make a stack version, each package has a snapshot taken, and it put into our packaging and distribution framework. On install. each package version is installed along side other versions of the package. To then make use of the stack, there is software which sets the appropriate LD_LIBRARY_PATH, PATH, PYTHONPATH, etc environment variables to make the version you selected active in your current shell session. We have various requirements that we be able to reproduce data processings with various old software version for scientific reproducibility reasons (and not all our users update at the same frequency). These software stacks can be used on a personal laptop, but are also deployed in shared environments in our high performance compute centers, with different users being on different version at the same time. PYTHONPATH is perfect for managing this dynamic per shell setup, exactly like dynamic linkers for our c++ code.

The development workflow for building features or fixing bugs is then to clone the packages which need changes, and set them up. This then replaces the standard version installs of the packages with the git versions by changing the respective environment variables such that we can make use of the already compiled shared packages at a certain version for most packages and only need to have git versions for what we are currently developing. This also extends to scenarios where you are working on a ticket branch in packages A and B, sitting on top of others branches in C and D, and using the versioned stack install for the rest. 

All this work eventually comes back in line when we merge to the upstream package, but during development it is nice for the type checker to be able to see the signatures of the other packages that are also in development, but are not in the tree of the current package. There are of course workarounds by writing out files to manage these paths, but this means that the files will be frequently modified to reflect the modules that python will actually be reading when it executes, and it raises the likelihood that a file might not get updated when it should be and wrong signatures are used.

As for the other path change, the motivation is similar, though I think this too will affect others in various ways. When we develop our packages, each one is put under a common python module scope i.e. `lsst` . This lets users (and our software developers) to group all our code such that the imports read `from lsst.packageA import TaskA` and `from lsst.packageB import TaskB`. Even though `TaskA` and `TaskB` are provided by different packages the import mechanisms resolve this just fine, and it provides a consistent user interface.

Under the current behavior of ty, as it searches through the list of search  paths (say packageA path first) it will encounter `lsst` in one path, but fail to find `packageB` and it will report that it found the stub `lsst` but that no `packageB` is contained in it. The only change I made is to defer this return until all paths are searched.

I outlined this using our `lsst` namespace, but could be generic to many others. If your PYTHONPATH contained paths all which ended in a directory named `python`, as that is where python code may live, the same sort of failure to find modules would occur.

I don't have a strong feeling that it must be attached to the site-package implementation, it just seemed like a convenient fit as it is paths that python itself discovers and reads. As you say though resolution order is important, and I see your point that python will read PYTHONPATH before it reads site-packages, and so getting the same resolution behavior in the type checker is probably important as well.

I really apologize about the wall of text. I tried to be as informative as I could be to motivate my thinking on this PR, but not overwhelm with unimportant details. I hope I struck the right balance. I'm happy discuss more or make any changes you might see fit.

---

_Comment by @carljm on 2025-08-21 17:58_

The scheme you are describing with `lsst` is called a "namespace package", and it works as long as there is no `lsst/__init__.py` file. (If we do find a `lsst/__init__.py`, the correct behavior -- and the runtime import resolution behavior -- is to stop there and not continue looking for any later search path with `lsst/` in it.)

But we definitely already support this behavior of namespace packages, so I'm still not sure why any change was needed in this PR. Probably this requires more investigation. If a problem can be reproduced outside this PR (e.g. with our existing `extra-paths` feature), it would be useful to investigate that separately.

(Edited to add: here is a [playground example](https://play.ty.dev/7a0ed4a5-63bc-4df8-8d3a-760485bc6693) showing that we already support this "namespace package" layout in extra-paths.)

---

_Comment by @natelust on 2025-08-21 19:40_

@carljm yes, you are right that indeed that is akin to namespace packaging. I should have been a bit more explicit in mentioning we do in fact use `__init__`. Unfortunately, namespace packages did not exist until python 3.3, and we have been building things since python 2.4. Our `__init__.py ` files have been used to both link everything together through sys.path, and re-work what apis are exposed through imports on a per package basis.

I can completely respect not wanting to support this legacy style of cross package linking in your codebase, though the  overhead is a single if statement, every bit of code is more behavior. I would of course appreciate support for that behavior,  don't get me wrong. This discussion might help me in pushing for more modernization of the import system.

---

_Comment by @carljm on 2025-08-21 20:07_

@natelust my concern here isn't primarily code complexity, it's correctness; we shouldn't consider packages to be importable that aren't actually importable at runtime. As far as I'm aware, our current behavior is correct (matches runtime). What confuses me here is that you say you do have `__init__.py` files in these packages that you spread across multiple search paths, and yet you say it works at runtime. As far as I'm aware, that shouldn't work at runtime, unless you do some extra dynamic stuff somewhere (e.g. in the `__init__.py` files themselves, or in an import hook) to make it work. The normal behavior of Python's import system is that if you `from foo import whatever`, and Python finds a `foo/__init__.py` on your search path without a `whatever.py` submodule, it stops there and fails the import and doesn't look at any additional search paths. It's easy to confirm this with a quick test. So the question here is what you're doing that makes this work for you, and whether that's something we should be able to recognize.

---

_Comment by @natelust on 2025-08-21 20:22_

Ok, I understand where you are coming from now. You are correct that there is an additional hook in the `__init__.py` to make it work. Specifically this snippet:
```
import pkgutil
__path__ = pkgutil.extend_path(__path__, __name__)
```
As found in the [python docs](https://docs.python.org/3/library/pkgutil.html)

This was the common work around in init files to get this behavior pre-namespaces, though it is still used in various cases.

As to your question, is this something you should recognize. I kind of see a few options:
 * you just say ty does not support this python way of extending paths, which is reasonable
 * you get into the business of reading a `__init__` at least as far as seeing if it uses pkgutil, which may or may not be desirable. 
 * Do what I did in this code and say, well in case someone used pkgutil we will keep scanning all the paths the user provided. It will either resolve to something eventually, or not and end up with the current behavior prior to this PR.
 * Maybe something more cleaver that I have not thought about.

---

_Comment by @carljm on 2025-08-21 20:48_

I think we will not go for the third option; that opens up too many possibilities of giving incorrect results in (the vast majority of) cases which do not use `pkgutil.extend_path`. The second option (explicit support for this documented use of `extend_path`) is not out of the question, but I think won't be a priority in the short term, so for now we will not support this.

You're welcome to open a separate issue for `pkgutil.extend_path` support, but it should be considered and implemented separately from the `PYTHONPATH` change.

---

_Comment by @natelust on 2025-08-21 22:42_

Though that makes this PR less useful for my immediate use, it is quite reasonable. My next actions will be:
* pull the path extension logic out
* explore options for pkgutil use, and if I come up with anything reasonable put it in a new PR (unless that is something you would prefer to design internally)
* rework where the PYTHONPATH paths get added and order they get searched as requested in the thread above.

Is there anything else I should consider when re-working this?

---

_Comment by @carljm on 2025-08-21 23:05_

I think the spec for `pkgutil.extend_path` support is pretty clear (if we find that pattern in an `__init__.py`; then we are willing to look to other path entries for submodules). Not sure how complex the implementation will be. No problem if you want to work on a PR.

It seems like @AlexWaygood and I are agreed that we should do this `PYTHONPATH` feature, and make it opt-out, so unless @MichaReiser has concerns, I think you can go ahead with that. It would be useful to review other type checkers to see if/how they allow opt-out, and how they name that option. I think the paths should probably go in the same place that paths from the existing `--extra-paths` option go.

---

_Comment by @natelust on 2025-08-21 23:14_

Thanks for the feedback I will work on that then, it should be a quick switch.

As far as the extend_path functionality its clear to only run that additional check in the one mach arm it will pertain to, which should limit any io overhead. I can think of a few options off the top of my head for detection
* include a regex crate and search, which should be fast but include another crate, I'm not sure what your policy is for that
* init files should be reasonably small, so reading to a byte vec and doing direct comparison should be quick and small memory footprint.
* leverage some of the parsers from ruff to understand the init file and look for this pattern (or sub pattern). I'm currently reading the source to determine what this all would entail. 

If you have opinions I am happy to follow what you think here. Otherwise I will go mock up some of those and see what seems to be the best fit.


---

_Comment by @carljm on 2025-08-21 23:47_

It should be the third option, and use the existing `parsed_module` salsa query to parse to AST so that we get Salsa caching on that. 

---

_Comment by @natelust on 2025-08-22 00:00_

👍 thanks for the input again

---

_Comment by @MichaReiser on 2025-08-22 06:08_

The main challenge that I see with supporting `pkutil.extend_path` is that we no longer know the search paths upfront. We would need to traverse the entire project upfront (including third party files?) just to find the search paths. This seems horrible for performance and isn't something I want to implement unless there's significant demand for it. 

https://github.com/astral-sh/ty/issues/838 outlines an alternative: Allow users to manually specify some packages that should be treated as namespace packages even if there's an `__init__.py` file. This approach is simpler to implement and doesn't require an entire project scan

---

_Comment by @carljm on 2025-08-22 06:17_

Does airflow use `pkgutil.extend_path`? That would explain my confusion in https://github.com/astral-sh/ty/issues/838 -- I never did understand how that setup actually works for them at runtime.

> The main challenge that I see with supporting `pkutil.extend_path` is that we no longer know the search paths upfront. We would need to traverse the entire project upfront (including third party files?) just to find the search paths.

I don't think this is true. `pkgutil.extend_path` (at least when used as documented and shown above, which is the only form I think we should consider supporting) doesn't add any new search paths, or change the search paths. All it does is say "treat this package like a namespace package, as if this `__init__.py` weren't here -- keep looking for missing submodules in later search paths." This is something we can lazily look for in the specific case when we have a missing submodule of a package with an `__init__.py`.

---

_Comment by @MichaReiser on 2025-08-22 06:33_

Yes, airflow uses `__import__("pkgutil").extend_path(__path__, __name__)` (the legacy namespace packages)

This sounds worth exploring if we can support it lazily. But only if it comes at a minimal overhead (this is legacy after all). I suggest that we first do a simple text search for `extend_path`) before calling the parser to avoid unnecessary (and potentially expensive parse calls). I'd otherwise prefer to wait for more user feedback before implementing this or explore a settings approach

---

_Comment by @carljm on 2025-08-22 06:40_

I'm pretty sure if we are looking for `foo.sub` module and a `foo/__init__.py` exists, it's 100% guaranteed that we've already parsed the `__init__.py`, because a `sub = ...` there takes precedence over the sub module. So I don't think we should add complexity prematurely due to worries about parsing cost, unless there's evidence of a problem. 

I don't think supporting this is high priority, but if we do support it I think a solution that "just works" when it works at runtime is much, much better for users than a solution that requires users to manually repeat their codebase structure in config (and to first even understand that's what they need to do, which is likely the biggest hurdle.) So I don't think we should even consider a settings-based approach except as a last resort if we can't make anything else work. 

---

_Review requested from @Gankra by @Gankra on 2025-08-22 14:17_

---

_Comment by @liferooter on 2025-09-05 11:06_

> I'm not yet convinced that we want to read PYTHONPATH but I'm curious to hear what other thinks. But I also must admit that I don't fully understand the use case.

I've found this PR trying to find out how to use `ty` with my Nix-based Python environments. They're not virtual environments and they rely on `PYTHONPATH`. They're currently don't work with `ty`, but it will be fixed if `ty` supports `PYTHONPATH`. That's my use case for `PYTHONPATH`.

---

_Comment by @liferooter on 2025-09-05 11:38_

I don't see any use cases for ignoring `PYTHONPATH` :)

---

_Comment by @carljm on 2025-09-05 17:16_

I think we are agreed on respecting `PYTHONPATH`, all we are lacking is a PR (or an update to this one) that makes it opt-out instead of opt-in, adds the paths to extra-search-paths instead of to site-packages-paths, and doesn't include the additional undesired change (that's still currently in this PR) that treats all packages as if they were namespace packages.

---

_Comment by @natelust on 2025-09-05 17:55_

Hey all, sorry I had an emergency come up recently and was away from things for a little while. I should have all the above points addressed fairly soon and update the pr. Again sorry for the delay.

---

_Comment by @mmlb on 2025-09-16 20:12_

I've done the requested modifications and opened up #20441, based off of this code/PR.

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-17 06:41_

---

_Comment by @MichaReiser on 2025-09-20 11:46_

Thank you for working on this. I'll close this PR because `PYTHONPATH` is now supported thanks to #20441. I changed https://github.com/astral-sh/ty/issues/838 to track adding support for legacy namespace packages. 

---

_Closed by @MichaReiser on 2025-09-20 11:46_

---
