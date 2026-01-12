```yaml
number: 11181
title: UV support at SNYK
type: issue
state: closed
author: etele
labels:
  - question
assignees: []
created_at: 2025-02-03T12:08:06Z
updated_at: 2025-09-08T21:12:40Z
url: https://github.com/astral-sh/uv/issues/11181
synced_at: 2026-01-12T16:00:30Z
```

# UV support at SNYK

---

_@etele_

### Question

Hi, 
not a code/feature related, but support related on third party side.

[SNYK](https://snyk.io/) is a cloud native security platform, it can check the version of installed packages and report vulnerabilities. At the moment it has no support for `uv` which is understandable as its new.
Currently they [support](https://docs.snyk.io/supported-languages-package-managers-and-frameworks#package-managers-and-frameworks) poetry.lock and pipenv.lock. Adding  uv to the list might be beneficial for both, considering how fast uv gains on popularity.

Can you consider trying to reach out to them to support uv.lock files ?
Personally i wrote a question to their support, but a question from the authors might be a good motivation also.

For us at the moment is a blocker to migrate to `uv`.

Edit: they confirmed not supporting it, forwarded to the product owner (so any support on the issue is welcome), either from community (express your need for it if you use their services) or from developer side.

Thanks in advance.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @etele on 2025-02-03 12:08_

---

_Renamed from "UV and SNYK support" to "UV support at SNYK" by @etele on 2025-02-03 21:56_

---

_Comment by @chenchanglew on 2025-02-21 09:23_

Since switching our dependency management from Poetry to uv last year, maintaining Snyk support in our project has become inconvenient. We currently rely on `uv export --format requirements-txt > requirements.txt` to generate our requirements file. It would greatly improve our workflow if Snyk could support uv-exported files.


---

_Comment by @gpadavala on 2025-02-27 14:22_

we have the same issue, using requirements.txt instead of lock file in snyk has disadvantage that it loses on the dependency tree, so i would like to see snyk work with the uv universal lock files

---

_Comment by @marioweid on 2025-03-11 07:25_

We have the same issue and i came uppon this thread while googling.
Would be great if uv and snyk would work together but isn't this a snyk related topic?


---

_Comment by @Rashik-raj on 2025-03-11 10:02_

I have been doing `uv pip freeze > requirements.txt` to create `requirements.txt` in the same format to that of `pip` and `snyk` works fine.

---

_Comment by @Rashik-raj on 2025-03-11 10:03_

> I have been doing `uv pip freeze > requirements.txt` to create `requirements.txt` in the same format to that of `pip` and `snyk` works fine.

This one is just for reference so that snyk can track, whereas `pyproject.toml` is still used for majority of use cases.

---

_Comment by @jasonslay on 2025-03-20 22:21_

We are also switching over to using Snyk and flexing the workaround mentioned by @Rashik-raj.

---

_Comment by @jeffgabhart on 2025-04-30 13:58_

> I have been doing `uv pip freeze > requirements.txt` to create `requirements.txt` in the same format to that of `pip` and `snyk` works fine.

Another option for those defining in `pyproject.toml`  is `uv pip compile pyproject.toml -o requirements.txt`
Also hoping snyk starts to support the `uv.lock`

---

_Comment by @SteveGoodenoughWork on 2025-06-20 10:16_

I know our account manager has added us to the list of paying customers wanting this, but I'm weighing in here to add to the crowd hoping Snyk add support for uv

We are a platform team maintaining a huge number of Python repositories and I've got us to accept we'll move from good 'ol pipenv (plus a few poetry) to uv

Renovate is doing a great job of updating dependencies since they added uv support so I'd much prefer a proper solution from Snyk than the workaround of creating a requirements file (and that's sort of ok in CICD pipelines but not when you want to use Snyk in your ide).


---

_Comment by @LucasAdamsTR on 2025-06-20 20:04_

Please support this!

---

_Comment by @zanieb on 2025-06-20 20:21_

I'd encourage you to ask Snyk to add this if you're just posting here. We're happy to help, but we can't make product decisions for Snyk.

---

_Comment by @ns-mkusper on 2025-07-02 22:25_

It's honestly shocking to me that this is not currently supported this long after the initial request for a tool that has gained such popularity in the Python space.

---

_Comment by @MarcDuQuesne on 2025-07-22 08:48_

This is relevant for our community as well. Please support this!

---

_Comment by @ActionScripted on 2025-09-08 14:56_

If you're using `pre-commit` you can use the provided export hook to generate the requirements file automatically:

```python
repos:
  - repo: https://github.com/astral-sh/uv-pre-commit
    # uv version.
    rev: 0.8.15
    hooks:
      - id: uv-export
```

https://docs.astral.sh/uv/guides/integration/pre-commit/

---

_Comment by @woodruffw on 2025-09-08 21:03_

Hey folks, as a gentle reminder: this is uv's issue tracker, **not** Snyk's. If you're a paying customer of Snyk, you should definitely reach out to your contact/account manager there and advocate directly to them, because they almost certainly aren't monitoring this issue.


---

_Closed by @woodruffw on 2025-09-08 21:03_

---

_Reopened by @woodruffw on 2025-09-08 21:09_

---

_Closed by @woodruffw on 2025-09-08 21:10_

---

_Comment by @jkugler on 2025-09-08 21:12_

If someone is looking for a tool that will pull dependencies out of `uv.lock` files, Syft has this as of version v1.29.0 https://github.com/anchore/syft/releases/tag/v1.29.0 Disclaimer: I contributed the feature.

---
