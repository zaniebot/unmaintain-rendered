```yaml
number: 1456
title: Globs and filepath depth
type: issue
state: closed
author: wcpines
labels: []
assignees: []
created_at: 2020-01-08T19:06:35Z
updated_at: 2020-01-24T11:37:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1456
synced_at: 2026-01-12T16:13:23Z
```

# Globs and filepath depth

---

_@wcpines_

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

`macos 10.5.1`

#### Describe your question, feature request, or bug.

NB: There have been questions around this topic before, but I haven't been able to find the answer in past issues.  Apologies if I missed something!

`rg -g '*glob*' ` does not match against filespaths of any depth

In plain english, I want to search files whose path contains `foo` and whose content contains the pattern `.*bar$`

#### Expected behavior 

My understanding is that I should be able to accomplish this with 

`rg -g  '*foo*' .*bar$`

As a concrete example (though I've removed the code and only returned result counts) 

```sh
rg -g '*ulia*' titleize\|titlecase | wc -l
# => 0
```
^ I expect to get 3. I can achieve this with the following: 

```sh
rg --files | rg ulia | xargs rg -l titleize\|titlecase | wc -l
# => 3
```

I think I expect the globbing to work somewhat similarly to the `ag` interface, e.g. 
` ag -G ulia "titleize|titlecase"`

Perhaps I don't understand how unix globs work with nested file paths.  

Thanks for your time!

---

_Comment by @wcpines on 2020-01-09 18:15_

Possibly related: https://github.com/BurntSushi/ripgrep/issues/1442

More details in the docs for globs would be great!

---

_Comment by @BurntSushi on 2020-01-09 18:23_

Thanks for the report. Unfortunately, I don't think I see a reproducible test case here. Just trying in the ripgrep repo seems to work just fine:

```
$ rg fixed-strings -g '*args*'
src/args.rs
690:            if self.is_present("fixed-strings") {
1366:    /// Note that if -F/--fixed-strings is set, then all patterns will be
1420:    /// if -F/--fixed-strings is set.
1429:    /// if -F/--fixed-strings is set.
1456:    /// -F/--fixed-strings flag is set. Otherwise, the pattern is returned
1459:        if self.is_present("fixed-strings") {
```

---

_Comment by @BurntSushi on 2020-01-09 18:23_

#1442 is different. #1442 is providing a glob pattern that matches a directory which then expects ripgrep to match everything _inside_ that directory as well. But the glob pattern is also applied to everything inside the directory as-is, and it correctly doesn't match.

---

_Comment by @wcpines on 2020-01-09 20:29_

I think what I'm looking for is directory matches as well. Using the source tree again:

( A )
```sh
==$ rg -l -g '*src*' LineIter | wc -l
       0
```

( B )
```sh
==$ rg --files | rg src | xargs rg -l LineIter | wc -l
       4
```

I think (A) is not how you intend `rg` to be used, or at least not in order to achieve (B). But I think this is a common mistake for folks coming from `ag`. I'm hoping to better understand how to achieve something like (B) with the `-g` flag.  


---

_Comment by @BurntSushi on 2020-01-09 20:36_

@wcpines Ah I see. The key thing you're missing is that, in globs, wildcards like `*` often do not match path separators such as `/`. You can see that `grep` behaves the same:

```
$ grep -r LineIter -l --include '*src*'
$
```

 Usually, the way to do what you want is with recursive globs:

```
$ rg LineIter -l -g '**/src/**'
grep-printer/src/util.rs
grep-searcher/src/sink.rs
grep-searcher/src/lib.rs
grep-searcher/src/lines.rs
```

Where `**/src/**` basically says, "match any file path where `src` is a component in that path."

For `ag`, it uses regexes, which often do not have the same "wildcards do not match path separators" restriction. ripgrep does not support filtering files via regex. So if you want that, then you'll want to use `xargs`.

---

_Comment by @wcpines on 2020-01-09 22:31_

If it's part of the path OR the filename, my understanding is to either 
- pass multiple globs ( `-g '*src*' -g '**/src/**'`)
- use the pipeline + xargs approach in (B)

Is this correct?

Also, thanks so much for your time and help here.  (Feel free to close this issue out!) 



---

_Comment by @BurntSushi on 2020-01-09 23:10_

@wcpines Almost. Just `-g '**/src/**'` is enough, since that will match a file named `src`. The key is that `**/src/**` matches any path that has `src` as part of its name. If you want to write a glob that matches `src` literally anywhere, e.g., even in `foo/foosrcfoo/foo`, then I guess you would need `**/*src*/**`.

---

_Closed by @BurntSushi on 2020-01-09 23:10_

---

_Comment by @wcpines on 2020-01-10 01:33_

Thanks again!



---

_Comment by @balki on 2020-01-24 03:07_

> @wcpines Almost. Just `-g '**/src/**'` is enough, since that will match a file named `src`. The key is that `**/src/**` matches any path that has `src` as part of its name. 

@BurntSushi The glob works with `-g` but not when added as a type using `--type-add`. Is there a way to add as a type?

```
% mkdir -p foo/{src,test}
% echo "hello" > foo/src/main.c
% echo "hello" > foo/test/main.c
% rg -g "**/src/**" hello 
foo/src/main.c
1:hello
% rg hello 
foo/test/main.c
1:hello

foo/src/main.c
1:hello
% rg --type-add "src:**/src/**" -t src hello 
% 

```



---

_Comment by @BurntSushi on 2020-01-24 11:37_

@balki Yes, that is intended behavior. "file types" don't apply to directories: https://github.com/BurntSushi/ripgrep/blob/98de8d248a677bbdb5ab46f53a58ceabbc24a2df/ignore/src/types.rs#L503-L507

---
