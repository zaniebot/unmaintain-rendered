```yaml
number: 422
title: "Add an option similar to -o, --only-matching #34"
type: pull_request
state: closed
author: kpp
labels: []
assignees: []
base: master
head: feature_34_only_matching
created_at: 2017-03-28T18:42:43Z
updated_at: 2017-04-10T11:04:46Z
url: https://github.com/BurntSushi/ripgrep/pull/422
synced_at: 2026-01-12T18:23:13Z
```

# Add an option similar to -o, --only-matching #34

---

_@kpp_

Fixes #34

Example:
```
~/ripgrep$ ./target/debug/rg "only-matching" -o --column
doc/rg.1.md
252:7:only-matching

tests/tests.rs
1145:16:only-matching
1158:16:only-matching

src/args.rs
370:45:only-matching

src/app.rs
162:20:only-matching
439:18:only-matching
```

---

_Comment by @BurntSushi on 2017-03-28 22:46_

@kpp Thank you! You're on a roll. I've wanted this for a while but just didn't get the chance to ever sit down and do it.

I think things might be a bit trickier than this though. For example, what does it mean to combine the `--only-matching` flag with the various context flags? GNU grep seems to get *really* confused, for example:

```
$ cat /tmp/scratch
foo foo foo
foo foo foo
foo foo foo
foo bar foo
foo foo foo
foo foo foo
foo foo foo
foo bar foo
foo foo foo
foo bar foo
foo foo foo
foo foo foo
foo foo foo
foo foo foo
$ grep -C2 -o bar /tmp/scratch
bar
bar
bar
$ grep -C1 -o bar /tmp/scratch
bar
--
bar
bar
$ grep -A1 -o bar /tmp/scratch
bar
--
bar
bar
$ grep -A2 -o bar /tmp/scratch
bar
--
bar
bar
$ grep -B2 -o bar /tmp/scratch
bar
--
bar
bar
$ grep -C2 -o bar /tmp/scratch
bar
bar
bar
grep -C1 -o bar /tmp/scratch -n
4:bar
--
8:bar
10:bar
```

So basically, GNU grep doesn't show any of the surrounding context lines, but it continues to print the context separators when applicable. What we need to do is decide if this behavior was actually intended or not, and think through what option actually makes sense to do.

I think we should also address the interaction between this flag and others like `--replace` and `--vimgrep`.

---

_Comment by @kpp on 2017-03-29 05:51_

Well, it depends on how it is documented ;D . Would you please prepare examples for me showing expected behavior?

---

_Comment by @kpp on 2017-03-29 05:57_

Seems like this behavior even better for me:

```
humbug@home:~/ripgrep$ ./target/debug/rg "only-matching" -o  -C1
doc/rg.1.md
251-
252:only-matching
253-: Print only the matched (non-empty) parts of a matching line, with each such

tests/tests.rs
1144-|wd: WorkDir, mut cmd: Command| {
1145:only-matching
1146-
--
1157-|wd: WorkDir, mut cmd: Command| {
1158:only-matching
1159-

src/args.rs
369-            null: self.is_present("null"),
370:only-matching
371-            path_separator: try!(self.path_separator()),

src/app.rs
161-        .arg(flag("null").short("0"))
162:only-matching
163-        .arg(flag("path-separator").value_name("SEPARATOR").takes_value(true))
--
438-              with xargs.");
439:only-matching
440-             "Print only the matched (non-empty) parts of a matching line, \
```


---

_Comment by @kpp on 2017-03-29 05:58_

From grep man page:
```
       -C NUM, -NUM, --context=NUM
              Print NUM lines of output context.  Places a line containing a group separator (--) between contiguous
              groups of matches.  With the -o or --only-matching option, this has no effect and a warning is given.
```

---

_Comment by @BurntSushi on 2017-03-29 11:52_

> Well, it depends on how it is documented ;D . Would you please prepare examples for me showing expected behavior?

Sure. Sorry I wasn't clear, but I'm not quite sure myself what the behavior should be.

Looking at your examples, I'm inclined to agree with you. I guess I'm just nervous... Why does GNU grep stomp on context support when `--only-matching` is used?

We still need to figure out `--replace` and `--vimgrep`. I'll try to mull it over at some point in the next day.

---

_Comment by @kpp on 2017-03-29 22:50_

Seems like `--vimgrep` is handled properly.

`--vimgrep` without `-o`:
```
$ ./target/debug/rg "only-matching"   --vimgrep -C 1
tests/tests.rs
1144-|wd: WorkDir, mut cmd: Command| {
1145:16:    cmd.arg("--only-matching");
1146-
--
1157-|wd: WorkDir, mut cmd: Command| {
1158:16:    cmd.arg("--only-matching").arg("--column").arg("--line-number");
1159-

doc/rg.1.md
251-
252:7:-o, --only-matching
253-: Print only the matched (non-empty) parts of a matching line, with each such

src/app.rs
161-        .arg(flag("null").short("0"))
162:20:        .arg(flag("only-matching").short("o"))
163-        .arg(flag("path-separator").value_name("SEPARATOR").takes_value(true))
--
438-              with xargs.");
439:18:        doc!(h, "only-matching",
440-             "Print only the matched (non-empty) parts of a matching line, \

src/args.rs
369-            null: self.is_present("null"),
370:45:            only_matching: self.is_present("only-matching"),
371-            path_separator: try!(self.path_separator()),
```

`--vimgrep` with `-o`:
```
$ ./target/debug/rg "only-matching" -o  --vimgrep -C 1
doc/rg.1.md
251-
252:7:only-matching
253-: Print only the matched (non-empty) parts of a matching line, with each such

tests/tests.rs
1144-|wd: WorkDir, mut cmd: Command| {
1145:16:only-matching
1146-
--
1157-|wd: WorkDir, mut cmd: Command| {
1158:16:only-matching
1159-

src/args.rs
369-            null: self.is_present("null"),
370:45:only-matching
371-            path_separator: try!(self.path_separator()),

src/app.rs
161-        .arg(flag("null").short("0"))
162:20:only-matching
163-        .arg(flag("path-separator").value_name("SEPARATOR").takes_value(true))
--
438-              with xargs.");
439:18:only-matching
440-             "Print only the matched (non-empty) parts of a matching line, \
```

---

_Comment by @kpp on 2017-03-30 11:41_

[12:18] -ChanServ- [#gnu] Welcome to #gnu, the official channel of the GNU Project. Please read and follow http://www.gnu.org/server/irc-rules.html.
[12:18] <Learn_C> Hi! Why doesnot grep support -A -B -C with -o ?
[12:28] <AliciaC> Learn_C: well, the context doesn't match
[14:22] <Learn_C> AliciaC: is there more reasonable answer?
[14:24] <AliciaC> not really
[14:24] <AliciaC> grep does exactly what you tell it to do, print only what matches, and that cannot include context
[14:56] <bill-auger> A B and C print surrounding lines of context - but -o only prints partial lines - so they really do not fit any use case
[14:57] <bill-auger> is that clear enough?
[15:00] <bill-auger> -o says only print the parts of a line that match and the others say print extra stuff that does not match - so it is hard to imagine how to combine them or why would you want to

---

_Comment by @kpp on 2017-03-30 12:20_

Does not seem confusing to me
```
$ echo -e "bar baz\nfoo bar food fed\nbar baz" | ./target/debug/rg foo -C 1 -n --column -o
1-bar baz
2:1:foo
2:9:foo
3-bar baz
```

---

_Comment by @BurntSushi on 2017-03-30 12:52_

@kpp Yeah, I'm with you. I'm not really buying what they're selling, especially when the context separators are still printed.

So I think the only thing left to think about is the `-r/--replace` flag. How does it interact with `-o/--only-matching`?

---

_Comment by @kpp on 2017-03-30 13:09_

[15:13] <Learn_C> bill-auger, not clear enough. MAN page says "grep, egrep, fgrep, rgrep - print lines matching a pattern", according to that, there should be no -A -B -C, because they don't match a pattern
[15:14] <bill-auger> they show you the lines before and after the match
[15:14] <Learn_C> "-o Print  only  the  matched  (non-empty)  parts of a matching line, with each such part on a separate output line."
[15:14] <bill-auger> just in case you want to see that
[15:15] <Learn_C> So "-o" deals with matching lines, not with context
[15:15] <bill-auger> yes
[15:15] <bill-auger> try it
[15:15] <bill-auger> echo "foo bar food fed" | grep foo
[15:16] <Learn_C> why does not -A -B -C with -o print both context and matched parts of a matching line?
[15:16] <bill-auger> that would be confusing
[15:17] <Learn_C> according to grep definition, the existance of -A -B -C is confusing either
[15:18] <Learn_C> I am not trolling, I want to deep inside
[15:22] <Learn_C> bill-auger: does this behaviour seem confusing to you? https://pastebin.com/ExJgftai
[15:35] <bill-auger> yes that is totally unclear
[15:43] <AliciaC> not only unclear but contrary to the description of the option
[15:45] <bill-auger> anynot only that - but that website made me watch a commercial before i could see the paste
[15:59] <AliciaC> hm, I've never seen it do that

---

_Comment by @BurntSushi on 2017-03-30 13:14_

Using the `-o/--only-matching` flag with GNU grep will show context separators without the context lines:

```
$ echo -e 'bar\nfoo\nfoo\nfoo\nbar\nfoo\n' | grep -o -C1 bar
bar
--
bar
```

---

_Comment by @BurntSushi on 2017-03-30 13:16_

The man page for GNU grep says this about the `-o/--only-matching` flag:

> Print only the matched (non-empty) parts of a matching line, with  each  such  part  on  a  separate output line.

To me, this doesn't imply anything about contexts. The option only applies to matching lines. It doesn't say, "print only matching lines," it says, "print only the non-empty matched parts of a matching line."

... In any case, this navel gazing about whether GNU grep does the "right" thing or not is probably not going to go anywhere productive. I think what I'd like to know is if there's a practical purpose for the `-o/--only-matching` flag to effectively suppress the context flags. It seems to me like if you didn't want the context shown, then you shouldn't pass the flags.

---

_Comment by @kpp on 2017-03-30 13:22_

Well, I agree with you. Lets talk about `--replace`.

no `-o`
```
$ echo -e "bar baz\nfoo bar food fed\nbar baz" | ./target/debug/rg foo --replace "it_was_foo" -C 1 -n --column --color always | hexdump -C
00000000  1b 5b 6d 1b 5b 33 32 6d  31 1b 5b 6d 2d 62 61 72  |.[m.[32m1.[m-bar|
00000010  20 62 61 7a 0a 1b 5b 6d  1b 5b 33 32 6d 32 1b 5b  | baz..[m.[32m2.[|
00000020  6d 3a 31 3a 69 74 5f 77  61 73 5f 66 6f 6f 20 62  |m:1:it_was_foo b|
00000030  61 72 20 69 74 5f 77 61  73 5f 66 6f 6f 64 20 66  |ar it_was_food f|
00000040  65 64 0a 1b 5b 6d 1b 5b  33 32 6d 33 1b 5b 6d 2d  |ed..[m.[32m3.[m-|
00000050  62 61 72 20 62 61 7a 0a                           |bar baz.|
00000058
```

with `-o`
```
$ echo -e "bar baz\nfoo bar food fed\nbar baz" | ./target/debug/rg foo --replace "it_was_foo" -C 1 -n --column --color always -o | hexdump -C
00000000  1b 5b 6d 1b 5b 33 32 6d  31 1b 5b 6d 2d 62 61 72  |.[m.[32m1.[m-bar|
00000010  20 62 61 7a 0a 1b 5b 6d  1b 5b 33 32 6d 32 1b 5b  | baz..[m.[32m2.[|
00000020  6d 3a 31 3a 69 74 5f 77  61 73 5f 66 6f 6f 20 62  |m:1:it_was_foo b|
00000030  61 72 20 69 74 5f 77 61  73 5f 66 6f 6f 64 20 66  |ar it_was_food f|
00000040  65 64 0a 1b 5b 6d 1b 5b  33 32 6d 32 1b 5b 6d 3a  |ed..[m.[32m2.[m:|
00000050  39 3a 69 74 5f 77 61 73  5f 66 6f 6f 20 62 61 72  |9:it_was_foo bar|
00000060  20 69 74 5f 77 61 73 5f  66 6f 6f 64 20 66 65 64  | it_was_food fed|
00000070  0a 1b 5b 6d 1b 5b 33 32  6d 33 1b 5b 6d 2d 62 61  |..[m.[32m3.[m-ba|
00000080  72 20 62 61 7a 0a                                 |r baz.|
00000086

```

no `-o` no lines/columns:
```
$ echo -e "bar baz\nfoo bar food fed\nbar baz" | ./target/debug/rg foo -C 1 --replace '___' --color always -o | hexdump -C
00000000  62 61 72 20 62 61 7a 0a  5f 5f 5f 20 62 61 72 20  |bar baz.___ bar |
00000010  5f 5f 5f 64 20 66 65 64  0a 5f 5f 5f 20 62 61 72  |___d fed.___ bar|
00000020  20 5f 5f 5f 64 20 66 65  64 0a 62 61 72 20 62 61  | ___d fed.bar ba|
00000030  7a 0a                                             |z.|
00000032
```

---

_Comment by @BurntSushi on 2017-03-30 13:31_

@kpp Sorry, but could you use simpler commands please? Drop the `hexdump`, colors, `--column`, etc...

---

_Comment by @kpp on 2017-03-30 13:33_

It tries to multiple lines by n found occurrences. Than it replaces found part with replacer and we get duplicate lines with no meaning.
```
$ echo -e "bar baz\nfoo bar food fed\nbar baz" | ./target/debug/rg foo -C 1 --replace '___' 
bar baz
___ bar ___d fed
bar baz

$ echo -e "bar baz\nfoo bar food fed\nbar baz" | ./target/debug/rg foo -C 1 --replace '___' -o
bar baz
___ bar ___d fed
___ bar ___d fed
bar baz
```

---

_Comment by @kpp on 2017-03-30 13:39_

I bet we should prohibit the use of `-o` with `--replace`. See example:

```
$ echo -e "bar baz\na fofooo b\nbar baz" | ./target/debug/rg foo -C 1 --replace '' 
bar baz
a foo b
bar baz
humbug@home:~/ripgrep$ echo -e "bar baz\na fofooo b\nbar baz" | ./target/debug/rg foo -C 1 --replace '' -o
bar baz
a foo b
bar baz
```

Should we show only `foo` in the second example? 

---

_Comment by @kpp on 2017-03-30 14:04_

```
$ echo -e "bar baz\na fofooo b\nbar baz" | ./target/debug/rg foo -C 1 --replace '' -o
error: The argument '--only-matching' cannot be used with '--replace <ARG>'

USAGE:
    
    rg [OPTIONS] <pattern> [<path> ...]
    rg [OPTIONS] [-e PATTERN | -f FILE ]... [<path> ...]
    rg [OPTIONS] --files [<path> ...]
    rg [OPTIONS] --type-list

For more information try --help
```

---

_Comment by @BurntSushi on 2017-03-30 14:07_

@kpp I don't quite have the time to think about this. Is it possible to modify how `-r/--replace` works when the `-o/--only-matching` flag is set?

---

_Comment by @kpp on 2017-03-30 14:09_

Yes, it is possible. Just tell me your expectations)

---

_Comment by @kbknapp on 2017-03-30 17:02_

I think it doesn't really matter too much which method you take (suppressing `-A -B -C` or not)  so long as it's clearly articulated in the help message of affected flags/options.

I tend to lean towards grep's way being more "correct" (minus still printing context separators...that doesn't make sense), but the proposed ripgrep way is more flexible and I'd totally find it fine to see it implemented. I say. "correct" as in "do what I *want*, not what I say."

How does the proposed ripgrep handle this:

```
$ cat /tmp/scratch
foo foo foo
foo foo foo
foo foo bar
foo foo bar
foo foo foo
foo foo foo
$ grep bar -o -B1 scratch
bar
bar
```

I would find this less correct, and harder to pipe:

```
$ cat /tmp/scratch
foo foo foo
foo foo foo
foo foo bar
foo foo bar
foo foo foo
foo foo foo
$ rg bar -o -B1 scratch
2-foo foo foo
3:bar
4:bar
```

Because *technically* I'd think of the `-B1` of the second match as `foo foo bar`, so if we're modifying that line to show "only matches" the `-B1` of the first match should *also* be modified to show "only matches" to be consistent.

Having said this, the proposed ripgrep way allows one to actually see (minus caveats above) context when showing `-o`, whereas there is no way that I know of in grep to do this. Hence, why I said ripgrep being more flexible.

A way to kind of sidestep this is to use a [soft override](https://docs.rs/clap/2.22.1/clap/struct.Arg.html#method.overrides_with). Whichever conflicting arg comes last wins.  Meaning `-o -C1` is equivalent to `-C1`, likewise `-C1 -o` is equivalent to `-o`. But this would need to be clearly articulated in the help. This would allow one to script a *default behavior* (either `-o` or using contexts) and then opt-out of that default by passing in something additional. It does however go back to the grep issue of being slightly less flexible and not providing a way to see *both* contexts and `-o` at the same time (which is still somewhat counter-intuitive IMO).

Just my thoughts. :-)

---

_Comment by @BurntSushi on 2017-03-30 17:13_

@kbknapp That's an interesting point. I suppose the only way to really figure out why using `-B1` makes it harder to pipe is to ask why `-B1` was used at all in the first place. If you care about the "before context" in the case of the second match, then that would be shown whether or not `-B1` is passed. So if you didn't want the line before the first match, you probably should just drop `-B1` completely.

What I'm trying to say is this: under what circumstances is the behavior of GNU grep desirable (modulo the weirdness of emitting context separators)?

---

_Comment by @kbknapp on 2017-03-30 17:23_

>So if you didn't want the line before the first match, you probably should just drop `-B1` completely.

I totally agree, and I can't really think of a valid reason why someone would do that unless it's just scripted as a default behavior...which still seems strange but it's all I can come up with 😟 

---

_Comment by @kpp on 2017-03-30 19:19_

@BurntSushi, @kbknapp are you happy to see how context and `-o` interact? Is there anything I should change?

---

_Comment by @BurntSushi on 2017-03-30 19:22_

@kpp I think I'm happy with the current interaction. My mind could be changed by use cases. I realize I'm still on the hook for figuring out how `-r/--replace` should work.

---

_Comment by @kpp on 2017-04-01 11:42_

How about:
```
$ rg '(?P<first>[A-Z][a-z]+)\s+(?P<last>[A-Z][a-z]+)'  tests/test.txt -o
7:Baker Street
```
```
$ rg '(?P<first>[A-Z][a-z]+)\s+(?P<last>[A-Z][a-z]+)'  tests/test.txt -o --replace '$last, $first'
7:Street, Baker
```

---

_Comment by @BurntSushi on 2017-04-09 12:50_

I've merged this in https://github.com/BurntSushi/ripgrep/commit/90a11dec5e5765af560d343e879869a60363ef54 after a rebase, squash and a tweak to the `-h` output.

While I don't particularly like that we now have conflicting flags, I also don't know what the right behavior for combining `-o` and with `-r` is. I hope that by banning their combinations, some folks will come along and tell us how they should be used together. At that point, someone should **write a specification that we agree on** before submitting a PR.

---

_Comment by @mernen on 2017-04-09 20:13_

Guess someone beat me to implementing `-o`! (Which is fair, I’ve been postponing this since January)

So, as I mentioned in #308, I think the best combination for `--only-matching` and `--replace` would be an approximation of Ack’s `--output`: perform a replace on the matched part, and output only the result of the replacement, not anything else on the line. If there are multiple matches on a single line, output multiple lines. Same as on @kpp’s last comment, if I understood it correctly.

---

_Comment by @BurntSushi on 2017-04-10 10:41_

@mernen Thanks for the ideas! Please note that commenting on closed issues/PRs is a surefire way for me to completely lose track of your feedback. I created a new issue, #443, to track combining `-o` with `-r`.

---

_Comment by @BurntSushi on 2017-04-10 10:42_

Errmm, OK, so I forgot to close this PR after I did a manual merge in https://github.com/BurntSushi/ripgrep/commit/90a11dec5e5765af560d343e879869a60363ef54. :-)

---

_Closed by @BurntSushi on 2017-04-10 10:42_

---

_Branch deleted on 2017-04-10 11:04_

---
