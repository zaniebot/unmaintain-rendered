---
number: 5902
title: "Dependabot updates for `clap` uses changelog for `clap-complete`"
type: issue
state: closed
author: duckinator
labels: []
assignees: []
created_at: 2025-02-05T18:43:12Z
updated_at: 2025-02-06T17:33:02Z
url: https://github.com/clap-rs/clap/issues/5902
synced_at: 2026-01-10T01:28:18Z
---

# Dependabot updates for `clap` uses changelog for `clap-complete`

---

_Issue opened by @duckinator on 2025-02-05 18:43_

Dependabot PRs to update `clap` are mostly correct, but link to changelogs for `clap-complete`, which presents conflicting information about what's in an update.

I don't know if/how this can be fixed on your end, but I wanted to make sure you were aware of it!

axodotdev/cargo-dist#1742 is an example of this problem.

![Image](https://github.com/user-attachments/assets/190b52bc-da7a-44a4-9580-15d72f044485)

---

_Comment by @epage on 2025-02-05 19:10_

So the Releases and Changelog sections are correct.

The Commits section is not.  The range is using the wrong tags.  The version number is correct but not the package name.  Our [Releases](https://github.com/clap-rs/clap/releases) do correctly associate the release with the tag, so its not that.

Looking at their code, when they `findTagOfRelease`, they assume there will be a prefix to the tag which ours does not have.  However, I'm not seeing why it would select one of the other tags.  Everything after the prefix must be a version to select a tag, otherwise it falls back to other information
https://github.com/renovatebot/renovate/blob/55b5919dd1045df9275e80d4cafd03a6e325cc06/lib/workers/repository/update/pr/changelog/source.ts#L200-L219

I'd prefer not to change our tag structure for just this.  It might be good to report this to RenovateBot as it should be able to
- Get the tag from the Release
- Get the sha from the Release
- Get the sha from the `.crate` file

---

_Comment by @duckinator on 2025-02-06 04:04_

I agree, that definitely sounds like a bug on their end. I'll go ahead and close this, and make sure there's an issue on their repo. 👍

**EDIT:** I missed that you were talking about rennovatebot, which is different than dependabot. I do agree that it's probably not a problem with this repo itself, though.

---

_Closed by @duckinator on 2025-02-06 04:05_

---
