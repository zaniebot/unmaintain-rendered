```yaml
number: 1807
title: Non-deterministic number of results
type: issue
state: closed
author: minad
labels: []
assignees: []
created_at: 2021-02-26T16:37:06Z
updated_at: 2021-06-01T00:17:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1807
synced_at: 2026-01-12T16:13:24Z
```

# Non-deterministic number of results

---

_@minad_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

apt-get on Debian buster

#### What operating system are you using ripgrep on?

Debian buster on x86_65, Linux 5.4.88

#### Describe your bug.

Give a high level description of the bug.

#### What are the steps to reproduce the behavior?

* Running `rg -j 1 . -e test | sort | uniq | wc` always yields the same number of results
* In contrast, running `rg -j 2 . -e test | sort | uniq | wc` yields a different number each time

(big corpus)

#### What is the actual behavior?

Non deterministic number of matches

#### What is the expected behavior?

Non deterministic ordering of matches, but deterministic number of matches, no matches should be lost

---

_Comment by @BurntSushi on 2021-02-26 16:40_

This isn't actionable without a reproduction. Please find a way to reproduce the problem on a corpus we both have access to.

Also, your version of ripgrep is almost 2.5 years old. I don't provide support for anything other than the most recent version. So you might try the most recent version of ripgrep, although I'm not aware of any changes between the 0.10.0 and 12.1 releases that would impact this particular sort of issue.

---

_Comment by @minad on 2021-02-26 16:43_

That's understandable. I see if I can test an updated version. However given that you are not aware of any changes which would affect that I wonder if something like this has been observed before? I mean this looks like a serious threading issue. It could also be a bug on some other layer, kernel, fs or memory problems.

---

_Comment by @BurntSushi on 2021-02-26 16:53_

It could be a threading issue. Dunno. Or a problem with the parallel file searcher terminating too early. The easiest thing is to try a newer version. If it's fixed, well, then nothing more to do. If not, then yeah, I'll need a repro to do anything about it.

---

_Comment by @BurntSushi on 2021-02-26 16:54_

The github releases for this repo contain a binary debian package you can try. Or even just a the x86_64 tarball has a statically linked executable. No need to compile from source.

---

_Comment by @minad on 2021-02-26 17:05_

> Or a problem with the parallel file searcher terminating too early.

Yes, I suspected something like this too. I am trying to narrow down a bit where the problem could be. I am comparing with grep, some of the files are recognized as binary by grep and not counted. In contrast ripgrep counts them. I am not familiar with the ripgrep internals, does it split files in chunks and then distribute the chunks over the threads? Maybe this creates problems with the detection of binary files?

---

_Comment by @minad on 2021-02-26 17:14_

I am observing this issue for example when running rg on my ".emacs.d/elpa" folder. When running `rg -j2` the string "test" matches some info files, which are recognized as binary by grep. Exactly the same files are ignored by `rg -j1`, since probably rg also recognizes them as binary. But in the multithreaded version the second chunk could match before the first thread detects that the file is binary. Does that sound plausible?

---

_Comment by @BurntSushi on 2021-02-26 19:42_

No, ripgrep's parallelism only happens at the granularity of files. Parallelism does not exist within a file.

With that said, binary detection is a bit subtle and can change based on the method of searching. For example, ripgrep can search using standard `read` syscalls or via memory maps, and the binary detection between them differs. However, when doing recursive search, ripgrep should always use standard `read` syscalls.

Binary detection shouldn't be impacted by parallelism.

If you pass `--binary --no-mmap` to ripgrep, then it should behave like grep. It should also rule out memory maps being responsible for different binary detection algorithms being employed.

---

_Comment by @minad on 2021-02-26 20:16_

I don't have `--binary` since my version is too old I guess. With `--text` I get deterministic results with `-j<n>` for different `n`. Using `--mmap` or `--no-mmap` does not make a difference. The problem in any case seems to be the binary detection. But I wonder how this can be broken, if as you say, parallelism is done at file granularity.

---

_Comment by @BurntSushi on 2021-02-26 21:04_

Ah I see. Yeah I can't think of anything that would explain what you're
seeing. But my mental model of a 2.5 year old ripgrep is pretty weak. I
think at this point I have to insist that you try the latest release. It
should be easy to do and not require any compilation.

On Fri, Feb 26, 2021, 15:16 Daniel Mendler <notifications@github.com> wrote:

> I don't have --binary since my version is too old I guess. With --text I
> get deterministic results with -j<n> for different n. Using --mmap or
> --no-mmap does not make a difference. The problem in any case seems to be
> the binary detection. But I wonder how this can be broken, if as you say,
> parallelism is done at file granularity.
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1807#issuecomment-786871678>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADPPYQPAE5PNUVLMKXWMJTTA76TVANCNFSM4YIXV3RQ>
> .
>


---

_Comment by @BurntSushi on 2021-06-01 00:17_

Closing due to inactivity. Happy to re-open given actionable data.

---

_Closed by @BurntSushi on 2021-06-01 00:17_

---
