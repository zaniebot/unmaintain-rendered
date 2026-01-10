```yaml
number: 2283
title: "uv-fs: transparently support reading UTF-16 files"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: ag/read-utf16
created_at: 2024-03-07T18:05:50Z
updated_at: 2024-03-08T15:51:50Z
url: https://github.com/astral-sh/uv/pull/2283
synced_at: 2026-01-10T14:54:43Z
```

# uv-fs: transparently support reading UTF-16 files

---

_Pull request opened by @BurntSushi on 2024-03-07 18:05_

This PR tweaks uv to support reading `requirements.txt` regardless of
whether it is encoded as UTF-8 or UTF-16. This is particularly relevant
on Windows where `uv pip freeze > requirements.txt` will likely write a
UTF-16 encoded `requirements.txt` file.

There is some discussion on #1666 where it's suggested that perhaps
we should explicitly not support this. I didn't see that until I
had already put this PR together, but even so, I think it's worth
considering this. UTF-16 is predominant on Windows. It is very easy
to produce a UTF-16 encoded file. Moreover, there is an easy and well
specified way to recognize and transcode UTF-16 encoded data to UTF-8.

I think the downside of this is that it could encourage the use UTF-16
encoded `requirements.txt` files *in addition* to UTF-8 encoded
files, and it would probably be nice to converge and standardize on
one encoding. One possible alternative to this PR is that we provide
a better error message. Another alternative is to ensure that a
`-o/--output` flag exists for all commands (neither `uv pip freeze` nor
`pip freeze` have such a flag) so that users can always write output
to a file without relying on their environment's piping behavior.
(Although this last alternative seems a little sad to me.)

It's also worth noting the [PEP-0508] doesn't seem to mention file
encoding at all. So I think from a "do the standards allow this"
perspective, this change is OK.

Finally, `pip` itself seems to work with UTF-16 encoded
`requirements.txt` files.

I think I personally overall lean towards supporting UTF-16 for
`requirements.txt` files. In part because I think it smoothes out the
UX a little bit, in part because there is no obvious specification
(that I'm aware of) that mandates that these files are UTF-8, and
finally in part because `pip` supports it too.

Fixes #1666, Fixes #2276

[PEP-0508]: https://peps.python.org/pep-0508/


---

_Comment by @BurntSushi on 2024-03-07 18:08_

cc @zanieb @mitsuhiko given discussion in #1666 about supporting UTF-16 encoded `requirements.txt` files.

cc @yashranjan1 [per request](https://github.com/astral-sh/uv/issues/2276#issuecomment-1983903059).

---

_@zanieb approved on 2024-03-07 18:09_

👍 I agree with your principles

---

_Review requested from @mitsuhiko by @BurntSushi on 2024-03-07 18:21_

---

_Review requested from @konstin by @BurntSushi on 2024-03-07 18:21_

---

_@charliermarsh approved on 2024-03-07 19:46_

---

_Comment by @charliermarsh on 2024-03-07 19:46_

I think it's wise to support these as long as PowerShell is going to use UTF-16 by default when piping -- that's a very common use-case.

---

_Review comment by @MichaReiser on `crates/uv-fs/src/lib.rs`:19 on 2024-03-07 21:01_

Are there call sites to this function for which we may not want to accept UTF-16? 

I suggest to at least document that this function supports UTF16 decoding.

---

_Review comment by @MichaReiser on `Cargo.toml`:44 on 2024-03-07 21:03_

Could you expand your reasoning for using `encoding_rs` (shamelessly adding his own crates ;) ) instead of implementing a "simple" BOM check to determine UTF-16 and using `String::from_utf16`?

---

_Review comment by @MichaReiser on `crates/uv-fs/src/lib.rs`:32 on 2024-03-07 21:05_

What's your reasoning behind spawning a background task for reading the file. My understanding of the use case is that a) we read the file as input before there's much async work anyway, b) the files are small text files. That makes me wonder if it's worth to decode and read the file on another thread (scheduling overhead etc).

---

_@MichaReiser reviewed on 2024-03-07 21:05_

---

_@BurntSushi reviewed on 2024-03-07 21:12_

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:19 on 2024-03-07 21:12_

Yeah, I'll probably split this into two functions so that callers can specifically opt into it.

---

_@BurntSushi reviewed on 2024-03-07 21:20_

---

_Review comment by @BurntSushi on `Cargo.toml`:44 on 2024-03-07 21:20_

`encoding_rs` isn't mine, but `encoding_rs_io` is. The latter puts an automatic BOM sniffing streaming reader on top of the former.

I suppose since we don't actually need streaming here, so this could be done more simply. Note though that `encoding_rs_io` will also seamlessly handle the UTF-8 BOM, which is not uncommon on Windows AFAIK. (Handling the UTF-8 BOM is basically just a matter of detecting it and then dropping it.)

I'll consider just using `std` for this. Honestly we have so many dependencies (including a dependency on `encoding_rs`) that I don't feel too bad about adding another...

---

_@BurntSushi reviewed on 2024-03-07 21:23_

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:32 on 2024-03-07 21:23_

It's an `async` function and file I/O is blocking. So IMO, that means it should be spawned as a blocking task. Moreover, the code being replaced was also spawning a blocking task (it was just done inside of `tokio`), so this is maintaining that status quo. It's plausible that this isn't necessary in a holistic sense, but this particular routine isn't necessarily limited to use during startup.

---

_@MichaReiser reviewed on 2024-03-07 21:26_

---

_Review comment by @MichaReiser on `Cargo.toml`:44 on 2024-03-07 21:26_

Hmm. Have we lost when even you are saying this

By the way. I'm not against the dependency. I was just wondering if it safes us like 3 branches or if more is involved (which seems to be the case)

---

_@MichaReiser approved on 2024-03-07 21:27_

---

_@BurntSushi reviewed on 2024-03-07 21:36_

---

_Review comment by @BurntSushi on `Cargo.toml`:44 on 2024-03-07 21:36_

Yeah it's really about supporting streaming. That's the main win of `encoding_rs_io`.

---

_Comment by @mitsuhiko on 2024-03-07 21:51_

~~I _believe_ that Microsoft is slowly phasing out UTF-16 output on the console. On my machine it only outputs UTF-8 (Windows 11). Might be worthwhile figuring out if this is really something that happens regularly and will continue to do so.~~

Now I realize that this is because i use `cmd.exe` and not powershell.

I'm not sure what to do here. It does seem weird to me that uv would support this encoding since nothing else does, but maybe this can be useful for the user. I do fully expect that this will create questions though about why those files don't work with other tools.

---

_Comment by @BurntSushi on 2024-03-07 21:59_

> I _believe_ that Microsoft is slowly phasing out UTF-16 output on the console. On my machine it only outputs UTF-8 (Windows 11). Might be worthwhile figuring out if this is really something that happens regularly and will continue to do so.
> 
> //EDIT: now I realize that this is because i use `cmd.exe` and not powershell

This is interesting because I think [Rust is the one who is writing UTF-16 to the console](https://github.com/rust-lang/rust/blob/52f8aec14c616387c5f793687f2d9026de6c78ca/library/std/src/sys/pal/windows/stdio.rs#L175-L229). Is that somehow getting transcoded back to UTF-8 for you?

---

_Comment by @mitsuhiko on 2024-03-07 22:02_

@BurntSushi this is what it does on my machine:

```
C:\Users\armin\Development\foo>cargo run
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target\debug\foo.exe`
Fußball

C:\Users\armin\Development\foo>cargo run > foo.txt
    Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target\debug\foo.exe`

C:\Users\armin\Development\foo>python -c "print(open('foo.txt', 'rb').read())"
b'Fu\xc3\x9fball\n'
```

I think on this machine the default code page is `65001` but I also just managed to get a shell running earlier that had an ISO encoding. Not sure how that happened.

---

_Review comment by @konstin on `crates/uv-fs/src/lib.rs`:32 on 2024-03-08 09:53_

In general, i agree, in this specific place we're not doing anything else in parallel (we're not a webserver where we have other clients meanwhile) and the work is tiny, so i'd go for simplicity. I'd probably do a `fs_err::tokio::read` and then convert it using the other reader, even the double read should be far to small to show up in profiles.

---

_Review comment by @konstin on `crates/uv/tests/pip_install.rs`:2431 on 2024-03-08 09:56_

Could you use `tomli`? This should be faster

---

_Review comment by @konstin on `crates/uv/tests/pip_install.rs`:2440 on 2024-03-08 09:56_

```suggestion
```

---

_Review comment by @konstin on `crates/uv/tests/pip_install.rs`:2460 on 2024-03-08 09:56_

```suggestion
```

---

_@konstin approved on 2024-03-08 09:57_

---

_Comment by @mitsuhiko on 2024-03-08 11:29_

> This is interesting because I think [Rust is the one who is writing UTF-16 to the console](https://github.com/rust-lang/rust/blob/52f8aec14c616387c5f793687f2d9026de6c78ca/library/std/src/sys/pal/windows/stdio.rs#L175-L229). Is that somehow getting transcoded back to UTF-8 for you?

So this machine is a few months old and I'm really not sure how any of this works. What I can see on my machine:

* cmd.exe: code page 65001
* powershell.exe: code page 65001
* cmd.exe launched from powershell: code page 1252

No clue why it does that. I tried on a VM and it always shows me code page 850. No matter though how, powershell always transcodes to UTF-16 but that is also reconfigurable with a flag (that probably nobody ever sets). Whatever API Python and Rust use to write, it clearly transcodes somehow somewhere.

EDIT: i unticked a beta box for utf-8 support in the `intl.cpl` and now getting consistently `code page 850` in all situations. So whatever I did, that was a thing that most people probably don't have.

---

_Comment by @MichaReiser on 2024-03-08 11:43_

> (that probably nobody ever sets).

I did :laughing: 

---

_@BurntSushi reviewed on 2024-03-08 13:32_

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:32 on 2024-03-08 13:32_

I went with @konstin's suggestion.

---

_Review comment by @BurntSushi on `Cargo.toml`:44 on 2024-03-08 13:34_

I looked into this, and it turns out the [UTF-16LE and UTF-16BE APIs](https://doc.rust-lang.org/std/string/struct.String.html#method.from_utf16be) are actually unstable. Do'h. That means doing this correctly is a fair bit more work than just checking the BOM and calling the right routine. We'd have to flip the byte order for an endian mismatch.

`encoding_rs_io` is a small battle tested dependency and it does the right thing for us automatically here, so I'm going to keep it.

---

_@BurntSushi reviewed on 2024-03-08 13:34_

---

_@BurntSushi reviewed on 2024-03-08 13:36_

---

_Review comment by @BurntSushi on `crates/uv-fs/src/lib.rs`:19 on 2024-03-08 13:36_

I split it into two functions. This was also being used to read `pyproject.toml`, and it's not obvious to me that we should support UTF-16 there. So that behavior remains unchanged.

---

_Comment by @BurntSushi on 2024-03-08 14:10_

I'm going to bring this in given most folks seem to be in support of it. Moreover, it addresses an easy footgun for an existing common pip workflow: `uv pip freeze > requirements.txt` is likely to produce a UTF-16 file on Windows, although there are perhaps configurations and environments where that is not true. Still, it doesn't cost us much to support UTF-16 here.

---

_Merged by @BurntSushi on 2024-03-08 14:10_

---

_Closed by @BurntSushi on 2024-03-08 14:10_

---

_Branch deleted on 2024-03-08 14:10_

---

_Label `bug` added by @BurntSushi on 2024-03-08 15:51_

---

_Label `windows` added by @BurntSushi on 2024-03-08 15:51_

---
