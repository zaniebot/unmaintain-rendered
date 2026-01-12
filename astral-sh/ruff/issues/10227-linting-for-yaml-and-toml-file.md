```yaml
number: 10227
title: Linting for yaml and toml file
type: issue
state: open
author: VascoSch92
labels:
  - core
  - wish
  - linter
assignees: []
created_at: 2024-03-04T09:48:31Z
updated_at: 2025-10-21T17:14:09Z
url: https://github.com/astral-sh/ruff/issues/10227
synced_at: 2026-01-12T15:54:50Z
```

# Linting for yaml and toml file

---

_@VascoSch92_

Hey 👋 

If I'm correct, there are not rules which target `.yaml` or `.toml` config files.

- Could be interesting to format also these types of files?
- Is this already a things?


---

_Comment by @ashrub-holvi on 2024-03-04 11:44_

I am sure they have a lot of work with Python rules, 677 open issues at that moment.
But, yes, would be nice to have `yaml`, `json`, `toml`, `xml` formatters included.
For example, I tried many different tools for autoformating `xml` and almost all of them had some issues (or I had issues with configuring them), perhaps [prettier](https://github.com/prettier) was ok, but still not started using it, partially because of `node.js` dependency.

---

_Label `wish` added by @MichaReiser on 2024-03-04 12:42_

---

_Comment by @MichaReiser on 2024-03-04 12:42_

Yeah that would be neat. It's not our primary focus for now. There's still a lot to do on the Python front. 

Related to https://github.com/astral-sh/ruff/issues/9244

---

_Comment by @ashrub-holvi on 2024-03-04 17:15_

I am even thinking that pre-commit hook should have a check which will fail if you are trying to add a file of type which has no autoformatter configured )

---

_Label `core` added by @zanieb on 2024-03-11 17:26_

---

_Label `linter` added by @zanieb on 2024-03-11 17:26_

---

_Comment by @Kilo59 on 2024-03-30 00:09_

@ashrub-holvi 

I agree it would be nice if ruff could format/lint toml + yaml but it is possible to use `prettier` for this without requiring node to be a project dependency. Just use the [pre-commit hook](https://prettier.io/docs/en/precommit#option-2-pre-commithttpsgithubcompre-commitpre-commit) and the [pre-commit-ci](https://pre-commit.ci/).


Example 
https://github.com/great-expectations/great_expectations/blob/200c968b5b5c6b19d5d2d63ca0d6fe1835b94b62/.pre-commit-config.yaml#L27-L32

---

_Comment by @ashrub-holvi on 2024-04-02 07:11_

@Kilo59 yes, thanks, I know it't not a project dependency, in my case it was minor/temporary issue because of outdated node version in CI - upgrade was blocked by other stuff for some time. But still - less js is good )

---

_Comment by @ashrub-holvi on 2025-10-17 15:45_

Not used yet, but found prettier for YAML written in Rust https://github.com/g-plane/pretty_yaml

---

_Comment by @ashrub-holvi on 2025-10-21 12:45_

I guess this issue can be closed as duplicate, main discussion is going in https://github.com/astral-sh/ruff/issues/10738 and it's mostly about using [dprint](https://dprint.dev/).

---

_Comment by @MichaReiser on 2025-10-21 17:13_

I think it's different as it's about linting and not formatting (although it does mention formatting..)

---
