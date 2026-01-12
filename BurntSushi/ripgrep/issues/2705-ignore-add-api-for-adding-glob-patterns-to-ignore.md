```yaml
number: 2705
title: "[ignore] Add API for adding glob patterns to ignore"
type: issue
state: open
author: tmccombs
labels:
  - enhancement
assignees: []
created_at: 2024-01-07T09:25:17Z
updated_at: 2024-02-20T07:07:08Z
url: https://github.com/BurntSushi/ripgrep/issues/2705
synced_at: 2026-01-12T16:13:24Z
```

# [ignore] Add API for adding glob patterns to ignore

---

_@tmccombs_

#### Describe your feature request

In the `ignore` crate, `WalkerBuilder::override` allows you to pass in overrides, which use ignore globs with the meaning of "!" inverted. For ripgreps `-g` option that inversion make sense. 

In `fd`, there is an `--exclude` option, which takes additional patterns to exclude. In order for that to work we use the override mechanism but currently prepend a "!" to the glob to counteract the inversion that overrides performs.  It would be nice if there was an API that we could use to add an override to exclude instead of include a pattern directly. 

---

_Label `enhancement` added by @BurntSushi on 2024-01-07 13:56_

---

_Comment by @BurntSushi on 2024-01-07 13:57_

I personally don't think needing to add a `!` to the glob justifies a new API for this.

With that said, I am hoping to do a refresh of the `ignore` crate soonish. Probably within the next year or so. My guess is that the API will be completely redesigned. There is little to like about the current one IMO.

---

_Comment by @tmccombs on 2024-01-08 08:11_

Here's my actual problem:

In order to exclude the `.git` directory itself when hidden files are shown, and gitignore is active, we add "!.git" to the overrides.  

I still think this is a reasonable default to have (and maybe it would make sense for the ignore crate itself to do this?), but there are a few users who do want to include the `.git` directory in the output. I was hoping that I could get something like `--exclude !.git` or `--include .git`  to work where it would negate the `!.git` in the overrides. However, due to the way overrides are implemented, `!.git` has higher priority than `.git` in the overrides.  I could check for the specific string ".git" in the list of options passed to `--include`, before adding `!.git`, but that feels kind of hacky, and wouldn't work if the user used something like `.git*`. 

What I _really_ want is a way to add additional rules at the same level as the .gitignore file, at lower priority than the override rules. Or, having native support for .git would be cool too. :)

I'm looking forward to seeing your redesign.

---

_Comment by @tmccombs on 2024-01-08 08:52_

After a bit of experimentation, implementing an `--include` option that is able to re-include things that have been excluded by other rules, also doesn't work as I hoped. It appears that if I add a non-negated rule, then the results will only include directories, and files that match the included patterns.

---

_Comment by @BurntSushi on 2024-01-08 16:28_

> However, due to the way overrides are implemented, `!.git` has higher priority than `.git` in the overrides.

That's not quite what's happening. The issue is one of whitelist/blacklist semantics. Namely, if all you have are blacklist rules, then a path passes the filter so long as it doesn't match anything in the blacklist. But if you have any whitelist rules, then a path passes the filter only when it both matches a rule _and_ the _last_ rule it matches is a whitelist rule. That applies when you have 0 or more blacklist rules.

This is easier with examples (using this repo):

```
$ rg burntsushi -l --hidden
GUIDE.md
CHANGELOG.md
README.md
tests/regression.rs
FAQ.md
.git/rr-cache/347ea8452dc5a9833914ef7fa86e08d8c618a57a/postimage
.git/rr-cache/347ea8452dc5a9833914ef7fa86e08d8c618a57a/preimage

$ rg burntsushi -l --hidden -g '!/.git/'
README.md
FAQ.md
CHANGELOG.md
tests/regression.rs
GUIDE.md

$ rg burntsushi -l --hidden -g '!/.git/' -g '/.git/'
rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
Running with --debug will show why files are being skipped.

$ rg burntsushi -l --hidden -g '!/.git/' -g '/.git/' -g '/.git/**'
.git/rr-cache/347ea8452dc5a9833914ef7fa86e08d8c618a57a/postimage
.git/rr-cache/347ea8452dc5a9833914ef7fa86e08d8c618a57a/preimage
```

The second to last example is the interesting one. The issue is that there is one whitelist pattern, so the only thing ripgrep will search are things matching `/.git/`. But the only thing that `/.git/` matches is a directory. But not anything inside that directory. So you need the extra `/.git/**` rule.

Perhaps this will help with your use case.

> I still think this is a reasonable default to have (and maybe it would make sense for the ignore crate itself to do this?)

I don't think so. I find this to be surprising behavior personally. If I ask to see hidden files, I want to see all of them, unless I've explicitly configured it to do otherwise.

For example, another approach here is `echo /.git/ >> .ignore` (or `echo /.git/ >> .fdignore`). That rule will suppress `/.git/` when searching hidden folders. And users can opt into it.

---
