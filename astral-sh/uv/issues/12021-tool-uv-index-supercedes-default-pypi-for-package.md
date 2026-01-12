```yaml
number: 12021
title: "`tool.uv.index` supercedes default PyPI for package resolution"
type: issue
state: closed
author: daviles-gcs
labels:
  - bug
assignees: []
created_at: 2025-03-06T21:18:40Z
updated_at: 2025-03-14T12:29:53Z
url: https://github.com/astral-sh/uv/issues/12021
synced_at: 2026-01-12T16:00:52Z
```

# `tool.uv.index` supercedes default PyPI for package resolution

---

_@daviles-gcs_

### Summary

### Goal:
Build package and publish to PyPi and Test PyPI

### Assumption (based on Docs):
Adding an index to test-pypi will allow me to use `uv publish --index testpypi`
```py
[[tool.uv.index]]
name = "testpypi"
url = "https://test.pypi.org/simple"
publish-url = "https://test.pypi.org/legacy/"
default = false
```
`uv publish`  with no `--index` will publish to PyPI


### Problem:
Running other commands specific to package resolution after adding an index to `pyproject.toml` result in errors

### Example:
Using `uv add pandas` results in:

![Image](https://github.com/user-attachments/assets/71022829-4572-46a8-9574-9def432f9115)


Changing the index to:
```py
[[tool.uv.index]]
name = "testpypi"
url = "https://example.pypi.org/simple"
publish-url = "https://test.pypi.org/legacy/"
default = false
```

yields

![Image](https://github.com/user-attachments/assets/266e1833-822f-4b56-8700-7742ec4b4966)

This is confirmation that PyPI is no longer the default index to search for packages. 

`uv`, ideally, should default to PyPI for package resolution unless specified otherwise by an `--index` flag which would require a `tool.uv.index` in the `pyproject.toml` file

### Platform

Windows 11

### Version

0.6.4

### Python version

3.12.7

---

_Label `bug` added by @daviles-gcs on 2025-03-06 21:18_

---

_Comment by @fellnerse on 2025-03-13 08:42_

I have the same issue! The docs are not reflecting the behavior of uv:
https://docs.astral.sh/uv/configuration/indexes/#defining-an-index

> Indexes are prioritized in the order in which they’re defined, such that the first index listed in the configuration file is the first index consulted when resolving dependencies, with indexes provided via the command line taking precedence over those in the configuration file.
>
> By default, uv includes the Python Package Index (PyPI) as the "default" index, i.e., the index used when a package is not found on any other index. To exclude PyPI from the list of indexes, set default = true on another index entry (or use the --default-index command-line option):
>
```
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
default = true
```
>The default index is always treated as lowest priority, regardless of its position in the list of indexes.

---

_Comment by @charliermarsh on 2025-03-14 00:51_

Ah, so I think this is working as intended. We query indexes in order, so in the above example, we query the `https://example.pypi.org/simple ` first. When it fails, we throw an error. If it succeeded but returned a 404 (i.e., if the package didn't exist on the index), then we'd move on to the next index (in that case, PyPI, the default).

The reason you're seeing that failure with Test PyPI is that Pandas _does_ exist on Test PyPI, so we stop looking. But it doesn't contain any _distributions_ for Pandas. That's sort of a very-specific-weird-case where the package exists but doesn't have any versions.

---

_Comment by @daviles-gcs on 2025-03-14 00:59_

> Ah, so I think this is working as intended. We query indexes in order, so in the above example, we query the `https://example.pypi.org/simple ` first. When it fails, we throw an error. If it succeeded but returned a 404 (i.e., if the package didn't exist on the index), then we'd move on to the next index (in that case, PyPI, the default).
> 
> The reason you're seeing that failure with Test PyPI is that Pandas _does_ exist on Test PyPI, so we stop looking. But it doesn't contain any _distributions_ for Pandas. That's sort of a very-specific-weird-case where the package exists but doesn't have any versions.

Understood. I would question if the uv tool should always default to pypi so long as the `--index` flag is not used. 

If the `--index` flag is used, it should use `tool.uv.index` according to its name. 

Defaulting package resolution to your custom index just seem counter intuitive. 

Almost always will package resolution come from PyPI, at least in my experience. There are a few scenarios that I want to pull a distribution from  Test PyPI but makes more sense to use an `--index` flag that would distinguish the separation between PyPI as the source as well as any other _**sources**_

---

_Comment by @charliermarsh on 2025-03-14 01:04_

> Defaulting package resolution to your custom index just seem counter intuitive.

I think this is very normal, unless I'm misunderstanding. If you have a custom index, and a package exists on both that index and PyPI, you almost certainly want to get it from your custom index. If it doesn't exist on your custom index, then you should get it from PyPI (which we do). I believe this all working as intended though.


---

_Comment by @daviles-gcs on 2025-03-14 01:24_

> > Defaulting package resolution to your custom index just seem counter intuitive.
> 
> I think this is very normal, unless I'm misunderstanding. If you have a custom index, and a package exists on both that index and PyPI, you almost certainly want to get it from your custom index. If it doesn't exist on your custom index, then you should get it from PyPI (which we do). I believe this all working as intended though.
> 

I appreciate the feedback!

My understanding is PyPI serves as the defacto standard for production ready distributions.

I will use my particular use case as an example. 

I have a package I want to publish to both pypi for production and test-pypi for development purposes. I add tool.uv.index to be able to publish using an index name as opposed to having to use the full url in the CLI each time. 

Publishing works well. 

As time progresses and I realize I need a library to add, let's say Semver, when I run `uv add semver` and semver also happens to have a package on test-pypi but, perhaps outdated and with bugs. `uv` adds the packag,  and unbeknownst to me, it was added from the latest published version in test-pypi because that is the highest priority. 

This will continue to happen so long as the package you want to add also has an existing package in test-pypi. 

Which I believe could lead to unforseen issues using code not meant for production use. 

A quick excerpt from Python packaging details
>TestPyPI is a separate instance of the Python Package Index (PyPI) that allows you to try out the distribution tools and process without worrying about affecting the real index. TestPyPI is hosted at test.pypi.org

This would confirm that test-pypi is a testing ground and I would not want it to take precedence over PyPI. 

This is one particular scenario, but any amount of custom indexes would fall under this issue. 

This description, I believe, adds validity to what the default package resolution should be. Any other sources you want to use should be distinctively annotated through CLI flags, `--index` in particular. 

Thank you in advance for your time and consideration.




---

_Comment by @fellnerse on 2025-03-14 07:51_

@charliermarsh thanks for your quick answers, that clears up a bit on how the indexes are working. I didn't understood that from the documentation.
So let me repeat:
When I want to use pypi as `default` index in such a way, that it is **always** queried first, I would have to add it manually as index as well, like this:
```toml
[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple/"
default = true
```
Then in any case a package is not found, uv will try to use the next index in line to query a package.

My understanding from the docs was that this is the **default** behavior, but I guess I got confused from that sentence:
> The default index is always treated as lowest priority, regardless of its position in the list of indexes.

I thought that was meant in regards to lowest index priority..

As our `secondary` index automatically mirrors automatically dependencies from pypi, this lead to undesired behavior. But I suppose, this is again one of those `edge cases`. Maybe a bit more info in the docs would have helped in my case.

In any case, awesome tool, I love using UV! ❤️

@daviles-gcs I believe you can solve this issue by adding `explicit = True` to your test index. Then only the packages that you **really** want to add from the test repo are pulled from there. I think in our case it's the same, using the `explicit` keyword solves it, and uv uses the `default` pypi index first.

I think I found a open MR yesterday, adding this `explicit` keyword to some examples with the pypi test index even..
Edit:
found it: https://github.com/astral-sh/uv/pull/12148

---

_Comment by @daviles-gcs on 2025-03-14 12:19_

@fellnerse @charliermarsh 

Thank you both for your time and collaboration on this topic. 

I confirm `explicit = true` solves my concern. This flag makes the tool much more flexible which I prefer over my original solution. 

Having this flag allows us to add any index and we can include or exclude it from the package resolution hierarchy. Incredible!

`uv` has been a gamechanger :heart:

Thank you again!!

---

_Closed by @daviles-gcs on 2025-03-14 12:19_

---

_Comment by @charliermarsh on 2025-03-14 12:29_

Thanks for following up!

---
