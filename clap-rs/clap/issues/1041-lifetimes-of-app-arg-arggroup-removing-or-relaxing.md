```yaml
number: 1041
title: "Lifetimes of App, Arg, ArgGroup: removing or relaxing"
type: issue
state: closed
author: Rupsbant
labels:
  - C-enhancement
  - M-breaking-change
  - A-builder
  - E-medium
assignees: []
created_at: 2017-09-07T13:32:18Z
updated_at: 2022-08-22T21:48:16Z
url: https://github.com/clap-rs/clap/issues/1041
synced_at: 2026-01-12T16:14:10Z
```

# Lifetimes of App, Arg, ArgGroup: removing or relaxing

---

_@Rupsbant_

### Affected Version of clap
All 2.x. Breaking API change

### Expected Behavior Summary
I want to decouple groups of options to several structs. I want to have a pair functions: one that adds arguments to an App with generated names, one that generates a structure with the added arguments.

### Actual Behavior Summary
Uncompilable: the types don't allow to create String in a function and return a borrow.

### Sample code
```
struct A {
...
}
impl A {
  fn arguments<'a, 'b>(prefix: &str) -> Vec<Arg<'a, 'b>> {
    let name = format!("{}.name", prefix);
    ...
  }
}
```
### Proposed solution
Since clap is dependent on its speed it's probably not an option to force 'String' instead of '&str'. However I think Cow<str> should give enough options.

### Alternative for me
Create a struct `AParse` to contain the owned strings, split the function `arguments` in two parts. A can generate an `AParse`struct before creating the App object.

---

_Comment by @kbknapp on 2017-09-14 14:55_

This is a focus of 3.x so stay tuned. I've been able to get some work done in spurts during lulls in my day job (which have been few and far between this year). Watch the 3x-master branch for details :wink: 

---

_Label `W: 3.x` added by @kbknapp on 2017-09-14 14:55_

---

_Comment by @kbknapp on 2017-11-12 20:07_

Personally, I'd like to move away from storing `&str`s inside the `Arg`s as the name. This would solve lifetime issues as I want 3.x to be able to remove the `Arg|App<'a, 'b>` lifetimes. This could be solved by moving to owned `String`s which takes a slight performance hit. 

## History

clap has two lifetimes for both `Arg` and `App` which reference the name, or *key* of the struct and data related to displaying of the help message or errors. clap uses `name` and `key` interchangeably.

clap used to only accept `&'static str`s because that's what 99% of users use. However, some users *do* dynamically create args and such, so we moved to `&'key str` and `&'data str` to refer to the name/key and display data. There reason these two are separate is because tying them to the same lifetime was cumbersome when one wanted to use `'static` for one, but dynamically mess with the other.

### Why not use String?

clap originally *could* have used an owned `String` to hold both the name and data, however for performance reasons we elected not to. I'm considering moving to String for the name/key as these are typically small, but am reluctant to use `String` for the data piece as this could be huge and I have yet to see someone who uses anything other than `&'static str` for such data. 

## Moving Forward

I want to get away from using `&str` for the name/key as this is preventing several key features like the ability to create `no-*` args on the fly. 

We *could* move to `Cow<'static, str>` for data piece, but so far all attempts to do this have been a hassle. However, I'm OK with it being a hassle if the end result for *users* is better.

I'm open to ideas.

---

_Referenced in [clap-rs/clap#1104](../../clap-rs/clap/issues/1104.md) on 2017-11-12 20:11_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-13 02:18_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 02:05_

---

_Assigned to @kbknapp by @kbknapp on 2018-07-22 02:41_

---

_Label `T: enhancement` added by @kbknapp on 2018-07-22 02:42_

---

_Label `E: breaking change` added by @kbknapp on 2018-07-22 02:42_

---

_Label `P2: need to have` added by @kbknapp on 2018-07-22 02:42_

---

_Label `D: hard` added by @kbknapp on 2018-07-22 02:42_

---

_Referenced in [sharkdp/pastel#84](../../sharkdp/pastel/pulls/84.md) on 2019-08-30 21:28_

---

_Removed from milestone `v3-alpha.2` by @pksunkara on 2020-02-01 07:45_

---

_Added to milestone `v3.0` by @pksunkara on 2020-02-01 07:45_

---

_Referenced in [clap-rs/clap#1728](../../clap-rs/clap/pulls/1728.md) on 2020-03-05 18:55_

---

_Referenced in [PyO3/pyo3#1118](../../PyO3/pyo3/pulls/1118.md) on 2020-08-26 23:17_

---

_Referenced in [ramsayleung/rspotify#161](../../ramsayleung/rspotify/pulls/161.md) on 2020-12-17 17:26_

---

_Referenced in [ducaale/xh#142](../../ducaale/xh/issues/142.md) on 2021-05-23 14:19_

---

_Label `W: 3.x` removed by @pksunkara on 2021-05-26 11:14_

---

_Removed from milestone `3.0` by @pksunkara on 2021-05-26 11:14_

---

_Comment by @epage on 2021-07-06 13:41_

I wonder if [`kstring`](https://docs.rs/kstring/1.0.1/kstring/) would be a good fit.  Internally, its an `Enum` with variants for
- Owned String
- `&'static str`
- Inline strings, when small

I implemented this for `liquid` template engine for generating the data context to pass in when rendering.  I had a similar situation where most data was `'static` but not all.  I wanted a more ergonomic `Cow`.  I then also threw in small string optimization to bypass `String` where possible, because most of my strings were short.

---

_Added to milestone `4.0` by @pksunkara on 2021-07-06 18:56_

---

_Comment by @pksunkara on 2021-07-06 18:56_

@epage Thanks for pointing it out. Do you know if there are any performance changes?

---

_Comment by @epage on 2021-07-06 21:22_

I just ran some of the benchmarks to double check.

For a rough comparison, it looks like `Cow::Borrowed` < `KString::from_static` < `KString::from_string` (small string, 16 or less) < `String::from` < `KString::from_string` (large string, 17+)

In terms of memory usage:
```
---- cow::test::test_size stdout ----
Cow: 32
KStringCow: 32

---- r#ref::test::test_size stdout ----
str: 16
KStringRef: 24

---- string::test::test_size stdout ----
String: 24
Box<str>: 16
Box<Box<str>>: 8
KString: 24
```

---

_Comment by @pksunkara on 2021-07-06 21:26_

My bad. Let me rephrase, I am concerned about the performance issues related to running time because of the indirections introduced by kstring or similar such library. Also pure `Cow` implementation has that IIUC (https://github.com/clap-rs/clap/pull/2504).

I haven't actually worked with something like this before, so my entire theory can be wrong.

---

_Comment by @epage on 2021-07-06 21:46_

My first comment was meant to give a rough idea of what runtime performance looks like compared to other options according to `kstring`s benchmarks.  If there is interest in this direction, I can go implement support and run clap's benchmarks.  I didn't want to spend time on something if there wasn't interest.

---

_Comment by @pksunkara on 2021-07-06 21:55_

You don't have to implement it for clap and run the benchmarks. What I am looking for is a small bench implemented purely on a struct containing a string with large number of iterations doing something which involves accessing the string inside the struct and cloning the struct. 

* With a lifetime str:

  ```
  struct Test<'a> {
    inner: &'a str;
  }
  ```

* Non-ergonomic with cow:

  ```
  struct Test<'a> {
    inner: Cow<'a, str>;
  }
  ```

* With kstring

Once we have these, if we make sure that the performance is not too bad, we can go ahead with implementation.

---

If the performance is concerning, we can still implement it but should. provide cargo features to let user pick and choose. I haven't looked into kstring, but hopefully it allows something like this.

---

_Comment by @epage on 2021-07-08 14:35_

https://github.com/cobalt-org/kstring/pull/6 has the code used for the benchmarks as well as the numbers with some basic analysis.

The summary is:
- `clone` of strings <=16 bytes, `kstring` is faster than `String` but slower than `Cow::Borrowed`
- `clone` of strings >16 bytes, `kstring` is slower than `Cow::Owned`
- Derefs are pretty much the same across the board.

For `clone` there seems to be a 10ns overhead that, if it can be diagnosed and fixed, would close the gap with `Cow`.  I suspect this is fixable or else we would see this same slowdown on deref.

EDIT: I've not updated my charts above but I have dropped the overhead from 10ns to 5ns by **not** inlining `clone()`.  My guess is we were thrashing the icache.

---

_Comment by @kbknapp on 2021-07-08 14:48_

The only "downside" my naive glance at `kstring` presents to `clap` specifically in regards to the spirit of this issue is that not *all* strings of the App/Arg/etc structs could be made into `kstring`s, i.e. the help/about strings can and often do extend well beyond the 16 bytes mentioned (often into the hundreds or thousands of bytes). So if we still have *some* lifetimes around for those help/about strings, the signature of the struct ultimately remains unchanged for end users.

Performance is another matter entirely though, and we may well benefit from something other than `&str`, even if the struct signature doesn't change.

---

_Comment by @epage on 2021-07-08 14:59_

`kstring::KString` overflows from inline strings to the heap, so it can handle any arbitrary string.  Its effectively a `enum { Static(&'static), Inline(...), Heap(Box<str>) }`.

---

_Comment by @pksunkara on 2021-07-09 11:16_

The performance actually looks good to me.

If we were to use this, kstring needs to implement `From` for all inputs we want to take. I think `&'help str` is missing.

---

_Comment by @epage on 2021-07-09 13:52_

`kstring::KString` [implements From](https://docs.rs/kstring/1.0.4/kstring/struct.KString.html#impl-From%3C%26%27s%20Box%3Cstr%2C%20Global%3E%3E) for
- `&'static str` instead of `&str`
- `String`
- `&String` (now, looks like I missed this case before)

My assumption is that clap argument types break down as:
- 80% `&'static`
- 10% `String` or `&String` (from `std::env:var`, `format!`)
- 5% yaml (which internally turns into `&String`)
- 5% `&str`

If that lines up with others thoughts, then `KString` covers the majority cases and makes the optimal case easy.  In rare cases, someone will have to reach for `KString::from_ref`.

EDIT: If not, I guess expanding the scope of `From<&str>` could be put behind a non-default feature flag since it would be expanding what types are expected and only have an impact on performance and only then when going to the heap.

---

_Referenced in [clap-rs/clap#853](../../clap-rs/clap/issues/853.md) on 2021-07-20 17:26_

---

_Referenced in [clap-rs/clap#918](../../clap-rs/clap/issues/918.md) on 2021-07-20 17:31_

---

_Referenced in [clap-rs/clap#2150](../../clap-rs/clap/issues/2150.md) on 2021-10-04 17:53_

---

_Referenced in [clap-rs/clap#2870](../../clap-rs/clap/issues/2870.md) on 2021-10-14 00:35_

---

_Referenced in [clap-rs/clap#2976](../../clap-rs/clap/pulls/2976.md) on 2021-11-01 14:33_

---

_Referenced in [epage/clapng#70](../../epage/clapng/issues/70.md) on 2021-12-06 16:32_

---

_Referenced in [epage/clapng#79](../../epage/clapng/issues/79.md) on 2021-12-06 16:35_

---

_Label `P2: need to have` removed by @epage on 2021-12-09 17:10_

---

_Label `A-builder` added by @epage on 2021-12-09 17:10_

---

_Label `E-hard` removed by @epage on 2021-12-09 17:10_

---

_Label `E-medium` added by @epage on 2021-12-09 17:10_

---

_Referenced in [clap-rs/clap#1788](../../clap-rs/clap/issues/1788.md) on 2021-12-09 21:22_

---

_Referenced in [clap-rs/clap#3287](../../clap-rs/clap/issues/3287.md) on 2022-01-12 15:30_

---

_Referenced in [aobatact/clap-serde#1](../../aobatact/clap-serde/issues/1.md) on 2022-01-21 15:33_

---

_Comment by @epage on 2022-02-02 20:55_

When we do this, we should keep the workarounds in https://github.com/clap-rs/clap/issues/1104 in mind and have an `ToId` trait that we use on builder functions that can be implemented by an enum for type chekcing.

---

_Referenced in [clap-rs/clap#1206](../../clap-rs/clap/issues/1206.md) on 2022-05-18 11:33_

---

_Referenced in [clap-rs/clap#4035](../../clap-rs/clap/issues/4035.md) on 2022-08-09 18:24_

---

_Referenced in [clap-rs/clap#4072](../../clap-rs/clap/pulls/4072.md) on 2022-08-12 21:48_

---

_Referenced in [clap-rs/clap#4081](../../clap-rs/clap/pulls/4081.md) on 2022-08-15 18:27_

---

_Referenced in [clap-rs/clap#4084](../../clap-rs/clap/pulls/4084.md) on 2022-08-16 16:54_

---

_Referenced in [clap-rs/clap#4096](../../clap-rs/clap/pulls/4096.md) on 2022-08-19 16:55_

---

_Referenced in [clap-rs/clap#4097](../../clap-rs/clap/pulls/4097.md) on 2022-08-19 17:13_

---

_Referenced in [clap-rs/clap#4103](../../clap-rs/clap/pulls/4103.md) on 2022-08-22 21:02_

---

_Closed by @epage on 2022-08-22 21:48_

---

_Referenced in [solana-labs/solana-program-library#6376](../../solana-labs/solana-program-library/pulls/6376.md) on 2024-03-12 01:15_

---
