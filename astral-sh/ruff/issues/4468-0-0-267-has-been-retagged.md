```yaml
number: 4468
title: 0.0.267 has been retagged
type: issue
state: closed
author: dvzrv
labels:
  - question
  - release
assignees: []
created_at: 2023-05-17T09:16:00Z
updated_at: 2023-05-17T15:27:02Z
url: https://github.com/astral-sh/ruff/issues/4468
synced_at: 2026-01-12T15:54:44Z
```

# 0.0.267 has been retagged

---

_@dvzrv_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi! I'm one of the package maintainers for ruff on Arch Linux (cc @inglor, @alerque).
We noticed (see downstream ticket: https://bugs.archlinux.org/task/78534), that [0.0.267](https://github.com/charliermarsh/ruff/releases/tag/v0.0.267) has been retagged (i.e. the tag has been deleted and added again, on a different commit).

I would like to point out, that this is problematic (for us and in fact all consumers of this project):

* It may mean malicious activitiy on a project: An attacker may have acquired credentials of one of the developers and changed a release, adding potentially harmful functionality ([supply chain attacks](https://en.wikipedia.org/wiki/Supply_chain_attack) are a thing). This scenario requires all downstreams to manually verify what is going on upstream and write a ticket (e.g. this one).
  As this project does not rely on signed tags, that can be traced back to a separately verifiable entity (btw, the github signing service is pointless to use for tags, as all developers have access to it) and the newly added tag is not signed either, this raises suspicion.
* It breaks all downstream builds relying on the initial tarball, as it will no longer be available. This by proxy breaks [reproducible builds](https://reproducible-builds.org/), which Arch Linux works on: https://reproducible.archlinux.org/.
  In effect this also breaks any derivate distribution of Arch Linux (or other distribution), that relies on their parent distribution's package sources (in our case e.g. Arch Linux 32, Arch Linux ARM, etc.)

TL;DR: Please never retag, just create a new tag.

---

_Comment by @alerque on 2023-05-17 10:15_

I have the original tar file used to build the Arch package. There are several differences but they all look safe enough to me. They added an svg file to the repo, used it to add branding to the readme, re-indented some YAML, and changed the release workflow from a release dispatch trigger to a tag push trigger. Definitely not a good reason for re-tagging, but not malicious.

As David said, please don't retag after a release goes out. If something goes wrong in the release cycle just increment the tag and try again. That is much less workload on downstream packagers to deal with.

No time is "short enough" either, many of us get instant release notifications and can have a tarball downloaded for packaging in seconds from the tag being posted. If it later gets retagged we may not even be aware of the change until things start going wrong further downstream. Worst case scenario if something is actively harmful in a release yank the tag, but don't repost the same one, increment it again in the next release and explain in release notes what happened to the missing release.


---

_Comment by @charliermarsh on 2023-05-17 13:53_

Sorry about this -- entirely my fault, and I appreciate the clear explanation of why it's problematic. I hadn't really matured from thinking about release-publishing as "a thing that triggers an upload to PyPI" to "a thing that creates an immutable tag that's depended-on by many downstream clients", which is an important conceptual transition for me to make.

Is there anything I can do to be helpful with this specific case of re-tagging? Walk through the changelog? (IIRC, the issue was that we introduced a new sanity-check within the release job between 0.0.266 and 0.0.267, but that sanity-check didn't _actually_ get tested on all platforms until we attempted to do the release, so I then had to revert them on some platforms to get the release passing, and to keep building off `main`, that meant to pulling in an unrelated PR or two when I went to force-update the tag.)

---

Separate from this specific case: I think our current release workflow is not ideal.

In short: we create the GitHub Release, which creates the tag, which triggers the creation of the wheels, which are then attached to the GitHub Release and uploaded to PyPI. However, the wheel creation step can fail, and in a way that could affect downstream clients too (e.g., our release build could be entirely broken), in which case, we're now stuck with a "bad" tag.

The way I tend to "solve" this is by running the Release workflow prior to creating the GitHub Release (which builds the wheels, but doesn't upload them). This is fine, but (1) it's slow and wasteful, because we end up re-building all the wheels; and (2) it could, in theory, still fail, since we might fail to upload to PyPI in a way that then requires code changes to the job which then requires re-releasing (may not affect Arch, but just an example).

The other "solution" is what I did here: push a fix, then re-tag the release (which, as pointed out, is not good).

You two are of course not responsible for figuring this out, but I thought I'd ask while you're here what you view as the "correct" release workflow. E.g., perhaps we should build the wheels from a specific commit, then create the GitHub Release and tag it, which would attach the wheels to the release and upload them to PyPI?

(Relatedly, does Arch build all tags, or only those that are used to publish GitHub Releases?)


---

_Label `question` added by @charliermarsh on 2023-05-17 13:53_

---

_Label `release` added by @charliermarsh on 2023-05-17 13:53_

---

_Comment by @alerque on 2023-05-17 14:07_

We already resolved this specific instance by manually diffing tarballs and re-publishing a new build (incrementing our release number of course).

If rebuilding things twice is an issue, I would suggest a different approach to the GH workflows. Either way I think you already need to build everything on every commit anyway and test it. There shouldn't be extra testing happening on a release than on the previous commit. But to avoid two rebuilds on releases, rather than trying to have a different workflow for releases, just add a step to the build workflow that checks whether the commit is tagged and if so, uploads the artifacts to the tag. Here is an example of a workflow that does just that: https://github.com/sile-typesetter/sile/blob/master/.github/workflows/build.yml#L59. The `make dist` generates the source tarballs and the later Release step (not a workflow) makes it into a release. Also note the previous artifact upload step does just the opposite, it uploads the build artifacts to the CI artifacts system if it is *not* a tagged release.

Theoretically we build all tags, but the GH Releases system sometimes brings them to our attention sooner.

---

_Comment by @charliermarsh on 2023-05-17 15:24_

Okay, thank you for the pointers. I'll take this into account as we rethink our release process.

---

_Closed by @charliermarsh on 2023-05-17 15:24_

---
