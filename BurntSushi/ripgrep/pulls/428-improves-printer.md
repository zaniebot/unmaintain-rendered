```yaml
number: 428
title: Improves Printer
type: pull_request
state: merged
author: kpp
labels: []
assignees: []
merged: true
base: master
head: improve_printer
created_at: 2017-03-30T19:27:57Z
updated_at: 2017-03-31T19:53:46Z
url: https://github.com/BurntSushi/ripgrep/pull/428
synced_at: 2026-01-12T18:23:13Z
```

# Improves Printer

---

_@kpp_

I added coloring for columns and separators. Also I fixed some bugs, when paths in context were not colored.

![Image of usage](http://oi63.tinypic.com/2zixans.jpg)

---

_Review comment by @BurntSushi on `src/app.rs`:257 on 2017-03-30 19:38_

These docs also need to be added to `doc/rg.1.md` and the `doc/rg.1` file needs to be regenerated. (If it's hard for you to install pandoc, then I can do the regeneration.)

---

_Review comment by @BurntSushi on `src/printer.rs`:222 on 2017-03-30 19:39_

Why change the coding style here? I much prefer early returns.

---

_Review comment by @BurntSushi on `src/printer.rs`:223 on 2017-03-30 19:40_

The previous code called out the borrowing restriction, but you removed that comment and added a `clone`. The whole point of the previous code was to avoid the `clone`. ;-)

---

_Review comment by @BurntSushi on `src/args.rs`:734 on 2017-03-30 19:41_

Please see #377, which asks for colors on column numbers. What's the use case? Similarly for separators.

---

_Review comment by @BurntSushi on `src/printer.rs`:294 on 2017-03-30 19:45_

I think the braces in your closure here are superfluous.

---

_Review comment by @BurntSushi on `src/printer.rs`:312 on 2017-03-30 19:45_

I think the braces in your closure here are superfluous.

---

_Review comment by @BurntSushi on `src/printer.rs`:363 on 2017-03-30 19:45_

I think the braces in your closure here are superfluous.

---

_Review comment by @BurntSushi on `src/printer.rs`:322 on 2017-03-30 19:45_

I think the braces in your closure here are superfluous.

---

_Review comment by @BurntSushi on `src/printer.rs`:405 on 2017-03-30 19:47_

This introduces an additional allocation that wasn't there before, probably as a result of refactoring `path_to_str_native` to return a slice of bytes. Maybe it should return a cow instead?

---

_Review comment by @BurntSushi on `src/printer.rs`:363 on 2017-03-30 19:52_

OK, this happens a lot. I'll stop picking on it.

---

_@BurntSushi requested changes on 2017-03-30 19:54_

I think I'm mostly in favor of the changes here, although I noted a couple places where an additional allocation was added.

If you think you fixed bugs in the printer, then I think we need at least one regression test for each bug.

Finally, I'm not totally sold on adding colors for columns and separators. It seems a bit much, although, I suppose it's not a big deal. See #377.

---

_@kpp reviewed on 2017-03-30 20:28_

---

_Review comment by @kpp on `src/args.rs`:734 on 2017-03-30 20:28_

The use case is to improve readability. Color helps humans to orient. See attached image in the description of PR

---

_@BurntSushi reviewed on 2017-03-30 20:31_

---

_Review comment by @BurntSushi on `src/args.rs`:734 on 2017-03-30 20:31_

Right, but if you read #377, then I'm specifically calling into question why humans are reading column numbers, especially when the match itself is colored.

---

_@kpp reviewed on 2017-03-30 20:33_

---

_Review comment by @kpp on `src/printer.rs`:222 on 2017-03-30 20:33_

U r the boss.

---

_Review comment by @kpp on `src/args.rs`:734 on 2017-03-30 20:39_

I don't know =( I wanted to color everything in the output...

---

_Review comment by @kpp on `src/printer.rs`:405 on 2017-03-30 20:42_

https://doc.rust-lang.org/std/primitive.slice.html#method.to_vec

The above `let mut path = path.to_vec();` was that allocation. I replaced for with map. No additional allocations are made by this code.

---

_Review comment by @kpp on `src/printer.rs`:225 on 2017-03-30 20:46_

I thought that possibility to color it overcomes 1 additional allocation?

---

_@kpp reviewed on 2017-03-30 20:46_

---

_@kpp reviewed on 2017-03-30 20:55_

---

_Review comment by @kpp on `src/app.rs`:257 on 2017-03-30 20:55_

I will update `doc/rg.1.md`. There is only `pandoc=1.16.0.2` in my system, so do the regeneration on your on, please.

---

_@BurntSushi reviewed on 2017-03-30 20:55_

---

_Review comment by @BurntSushi on `src/printer.rs`:405 on 2017-03-30 20:55_

D'oh. I missed the `to_vec` in the old code.

---

_@BurntSushi reviewed on 2017-03-30 20:56_

---

_Review comment by @BurntSushi on `src/printer.rs`:225 on 2017-03-30 20:56_

I don't think those are our only choices. You should be able to add color without additional allocations. The code just might be a bit more circuitous.

---

_@BurntSushi reviewed on 2017-03-30 20:57_

---

_Review comment by @BurntSushi on `src/args.rs`:734 on 2017-03-30 20:57_

I think I'd prefer if you backed out the addition of `column` and `separator` color styles. Whether we add that or not should really be decided in #377.

---

_@kpp reviewed on 2017-03-30 21:02_

---

_Review comment by @kpp on `src/printer.rs`:225 on 2017-03-30 21:02_

Will you help me? I am a rust newbie)

---

_@kpp reviewed on 2017-03-30 21:03_

---

_Review comment by @kpp on `src/args.rs`:734 on 2017-03-30 21:03_

But... the pic... prettier... =(

---

_@BurntSushi reviewed on 2017-03-30 21:05_

---

_Review comment by @BurntSushi on `src/printer.rs`:225 on 2017-03-30 21:05_

The problem is that `self.separator` borrows `self` mutably, so you can't also send in a reference to `self.context_separator`. There are a few ways to solve this, but the simplest is to do what the old code did: duplicate the code and inline the call to `separator` manually.

---

_@BurntSushi reviewed on 2017-03-30 21:07_

---

_Review comment by @BurntSushi on `src/args.rs`:734 on 2017-03-30 21:07_

More features means more code that I need to maintain. For example, if we ever added code to auto-detect the best color settings (which seems like something Windows users might appreciate), then the more color settings we have, the harder that code will be to write.

Features need to be decided upon carefully. Once they're added, we can essentially never remove them. Sorry.

---

_Comment by @kpp on 2017-03-30 21:28_

See how it looks without colors for column and separator:

![Image of example](http://oi67.tinypic.com/2lxdxsi.jpg)

It is easy to mix up column `:22:` with the text itself.

---

_Comment by @BurntSushi on 2017-03-30 21:38_

I'm not debating whether colors makes things easier to read. ripgrep has colors already, so I obviously see the utility. What I'm trying to ascertain is whether it's worth it to add more colors. I'm trying to do that by asking for use cases for reading column numbers. I don't understand why a human would find a column number to be useful information on a regular basis. Please see the discussion in #377. 

---

_Comment by @kpp on 2017-03-30 22:23_

@BurntSushi +regressions

---

_Comment by @kpp on 2017-03-31 18:19_

@BurntSushi is there any unresolved question about this PR? 

---

_@BurntSushi approved on 2017-03-31 18:44_

---

_Merged by @BurntSushi on 2017-03-31 18:44_

---

_Closed by @BurntSushi on 2017-03-31 18:44_

---

_Comment by @BurntSushi on 2017-03-31 18:44_

Looks good, thank you!

---

_Renamed from "Improves Printer, adds ColorSpecs for column and separator" to "Improves Printer" by @kpp on 2017-03-31 19:52_

---

_Branch deleted on 2017-03-31 19:52_

---
