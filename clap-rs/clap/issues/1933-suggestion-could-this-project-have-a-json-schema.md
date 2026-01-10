---
number: 1933
title: "Suggestion: Could this project have a JSON schema for the YAML CLI options?"
type: issue
state: closed
author: strega-nil
labels:
  - E-medium
assignees: []
created_at: 2020-05-16T21:06:14Z
updated_at: 2020-05-18T10:16:00Z
url: https://github.com/clap-rs/clap/issues/1933
synced_at: 2026-01-10T01:27:09Z
---

# Suggestion: Could this project have a JSON schema for the YAML CLI options?

---

_Issue opened by @strega-nil on 2020-05-16 21:06_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

I am writing a clap YAML (which is a superset of JSON) file, and I wish to get errors and warnings from VS Code if my YAML file is not a valid clap YAML file.

### Describe the solution you'd like

There is a `clap.schema.json` file somewhere in the repo that I can set the `"$schema"` property to.

### Additional context

[JSON schemas](https://json-schema.org/)
[Yaml's relationship to JSON](https://yaml.org/spec/1.2/spec.html#id2759572)

---

_Label `T: new feature` added by @strega-nil on 2020-05-16 21:06_

---

_Label `C: other` added by @CreepySkeleton on 2020-05-17 10:14_

---

_Label `D: easy` added by @CreepySkeleton on 2020-05-17 10:14_

---

_Label `Z: mentored` added by @CreepySkeleton on 2020-05-17 10:14_

---

_Added to milestone `3.1` by @CreepySkeleton on 2020-05-17 10:14_

---

_Comment by @CreepySkeleton on 2020-05-17 10:15_

Why not? @strega-nil would you like to make a PR?

---

_Referenced in [clap-rs/clap#1934](../../clap-rs/clap/pulls/1934.md) on 2020-05-17 17:38_

---

_Closed by @bors[bot] on 2020-05-18 10:16_

---
