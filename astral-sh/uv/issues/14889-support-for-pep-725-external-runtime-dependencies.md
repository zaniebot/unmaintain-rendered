```yaml
number: 14889
title: Support for PEP 725 - external (runtime) dependencies with user defineable helper
type: issue
state: open
author: CarliJoy
labels:
  - enhancement
  - wish
  - needs-design
assignees: []
created_at: 2025-07-25T10:49:22Z
updated_at: 2025-09-22T16:18:37Z
url: https://github.com/astral-sh/uv/issues/14889
synced_at: 2026-01-12T16:01:58Z
```

# Support for PEP 725 - external (runtime) dependencies with user defineable helper

---

_@CarliJoy_

### Summary

[PEP 725](https://peps.python.org/pep-0725/) drafts an easy way to define external dependencies.

Even though it is still a draft, the metadata field for external runtime dependencies is [already defined](https://packaging.python.org/en/latest/specifications/core-metadata/#requires-external-multiple-use).

Still, the problem of ensuring an external program is installed is very hard.
It is very OS-dependent and may require superuser permission for installations.

So I don't see that this is within the scope of `uv` to handle this kind of problem.

But I would like `uv` to call a user-definable external helper that takes care of this task.

### Example

My suggestion is the following:

When installing packages, `uv` should do the following:

- Collect all external dependencies [^1]. If none are found, stop here.
- Check if an external dependency manager is defined.
  - If not, warn the user that the collected external dependencies might be missing.
  - If yes, simply call the external dependency manager with all collected external dependencies. If the manager fails, fail the installation. Do not hide the output of the manager from the user.

It is important that the manager is called exactly once with all dependencies in order to resolve or stop on potential conflicts.

The external dependency manager should:
- Check if all external dependencies are met and exit if yes.
- If not, try to install them; if it fails, exit as failure with a meaningful message.
- Given PEP 725 succeeds, the dependency manager could also delegate its task to sub-managers based on `scheme:type/namespace`.

This proposal:

- allows users to handle their external dependencies (i.e. with a system package manager or [conda](https://github.com/astral-sh/uv/issues/1703)) as they desire
- uses existing standards and at the same time prepares for a new one [^2]
- is future-safe: once there is a good general external dependency manager, `uv` could just define it as default

**Personal Background:**
In my organisation, we are planning to migrate from `conda` to wheels and `uv`. There are only two non-wheel-compatible dependencies that still should be handled by `conda`.
So my idea was to use the `Requires-External` field to define them with a custom PURL and create a post-installation helper that reads the metadata and installs the required dependencies.
My co-workers would prefer one command to run, so I thought about wrapping `uv` but then thought: Well, that is actually something that would be much easier and more performant if implemented in `uv`.

[^1]: Currently only runtime dependencies. In the future, maybe external build dependencies, once they are defined properly. So currently, this would mean only extracting all `Requires-External` fields from the metadata of all packages.
[^2]: As handling of external dependencies will be implemented only for `Requires-External` and delegated to an external helper, `uv` is fully within the specs. The only thing to consider is that `uv` should resolve [dependency specifiers](https://peps.python.org/pep-0725/#dependency-specifiers) within `Requires-External`, even though it is not yet part of the spec.

---

_Label `enhancement` added by @CarliJoy on 2025-07-25 10:49_

---

_Renamed from "Support for PEP 725 - external (runtime) dependencies with user definable helper" to "Support for PEP 725 - external (runtime) dependencies with user defineable helper" by @CarliJoy on 2025-07-25 11:31_

---

_Label `wish` added by @zanieb on 2025-07-25 12:58_

---

_Comment by @zanieb on 2025-07-25 12:59_

I'm pretty interested in external dependency support, but we'll need to do some significant design before we can pursue adding something like this.

---

_Label `needs-design` added by @zanieb on 2025-07-25 12:59_

---

_Comment by @CarliJoy on 2025-07-28 14:18_

Could you explain a bit more what design means in detail?

Can externals help with this or is the a process you do internally?

---

_Comment by @jaimergp on 2025-07-30 16:28_

Hello folks, just a note on behalf of the PEP 725 authors!

We are about to submit a revised version of the PEP at some point within the next two weeks. You can see [the current status in this fork](https://github.com/rgommers/peps/blob/main/peps/pep-0725.rst). It contains a substantial amount of changes wrt the version available at https://peps.python.org/pep-0725/. Some highlights:

- The introduction of `dep:` as a `pkg:` derivative to improve ergonomics
- `Requires-External` being deprecated in favor of `Requires-External-Dep`.
- An accompanying PEP with the specifics of cross-ecosystem mappings for external dependencies ([draft in this branch](https://github.com/rgommers/peps/blob/pep-name-mapping/peps/pep-9999.rst), no number assigned yet). Currently undergoing a small revision process to get it up-to-date with the latest developments.

A few supporting libraries and projects have been added to demo the usefulness of the PEP, which you can also check in the following repositories:

- [pyproject-external](https://github.com/jaimergp/pyproject-external): Proof of concept CLI (plus a Python API) to interact with [PEP 725](https://peps.python.org/pep-0725/) [external] metadata and the associated mappings.
- [external-deps-build](https://github.com/rgommers/external-deps-build): CI workflows to build popular PyPI packages patched with the necessary [external] metadata.
- [external-metadata-mappings](https://github.com/jaimergp/external-metadata-mappings): Schemas, registries and mappings to support [external] metadata for different ecosystems and package managers.

Let us know if there's anything else you'd like to know about it and how we can help/collaborate!

---

_Comment by @jaimergp on 2025-09-22 16:18_

As promised (although a bit later than expected), we have now published:

- [An update to PEP 725](https://discuss.python.org/t/pep-725-specifying-external-dependencies-in-pyproject-toml-round-2/103890/2)
- A new, complementary [PEP 804: An external dependency registry and name mapping mechanism](https://discuss.python.org/t/pep-804-an-external-dependency-registry-and-name-mapping-mechanism/103891)

---
