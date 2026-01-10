---
number: 15625
title: Please support getting user consent for ruff download in uv format
type: issue
state: open
author: musicinmybrain
labels:
  - enhancement
assignees: []
created_at: 2025-09-02T08:05:31Z
updated_at: 2025-09-21T15:20:15Z
url: https://github.com/astral-sh/uv/issues/15625
synced_at: 2026-01-10T01:25:57Z
---

# Please support getting user consent for ruff download in uv format

---

_Issue opened by @musicinmybrain on 2025-09-02 08:05_

### Summary

In Fedora, we are generally uncomfortable with programs installing software from outside of Fedora without the user explicitly requesting it. (Programs that are *only* useful after downloading external code are actually [prohibited in Fedora](https://docs.fedoraproject.org/en-US/packaging-guidelines/what-can-be-packaged/#_packages_which_are_not_useful_without_external_code).)

In [our `uv` package](https://src.fedoraproject.org/rpms/uv), these considerations have lead us to ship an [`/etc/uv/uv.toml`](https://src.fedoraproject.org/rpms/uv/blob/3d17b4874d5ecca6b76ddaf2478fcd651451e55f/f/uv.toml) that sets `python-downloads = "manual"` and `python-preference = "system"` for Fedora-packaged `uv`. This has worked quite satisfactorily, and it seems to be working well enough for our users.

**From the same perspective, the new experimental `uv format` command is a bit concerning for distribution packaging, since its default behavior is to download and execute a pre-compiled binary of a particular version of `ruff`, and since I suspect many users will not be aware that this is what is happening.** I think that the `uv format --version=x.y.z` case is not too worrisome, because the user is explicitly requesting a particular version of `ruff` (and we obviously don’t have a variety of system-wide `ruff` versions to choose from).

Obviously, it would be best if the `ruff` for `uv format` could instead be our own [system `ruff` package](https://src.fedoraproject.org/rpms/ruff). However, it’s not reasonably possible, or even necessarily desirable, for us to keep our `ruff` aligned to the version that’s the default for `uv format` in the version of `uv` shipped in each branch. Both packages are nontrivial to maintain (considering their large dependency trees) and have different primary downstream maintainers and current downstream update cadences. Tying them together would not only require even more disciplined and coordinated maintenance, but it would also mean both packages had to be blocked on each other’s dependencies. Besides, we may have other reasons to update one package ahead of the other. I don’t think letting `uv format` control the version of  `ruff` in Fedora would be successful in the long run.

I don’t think we would be wise to patch `uv` to use a the system `ruff` *regardless of version*, either: the `ruff` version guarantee (and therefore the guarantee of stable/predictable default formatting behavior for a particular `uv` version) is designed into the `uv format` interface. If Fedora’s `uv format` produced different formatted code for a particular `uv` release than a pip-installed copy of the same `uv` release, or if the results of `uv format` changed without updating `uv` because `ruff` was updated separately, it seems like that would be a big enough divergence from the upstream behavior that it could be an ugly surprise for users.

What remains, as [suggested](https://src.fedoraproject.org/rpms/uv/pull-request/72#comment-280406) by @hroncok, is that perhaps there could be a configuration option in `uv.toml` to change the behavior such that, when `uv format` needs to install the default `ruff` version, it complains something like “uv format needs ruff 1.2.3.4; use uv format --download-ruff to get it” and exits with a nonzero code. What do you think? Is this a viable avenue? Do you have any alternative ideas for how we could ensure that users of Fedora’s `uv` package don’t end up installing non-Fedora binaries by accident?

(Mentioning @decathorpe, who may also be interested in this.)

### Example

_No response_

---

_Label `enhancement` added by @musicinmybrain on 2025-09-02 08:05_

---

_Comment by @zanieb on 2025-09-02 14:10_

Thanks for raising this!

Can you explain a bit how this is different from `uvx ruff` downloading Ruff? That's an explicit request? 

If we had used a wheel from PyPI instead of the binary from GitHub, would that be different? If `uv format` took a configurable Python package to use for formatting, e.g., `black` or `ruff` and `uv format` in a project downloaded it, would that be different? How does this differ from other uv operations in a project which implicitly download packages, e.g., `uv run`?

(I want to understand how you're thinking about this before proposing a solution)



---

_Comment by @zanieb on 2025-09-02 18:03_

Would it be sufficient for `uv format` to prompt for consent?

---

_Comment by @zanieb on 2025-09-05 13:50_

I'm also supportive of just trying to use the system Ruff version that you provide. I think users that _really_ care about stable formatting should be pinning their formatter version. The feature is young and in preview, so I think it's okay if we learn some lessons here and change the approach in the future.

---

_Comment by @notatallshaw on 2025-09-05 15:35_

FWIW, like uv provides the option to use the local system Python, I would use and be supportive of uv having an option here for me to tell uv to just use the ruff already on the path and don't try downloading one.

That would allow me to solve a lot of different use cases (that aren't distro maintainer related) where I don't want uv going out to the Internet, but I can provide a ruff that I have already installed through some other means.

---

_Comment by @musicinmybrain on 2025-09-06 07:55_

> Thanks for raising this!
> 
> Can you explain a bit how this is different from `uvx ruff` downloading Ruff? That's an explicit request?

Please take the following as my personal view on the reasonable application of Fedora’s written and unwritten principles, writing as an experienced packager and as the primary maintainer of the `uv` package in Fedora – no more and no less. I think what I’m going to write would be widely agreed upon among Fedora packagers, but in the end it’s just me writing, and other Fedora contributors might have somewhat different views.

We want to make it easy for people to use Fedora-packaged versions of software whenever possible, because we believe that distribution maintenance and integration work has value. That doesn’t mean users should be unable to download and run software built elsewhere if they explicitly choose to do so.

I think that when a user writes `uvx ruff` or `uv pip install ruff`, it’s relatively clear that they are naming an arbitrary PyPI project and expecting it to be installed and/or executed from PyPI. This is the core functionality of the respective commands (`uvx`/`uv run`, `uv pip`) and it’s clear from a casual, careless look at their documentation that this is what they do.

When a user writes `uv format`, it is not immediately obvious that this is using Ruff, although Ruff is mentioned a couple of times in reading `uv format --help` , so this information is available. Still, it would be easy for people who are just following an example that uses `uv format` to think that the formatting functionality is built in to the packaged `uv`. Some people have already [misread the announcement of `uv format`](https://news.ycombinator.com/item?id=44977645#44978500) and thought that `uv` and `ruff` were being merged.

For users who are already using system packaged versions of `uv` and `ruff`, it is even less obvious that calling `uv format` does something different from calling `ruff` directly. This is where it starts to feel a bit like the download and installation of a pre-compiled `ruff` is happening “behind the user’s back” – not nefariously so, but still without the user clearly specifying it.

> If we had used a wheel from PyPI instead of the binary from GitHub, would that be different?

I don’t think so.

> If `uv format` took a configurable Python package to use for formatting, e.g., `black` or `ruff` and `uv format` in a project downloaded it, would that be different? How does this differ from other uv operations in a project which implicitly download packages, e.g., `uv run`?

I’m not sure. Maybe somewhat, but maybe not entirely? Choosing from a list of formatters still feels a little different from naming an arbitrary PyPI project as in `uv run`, and it feels like the documentation of `uv run` is quite clear that downloading a command or script and its dependencies into a virtual environment is the core purpose of the command.

I do think that the case where `--version` is given to  `uv format` is given is less problematic in terms of user intent, since a reasonable user should probably expect that using an arbitrary exact version of Ruff will mean downloading it from somewhere.

> I'm also supportive of just trying to use the system Ruff version that you provide. I think users that really care about stable formatting should be pinning their formatter version. The feature is young and in preview, so I think it's okay if we learn some lessons here and change the approach in the future.

I think allowing us to somehow configure `uv format` to use the system Ruff by default would be optimal from a Fedora perspective, if this wouldn’t otherwise be too surprising and disruptive to users – and understanding that in general we won’t be able to reliably align the system Ruff to match the default Ruff for `uv format` upstream. This seems like it would satisfy @notatallshaw’s needs above, too.

---

_Comment by @onerandomusername on 2025-09-09 16:08_

> **From the same perspective, the new experimental `uv format` command is a bit concerning for distribution packaging, since its default behavior is to download and execute a pre-compiled binary of a particular version of `ruff`, and since I suspect many users will not be aware that this is what is happening.** I think that the `uv format --version=x.y.z` case is not too worrisome, because the user is explicitly requesting a particular version of `ruff` (and we obviously don’t have a variety of system-wide `ruff` versions to choose from).

I had no idea that uv format downloads a copy of ruff by default, I thought that it was vendored into uv at this point. Thanks for pointing it out!

---

_Comment by @powercoconola on 2025-09-10 00:50_

> I think allowing us to somehow configure `uv format` to use the system Ruff by default would be optimal from a Fedora perspective, if this wouldn’t otherwise be too surprising and disruptive to users – and understanding that in general we won’t be able to reliably align the system Ruff to match the default Ruff for `uv format` upstream.

If `uv format` worked this way by default and didn't find a system-installed Ruff, what do you propose `uv format` to do in that case? I assume the maintainers of uv would want it to then download and install Ruff as it does today.

---

_Comment by @musicinmybrain on 2025-09-10 06:18_

> If `uv format` worked this way by default and didn't find a system-installed Ruff, what do you propose `uv format` to do in that case? I assume the maintainers of uv would want it to then download and install Ruff as it does today.

I assume it would fail with nonzero exit status and prompt the user to either install the necessary system package or run a manual download command, similar to what happens when [`python-downloads = "manual"`](https://docs.astral.sh/uv/reference/settings/#python-downloads) / [`python-preference = "system"`](https://docs.astral.sh/uv/reference/settings/#python-preference) are set in `uv.toml` and a needed Python interpreter version is not installed.

---

_Comment by @onerandomusername on 2025-09-10 07:34_

I made [an issue](https://github.com/astral-sh/uv/issues/15760) regarding my thoughts on the format command as it stands now, and I feel like there's a solution that could satisfy both ends of the scale, from respecting the user's download preferences, to plug and play performance.

Though if uv supported and only downloaded ruff to use if a particular field was set, I think that would solve both problems

```toml
[tool.uv]
format.requirements = ["ruff==0.11.2"] # pep 440 identifier
```
This seems like it would solve the concerns from both sides: user has to opt in and version can be changed and provide room for future expansion even if the only package allowed for now is ruff.

That said, this config line could be included in `uv init` to begin projects with this feature enabled.

---

_Comment by @musicinmybrain on 2025-09-21 06:28_

Any progress on considering how and whether this might be implemented? I’ve held off on updating uv past 0.8.11 in Fedora, hoping for a resolution, but at some point I will need to choose a path forward regardless of what happens with `uv format`. Thanks!

---

_Comment by @zanieb on 2025-09-21 15:20_

I think either

1. A build time feature flag which enables a prompt for consent to download
2. A configuration option to use a specific Ruff executable

(1) seems easier and would be nice to have for, e.g., Python downloads, so you don't need to turn those off via the uv configuration file. (2) seems a little tricky when the user requests a different Ruff version — I think there's some additional design work we'd need to do to figure out how to make the experience reasonable.

I don't have the time to implement either patch right this second, but I'm happy to review.



---

_Referenced in [python-discord/site#1566](../../python-discord/site/pulls/1566.md) on 2025-10-21 20:36_

---
