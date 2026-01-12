```yaml
number: 18883
title: "Setup renovate for `dist-workspace.toml`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - release
  - ci
assignees: []
base: main
head: micha/add-missing-version-specifiers
created_at: 2025-06-23T06:19:17Z
updated_at: 2025-06-24T06:24:48Z
url: https://github.com/astral-sh/ruff/pull/18883
synced_at: 2026-01-12T15:56:27Z
```

# Setup renovate for `dist-workspace.toml`

---

_@MichaReiser_

## Summary

Sets up renovate for the `dist-workspace.toml` because and disables renovate for the `release.yml`

1. It's annoying to fixup the commit hashes for renovate PRs updating `release.yml`
2. Renovate updates a dependency with a missing verison comment to the next available commit, which isn't necessarily part of a release (yet).




---

_Label `ci` added by @MichaReiser on 2025-06-23 06:19_

---

_Renamed from "Add missing GitHub Actions version comments" to "Setup renovate for `dist-workspace.toml`" by @MichaReiser on 2025-06-23 06:34_

---

_Label `release` added by @MichaReiser on 2025-06-23 06:38_

---

_Closed by @MichaReiser on 2025-06-23 06:38_

---

_Reopened by @MichaReiser on 2025-06-23 06:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-23 09:05_

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:45 on 2025-06-23 10:38_

uff, this config file gets more and more complex 🙃 I guess this isn't the first regex in this file, but it definitely seems like the most complicated one we've added so far.

Could we at least add a comment here to document a bit better:
1. What the regex matches and
2. Why we're using a custom manager here rather than using renovate's builtin GitHub Actions support

I also suggest linking directly to https://docs.renovatebot.com/modules/manager/regex/ somewhere

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:45 on 2025-06-23 10:48_

Am I also correct that if Renovate creates a PR to update the pin in `dist-workspace.toml`, we'll then need to checkout the Renovate PR locally and run `dist generate` before CI will pass on the PR (because otherwise the pins in `release.yml` will be out of date with the pins in `dist-workspace.toml`)? That could also be worth documenting in a comment, maybe?

---

_Review comment by @AlexWaygood on `.github/renovate.json5`:45 on 2025-06-23 10:54_

I think we should probably be less exact about the exact amount of whitespace, and the exact number of characters made up by the hash. The docs also state:

> The regex manager matches are done _per-file_, not per-line! This means the `^` and `$` regex assertions only match the beginning and end of the entire file. If you need to match line boundaries you can use `(?:^|\r\n|\r|\n|$)`.

I think that means that the `.*` at the end currently will match the entirety of the rest of the `dist-workspace.toml` file, meaning that there will only ever be at most one match for any Renovate run?

```suggestion
      matchStrings: [
        '"(?<depName>actions/[^"]+)"\\s*=\\s*"(?<currentDigest>[a-f0-9]{4,})"\\s*#\\s*(?<currentValue>v[\\d\\.]+).*?\\n'
      ],
```

---

_@AlexWaygood approved on 2025-06-23 11:01_

I don't love the complexity that this adds to our renovate config, but I can definitely see that this in particular is annoying:

> Renovate updates a dependency with a missing verison comment to the next available commit, which isn't necessarily part of a release (yet).

I'm also not the person usually reviewing/merging these PRs, so I'm happy to defer to you if you think it's worth it!

---

_Comment by @MichaReiser on 2025-06-23 15:51_

Hmm. That's a good point that it now still requires manually updating the `release.yml`. @Gankra is there a way we could preserve the version comment in the `yml` that cargo dist generates? That would allow us to keep the current setup but without renovate getting all confused and picking up every commit instead of only releaes-commits.

---

_Comment by @Gankra on 2025-06-23 20:01_

I could have us insert a comment sure -- alternatively can you just have renovate edit release.yml *and* the dist-workspace.toml?

...oh right the comment would be read as "dirty" by cargo-dist. ok yeah I can add a feature for this.

---

_Closed by @MichaReiser on 2025-06-24 06:24_

---
