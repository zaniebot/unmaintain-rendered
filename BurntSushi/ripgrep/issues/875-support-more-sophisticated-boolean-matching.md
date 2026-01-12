```yaml
number: 875
title: support more sophisticated boolean matching operations
type: issue
state: open
author: mqudsi
labels:
  - enhancement
  - question
  - icebox
assignees: []
created_at: 2018-04-03T00:16:07Z
updated_at: 2025-10-30T09:39:50Z
url: https://github.com/BurntSushi/ripgrep/issues/875
synced_at: 2026-01-12T16:13:22Z
```

# support more sophisticated boolean matching operations

---

_@mqudsi_

With the new convention to use the capitalized version of a short flag to indicate the opposite it's too bad that `-E` is already used to mean `--encoding`, as I would like to suggest an "inverse pattern" mode where only lines/words (depending on other parameters as normal) matching pattern `e` but not matching pattern `E` are included in the result set.

Andrew, I know you are loathe to add more `!` support but given the pre-existing `-E`, perhaps a `-e !PATTERN`?

---

_Comment by @BurntSushi on 2018-04-03 00:29_

The name of the flag is really not the interesting part of this feature request. The interesting part is the request to support more sophisticated boolean tests.

I think if we were to decide to do this, then it needs to be part of a larger story that encompasses more sophisticated expressions. We also need to address the fact that, today, we can actually express quite a bit, but it requires piping. Namely, piping permits expressing "and". Piping plus the `-v` flag permits any arbitrary boolean expression you might want. For example, `rg foo | rg -v bar` says "show lines matching `foo` but do not contain `bar`," which is *exactly* your feature request.

`git grep` has support for this via `-not`, `-and` and `-or`. I don't know if I'm willing to add this to ripgrep. There *must* be a point at which we say, "piping is good enough."

An alternative way to implement this feature is in the regex engine itself (since intersection and complement are available as operations on regular languages), but this is extremely non-trivial to do.

I try not to speak in absolutes, but, "I don't want to add anything else that uses `!` in a shell" is as close to an absolute that I can get. Let's drop that idea.

---

_Label `question` added by @BurntSushi on 2018-04-03 00:29_

---

_Label `icebox` added by @BurntSushi on 2018-04-03 00:29_

---

_Label `enhancement` added by @BurntSushi on 2018-04-03 00:29_

---

_Renamed from "Support inverse -e flag" to "support more sophisticated boolean matching operations" by @BurntSushi on 2018-04-03 00:31_

---

_Comment by @mqudsi on 2018-04-03 00:31_

I understand completely. I currently pipe (to `grep`, I didn't realize I could pipe to `rg` itself!) but was wondering from a performance perspective basically about using the regex engine itself to optimize the search with the additional boolean constraints.

Thanks.

---

_Comment by @BurntSushi on 2018-04-03 00:40_

> but was wondering from a performance perspective basically about using the regex engine itself to optimize the search with the additional boolean constraints.

Well, the "best" way is to, as I hinted at, build complement and intersection into the regex engine. But as I said, this is extremely non-trivial to do efficiently. If we were to implement this, then we'd need an algorithm that selects the (attempted) optimal matching path given all of the boolean conditions. e.g., if you said "x and not y and not z," then ripgrep would search for `x` and only apply the `y` and `z` blacklist on matches to filter them out. If you had `x or y or z`, then ripgrep would, as it does today, combine them into one regex joined by `|`. If you had `not x and not y and not z`, then ripgrep behave as it would today if you ran `rg -v x` and then use the `y` and `z` blacklists to filter our matches. If you had `not x or not y or not z`, then ripgrep could behave as it does today if you ran `rg -v 'x|y|z'`. And so on...

It is plausible that this would result in a performance improvement. But you can't just throw that out there as a benefit and expect it to stick. :-) Performance does not exist in a vacuum. Pipelines tend to be constructed in a way that iteratively reduces the search space, which in turn makes performance less and less of an issue. The interesting bits are probably pipelines that start with an inverted match on a rarely occurring pattern, which would not reduce the search space much. Regardless, I personally find this to be a somewhat flimsy motivation for a feature like this unless someone can convince me otherwise. IMO, if we add a feature like this, it should be primarily for the UX.

---

_Comment by @kenorb on 2018-04-11 00:59_

Example of using `git grep` with AND patterns:

    git grep -e pattern1 --and -e pattern2 --and -e pattern3


---

_Comment by @kenorb on 2018-04-11 01:17_

Example of AND operation using [Rust's regex engine](https://github.com/rust-lang-nursery/regex):

    rg -N '(?P<p1>.*pattern1.*)(?P<p2>.*pattern2.*)(?P<p3>.*pattern3.*)' file.txt

---

_Comment by @BurntSushi on 2018-04-11 01:28_

@kenorb That's presumably not the same as what `git grep` does. `git grep -e pattern1 --and -e pattern2` will match `pattern2pattern1` but `(.*pattern1.*)(.*pattern2.*)` will not. The standard way to perform "and" queries in ripgrep is with piping, as I mentioned above in my comment.

---

_Comment by @peterbe on 2018-06-06 12:05_

I quite like the simplicity and "natural feel" of using `rg foo | rg bar` to do the equivalent of `git grep -e foo --and -e bar`. The only significant difference is the color. 

**`git grep -e foo --and -e bar`**
<img width="757" alt="screen shot 2018-06-06 at 8 03 13 am" src="https://user-images.githubusercontent.com/26739/41037089-32d8aed8-6960-11e8-8fa1-72828c827aef.png">

**`rg string | rg query`**
<img width="725" alt="screen shot 2018-06-06 at 8 04 42 am" src="https://user-images.githubusercontent.com/26739/41037120-4c81bdf2-6960-11e8-9dfc-95e4165dca55.png">

See, no highlight of the word `string` in the `rg` pipe. 

---

_Comment by @BurntSushi on 2018-06-06 12:10_

@peterbe You should be able to fix that by adding `--color always` to your first invocation of ripgrep. Not ideal of course.

---

_Comment by @peterbe on 2018-06-06 12:38_

I don't even know if it's possible with pipes but if you could know that that the next pipe is another `rg` the `--color always` could be on by default. One can dream.

---

_Comment by @elbaro on 2018-06-29 07:51_

Piping loses the file headers.

```
rg abc

a.txt
4: ...abc...xyz...
7: ...abc...

b.txt
3: ...abc...xyz...
```

```
rg abc | rg xyz

4: ...abc...xyz...
3: ...abc...xyz...
```


---

_Comment by @BurntSushi on 2018-06-29 09:04_

That example doesn't look right. It should retain file names not as headers but in each line in standard grep format.

---

_Comment by @elbaro on 2018-06-29 15:00_

Sorry my bad. It looks like this:
```
rg abc | rg xyz
a.txt: ...abc...xyz...
a.txt: ...abc...xyz...
b.txt: ...abc...xyz...
b.txt: ...abc...xyz...
```
Still hard to parse when there are many files.
I think it's an example where the built-in op can provide better UX than piping.

Another example is piping with -A or -B.

```
// want to print a line including "abc" and "xyz" with +- 3 lines
rg abc -A 3 -B -3 | rg xyz -A 3 -B 3  // not what we want
```


---

_Comment by @BurntSushi on 2018-06-29 15:08_

That's certainly part of an argument in favor of this, but I will not allow that argument to be used as a hammer. Taken to its logical conclusion, ripgrep should bundle every conceivable transform on its data. At some point, people need to become OK with piping ripgrep's output and dealing with the different format. Different people will have different opinions on where that line is drawn.

---

_Comment by @BatmanAoD on 2018-09-21 19:21_

I have definitely wished for an easy way to preserve headers when piping `rg` to `rg`. Maybe a flag for "header passthrough" would be useful on its own.

---

_Comment by @aldanor on 2019-01-07 11:09_

> I have definitely wished for an easy way to preserve headers when piping rg to rg. Maybe a flag for "header passthrough" would be useful on its own.

That would be nice but won't work in all cases. E.g., consider

```sh
rg -C5 foo | rg -v bar
```

Now the context lines around the matched lines in the first rg call are being matched by the second rg call and your output may end up being a bit of a mess and not what you might expect.

---

> IMO, if we add a feature like this, it should be primarily for the UX.

Looking at a few now-closed duplicate issues, what most people want is just "a and not b" with all of headers/context preserved which might make sense to special-case if that's much simpler that the general case.

---

_Comment by @amitbha on 2019-02-23 00:35_

Files looks like this:

a.txt
4: ...abc...
30: ...xyz...

b.txt
4: ...abc...
 .....
(no 'xyz' in content)

How to find files like a.txt with 'abc' and 'xyz' in different lines?

---

_Comment by @BurntSushi on 2019-02-23 04:13_

Use multiline search.

On Fri, Feb 22, 2019, 19:35 amitbha <notifications@github.com> wrote:

> Files looks like this:
>
> a.txt
> 4: ...abc...
> 30: ...xyz...
>
> b.txt
> 4: ...abc...
> .....
> (no 'xyz' in content)
>
> How to find files like a.txt with 'abc' and 'xyz' in different lines?
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/875#issuecomment-466595243>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34iFvSILtyapoZbiWQTX9675DE3n0ks5vQIzHgaJpZM4TEQ9s>
> .
>


---

_Comment by @amitbha on 2019-02-23 08:25_

> Use multiline search.
> […](#)
> On Fri, Feb 22, 2019, 19:35 amitbha ***@***.***> wrote: Files looks like this: a.txt 4: ...abc... 30: ...xyz... b.txt 4: ...abc... ..... (no 'xyz' in content) How to find files like a.txt with 'abc' and 'xyz' in different lines? — You are receiving this because you commented. Reply to this email directly, view it on GitHub <[#875 (comment)](https://github.com/BurntSushi/ripgrep/issues/875#issuecomment-466595243)>, or mute the thread <https://github.com/notifications/unsubscribe-auth/AAb34iFvSILtyapoZbiWQTX9675DE3n0ks5vQIzHgaJpZM4TEQ9s> .

Thanks for reply.
I tried `rg -U --multiline-dotall -e 'abc.*xyz`, the right files were found. But there were too many outputs like:
>  4: ...abc... 
5: xxxxx
6: xxxxx
...
29: xxxxx
30: ...xyz...

`rg -U --multiline-dotall -e 'abc.*xyz | rg abc`
No filename and line-numbers.

`rg -U --multiline-dotall -l -e 'abc.*xyz' | rg 'abc' -`
No result. How to read path from pipe?

`rg -U --multiline-dotall -l -e 'abc.*xyz' | while read line; do rg 'xyz' "$line"; done`
Almost done! But filenames are missing. 😔

`rg -U --multiline-dotall -l -e 'abc.*xyz' | while read line; do echo "$line"; rg 'xyz' "$line"; echo; done`
Done! 😌

---

_Comment by @BurntSushi on 2019-02-23 13:40_

Please skim the options in the man page. Use the -n and --with-filename
flags.

On Sat, Feb 23, 2019, 03:25 amitbha <notifications@github.com> wrote:

> Use multiline search.
> … <#m_6621645017383223918_>
> On Fri, Feb 22, 2019, 19:35 amitbha ***@***.***> wrote: Files looks like
> this: a.txt 4: ...abc... 30: ...xyz... b.txt 4: ...abc... ..... (no 'xyz'
> in content) How to find files like a.txt with 'abc' and 'xyz' in different
> lines? — You are receiving this because you commented. Reply to this email
> directly, view it on GitHub <#875 (comment)
> <https://github.com/BurntSushi/ripgrep/issues/875#issuecomment-466595243>>,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34iFvSILtyapoZbiWQTX9675DE3n0ks5vQIzHgaJpZM4TEQ9s
> .
>
> Thanks for reply.
> I tried rg -U --multiline-dotall -e 'abc.*xyz, the right files were
> found. But there were too many outputs like:
>
> 4: ...abc...
> 5: xxxxx
> 6: xxxxx
> ...
> 29: xxxxx
> 30: ...xyz...
>
> rg -U --multiline-dotall -e 'abc.*xyz | rg abc
> No filename and line-numbers.
>
> rg -U --multiline-dotall -l -e 'abc.*xyz' | rg -e 'abc' -
> No result. How to read path from pipe?
>
> rg -U --multiline-dotall -l -e 'abc.*xyz' | while read line; do rg -e
> 'xyz' "$line"; done
> Almost done! But filenames are missing.
>
> 😔
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/875#issuecomment-466628741>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34jwonl0CGHe9DS2PCPvcqLH8d2rFks5vQPr0gaJpZM4TEQ9s>
> .
>


---

_Comment by @amitbha on 2019-02-24 09:32_

`rg -U --multiline-dotall -l -e 'abc.*xyz' | while read line; do rg --with-filename 'xyz' "$line"; echo; done`
Got it! 
😌

---

_Comment by @BurntSushi on 2019-02-24 13:18_

Friendly note: the utility of this feature is not in question. More comments explaining how useful this is or the kinds of problems it solves that aren't solved well by the status quo aren't necessary. The key thing blocking this feature is the potentially immense complexity that it adds not only to the implementation, but to the UX. It requires serious design work first, and it's still not clear to me that this is a feature I want to add.

It is well known that `git grep` supports this stuff. If it does what you want, then just use that.

---

_Comment by @elazarl on 2019-12-04 14:23_

Please consider a utility `rg --compile-expr a -and b -and c` generates relevant DFA.

Usage something like `rg --dfa $(rg --compile-expr a -and -not b)`. This will seal complexity only in the `compile-expr` option. Rest UX will remain identical.

Also piping is problematic for huge files as data is being copied again for every pipe.

---

_Comment by @zachriggle on 2020-04-01 03:13_

Piping is also an issue when using e.g. `--heading`

---

_Comment by @BurntSushi on 2020-04-01 12:21_

@zachriggle That's already been mentioned.

---

_Comment by @hraban on 2020-05-20 11:39_

re --and, I'm not sure if this is blasphemy or even correct at all and I'm probably missing edge cases but we could demorgan it...

```
$ echo -e 'Hello, foo\nBye, baz\nHello, james\nHello, baz\nbaz likes yellow' | \
    rg --pcre2 '^(?!((?!.*baz.*$)|(?!.*ello.*$)))'
Hello, baz
baz likes yellow
```

for matching any line containing `baz` *and* `ello`. perhaps a useful stop-gap for anyone desperate for a work-around?

---

_Comment by @BurntSushi on 2020-05-20 12:30_

@hraban If you just want a simple and query, then I'd probably recommend just doing

```
$ echo -e 'Hello, foo\nBye, baz\nHello, james\nHello, baz\nbaz likes yellow' | rg baz | rg ello
```

With the downsides of course being that you lose the nice formatting and highlighting of `baz`.

---

_Comment by @sparrc on 2020-10-28 22:40_

I'm going to suggest that maybe this issue and https://github.com/BurntSushi/ripgrep/issues/473 should be two separate issues.

Personally I'm not that interested in using complex boolean or regex patterns with ripgrep. I just want to be able to specify multiple patterns. Perhaps this could just be specified with a new flag like 

```
rg --patterns "level=error" --patterns "requestID"
```

Maybe that's too simplistic, but I've been using rg nearly since it was started and I've never had any desire for anything besides a simple 'and' match on multiple patterns.

---

_Comment by @BurntSushi on 2020-10-28 22:57_

@sparrc Conceptually, you might be right. But in terms of implementation, I don't think there is much of a difference, so I'm treating them the same. Also, ripgrep _does_ have the ability to search multiple patterns (using the same exact flags as `grep`). It's just that it's a "or" match.

On top of that, the reason why just wanting "and" match is a little weird is because you can do it with pipelines: `rg level=error | rg requestID`. It's just that the UX isn't quite as good...

---

_Comment by @elazarl on 2020-10-29 07:15_

@BurntSushi it's not just the UX (which is a major, unfixable problem IMHO. UX issues are much more important than "real" bugs, say, 100% slowdown of some cases).

One of the main reasons for me to use `ripgrep`, and one of its advantages is speed, so I'm picking it when I'm searching large files. Using multiple pipes slows things down in some cases, as it copies the data, adds syscalls, etc.

This is not 100% the same search, and of course I picked a 3GB file with search terms appearing in most lines, but

```
$ time ./ripgrep-12.1.1-x86_64-unknown-linux-musl/rg DMOD.\*LOW dr_agg  >/dev/null

real    0m5.772s
user    0m5.017s
sys     0m0.754s
$ time ./ripgrep-12.1.1-x86_64-unknown-linux-musl/rg DMOD.\*LOW dr_agg  >/dev/null

real    0m5.749s
user    0m4.987s
sys     0m0.760s
$ time ./ripgrep-12.1.1-x86_64-unknown-linux-musl/rg DMOD dr_agg |./ripgrep-12.1.1-x86_64-unknown-linux-musl/rg LOW >/dev/null

real    0m6.330s
user    0m7.147s
sys     0m2.781s
$ time ./ripgrep-12.1.1-x86_64-unknown-linux-musl/rg DMOD dr_agg |./ripgrep-12.1.1-x86_64-unknown-linux-musl/rg LOW >/dev/null

real    0m6.168s
user    0m7.245s
sys     0m2.777s
```

1 second hardly matter, but it is not uncommon for me to search 300GB of file.

---

_Comment by @gd4c on 2020-11-04 20:07_

+1 from me for multiple "AND" searches

---

_Comment by @BurntSushi on 2020-11-04 20:13_

@gd4c Please don't post +1 comments. They are noise that makes it into my inbox. If you feel obligated to +1 something, then use GitHub's emoji reactions. See also: https://github.com/BurntSushi/ripgrep/issues/875#issuecomment-466774329

---

_Comment by @gd4c on 2020-11-05 06:42_

Sorry. I initially upvoted the initial post, but it wasn't what I was after (which kind of evolved into the thread). I just wanted to make it clear what I was thumbing up.

---

_Comment by @gd4c on 2020-11-05 06:51_

Continuing from #1149
> The implementation complexity of more sophisticated boolean matching is precisely my main argument against it. And it requires a very thorough specification of behavior.
> 
> And the upsides are limited. Yes, you can't get the "nice" output when using pipelines, but you can still use pipelines and the output is still serviceable.

Is it much trouble to ask you to give us an idea of what you have in mind?
I would imagine it like this:
- Run rg with first expression as usual.
- Instead of formatting file matches as headings, pass them as a list of input files for the second expression.
- repeat n times.
- Show the results of the last run as usual with color for the last match only (as if you only ran rg with just the last expression).

That would be enough for me, and I suspect many of the people above.

---

_Comment by @BurntSushi on 2020-11-05 11:56_

@gd4c `rg foo | rg bar` will only print lines that contain both `foo` and `bar`.

---

_Comment by @gd4c on 2020-11-05 17:13_

True, but without the easily readable formatting. 🙂
Regular `grep` can do that for the matter. It is just that `ripgrep` is nicer to use which drives this request – to increase the nice UX rather than include an otherwise impossible feature!

In any case, if you can find an easy way to do it, that'd be great. If you consider it too much trouble, there are (less nice) workaround we can use!

---

_Comment by @gd4c on 2020-11-05 22:18_

What's confusing?

Also, forgot to say that piping to rg searches lines in previous stdout, not the matching _files_!

---

_Comment by @BurntSushi on 2020-11-05 22:32_

The upsides and downsides here are well known. I've stated repeatedly what the problems are with `rg foo | rg bar`. They don't need to keep being repeated. So I'm confused at why you're rehashing things.

Adding this feature reflects _significant_ work. The first step is to come up with a comprehensive UX specification of behavior. _That_ would be useful. Further argumentation about _why_ ripgrep should have this feature is _not_ useful. It's just noise and it's just filling up my inbox.

I said about as much almost [a year and a half ago](https://github.com/BurntSushi/ripgrep/issues/875#issuecomment-466774329), so now I'm just repeating myself. And I'm confused at why I need to do it.

---

_Comment by @gd4c on 2020-11-05 23:02_

Sorry for the misunderstanding. I recognize that you understand its usefulness and that the issue is the complexity. I was just responding to your solution [above](#issuecomment-722332162).

You made a great tool and I am grateful!

Cheers!

---

_Comment by @zachriggle on 2020-11-06 01:54_

My current work-around for this is effectively use `rg -l` to find all OR'ed matches, and then pass off to git grep.

A statement that looks roughly like this:

```
$ my-grep -e foo --and -e bar --and --not '(' -e fizz -e buzz ')'
```

Gets translated roughly to:

```
rg -e foo -e bar -l -0 | xargs -0 git grep --threads 12 --no-index -e foo --and -e bar --and --not '(' -e fizz -e buzz ')'
```

There's a LOT of extra plumbing in my shell script to achieve better performance (e.g. don't have ripgrep search for expressions in an `--and --not ( -e fizz -e buzz )` block, but ultimately `rg -l -0 | xargs -0 git grep --no-index` works pretty effectively, and is much faster than `git grep` by itself if you make use of e.g. `rg` type filters (e.g. `rg -t c -t py`).

This also allows you to specify some `git grep` specific formatting, like `--show-function`, in addition to those that `rg` also supports like `--break --heading --line-number`.

---

_Comment by @bionicles on 2021-03-19 15:27_

> The first step is to come up with a comprehensive UX specification of behavior. That would be useful.

for UX on multiple patterns with boolean logic, 

what if users wrote a double quoted string with 'AND' , 'OR', 'NOT' in it , but only within quotes and in all caps after a certain flag. if necessary could specifically surround query patterns with single quotes or curly brackets to make it easier to pluck them out. 

if each query result is a set of matches, the logic can be a tree of set operations like intersection ("AND"), set union ("OR"), complement ("NOT") to get the results which meet the criteria

```py
# rg --logic "practitioner AND surgery"
r1, r2 = find(["practitioner", "surgery"], data)
result = intersection(r1, r2)

# rg --logic "patient AND (diabetes OR cancer) AND statin NOT lung cancer"
r1, r2, r3, r4, r5 = find(["patient", "diabetes", "cancer", "statin", "lung cancer"], data)
result = intersection(r1, union(r2, r3), r4).difference(r5)
```

could follow a specific order of operations such as listed here: [order of operations on sets](http://www.cwladis.com/math100/Lecture4Sets.htm#:~:text=Order%20of%20operations%20on%20sets%3A&text=Just%20like%20with%20numbers%2C%20we%20always%20do%20anything%20in%20parentheses,all%20equal%20in%20the%20order.) 

as for performance, that could still be single pass over the data (presumably) but would require a check for multiple queries at each point O(N), plus each set operation is (i believe) linear in the number of matches, and thus fairly fast, but this can add a whole new dimension to the capabilities of ripgrep, AND / OR / NOT / Parenthesis is easily interpretable

---

_Comment by @BurntSushi on 2021-03-19 16:02_

@bionicles It's a nice idea, but it's only a start. You're basically saying, "use a DSL." A DSL comes with its own complications. Your examples are simple because none of your queries are regexes. A DSL that embeds regexes means you'll need to handle escaping, and that could become quite annoying. A DSL also implies, to me, that it needs facilities for bulk-including regexes, like via the `-f` flag.

If I had time to implement this, my first step would be to look at `git grep` and write a specification based on its man pages and behavior. Then evaluate whether that specification could be improved.

---

_Comment by @bionicles on 2021-03-19 20:42_

eh, git grep syntax looks pretty bad. weird nested flags `--and --e thing1 --e thing2` ... it doesn't read like english

i just believe ripgrep could be a super fast offline-first alternative to algolia ...but perhaps regex could be surrounded by curlies? 

`rg --logic "{^[A-Z]+_SUSPEND$} OR rustacean"` 

if somebody needs to regex for curly brackets, or regex for regex patterns, they could use double curly brackets, or suck it up and use something else. and even if the logic matching didnt support regex at all, i'd use it, and it would be extremely useful in the vast majority of use cases where one doesn't need regex

the plan i described is unclear about the order of operations tho, I'd suggest 

parenthesis > OR > AND > NOT
because "or" is more inclusive

and just error if there's ambiguity about order of operations

another alternative would be a lispy syntax like 

`rg --logic "((simple english NOT dsl) AND regex)"`

---

_Comment by @bionicles on 2021-03-19 20:52_

to de-spec and simplify, just some way to do AND queries alone would be 80/20
aka, find files which match 3 different queries:
`rg --multi "patient" --multi "pacemaker" --multi "expired"`
no DSL required

---

_Comment by @zachriggle on 2021-03-19 21:23_

git grep's advanced Boolean expression should be the model for this. It allows nesting and grouped expressions.

git grep -e foo —and —not ( -e fizz -e buzz ) —and -e bar

Will match anything with “foo” and “bar” but exclude anything that contains “fizz” or “buzz”.  You can recursively nest things and make very complex queries this way.

Defining a new DSL for ripgrep seems unwise, when there is a battle-proven
interface with git-grep.

IMHO ripgrep should emulate git-grep’s CLI interface.

---

_Comment by @BurntSushi on 2021-03-19 21:27_

Right. It being an existing and very popular implementation of the idea gives it credibility. It should be our starting point. Note also though, that I said we may evaluate its specification.

---

_Comment by @zachriggle on 2021-03-20 00:31_

If I can make one VERY small suggestion that differs from the git-grep API, would be to replace (or permit both of) `'('` with `[` for expression grouping since the `(` requires quoting like `'('` or else the shell tries to do weird things with it.  The `[` and `]` are not treated specially and don't need quotes.

I use a wrapper script (company internal only, can't share sorry) around RipGrep and Git Grep to get the best of both worlds, and it does substitution of `[` with `'('` to make things easier to read.

---

_Comment by @HK47196 on 2022-03-22 08:19_

> @gd4c `rg foo | rg bar` will only print lines that contain both `foo` and `bar`.

It will also match lines that merely contain foo but have bar in the filename.
```
~ echo 'foo' > bar
~ rg foo | rg bar
bar:foo
```

---

_Comment by @kenorb on 2022-05-02 12:13_

I've got the following simple use case to share (useful when preprocessing Markdown text or Wikipedia articles).

So I'm trying to match using OR operator words which have special characters around it and print both combinations. I've tried the following:

```
$ echo "{{Foo}} [[Bar]]" | rg -o '\w+|\S+'
{{Foo}}
[[Bar]]
```

With the OR operation, I would expect something like this:

```
$ echo "{{Foo}} [[Bar]]" | tee >(rg -o '\w+') >(rg -o '\S+') >/dev/null | cat
{{Foo}}
[[Bar]]
Foo
Bar
```

I've tried with the following pattern file (to use with `-f`):

```
\w+|\S+
\S+
\w+
```
but it's the same result - it stops processing after the first match (I've tried adding `-e`, but it doesn't make any matches). I've also tried to specify multiply pattern files, or `--passthru '^|(\w+)|(\S+)' -o -r '$0'`, no luck.

---

_Comment by @BurntSushi on 2022-05-02 12:21_

@kenorb "or" already exists with `|`. It looks like what you want is an overlapping query, which we [already talked about](https://github.com/BurntSushi/ripgrep/discussions/2189). That's completely unrelated to boolean queries.

---

_Comment by @aboueleyes on 2022-05-26 13:50_

Is there a way to have nice formatting while querying 
```bash
$ rg foo | rg -v bar 
```

---

_Comment by @BurntSushi on 2022-05-26 14:39_

@aboueleyes No.

---

_Comment by @timotheecour on 2024-04-07 21:34_

I don't think this was mentioned earlier:
`rg foo | rg bar` isn't the same as `rg foo --and bar`, because `rg bar` will match also filenames with `bar`, so will have false positives

and the workaround `rg 'foo.*bar|bar.*foo'` doesn't scale well

---

_Comment by @hraban on 2024-04-10 16:00_

For context the primary reason I was looking for this functionality is because I use rg mostly in Emacs through projectile. From what I understand it parses the highlighted output from rg (highlighting matches) and displays them similarly highlighted in the emacs projectile ripgrep search results window. Doing any kind of piping defeats this, and I'm not sure there's a clean way around it without first-class support from the tool.

Does this sound like a familiar problem to anyone? Did you find a work around?

Basically I have grown to love fzf's "regex-like" style of matching, where individual words are regexes, but everything that is separated by a `space` character is actually considered a separate match which are all allowed to match _anywhere_ on the line, not just in that order (so it's not like replacing `space` with `.*`). It's great UX. Not necessarily `ripgrep`s responsibility to fill this niche of course, just thought I'd give some context.

---

_Comment by @fabian-thomas on 2024-04-10 16:44_

I can recommend using ugrep (https://github.com/Genivia/ugrep), which supports boolean matching, for what you @hraban describe.

Together with this simple script:
```bash
args=()
query=()
while [[ $# -gt 0 ]]; do
    case "$1" in
        --)
            # for files
            break
            ;;
        -*)
            args+=("$1")
            shift
            ;;
        *)
            query+=(--and "$1")
            shift
    esac
done
ug --column-number "${args[@]}" "${query[@]}" "$@"
```

You can then find lines that match `one` and `two` in any order by calling `the-script one two -- /path/to/fle /other/file` or for recursive search `the-script one two`. This, together with ugrep's support for terminal hyperlinks results in a really nice experience I make use of daily.

---

_Comment by @BurntSushi on 2024-04-10 17:08_

> This, together with ugrep's support for terminal hyperlinks results in a really nice experience I make use of daily.

ripgrep also supports terminal hyperlinks.

---

_Comment by @bionicles on 2024-06-29 16:02_

Here's a NAND regex. I can confirm correct boolean regex is possible in the simple case of non-nested expressions foreach of AND/OR/NOT/XOR/NAND via TDD

one issue is, as @hraban mentioned, a natural way to input these is to pass multiple patterns, and the ripgrep cli expects a single pattern and paths as far as i understand right now. these boolean expression regexes get pretty long and arcane, decent user experience needs a builder pattern / DSL sort of thing. No doubt there are many ways to achieve this aim and cramming everything into one regex might not be the move, but since you have some of the best regex code around, compiling boolean expressions to regex is likely faster than doing logic queries outside of the regex machinery

![image](https://github.com/BurntSushi/ripgrep/assets/24532336/093052dc-48e6-4526-ad61-5488bb767a8b)

currently hella swamped on unpaid work ugh, maybe it's a fun thing to tinker with next month or this fall, wrote this in january, dont want to keep it bottled up, just want to post here in support of this valuable opportunity to further upgrade the already excellent ripgrep cli !

---

_Comment by @BurntSushi on 2024-06-29 16:06_

@bionicles Thanks! The problem there is that it relies on look-around, and thus will only work with the `-P/--pcre2` switch.

---

_Comment by @davideuler on 2025-05-26 13:21_

In case you need the multiple keyword match to implement an AND logic. I've implemented a version to support the feature.

https://github.com/davideuler/ripgrep

Sometimes you need to find lines that contain several specific words, but the order or exact positioning of these words doesn't matter. The `--and` flag is designed for this purpose. It allows you to specify a set of space-separated keywords, and only lines containing *all* of those keywords will be matched.

**Syntax:**

```
rg --and "keyword1 keyword2 keyword3" [path...]
```
or with a primary pattern:
```
rg --and keyword1 --and keyword2 --and keyword3 [path...]
```


---

_Comment by @leviyevalex on 2025-08-18 20:45_

@davideuler 

I've been looking for this exact feature. I'm curious if there are plans to incorporate these features into main?

---

_Comment by @insilications on 2025-09-29 15:05_

> In case you need the multiple keyword match to implement an AND logic. I've implemented a version to support the feature.
> 
> [davideuler/ripgrep](https://github.com/davideuler/ripgrep?rgh-link-date=2025-05-26T13%3A21%3A43.000Z)
> 
> Sometimes you need to find lines that contain several specific words, but the order or exact positioning of these words doesn't matter. The `--and` flag is designed for this purpose. It allows you to specify a set of space-separated keywords, and only lines containing _all_ of those keywords will be matched.
> 
> **Syntax:**
> 
> ```
> rg --and "keyword1 keyword2 keyword3" [path...]
> ```
> 
> or with a primary pattern:
> 
> ```
> rg --and keyword1 --and keyword2 --and keyword3 [path...]
> ```

This is a feature that would be very nice if upstreamed

---

_Comment by @sojusnik on 2025-10-30 09:17_

@BurntSushi do you plan to upstream [this awesome contribution](https://github.com/BurntSushi/ripgrep/issues/875#issuecomment-2909738421)?

---

_Comment by @BurntSushi on 2025-10-30 09:39_

I haven't reviewed it yet, so I can't say.

---
