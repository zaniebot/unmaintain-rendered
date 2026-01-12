```yaml
number: 9203
title: Using uv in CircleCI
type: issue
state: open
author: bolinocroustibat
labels:
  - documentation
  - wish
assignees: []
created_at: 2024-11-18T15:45:49Z
updated_at: 2024-11-19T08:31:26Z
url: https://github.com/astral-sh/uv/issues/9203
synced_at: 2026-01-12T15:59:44Z
```

# Using uv in CircleCI

---

_@bolinocroustibat_

There is great documentation about [using uv in GitHub Actions](https://docs.astral.sh/uv/guides/integration/github/) and in [GitLab CI](https://docs.astral.sh/uv/guides/integration/gitlab/), along with a [uv GitHub Actions setup](https://github.com/astral-sh/setup-uv).

Is there any plan for a documentation about how to use uv in CircleCI?

---

_Comment by @zanieb on 2024-11-18 15:47_

There's no particular plan for it, but if someone is interested in contributing a guide I will review and edit it.

---

_Label `documentation` added by @zanieb on 2024-11-18 15:47_

---

_Label `wish` added by @zanieb on 2024-11-18 15:47_

---

_Comment by @antoniomdk on 2024-11-18 22:51_

I'm trying to get `uv` as an "officially" supported package manager for CircleCI:
https://github.com/CircleCI-Public/python-orb/pull/124
https://github.com/CircleCI-Public/cimg-python/pull/257

But I feel it will take a while for those changes to get to the mainstream (I think CircleCI performs quarterly releases of their base images).

In the meantime, I've been able to integrate with CircleCI using something like this:

```yaml
  uv_install_packages:
    parameters:
      cache_key:
        type: string
      cache_dir:
        type: string
        default: /home/circleci/.cache/uv
    steps:
      - restore_cache:
          name: Restore python cache
          keys:
            - <<parameters.cache_key>>-uv-cache-{{ checksum "uv.lock" }}
            - <<parameters.cache_key>>-uv-cache-
      - run:
          name: Install python dependencies
          command: |
            mkdir -p "<<parameters.cache_dir>>" && \
            uv sync --frozen --compile-bytecode --cache-dir <<parameters.cache_dir>>
      - run:
          name: Clean pre-built wheels
          command: uv cache prune --ci --cache-dir <<parameters.cache_dir>>
      - save_cache:
          name: Save cache
          key: <<parameters.cache_key>>-uv-cache-{{ checksum "uv.lock" }}
          paths:
            - "<<parameters.cache_dir>>"
```

---

_Comment by @bolinocroustibat on 2024-11-19 08:31_

Amazing, very helpful, thank you.
I still have issues on uv command not being recognized in next jobs, even though the uv install job persists to workspaces and the next jobs attaches it, but I guess it's about fixing my CircleCI config...

---
