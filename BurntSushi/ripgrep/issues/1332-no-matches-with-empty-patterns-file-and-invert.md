```yaml
number: 1332
title: No matches with empty patterns file and invert match
type: issue
state: closed
author: wrodis
labels:
  - bug
  - question
  - rollup
assignees: []
created_at: 2019-07-29T09:52:17Z
updated_at: 2025-09-20T01:08:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1332
synced_at: 2026-01-12T16:13:23Z
```

# No matches with empty patterns file and invert match

---

_@wrodis_

#### What version of ripgrep are you using?

ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Compiled from repo.

#### What operating system are you using ripgrep on?

CentOS 7.

#### Describe your question, feature request, or bug.

ripgrep's behaviour seems inconsistent with grep's when using an empty file with --file and inverting match with -v.

```
$ echo text > /tmp/file; touch /tmp/empty_patterns; grep --file /tmp/empty_patterns -v /tmp/file; echo $?; rg --debug --file /tmp/empty_patterns -v /tmp/file; echo $?
text
0
1
```
Not 100% sure if it's a bug or a feature.

---

_Comment by @BurntSushi on 2019-07-29 10:54_

Interesting. This is _probably_ a bug. For corner cases like these, I've generally deferred to `grep`'s behavior, but this probably should get some careful thought. With that said, just because ripgrep and grep differ, that doesn't mean there's a bug.

One thing that is making this a bit harder to make sense of, is that if ripgrep detects that a match can never occur, then it exits immediately. One of those cases, on master right now, is when the number of patterns given to ripgrep is zero. The thinking there is that if you have nothing to search for, then you can't get any results. So, right now, this doesn't check whether `-v` is given or not. Whether `-v` should mean "match everything" even when there are no patterns isn't quite clear to me. Do you have a specific use case in mind?

Interestingly, if I _remove_ the optimization that causes ripgrep to bail early, then an empty pattern file gets compiled to an empty regex, and an [empty regex correctly matches at every position](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=d80819c8a984c4c07b17fc48123f6c98). So the question here, to me, is how an empty pattern file should be interpreted. Is it a single empty regex? Or is it zero regexes? If it's an empty regex, then `-v` should turn up nothing, but if it's zero regexes, then `-v` should turn up everything. Presumably, the latter is the choice grep made. It is possible to provide a single empty regex (provide a pattern file with a single new line), so it does kind of seem like an empty pattern file should be treated as providing no regexes. In which case, grep's behavior here is probably correct.

Do you have a particular use case in mind here, or did you just happen to notice a curious inconsistency?

---

_Label `bug` added by @BurntSushi on 2019-07-29 10:54_

---

_Label `question` added by @BurntSushi on 2019-07-29 10:54_

---

_Comment by @wrodis on 2019-07-29 12:57_

I do have a particular use case. I'm compiling a list of ids for objects to remove from a database and have some other checks that produce a list of ids that should *not* be removed. Instead of creating an arcane sql query (and because I might switch away from a database), I put the list of ids not to remove in a patterns file and use(d) rg to produce the final list of ids with
```
$ rg --file ids-not-to-remove ids-to-remove.i-think > ids-to-remove.actually
```
The list of ids-not-to-remove sometimes comes up empty and that's how I noticed.

I agree with your and grep's reasoning that an empty pattern file is "no regexes", not "an empty regex".

(Note that what you're saying about rg exiting early is a bit surprising, given that the below doesn't have any patterns, but still doesn't exit immediately.)
```
$ rg --files --glob '*.txt'
```

---

_Comment by @BurntSushi on 2019-07-29 13:01_

> Note that what you're saying about rg exiting early is a bit surprising

Yeah, I'm just being terse. It exits early when it is otherwise in search mode. If it's doing something else, then the logic doesn't apply. The case analysis in the code is probably clearer:

https://github.com/BurntSushi/ripgrep/blob/08ae4da2b79cc4e59a43e98f61ce4fd6e0adc81d/src/args.rs#L244-L260

---

_Comment by @pierreprinetti on 2020-04-10 14:49_

Hello,
I am experiencing this behaviour and I am perceiving it as a bug.

## Minimal example

```
$ echo -e 'one\ntwo\nthree' | rg --invert-match --file <(echo 'two')
one
three
$ echo -e 'one\ntwo\nthree' | rg --invert-match --file <(echo '')
$ echo $?
1
```

## My use-case

I use `rg` to remove stale resources on a cloud. I have a script that produces an exclusion-list of resources not to be deleted.

I use `rg` to filter the resources to be deleted, using `--invert-match`.

**When the exclusion-list is empty, no stale resource is deleted.**

```
$ list-resources | rg -vf <(./exclusion-list) | xargs delete-resources
```

My workaround is to add a dummy value:

```
$ list-resources | rg -vf <(echo 'dummy' && ./exclusion-list) | xargs delete-resources
```

Works, but man how ugly is it.

---

_Comment by @BurntSushi on 2020-04-10 14:57_

@pierreprinetti Aye, thanks for the details! I agree at this point that ripgrep should match grep's behavior.

---

_Comment by @pierreprinetti on 2020-04-10 16:28_

**\<opinion>**

It's confusing.

Personally: empty filter means no filter means everything passes. But that's just my point of view and I might be missing context.

Behaving differently with an empty file vs. a file containing newlines seems questionable UX (and that's precisely what `grep` does). However, I understand that backwards compatibility with `grep` represents a target.

In conclusion, I'd like `rg` to have at least one way of outputting everything, so that I don't have to use my hacky workaround anymore.

**\</opinion>**

## Recap of the current `-v` situation

`GNU grep 3.3` vs `rg 12.0.1`

| pattern file | grep's output | rg's output |
|:-|:-:|:-:|
| **empty** | everything |  nothing |
| **newline** | nothing | nothing |

```
$ echo -e 'one\ntwo\nthree' | grep --invert-match --file <(echo -ne '')
one
two
three
$ echo -e 'one\ntwo\nthree' | grep --invert-match --file <(echo -ne '\n')
$
```

```
$ echo -e 'one\ntwo\nthree' | rg --invert-match --file <(echo -ne '')
$ echo -e 'one\ntwo\nthree' | rg --invert-match --file <(echo -ne '\n')
$
```

---

_Comment by @BurntSushi on 2020-04-10 16:32_

@pierreprinetti Sorry, I'm not following you here? I'm agreeing that this is a bug and that ripgrep should match GNU grep's behavior here. Basically, if you interpret an empty file as "zero regexes" and a file containing a new line as "one empty regex," then everything should follow from there. Matching "zero regexes" yields nothing, so inverting that should return everything. (That's where the bug is in ripgrep today.) Similarly, matching a regex that matches the empty string matches everywhere. Inverting that matches nothing.

---

_Comment by @pierreprinetti on 2020-04-10 17:55_

I appreciate the logic and your solution (matching grep's behaviour) sounds solid :ok_hand:

(I was concerned about the different behaviour with empty vs newline, but you can't have everything :slightly_smiling_face:)

---

_Comment by @pierreprinetti on 2020-04-11 10:36_

I am looking into producing a PR; however my Rust skills “may” not be up to the task. Feel free to take over or assign elsewhere if I take too long!

---

_Comment by @theimpostor on 2022-08-17 14:59_

Here is another use case in favor of making the behavior the same as grep: https://stackoverflow.com/a/4079109

`grep -Fxvf file1 file2` returns all the lines in file2 which are not in file1 (without requiring files to be sorted, as the other answers in SO do). This works with `rg` except when file1 is empty, then it returns nothing.

---

_Label `rollup` added by @BurntSushi on 2025-07-27 19:13_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
