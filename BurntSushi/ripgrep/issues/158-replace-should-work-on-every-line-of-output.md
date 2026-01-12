```yaml
number: 158
title: "--replace should work on every line of output"
type: issue
state: closed
author: AshwinJay
labels:
  - bug
assignees: []
created_at: 2016-10-09T06:35:18Z
updated_at: 2016-11-06T18:53:40Z
url: https://github.com/BurntSushi/ripgrep/issues/158
synced_at: 2026-01-12T18:23:11Z
```

# --replace should work on every line of output

---

_@AshwinJay_

Hello, I could not find any switch to make ripgrep print all the column numbers if a pattern appears more than once on a single line. It seems to print only the first match but colors all the matches.

```
./rg IS LICENSE-MIT --column
15:14:THE SOFTWARE *IS* PROVIDED "AS *IS*", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
19:60:LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERW*IS*E, AR*IS*ING FROM,
```

My other question is more of an enhancement request. 

Here's what I would love to use ripgrep for:
Stream a long list of multiline strings, delimited by some special character and have ripgrep output the position of the string in the stream that contains a match(es) along with the column and length of the matching text.

Example:

```
echo "string 1\0string 2... \0string 3\0string..." | \
./rg "foo*bar" -stream -separator "\0" -print-locations-only
```

Output:

```
<stream-position>[column-start:column-end, column-start:column-end,...]
<stream-position>[column-start:column-end, column-start:column-end,...]
<stream-position>[column-start:column-end, column-start:column-end,...]
```

Thanks.


---

_Comment by @AshwinJay on 2016-10-10 00:20_

I think I almost got what I was looking for with the `vimgrep` switch but not quite.

```
./rg "(C?ON)" * --replace '«$1»' --vimgrep
LICENSE-MIT
17:39:FITNESS FOR A PARTICULAR PURPOSE AND N«ON»INFRINGEMENT. IN NO EVENT SHALL THE
19:30:LIABILITY, WHETHER IN AN ACTI«ON» OF «CON»TRACT, TORT OR OTHERWISE, ARISING FROM,
19:36:LIABILITY, WHETHER IN AN ACTI«ON» OF «CON»TRACT, TORT OR OTHERWISE, ARISING FROM,
20:14:OUT OF OR IN «CON»NECTI«ON» WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
20:22:OUT OF OR IN «CON»NECTI«ON» WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN

UNLICENSE
18:56:MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND N«ON»INFRINGEMENT.
20:36:OTHER LIABILITY, WHETHER IN AN ACTI«ON» OF «CON»TRACT, TORT OR OTHERWISE,
20:42:OTHER LIABILITY, WHETHER IN AN ACTI«ON» OF «CON»TRACT, TORT OR OTHERWISE,
21:28:ARISING FROM, OUT OF OR IN «CON»NECTI«ON» WITH THE SOFTWARE OR THE USE OR
21:36:ARISING FROM, OUT OF OR IN «CON»NECTI«ON» WITH THE SOFTWARE OR THE USE OR

rg.1
22:14:.SH DESCRIPTI«ON»
26:9:.SH COMM«ON» OPTI«ON»S
26:16:.SH COMM«ON» OPTI«ON»S
131:14:.SH LESS COMM«ON» OPTI«ON»S
131:21:.SH LESS COMM«ON» OPTI«ON»S
280:30:.SH FILE TYPE MANAGEMENT OPTI«ON»S
```

@BurntSushi It would be nice if we have a `-o` switch like grep combined with `ripgrep`'s `replace` or some way to not print the entire string repeatedly like this:

```
UNLICENSE
18:56:«ON»
20:36:«ON»
20:42:«CON»
21:28:«CON»
21:36:«ON»
```

Thanks!


---

_Comment by @BurntSushi on 2016-10-11 00:28_

@AshwinJay I had a hard time following what exactly you want here. I'm glad to see `--vimgrep` got you most of the way. It looks like you could technically go the rest of the way with more `--replace` trickery. e.g.,

```
rg --replace '«$1»' --vimgrep '.*(C?ON).*'
```

... ah, I see, this does not work because it only shows the replacement for the first match in each line. I think that's a bug. I'd expect `--replace` combined with `--vimgrep` to do the replacement for each match in each line.


---

_Label `bug` added by @BurntSushi on 2016-10-11 00:28_

---

_Renamed from "--column only prints the first match (and other questions)" to "--replace should work on every line of output" by @BurntSushi on 2016-10-11 00:30_

---

_Comment by @BurntSushi on 2016-10-11 00:31_

Note that #34 requests an `--only-matching` flag, and it even has a work-around that would appear to match your use case. The issue here is the interaction between printing each match on each line and the operation of `--replace`.

There is also #125 that adds an `--only-matching` flag, but there are several unresolved questions.


---

_Comment by @AshwinJay on 2016-10-11 02:36_

@BurntSushi I think #34 is exactly what I'm looking for. When that gets implemented, there will be no need to use `replace`. I was only using replace to illustrate what I'd have to do to demarcate the matching text programmatically (meaning, I'd have to parse the output of ripgrep and extract the matches).

With the combination of `--vimgrep` and `--only-matching` my plan is to produce output like this:

```
line:column:matching-text
line:column:matching-text
line:column:matching-text
...
```

Then stream the output to a program, perhaps a UI. If the user wants to dig into any specific output line, then I'd use the "line:column" combination to dive into the specific area of the text file and then read the entire text.

Hope that makes it clear. Thanks!


---

_Closed by @AshwinJay on 2016-10-11 02:36_

---

_Reopened by @BurntSushi on 2016-10-11 02:54_

---

_Comment by @BurntSushi on 2016-10-11 02:54_

Right, I understand, but I still consider the behavior between `--replace` and `--vimgrep` to be surprising, which is why I labeled this a bug. Reopening. :-)


---

_Comment by @AshwinJay on 2016-10-12 00:44_

I realized only now that `grep` has a `-b` option which is better than row and column. It's the byte offset within the file where the match was found. It would be great if we had that option in ripgrep over the `--vimgrep` option.

Thanks.


---

_Comment by @AshwinJay on 2016-10-13 05:55_

Outstanding performance compared to `grep`!! I have a 2.5GB file with 5,000,000 relatively nested JSON documents.

```
Me$ ls -al
total 5269584
drwxr-xr-x   3 Me  1384426549   102B Oct 12 22:40 .
drwxr-xr-x  19 Me  1384426549   646B Oct 12 21:46 ..
-rw-r--r--   1 Me  1384426549   2.5G Oct 12 22:41 user.json
```

```
Me$ time ~/Downloads/ripgrep-0.2.1-i686-apple-darwin/./rg -c --no-mmap Ste user.json
282855

real    0m1.082s
user    0m0.612s
sys 0m0.469s
```

```
Me$ time grep -c  Ste user.json
282855

real    0m56.202s
user    0m55.744s
sys 0m0.445s
```

```
2.4 GHz Intel Core i7
16 GB 1600 MHz DDR3
MacBook Pro (Retina, 15-inch, Early 2013)
```

If you implement #34 along with `-b` this will be a super fast text ETL tool like no other. Keep it up!


---

_Comment by @BurntSushi on 2016-10-13 09:36_

To be fair, if you are on a mac, then grep is probably bsdgrep, which is
known to be slow. You probably want to compare with GNU grep, which you can
install from brew. Its binary name is ggrep.

On Oct 13, 2016 01:55, "Ashwin Jayaprakash" notifications@github.com
wrote:

> Outstanding performance compared to grep!! I have a 2.5GB file with
> 5,000,000 relatively nested JSON documents.
> 
> Me$ ls -al
> total 5269584
> drwxr-xr-x   3 Me  1384426549   102B Oct 12 22:40 .
> drwxr-xr-x  19 Me  1384426549   646B Oct 12 21:46 ..
> -rw-r--r--   1 Me  1384426549   2.5G Oct 12 22:41 user.json
> 
> Me$ time ~/Downloads/ripgrep-0.2.1-i686-apple-darwin/./rg -c --no-mmap Ste user.json
> 282855
> 
> real    0m1.082s
> user    0m0.612s
> sys 0m0.469s
> 
> Me$ time grep -c  Ste user.json
> 282855
> 
> real    0m56.202s
> user    0m55.744s
> sys 0m0.445s
> 
> 2.4 GHz Intel Core i7
> 16 GB 1600 MHz DDR3
> MacBook Pro (Retina, 15-inch, Early 2013)
> 
> If you implement #34 https://github.com/BurntSushi/ripgrep/issues/34
> along with -b this will be a super fast text ETL tool like no other. Keep
> it up!
> 
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/158#issuecomment-253421045,
> or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34lU7FiTNOWjsIvrMhT9_nmW_-N6Fks5qzcfSgaJpZM4KR7CP
> .


---

_Comment by @AshwinJay on 2016-10-14 00:40_

Ok I tried ggrep. It's pretty fast but yours is faster.

```
time ~/Downloads/ripgrep-0.2.1-i686-apple-darwin/rg -c Ste user.json --no-mmap
282471

real    0m1.122s
user    0m0.603s
sys 0m0.517s

time ggrep -c Ste user.json
282471

real    0m3.943s
user    0m3.507s
sys 0m0.431s
```


---

_Comment by @BurntSushi on 2016-10-14 00:46_

@AshwinJay Nice! That's a great result. If you're curious about the specific reason why `ripgrep` beats GNU grep there, it's likely because `ripgrep` looks for `S` while GNU grep looks for `e`. `e` is probably much much more common than `S`, so it ends up with a lot more false positives. You can read more about it here: http://blog.burntsushi.net/ripgrep/#subtitles-literal

Also, if you upgrade to `ripgrep 0.2.3`, I believe memory maps will always be disabled on Mac (unless explicitly enabled).


---

_Comment by @BurntSushi on 2016-11-06 18:53_

I'm going to close this because I believe I was too hasty in marking this as a bug. Specifically, the regex `.*(C?ON).*` is going to match the entire line, so I think the behavior is correct.

Otherwise, I think the original use case is solved by #34 (which is still open).


---

_Closed by @BurntSushi on 2016-11-06 18:53_

---
