```yaml
number: 111
title: Max depth option
type: pull_request
state: merged
author: gsquire
labels: []
assignees: []
merged: true
base: master
head: max-depth
created_at: 2016-09-27T03:56:47Z
updated_at: 2016-09-27T23:47:31Z
url: https://github.com/BurntSushi/ripgrep/pull/111
synced_at: 2026-01-12T18:23:12Z
```

# Max depth option

---

_@gsquire_

Closes #109 


---

_Comment by @gsquire on 2016-09-27 05:06_

I'm not sure of how to write a unit test because I think I would just be testing `walkdir` if I did :)

Let me know if you are more clever than me in this regard.


---

_Review comment by @BurntSushi on `src/args.rs` on 2016-09-27 11:08_

Could you add this flag to `doc/rg.1.md` and run the convert script to create a new `doc/rg.1` file?


---

_@BurntSushi reviewed on 2016-09-27 11:08_

---

_@BurntSushi reviewed on 2016-09-27 11:09_

---

_Review comment by @BurntSushi on `src/args.rs` on 2016-09-27 11:09_

Maybe clarify that a value of `0` searches the current directory only.


---

_@BurntSushi reviewed on 2016-09-27 11:10_

---

_Review comment by @BurntSushi on `src/args.rs` on 2016-09-27 11:10_

I think I like this better, stylistically:

```
let mut wd = WalkDir::new(path).follow_links(self.follow);
if let Some(maxdepth) = self.maxdepth {
    wd = wd.max_depth(maxdepth);
}
```


---

_@BurntSushi reviewed on 2016-09-27 11:10_

---

_Review comment by @BurntSushi on `src/args.rs` on 2016-09-27 11:10_

I think we want to allow a value of `0` to mean "search current directory." In this case, `maxdepth` should be an `Option<usize>`, which docopt will handle correctly.


---

_Comment by @BurntSushi on 2016-09-27 11:11_

@gsquire Could you write an integration test please? It would be good to follow the pattern I've been using whenever I introduce new command line flags: https://github.com/BurntSushi/ripgrep/blob/master/tests/tests.rs#L781-L820

Other than that and a few nits, this looks good, thanks!


---

_Comment by @gsquire on 2016-09-27 17:14_

I rebased onto master and squashed. Is that the style of integration test you expected?


---

_@BurntSushi reviewed on 2016-09-27 18:16_

---

_Review comment by @BurntSushi on `tests/tests.rs` on 2016-09-27 18:16_

Could you add a file `one/foo` with `far` in it, and assert that you get results? That seems like a slightly better test since it makes sure search still works, but that it also respects the depth setting.


---

_Comment by @BurntSushi on 2016-09-27 18:16_

@gsquire That's perfect! I had one last suggestion and then this is ready to go. Thanks so much!


---

_Comment by @gsquire on 2016-09-27 18:40_

@BurntSushi Doing this causes the test to fail:

``` rust
wd.create_dir("one");
wd.create("one/foo", "far");
wd.create_dir("one/too");
wd.create("one/too/many", "far");

cmd.arg("--maxdepth").arg("1");

let lines: String = wd.stdout(&mut cmd);
let expected = path("one/pass:far\n");

assert_eq!(lines, expected);
```

But changing the depth to 2 passes...am I misunderstanding the depth used in `walkdir`?


---

_Comment by @BurntSushi on 2016-09-27 19:03_

I goofed. Looks like your initial implementation is correct. I was off by
one. Could you just make sure we are consistent with GNU find? The docs
will need to be updated too. Sorry for short response. On mobile.

On Sep 27, 2016 2:40 PM, "Garrett Squire" notifications@github.com wrote:

> @BurntSushi https://github.com/BurntSushi Doing this causes the test to
> fail:
> 
> wd.create_dir("one");
> wd.create("one/foo", "far");
> wd.create_dir("one/too");
> wd.create("one/too/many", "far");
> 
> cmd.arg("--maxdepth").arg("1");
> let lines: String = wd.stdout(&mut cmd);let expected = path("one/pass:far\n");
> 
> assert_eq!(lines, expected);
> 
> But changing the depth to 2 passes...am I misunderstanding the depth used
> in walkdir?
> 
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/pull/111#issuecomment-249957714,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34tN10dEsKkF0JR2Cd86vipvZiHLBks5quWMCgaJpZM4KHO4u
> .


---

_Comment by @gsquire on 2016-09-27 19:51_

It's okay, just wanted to verify.

Quoting from the [GNU find man page](http://man7.org/linux/man-pages/man1/find.1.html):

> Descend at most levels (a non-negative integer) levels of directories below the starting-points. 
> -maxdepth 0 means only apply the tests and actions to the starting-points themselves.

So no need to update anything? I have to say, I thought having max depth of one and having a file in a directory below `.` would be found. This is why I asked you to confirm. No rush on this, I want to make sure I understand it correctly is all.


---

_@BurntSushi reviewed on 2016-09-27 20:36_

---

_Review comment by @BurntSushi on `tests/tests.rs` on 2016-09-27 20:36_

OK, so I think with `one/foo`, you do indeed need to set `maxdepth` to `2`. The argument, `.`, corresponds to depth `0`. `one` is depth `1` and `one/foo` is depth `2`.


---

_Comment by @BurntSushi on 2016-09-27 20:38_

@gsquire Ah OK. Looks good. I think just updating the test to make sure it both ignores a deeper directory and gets search results from a shallower directory should be sufficient. Also, I think the docs need to be updated. Probably saying "current directory" is wrong and we should say "starting points" like `find` does. Thanks for sticking with me!


---

_@gsquire reviewed on 2016-09-27 20:42_

---

_Review comment by @gsquire on `tests/tests.rs` on 2016-09-27 20:42_

Interesting, I thought the count applied to directories and not their contents. I'll update it :)


---

_@BurntSushi reviewed on 2016-09-27 20:45_

---

_Review comment by @BurntSushi on `tests/tests.rs` on 2016-09-27 20:45_

The count applies to whether to descend into a directory or not. At depth `0`, you don't descend ever, you just yield whatever was given. So a walkdir of `./` at depth `0` yields `./`. At depth `1`, you descend once, so that a walkdir of `./` at depth `1` yields `one`, but doesn't descend into `one`. Finally, at depth `2`, a walkdir of `./` yields `./one` and then `./one/too` and also `./one/foo`, but doesn't descend into `./one/too`.


---

_Comment by @gsquire on 2016-09-27 20:53_

Sweet, I know the phrasing is important. I updated that and the test. Hope it's good to go!


---

_@gsquire reviewed on 2016-09-27 20:55_

---

_Review comment by @gsquire on `tests/tests.rs` on 2016-09-27 20:55_

That makes sense, thanks for a detailed explanation. I suppose it's really a "tree" after all.


---

_Comment by @gsquire on 2016-09-27 23:43_

Looks like the build failure happened on master too...strange.


---

_Merged by @BurntSushi on 2016-09-27 23:46_

---

_Closed by @BurntSushi on 2016-09-27 23:46_

---

_Comment by @BurntSushi on 2016-09-27 23:46_

Yup this looks great. The failure looks unrelated. Thanks so much for doing this and putting up with me! :-)


---

_Branch deleted on 2016-09-27 23:46_

---

_Comment by @gsquire on 2016-09-27 23:47_

I'm happy to be able to contribute, you're a Rust machine :)

Congrats on this project!


---
