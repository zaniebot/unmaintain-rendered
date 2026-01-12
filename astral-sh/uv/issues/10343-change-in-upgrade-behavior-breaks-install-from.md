```yaml
number: 10343
title: "Change in `--upgrade` behavior breaks install from private registry"
type: issue
state: closed
author: samuelcolvin
labels:
  - breaking
assignees: []
created_at: 2025-01-07T09:24:23Z
updated_at: 2025-01-07T14:21:04Z
url: https://github.com/astral-sh/uv/issues/10343
synced_at: 2026-01-12T16:00:11Z
```

# Change in `--upgrade` behavior breaks install from private registry

---

_@samuelcolvin_

(Mostly for the benefits of others encountering this problem.)

The changes introduced in #10097 and released in `v0.5.15` have broken our workflow for installing `mkdocs-material` and `mkdocstrings` from our private registry in [pydantic-ai](https://github.com/pydantic/pydantic-ai) - we were [using](https://github.com/pydantic/pydantic-ai/blob/72300ae7760c51163b8932866bd95f6695d11b12/Makefile#L87-L90):

```bash
	@echo 'installing insiders packages...'
	@uv pip install -U \
		--extra-index-url https://pydantic:${PPPR_TOKEN}@pppr.pydantic.dev/simple/ \
		mkdocs-material mkdocstrings-python
```

To install the insiders version of these packages, as a result of #10097 this no longer works since the versions of those packages we have in our private registry are older than the ones already installed.

The work around is to uninstall the packages first, e.g. change the above to

```bash
	@echo 'installing insiders packages...'
	@uv pip uninstall mkdocs-material mkdocstrings-python
	@uv pip install \
		--extra-index-url https://pydantic:${PPPR_TOKEN}@pppr.pydantic.dev/simple/ \
		mkdocs-material mkdocstrings-python
```

See https://github.com/pydantic/pydantic-ai/pull/622.

By the way, I'd love a better way to managing the option to install packages from a private repo 🙏.

Feel free to close this.

---

_Comment by @samuelcolvin on 2025-01-07 10:08_

... Acknowledging that there's a bit of [XKCD 1172](https://m.xkcd.com/1172/) about this issue.

---

_Comment by @T-256 on 2025-01-07 10:35_

I think for your workflow it is proper to use `--reinstall` instead of `--upgrade`. You can also see [this example](https://github.com/astral-sh/uv/pull/10097/files#diff-4decbdad65ae6c2e77b162950bc1a15907dacaff952fcee61559704bf370fb06R2684-R2702) from mentioned PR.

---

_Comment by @T-256 on 2025-01-07 11:15_

> See 
> 
> https://github.com/pydantic/pydantic-ai/actions/runs/12649998415/job/35247581389
> 
> vs 
> 
> https://github.com/pydantic/pydantic-ai/actions/runs/12650057969/job/35247762611
> 
> This is causing all packages to be reinstalled, rather than just installing the required ones which makes it slower, not a big problem in CI, but it will slow down local development, therefore I'm going to stick with what we have now.
> 

With https://github.com/pydantic/pydantic-ai/issues/625#issuecomment-2574982831 I think you can either add `--reinstall-package <PKG>` per package or use `--reinstall --no-deps`.


---

_Comment by @charliermarsh on 2025-01-07 13:48_

Gah, thanks. That's a funny one -- sorry for the disruption. I think `--reinstall` is probably what you want here.

---

_Closed by @charliermarsh on 2025-01-07 13:49_

---

_Label `breaking` added by @charliermarsh on 2025-01-07 13:49_

---

_Comment by @T-256 on 2025-01-07 14:21_

One question is in my mind, if in future, internal `mkdocs-material` updated it dependencies that are in conflict with original `mkdocs-material` dependencies, does still `--reinstall --no-deps` work for them?

---
