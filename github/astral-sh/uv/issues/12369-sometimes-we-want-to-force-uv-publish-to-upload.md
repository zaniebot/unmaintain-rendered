---
number: 12369
title: "sometimes we want to force `uv publish` to upload anyway"
type: issue
state: open
author: marc-planard
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-03-21T16:42:07Z
updated_at: 2025-10-07T09:14:52Z
url: https://github.com/astral-sh/uv/issues/12369
synced_at: 2026-01-07T13:12:18-06:00
---

# sometimes we want to force `uv publish` to upload anyway

---

_Issue opened by @marc-planard on 2025-03-21 16:42_

### Summary

We use a private Nexus Pipy repository and we regularly need/want to update a package on the server without bumping its version number.

`uv publish` currently prevent us from doing that (certainly for very good reasons) , nevertheless, we would like to be able to override this safeguard.

I've added a --force option to `uv publish` which fix this for us in this PR: https://github.com/astral-sh/uv/pull/12370 

I've found a couple of issues tangentially related to this topic (like https://github.com/astral-sh/uv/issues/7917 ) but I haven't found a way to achieve the behavior we need with the current set of available options (maybe I've missed something though).

I realize that this proposal may be rejected as a whole because you really really don't want to allow that period, otherwise I'd be happy to be mentored a little bit to carry the PR home as it's my first time contributing...)



### Example

Old behavior:

```bash
$ uv publish --index my-nexus
[...]
error: Local file and index file do not match for $FILE_NAME. Local: sha256=xxx, Remote: sha256=yyy
```

New behavior:
```bash
$ uv publish --index my-nexus
[...]
error: Local file and index file do not match for $FILE_NAME. If you know what you\'re doing to can still upload this file using the option `--force`. Local: sha256=xxx, Remote: sha256=yyy

$ uv publish --force --index my-nexus
[...]
Uploading $FILE_NAME ($SIZEKiB)
```

---

_Label `enhancement` added by @marc-planard on 2025-03-21 16:42_

---

_Referenced in [astral-sh/uv#12370](../../astral-sh/uv/pulls/12370.md) on 2025-03-21 16:49_

---

_Comment by @zanieb on 2025-03-21 16:50_

I'm fine with a `--force `option, if the registry prefers immutable releases then it can reject it. Note this might be problematic with caching and such? I'm not sure how clients will behave if you break the condition that the registry is immutable.

cc @konstin 

---

_Comment by @marc-planard on 2025-03-21 16:58_

Oh this can definitely lead to caching issues, we're using it in a CI context where we know we start from a clean cache each time, but it's really a "I hope you know what's your doing, user" situation, for sure :) 

---

_Comment by @charliermarsh on 2025-03-21 22:33_

I believe it was intentional that we did _not_ support this so we should make sure that @konstin is heard before committing any changes.

---

_Comment by @konstin on 2025-03-21 22:43_

> We use a private Nexus Pipy repository and we regularly need/want to update a package on the server without bumping its version number.

Do you mean you replace an already published file with a different file with the same name?

---

_Comment by @marc-planard on 2025-03-21 23:20_

> Do you mean you replace an already published file with a different file with the same name?

Yes, exactly.

---

_Comment by @konstin on 2025-03-21 23:24_

I don't want to support that, uv assumes that files don't change once their published. If they change, it breaks hash-checking and installers that depend on the old file.

---

_Comment by @marc-planard on 2025-03-24 09:38_

I’d like to explain (as briefly as possible but it seems I've already failed) why I believe this should be allowed and share our use case. That said, I don’t claim any kind of expertise in uv or Python package management in general, and I respect your broader understanding of these decisions.

As @zanieb pointed out, if the registry permits package overwrites, then they will inevitably happen—whether they should or not. If uv prevents re-uploading a file, we would simply resort to using curl and the Nexus API instead of uv publish. While that workaround would be less safe and more cumbersome, it would still achieve the intended outcome.

By providing an explicit escape hatch, however, you would have the opportunity to clearly communicate to users why this is discouraged and outline the potential consequences of proceeding down this path. Also I went for the simplest implementation with my PR with a simple flag but there's maybe something more appropriate UX wise. Heck you can rename the flag to "--unleash-the-kraken" if you prefer :)

### Regarding our use case:

We have two nexus pipy registries: let say `our-nexus-dev` and `our-nexus-releases`. The `-releases` is handled as you would expect and we shouldn't overwrite any package there. 
 The -dev on the other hand is used exclusively by the development team, where some packages are published several times per day as part of our CI pipeline. We have (too many) libraries in their own repositories which are used by even more micro-services (which are "packaged" as docker images). All of that is for internal consumption, we don't have external users so any caching inconsistency is kept manageable.
As we update these libraries sometimes several times per day, we don't want to (have to) bump the version number each time with push a change (dealing with a library inversion 1.2.6723 is no fun). We don't want to clutter our registry with 10 * 20 * 12 versions per year (99% of which are obsolete and takes space on the server for nothing). We don't want to tag a version with a random hash (like its git commit hash) because we don't want to keep the old ones and we want to be sure which one is the last one.
And of course once in a while, we tag our libraries with a new version number and we push that on our -releases repository.

(note that we are migrating from poetry to uv. Poetry let us overwrite a package without even a warning. I consider the fact that uv is checking and prevent overwriting a package an improvement and the right default. But I still wish it would let me override the security)

---

_Comment by @coolbeevip on 2025-03-28 13:52_

@marc-planard 
In the same scenario, I ultimately chose to use twine to publish the wheel to Nexus.

---

_Comment by @zanieb on 2025-03-28 15:14_

I am fine with amending it to `--unsafe-force-upload` or something. I think the use-case is weird, but I think it's also bad for us to force you to use another tool.

---

_Label `needs-decision` added by @zanieb on 2025-03-28 15:14_

---

_Comment by @konstin on 2025-04-08 14:14_

You can deactivate the check by not telling uv about the index URL, i.e., `uv publish --publish-url https://... dist/*`. But note that this will break any lock files with hash checking (`requirements.txt` with hashes, `uv.lock`, PEP 751).

I agree that testing and using packages from continuous integration currently does not have a good, recommendable workflows. There's e.g. build tags in the wheel filename we could use, but I don't think there's build backend support for that, similarly we're lacking something like a well-integrated concept of a package override that uses let's say an S3 bucket that the CI uploads too, without any expectation of consistent packages and consistent package metadata.

---

_Comment by @marc-planard on 2025-04-08 15:34_

@konstin good to know an escape hatch already exists. 

not trying to argue but just a couple of remarks:

- the `--publish-url` escape-hatch in its current form is not easily discoverable as I was unable to find it but so was @charliermarsh 
- it is also slightly less convenient from a UX / developer-experience? point of view: I like that the pyproject.yaml is the unique source of truth for the repositories urls when using indexes, with publish-url the script running the uv publish must repeat the destination url which can lead to discrepancies down the road if the urls must be changed and someone (me 🥹) forgets to update both files.
- in any case, either if the final decision is to add an option to force-publish or not, I highly suggest to add documentation in a more discoverable way to cover this situation (for example, extending the error message of `CheckUrlIndex` to include something on the line of "if you really really want to do that, use --publish-url instead of --index" or something.

---

_Comment by @lagamura on 2025-10-07 09:14_

@konstin 
> You can deactivate the check by not telling uv about the index URL, i.e., uv publish --publish-url https://... dist/*. But note that this will break any lock files with hash checking (requirements.txt with hashes, uv.lock, PEP 751).

I found out that `--publish-url` has this behavior accidentally, as I was triggering the hash mismatch and breaking the uv.lock file.
Isn't that ambiguous when `--publish-url` diverge from `--index` behavior?
Also, couldn't find that documented in the docs.

---
