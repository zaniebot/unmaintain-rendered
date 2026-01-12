```yaml
number: 2606
title: "VSCode expected search behaviour is incompatible with Ripgrep's actual behaviour"
type: issue
state: closed
author: Diggsey
labels: []
assignees: []
created_at: 2023-09-08T17:52:40Z
updated_at: 2025-10-16T01:42:28Z
url: https://github.com/BurntSushi/ripgrep/issues/2606
synced_at: 2026-01-12T16:13:24Z
```

# VSCode expected search behaviour is incompatible with Ripgrep's actual behaviour

---

_@Diggsey_

See https://github.com/microsoft/vscode/issues/192045 for details.

This is really a VSCode bug, but it can't easily be fixed in VSCode without changes or new features in Ripgrep.

In summary, the `-g foo` option in Ripgrep overrides .gitignore, which causes VSCode's search functionality to return incorrect results, since VSCode allows selecting both "use .gitignore" *and* "filter to directory".

---

_Comment by @BurntSushi on 2023-09-08 18:10_

I'm not really sure what is to be done here. The entire point of `-g/--glob` is to override gitignore. Changing that doesn't seem viable. I get that from your specific example, it _looks_ like the `-g` is the broader criteria where as the `.gitignore` is the narrower one, and thus the broader filter should get overridden by the narrower one. But that's just one example and it's not clear how to turn that into a consistent UX.

I'll note that there are many open issues with respect to ignore, and you are not the first one to bring up the occasionally non-ideal interaction between `-g/--glob` and gitignore rules. I've said in a few issues that the entire filtering logic really needs to be completely re-thought to account for outstanding bugs and at least some new functionality. I'm not sure exactly when that's going to happen. I was hoping to get to it during my sabbatical, but that's looking less and less likely.

The main problem here is that ripgrep's filtering logic has a ton of surface area right now with oodles of little knobs to control its behavior in corner cases. (e.g., `--no-require-git`.) I don't think I can abide increasing that surface area much more, if at all, before I have a chance to sit down and think through how everything should fit together. There needs to be a cohesive design that takes all the new feedback into account since the original release of the `ignore` crate.

---

_Label `enhancement` added by @BurntSushi on 2023-09-08 18:10_

---

_Comment by @BurntSushi on 2023-09-08 18:12_

I'll note that, VS Code aside, the "proper" way to do something like this should be to do `rg pattern foo` and not `rg pattern -g foo`. But I'm sure VS Code uses the latter for good reason.

In any case, there is probably also some stuff to figure out on the VS Code side here as well. But it's not a goal for ripgrep to start solving UX problems that VS Code has, and "upstream should just figure it out and add a new feature" is not really how I work.

---

_Comment by @Diggsey on 2023-09-08 23:08_

> The entire point of -g/--glob is to override gitignore. Changing that doesn't seem viable.

Yeah I don't see this as a *bug* in Ripgrep at all, it's just working as designed.

> But it's not a goal for ripgrep to start solving UX problems that VS Code has, and "upstream should just figure it out and add a new feature" is not really how I work.

Of course. I'm a bit disappointed with the way the VSCode maintainers have just closed the issue :(. I wasn't sure if you personally had been involved at all in getting Ripgrep into VSCode but it sounds like not...

> There needs to be a cohesive design that takes all the new feedback into account since the original release of the ignore crate.

Yeah this sounds like it would be a big improvement.

Perhaps a system like this:

```rust
trait PathMatcher {
    fn matches_path(&self, path: &Path) -> bool;
    fn can_skip_directory(&self, path: &Path) -> bool { false }
}

struct GitIgnoreMatcher;
impl PathMatcher for GitIgnoreMatcher { .. }

struct RegexMatcher;
impl PathMatcher for RegexMatcher { .. }

struct NotMatcher;
struct AllMatcher;
struct AnyMatcher;
... more ...
```

If interacting with Ripgrep procedurally, then you would pass in a structured (eg. json) value describing a set of matchers, so you could say "all(RegexMatcher, GitIgnoreMatcher)` or similar.

If interacting with Ripgrep manually then there could be a more friendly set of command line args that ultimately just get mapped to the structured representation.

If interacting with Ripgrep as a crate, you could even implement your own custom PathMatchers.

---

_Comment by @BurntSushi on 2023-09-08 23:26_

> I wasn't sure if you personally had been involved at all in getting Ripgrep into VSCode but it sounds like not...

I didn't do any of the work, but I did work with them during the initial push to get ripgrep included to land some stuff that they found very useful. Things like UTF-16 support and the `--json` switch. But there was a back-and-forth.

Thanks for the code snippet. I don't have the context to really evaluate it at the moment. The main challenge is in collecting all of the various design constraints outstanding (not just this one), writing them down in one place and then figuring out how to build something that deals with it gracefully.

---

_Comment by @BurntSushi on 2025-10-16 01:42_

I've re-read this issue, and it's hard for me to pull out anything concretely actionable here. So I'm going to close this for now.

---

_Closed by @BurntSushi on 2025-10-16 01:42_

---

_Label `enhancement` removed by @BurntSushi on 2025-10-16 01:42_

---
