```yaml
number: 79
title: Inconsistent interpretation of .gitignore
type: issue
state: closed
author: Geal
labels: []
assignees: []
created_at: 2016-09-25T09:00:21Z
updated_at: 2016-09-26T09:04:15Z
url: https://github.com/BurntSushi/ripgrep/issues/79
synced_at: 2026-01-12T18:23:11Z
```

# Inconsistent interpretation of .gitignore

---

_@Geal_

Hi!

First things first, great work on rg, it's amazingly fast :)

I got an issue with .gitignore usage, here's how you can reproduce:
1. get a local clone of https://github.com/rust-lang/rust
2. go in the `src/` subdirectory
3. With `grep`:

```
$ grep -nr "Index<Ran" .
./libcore/slice.rs:628:impl<T> ops::Index<RangeFull> for [T] {
./test/run-pass/slice.rs:23:impl Index<Range<Foo>> for Foo {
./test/run-pass/slice.rs:30:impl Index<RangeTo<Foo>> for Foo {
./test/run-pass/slice.rs:37:impl Index<RangeFrom<Foo>> for Foo {
./test/run-pass/slice.rs:44:impl Index<RangeFull> for Foo {
```
1. With `rg`:

```
$ rg -n "Index<Ran" .
libcore/slice.rs
628:impl<T> ops::Index<RangeFull> for [T] {
```
1. With `rg --no-ignore`:

```
$ rg --no-ignore -n "Index<Ran" .
libcore/slice.rs
628:impl<T> ops::Index<RangeFull> for [T] {

test/run-pass/slice.rs
23:impl Index<Range<Foo>> for Foo {
30:impl Index<RangeTo<Foo>> for Foo {
37:impl Index<RangeFrom<Foo>> for Foo {
44:impl Index<RangeFull> for Foo {
```

The `.gitignore` file contains `/test/` at [line 84](https://github.com/rust-lang/rust/blob/master/.gitignore#L84) so rg should ignore `<ROOT>/test/`, not `<ROOT>/src/test`.


---

_Comment by @BurntSushi on 2016-09-25 12:59_

This looks like a duplicate of some bugs I've fixed recently, specifically with respect to anchored patterns such as `/test/`. I'll get a new release out today. (Or you can try compiling master to confirm it is fixed.)


---

_Comment by @BurntSushi on 2016-09-25 18:33_

I can indeed confirm that this is fixed in current master:

```
[andrew@Cheetah src] rg "Index<Ran"
test/run-pass/slice.rs
23:impl Index<Range<Foo>> for Foo {
30:impl Index<RangeTo<Foo>> for Foo {
37:impl Index<RangeFrom<Foo>> for Foo {
44:impl Index<RangeFull> for Foo {

libcore/slice.rs
621:impl<T> ops::Index<RangeFull> for [T] {
```

Specifically, this is a dupe of #25.


---

_Closed by @BurntSushi on 2016-09-25 18:33_

---

_Comment by @Geal on 2016-09-26 09:04_

ok, thanks! I'll update.

On Sun, Sep 25, 2016 at 8:33 PM, Andrew Gallant notifications@github.com
wrote:

> Closed #79 https://github.com/BurntSushi/ripgrep/issues/79.
> 
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/79#event-801571210, or mute
> the thread
> https://github.com/notifications/unsubscribe-auth/AAHSADGr28KKGxoMs9MpJw8_9vfGARM7ks5qtr51gaJpZM4KF2zd
> .

## 

http://geoffroycouprie.com/


---
