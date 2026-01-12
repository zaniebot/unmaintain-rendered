```yaml
number: 10334
title: Add markdownlint settings
type: pull_request
state: closed
author: o-l-a-v
labels:
  - internal
assignees: []
base: main
head: add-markdownlint-settings
created_at: 2025-01-06T19:06:46Z
updated_at: 2025-04-28T00:47:42Z
url: https://github.com/astral-sh/uv/pull/10334
synced_at: 2026-01-12T16:09:14Z
```

# Add markdownlint settings

---

_@o-l-a-v_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In VSCode with Markdownlint (<https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint>) and default rules enabled, it'd autocorrect a few things that do not seem to be what the project wants for its doc. For instance:

### [`MD014` - Dollar signs used before commands without showing output](https://github.com/DavidAnson/markdownlint/blob/main/doc/md014.md)

Would correct

````md
```console
$ winget install --id=astral-sh.uv -e
```
````

to

````md
```console
winget install --id=astral-sh.uv -e
```
````

### [`MD046` - Code block style](https://github.com/DavidAnson/markdownlint/blob/main/doc/md046.md)

Says [installation.md](https://github.com/astral-sh/uv/blob/main/docs/getting-started/installation.md) uses inconsistent code block style.

Seems the doc must use it for it to look good with mkdocs?

## Test Plan

Affects developer experience.


---

_Comment by @zanieb on 2025-01-06 19:12_

I'm a little hesitant to add configuration for tools we're not using / enforcing. Does markdownlint catch anything we'd want to enforce?

---

_Label `internal` added by @zanieb on 2025-01-06 19:12_

---

_Comment by @o-l-a-v on 2025-01-06 19:28_

Very understandable. 😊

What IDE do contributors typically use, and do they lint markdown while editing in the IDE itself? How is that linting configured? I like immediate feedback vs. waiting for CI/CD tests. Markdownlint seems pretty universal for Markdown linting.

I linked a VSCode, but it has a lot more to it:

* Markdownlint itself: <https://github.com/DavidAnson/markdownlint>
* VSCode extension: <https://github.com/DavidAnson/vscode-markdownlint>
* CLI: <https://github.com/DavidAnson/markdownlint-cli2>
* GitHub action: <https://github.com/DavidAnson/markdownlint-cli2-action>

It's heavily influenced by:

* <https://github.com/markdownlint/markdownlint>

Other linting tools also uses markdownlint for markdown, like:

* <https://github.com/marketplace/actions/megalinter>
* <https://github.com/marketplace/actions/super-linter>
* <https://github.com/marketplace/actions/markdownlint-cli2-action>

---

If you'd like a config to only apply in VSCode, Markdownlint (the extension) configuration can also be added to `.vscode/settings.json`. Example:

```json
{
  "markdownlint.config": {
    "default": true,
    "MD013": {
      "line_length": 100
    },
    "MD014": false,
    "MD046": false
  }
}
```

If you have a different way to get feedback from a markdown linter while editing, I'll adapt.

If you don't want `.markdownlint.json` I'm fine with that too.

---

_Comment by @charliermarsh on 2025-04-28 00:47_

I'm going to close this for now based on the above, but thank you for your contribution.

---

_Closed by @charliermarsh on 2025-04-28 00:47_

---
