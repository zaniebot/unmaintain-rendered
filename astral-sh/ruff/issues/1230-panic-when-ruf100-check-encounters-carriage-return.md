```yaml
number: 1230
title: Panic when RUF100 check encounters carriage return
type: issue
state: closed
author: squiddy
labels:
  - bug
assignees: []
created_at: 2022-12-13T16:50:46Z
updated_at: 2023-03-13T04:03:19Z
url: https://github.com/astral-sh/ruff/issues/1230
synced_at: 2026-01-12T15:54:41Z
```

# Panic when RUF100 check encounters carriage return

---

_@squiddy_

Another one from #1206 

```bash
echo -e 'foo = """\r"""' | cargo run -- - --select RUF100 --fix
```

```
thread 'main' panicked at 'index out of bounds: the len is 1 but the index is 1', src/check_lines.rs:125:66
```

Not sure how likely this is to be an issue in practice. I think I'm doing something suboptimal to the input/output of the test runner over at #1206 and when testing the `W605_1.py` it eventually ends up in output similar to the minimal testcase.

Python will happily execute it though.

```bash
$ echo -e 'foo = """\rhello world""";print(foo)' | python

hello world
```

---

_Comment by @squiddy on 2022-12-13 16:52_

I was hoping this could be something https://github.com/charliermarsh/ruff/issues/76 could surface, but my attempts so far weren't successful in producing anything with it that shows a problem.

---

_Comment by @charliermarsh on 2022-12-13 21:01_

Does this have to do with passing over `stdin`? And how the strings are interpreted? It seems fine if I paste these contents into `foo.py` and run Ruff over that.

---

_Label `bug` added by @charliermarsh on 2022-12-13 21:01_

---

_Label `question` added by @charliermarsh on 2022-12-13 21:01_

---

_Comment by @squiddy on 2022-12-14 03:48_

It's not about stdin, let me attach that in the hopes that github doesn't mess with it.

[ruf100_panic.txt](https://github.com/charliermarsh/ruff/files/10224094/ruf100_panic.txt)

Most editors would show this like this. There is a `\r` after the opening quotes that is causing a line break, but it's not a typical newline ending (`\r\n` on windows, `\n` on linux machines).

```
foo = """
hello world""";print(foo)
```

---

_Label `question` removed by @charliermarsh on 2022-12-15 00:41_

---

_Comment by @squiddy on 2022-12-17 18:26_

I noticed that latest main doesn't have the issue anymore. bisect suggests that 9cb18a481b8241608ad8964d3f107247e758a251 fixed this issue.

---

_Comment by @charliermarsh on 2022-12-17 20:53_

It's odd because in https://github.com/charliermarsh/ruff/commit/7e45a9f2e2d536c06660ccff04b499f52e6d5a6f, I was able to re-enable the Black compatibility test for `W605_1.py`, but I didn't really understand why. After https://github.com/charliermarsh/ruff/commit/9cb18a481b8241608ad8964d3f107247e758a251, I had to re-add it, even though it seemed to work fine for me when I ran the series of commands manually. Maybe related somehow?


---

_Comment by @charliermarsh on 2022-12-17 20:54_

I guess we can close this though?

---

_Comment by @squiddy on 2022-12-18 07:04_

`W605_1.py` is the only test fixture that uses CRLF line endings, all the others just use LF. So I think there is still something odd going on.

```console
❯ find resources/test/fixtures/ -iname "*.py" -exec file {} \; | grep CRLF
resources/test/fixtures/pycodestyle/W605_1.py: ASCII text, with CRLF line terminators
```

---

One "fix" for this is to replace the CRLF line endings in that fixture file with LF, for example using `dos2unix`.

```console
❯ dos2unix resources/test/fixtures/pycodestyle/W605_1.py 
dos2unix: converting file resources/test/fixtures/pycodestyle/W605_1.py to Unix format...

❯ cargo test --package ruff --test black_compatibility_test -- --ignored
   Compiling ruff v0.0.185 (/home/squiddy/projects/ruff)
    Finished test [unoptimized + debuginfo] target(s) in 2.40s
     Running tests/black_compatibility_test.rs (target/debug/deps/black_compatibility_test-9e3e94cead0d0415)

running 1 test
[2022-12-18][08:03:39][black_compatibility_test][INFO] `blackd` not ready yet; retrying...
[2022-12-18][08:03:39][black_compatibility_test][INFO] `blackd` not ready yet; retrying...
[2022-12-18][08:03:39][black_compatibility_test][INFO] `blackd` ready
test test_ruff_black_compatibility ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out; finished in 42.08s
```

---

_Comment by @charliermarsh on 2022-12-21 19:34_

It's intentional that they use CRLF line endings though, I think we used to handle those incorrectly.

---

_Comment by @squiddy on 2022-12-26 09:38_

I'm not sure why running the steps manually doesn't trigger any issue, but if I grab the python file right before I get something that triggers it. Minimal test case is this (also works if you put that into a file first):

```console
> echo -e 'foo = """\r""" # noqa' | cargo run -- -
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/ruff -`
thread 'main' panicked at 'index out of bounds: the len is 1 but the index is 1', src/checkers/noqa.rs:41:27
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

The underlying issue here is that there is a disconnect between how we collect commented lines (their row number) and how we map that onto the line itself in `check_noqa`.

For collecting commented lines we utilize the tokenizer, look for comments and take the row of the starting location. If ran against my test case, it will report the `# noqa` being one row 2. So it seems to treat `\r` as a line break.

In `check_noqa` however we use `contents.lines()` to get individual lines. `.lines()` only deals with `\n` and `\r\n`. So in our example, it would only return one line.



---

_Comment by @squiddy on 2022-12-26 09:44_

I'd be happy to provide a fix, but I need look a bit further what the expected behaviour should be and if there are other places where we need to adapt our concept of lines.

The lexer of RustPython seems to replace `\r` with `\n` first before continuing. https://github.com/RustPython/RustPython/blob/247e815880766d556ef1ca6f0af69daf1a5fe59a/compiler/parser/src/lexer.rs#L194

---

_Comment by @squiddy on 2022-12-26 09:52_

Here we go: https://docs.python.org/3.11/reference/lexical_analysis.html?highlight=lexical#physical-lines

> A physical line is a sequence of characters terminated by an end-of-line sequence. In source files and strings, any of the standard platform line termination sequences can be used - the Unix form using ASCII LF (linefeed), the Windows form using the ASCII sequence CR LF (return followed by linefeed), or the old Macintosh form using the ASCII CR (return) character. All of these forms can be used equally, regardless of platform. The end of input also serves as an implicit terminator for the final physical line.

So `ASCII CR` (`\r`) is a valid line termination, so we should treat it as such. 

---

_Comment by @squiddy on 2022-12-27 09:58_

> or the old Macintosh form using the ASCII CR (return) character

Apparantely old here means `<= MacOS X`.

---

_Comment by @squiddy on 2023-01-01 10:06_

I see some options here.

### Properly support `CR` line endings

Supporting here means properly reading and writing source code. I think the writing part is covered by recent changes for the most part (ignoring the textwrap part) and is quite easy to do. 

The reading part is a bit more difficult. RustPython does support `CR (\r)` by replacing it with a `LF (\n)` and then not having to worry about it later on, so we're good here. What currently doesn't work is when we read the source code directly through `SourceCodeLocator` and make use of `lines()`. 
`lines` only cares about `LF`. There was a discussion on a rust board about expanding support here, but it didn't go anywhere. Just looking at a single byte is easier to implement. 

What can we do?

a) we either implement our own line splitting logic and see if that has some noticeable performance impact
b) we stick with `LF` as the line ending in the core of the code, and only deal with dialects on the edges, i.e. normalizing line endings when code comes in, adjusting to style when emitting code

### Don't support `CR` line endings

When linting a file, detect line endings and issue an error / warning and stop processing that file. I now see that `CR` stop being the default line ending in MacOS X (and not that it was the last version to do that). It was released in 2001. So any new code/projects in the past 22 years are likely to not use this line ending.

Given how tricky (and probably surprising) this line handling is, perhaps this is an acceptable tradeoff?

---

_Comment by @charliermarsh on 2023-01-01 22:12_

I think I'm comfortable not supporting this based on the above.

---

_Comment by @youknowone on 2023-02-10 20:34_

is this related to https://github.com/RustPython/RustPython/pull/4484?

---

_Comment by @charliermarsh on 2023-02-10 21:19_

@youknowone - No this is much older. CR line endings are pretty rare and we decided not to support them.

---

_Comment by @charliermarsh on 2023-03-13 04:03_

Okay, this is now resolved via #3454 and #3439.

---

_Closed by @charliermarsh on 2023-03-13 04:03_

---
