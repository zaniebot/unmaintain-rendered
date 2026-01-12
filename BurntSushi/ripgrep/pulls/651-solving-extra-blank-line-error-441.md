```yaml
number: 651
title: "Solving extra blank line error #441"
type: pull_request
state: closed
author: deg4uss3r
labels: []
assignees: []
base: master
head: master
created_at: 2017-10-23T20:13:01Z
updated_at: 2017-11-22T11:58:59Z
url: https://github.com/BurntSushi/ripgrep/pull/651
synced_at: 2026-01-12T18:23:13Z
```

# Solving extra blank line error #441

---

_@deg4uss3r_

Hi guys taking a stab at #441 

There are 3 tests that are failing:
* search_stream::tests::after_context_invert_one1
* search_stream::tests::before_context_invert_one1
* search_stream::tests::invert_match_line_numbers

This has to do with the way the tests are passed, since a command line test produces the correct assertion: 
```
> ../target/debug/rg 'Sherlock' -v -C 1 ./sherlock 
1-For the Doctor Watsons of this world, as opposed to the Sherlock
2:Holmeses, success in the province of detective work must always
3-be, to a very large extent, the result of luck. Sherlock Holmes
4:can extract a clew from a wisp of straw or a flake of cigar ash;
5:but Doctor Watson has to have it taken out for him and dusted,
6:and exhibited clearly, with a label attached.
```

---

_Review comment by @BurntSushi on `src/printer.rs`:311 on 2017-10-23 20:18_

Sorry, but this just won't work on multiple levels. Firstly, `buf` doesn't necessarily contain the entire contents of the file being searched (this *is* true for the memory map searcher only). Secondly, `buf.lines().count()` is recounting all of the lines in the buffer **every time a match is written**, which would be a devastating performance regression.

---

_@BurntSushi requested changes on 2017-10-23 20:19_

Thanks for trying, I really appreciate it! Unfortunately, fixing this bug requires quite a bit more work. You need to know when you're at the end of the input, and you need to know it without impacting performance.

---

_@BurntSushi reviewed on 2017-10-23 20:20_

---

_Review comment by @BurntSushi on `src/printer.rs`:311 on 2017-10-23 20:20_

Third, if the end user disabled line numbers, then this check won't run at all.

---

_Closed by @BurntSushi on 2017-11-22 11:58_

---
