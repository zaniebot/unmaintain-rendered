```yaml
number: 435
title: "Use child process worker to gracefully kill it on Crtl-C. Closes #281"
type: pull_request
state: closed
author: DoumanAsh
labels: []
assignees: []
base: master
head: sub_child
created_at: 2017-04-02T15:04:21Z
updated_at: 2017-07-21T10:50:04Z
url: https://github.com/BurntSushi/ripgrep/pull/435
synced_at: 2026-01-12T18:23:13Z
```

# Use child process worker to gracefully kill it on Crtl-C. Closes #281

---

_@DoumanAsh_

This PR should provide a graceful way to clean-up after sudden interruption by user.
It relies on libary `clonablechild` to provide way to wait and kill child in parallel
I looked briefly at code but haven't found anything bad with it.

Decided to go with setting particular environment variable instead if hidden variable.
I believe we can choose careful variable that could ensure more or less almost 100% that there will be no collision.
This way we do not need to bother correcting clap parsing and just have modification in main function.

I would appreciate help in testing on linux though

cc @BurntSushi @seabadger

**UPD**: For now it seems to bring some issues to tests as it is always reseting terminal color and therefore the extra character at the end that breaks tests

---

_@tiehuis reviewed on 2017-04-03 05:40_

---

_Review comment by @tiehuis on `src/main.rs`:91 on 2017-04-03 05:40_

Removing this reset will stop the control characters being printed which are causing your tests to fail.

---

_@tiehuis reviewed on 2017-04-03 05:42_

---

_Review comment by @tiehuis on `src/main.rs`:79 on 2017-04-03 05:42_

Pretty sure you will want to use `env::args_os` here instead of `env::args` so that invalid unicode is still passed properly as an argument. Else, this will fail the `regression_210` test.

---

_@DoumanAsh reviewed on 2017-04-03 06:12_

---

_Review comment by @DoumanAsh on `src/main.rs`:91 on 2017-04-03 06:12_

The point of commit is to add this reset to gracefully clean-up colors if necessary. There is no point in this commit otherwise.
I was waiting for @BurntSushi to discuss how to approach tests

---

_@DoumanAsh reviewed on 2017-04-03 06:12_

---

_Review comment by @DoumanAsh on `src/main.rs`:79 on 2017-04-03 06:12_

Ok, thanks for pointing it out.

---

_Comment by @BurntSushi on 2017-04-09 12:37_

@DoumanAsh Unfortunately, I think there are problems with this PR as-is, and I'm not sure that they are easy to solve.

First and foremost, if ripgrep exits successfully, then there the term colors should *never* be reset. Namely, if ripgrep runs successfully to completion, then it will have reset the colors on its own already.

Secondly, we shouldn't be resetting the colors if we didn't actually use them. This probably requires parsing `argv` and asking whether colors are actually going to be enabled or not, which might be tricky. Alternatively, perhaps we could emit colors *only* if the child is killed by `^C`, which I think should be apparent in the exit code?

---

_Comment by @DoumanAsh on 2017-04-10 05:08_

>First and foremost, if ripgrep exits successfully, then there the term colors should never be reset. Namely, if ripgrep runs successfully to completion, then it will have reset the colors on its own already.

Agree and corrected.

>This probably requires parsing argv and asking whether colors are actually going to be enabled or not, which might be tricky. 

Yes, at first i thought about it but then it leads us to parse CLI arguments twice...
I thought there would be no problem to just reset color anyway as it wouldn't affect terminal overall.

> Alternatively, perhaps we could emit colors only if the child is killed by ^C, which I think should be apparent in the exit code?

I'm not sure whether there is any special exit code for such cases?
WinAPI docs do not mention anything about it, but in my tests Crtl-C always triggers to return code 1.
I'm not sure how reliable it is.

**UPD**: We can though set flag in our handler of `^C` and determine through it whether user requested termination or not.
But it would require first to update library code as it accepts only `Fn` closures.

---

_Comment by @BurntSushi on 2017-04-10 11:40_

> But it would require first to update library code as it accepts only Fn closures.

That's not quite sufficient. Even if it accepted a `FnMut` (which it could, I don't know why it doesn't), then the `'static` bound will likely thwart your efforts to mutate a reference from inside the closure. In particular, the handler is run on a different thread. Instead, you should use an [`AtomicBool`](https://doc.rust-lang.org/std/sync/atomic/struct.AtomicBool.html), which will work in a `Fn + 'static` bound.

---

_Comment by @DoumanAsh on 2017-04-12 04:34_

@BurntSushi So how would you like to see solution to this problem?
As per your suggestion i moved reset to be done only when return code is non zero.
But what about initial parsing of arguments you suggested? Should i also do it? I'm a bit reluctant to do double parsing of arguments because there is no harm in resetting colors even if user requested no colors.

Is there something else you would like to point out?

---

_Comment by @BurntSushi on 2017-04-12 11:13_

@DoumanAsh I would like to experiment with @jethrogb's ideas in #281. I will get around to it eventually, but until then, I'd request we just hit pause on this PR unfortunately.

---

_Comment by @BurntSushi on 2017-04-12 11:14_

> because there is no harm in resetting colors even if user requested no colors

No, that's false. If a terminal doesn't support escape codes, then I imagine the escape code might show up as visible to the end user. We shouldn't be printing color codes if the end user requested that we shouldn't.

---

_Comment by @DoumanAsh on 2017-07-21 04:48_

Just wondering do you need this PR?

---

_Comment by @BurntSushi on 2017-07-21 10:50_

I think it's probably fine to close this. I almost certainly want to move in a different direction.

---

_Closed by @BurntSushi on 2017-07-21 10:50_

---
