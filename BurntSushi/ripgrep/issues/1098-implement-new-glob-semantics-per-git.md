```yaml
number: 1098
title: "Implement new ** glob semantics per Git"
type: issue
state: closed
author: okdana
labels: []
assignees: []
created_at: 2018-11-01T16:58:31Z
updated_at: 2019-01-24T00:59:58Z
url: https://github.com/BurntSushi/ripgrep/issues/1098
synced_at: 2026-01-12T16:13:22Z
```

# Implement new ** glob semantics per Git

---

_@okdana_

Hey! Looks like the new `**` glob semantics discussed on the Git mailing list have been merged:

https://github.com/git/git/commit/627186d0206dcb219c43f8e6670b4487802a4921

(Just to reiterate, the change makes a `**` used in any context other than `**/`, `/**/`, or `/**` act as two regular `*`s.)

I *believe* this change will appear not in the up-coming release (2.20), but the one right after that.

I'd been playing with `globset` in preparation for this landing, which is how i discovered the issues i mentioned in #1093, but i'm not happy with my changes, so i don't have a PR yet. I was also unsure whether you'd want to wait until this actually appears in a released Git or what. But i wanted to make a note of it, anyway, just to track it.

---

_Comment by @okdana on 2018-12-10 02:09_

I was wrong about when this would come out. It's in 2.20, released today: <http://lkml.iu.edu/hypermail/linux/kernel/1812.1/00293.html>

And the updated documentation is live: <https://git-scm.com/docs/gitignore>

The difference in behaviour can be confirmed with e.g. `git ls-files` in the ripgrep repo:

```
% ~cellar/git/2.19.2/bin/git ls-files --ignored -x '/**.md'
# (no output)
% ~cellar/git/2.20.0/bin/git ls-files --ignored -x '/**.md'
CHANGELOG.md
FAQ.md
GUIDE.md
ISSUE_TEMPLATE.md
README.md
```

(We'd already established this, but just to be clear, Git did actually accept `**` in many cases where it said it didn't, so some documented-illegal patterns, like `[A-Z]**.md`, won't demonstrate any difference)

---

_Comment by @BurntSushi on 2018-12-10 12:11_

Awesome! I'm happy to see the `**` issue finally moving towards a solid answer. :-)

Sorry I haven't addressed this sooner or your PR yet. Everything looks good, and thanks for all the legwork on this. From what I've skimmed, everything sounds right, but I haven't quite gotten the time to sink my teeth into the details yet.

---

_Closed by @BurntSushi on 2019-01-24 00:59_

---
