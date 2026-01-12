```yaml
number: 11185
title: Automatic resolution of merge conflicts in lockfiles
type: issue
state: open
author: JanPokorny
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-02-03T16:24:19Z
updated_at: 2025-10-17T20:03:10Z
url: https://github.com/astral-sh/uv/issues/11185
synced_at: 2026-01-12T16:00:30Z
```

# Automatic resolution of merge conflicts in lockfiles

---

_@JanPokorny_

### Summary

Yarn does automatic merge conflict resolution in lockfiles. In practice, you just run `yarn install` and the lockfile is merged as part of that process:

```
$ yarn install

yarn install v1.0.1
info Merge conflict detected in yarn.lock and successfully merged.
[1/4] Resolving packages...
```

This feature would be great for `uv`.

There's already a ticket for documenting the best practices for merging `uv.lock` (https://github.com/astral-sh/uv/issues/5633), this feature request is specifically for the automated option.


### Example

Currently:
```
$ uv sync
error: Failed to parse `uv.lock`
  Caused by: TOML parse error at line 146, column 1
    |
146 | <<<<<<< HEAD
    | ^
invalid key
```

Desired:
```
$ uv sync
Merge conflict detected in uv.lock and successfully merged.
Resolved 70 packages in 274ms
```

---

_Label `enhancement` added by @JanPokorny on 2025-02-03 16:24_

---

_Comment by @zanieb on 2025-02-03 16:52_

Interesting idea!

---

_Comment by @zanieb on 2025-02-03 16:53_

I am curious how yarn does this / how hard it is

---

_Comment by @JanPokorny on 2025-02-03 17:30_

@zanieb This seems to be the relevant code: https://github.com/yarnpkg/yarn/blob/master/src/lockfile/parse.js

---

_Comment by @zanieb on 2025-02-03 17:32_

Hm that looks like the parser, not sure where they "resolve" the conflict to a specific version.

---

_Comment by @JanPokorny on 2025-02-03 17:40_

@zanieb You're right, this code only seems to reconstruct the two versions from the conflicted files, parse them separately, and then merge them using `Object.assign`.

Given the structure of a `yarn.lock`...

```yaml
"@ai-zen/node-fetch-event-source@npm:^2.1.4":
  version: 2.1.4
  resolution: "@ai-zen/node-fetch-event-source@npm:2.1.4"
  dependencies:
    cross-fetch: "npm:^4.0.0"
  checksum: 10c0/9d2753f308bf6258c85d52328ef87e45dd4d2888f2bae7dca8b64b183d9d8f674a5f41aa57909d48f00402a0999aa7908dfa7da0f4b51ca22e82c333cd358ef5
  languageName: node
  linkType: hard

"@apidevtools/json-schema-ref-parser@npm:^11.5.5":
  version: 11.7.0
  resolution: "@apidevtools/json-schema-ref-parser@npm:11.7.0"
  dependencies:
    "@jsdevtools/ono": "npm:^7.1.3"
    "@types/json-schema": "npm:^7.0.15"
    js-yaml: "npm:^4.1.0"
  checksum: 10c0/0ae9ced2953918a14b17874dc0515d3367a95197d58d869f44e756223e23eb9996283e21cb7ef1c7e016ae94467920b4aca705e0ea3d61a61455431572ad4ab5
  languageName: node
  linkType: hard

"@aws-crypto/crc32@npm:5.2.0":
  version: 5.2.0
  resolution: "@aws-crypto/crc32@npm:5.2.0"
  dependencies:
    "@aws-crypto/util": "npm:^5.2.0"
    "@aws-sdk/types": "npm:^3.222.0"
    tslib: "npm:^2.6.2"
  checksum: 10c0/eab9581d3363af5ea498ae0e72de792f54d8890360e14a9d8261b7b5c55ebe080279fb2556e07994d785341cdaa99ab0b1ccf137832b53b5904cd6928f2b094b
  languageName: node
  linkType: hard
```

...I suppose that it just arbitrarily picks one of the versions over the other. Maybe this is an advantage of the Node.js ecosystem, where it can keep as many versions of any given package as it pleases, and a more complex solution would be needed in Python.

---

_Label `wish` added by @charliermarsh on 2025-02-03 21:10_

---

_Comment by @hmvp on 2025-05-26 12:44_

I imagine that you typically want the newest version that is found in both versions. Also when the user has already fixed merge conflicts in pyproject.toml (or there are none) then that can also be use to resolve some of the packages...

So I guess you want to do whatever `uv sync` does.  Except limit all the indirect dependencies to whatever the latest version was as found in any of the two versions of the lock file...

---

_Comment by @konstin on 2025-05-26 12:49_

Fwiw my usual strategy for feature branches is to accept the changes from the base branch (which often has dependency updates), then run `uv lock` to update the lockfile with the new requirements from the branch.

---

_Comment by @Avasam on 2025-06-06 18:24_

op mentions yarn, but I'll mention that npm itself also does it. Idk if it's still the case as I enabled it months or years ago, but it prompted me to opt-in to the automated resolution.

---

_Comment by @namurphy on 2025-10-17 01:34_

If `uv.lock` contains a merge conflict or is otherwise invalid, would it make sense for `uv lock --upgrade` to issue a warning and replace `uv.lock` instead of failing?  🤔  

When using `--upgrade`, users are looking for an environment with the most recent versions of dependencies, so recreating `uv.lock` from scratch seems reasonable. Recreating `uv.lock` would also bypass the need to resolve merge conflicts.  This doesn't apply when running `uv lock` without `--upgrade`, though.

An alternative would be to implement a new flag like `--recreate`, `--replace`, or `--ignore-lockfile` to indicate that `uv.lock` should be recreated from scratch if necessary, but I'd prefer to have `--upgrade` do this.

Once again, thank you all for being awesome! 😺 

---

_Comment by @konstin on 2025-10-17 08:13_

> If `uv.lock` contains a merge conflict or is otherwise invalid, would it make sense for `uv lock --upgrade` to issue a warning and replace `uv.lock` instead of failing? 🤔

Can you make a separate issue for that? We can discuss this separate the specific behavior for merge conflicts.

---

_Comment by @namurphy on 2025-10-17 20:03_

> Can you make a separate issue for that? 

Good idea!  We can branch that part of the conversation off into #16348.



---
