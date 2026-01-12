```yaml
number: 2688
title: "Simplify the `takes_value` API (range-based `takes_values`)"
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - M-breaking-change
  - A-builder
  - E-hard
assignees: []
created_at: 2021-08-13T19:28:14Z
updated_at: 2022-08-09T22:04:36Z
url: https://github.com/clap-rs/clap/issues/2688
synced_at: 2026-01-12T16:14:13Z
```

# Simplify the `takes_value` API (range-based `takes_values`)

---

_@epage_

### Discussed in https://github.com/clap-rs/clap/discussions/2676

<sup>Originally posted by **epage** August 10, 2021</sup>
Currently, we have
- `takes_value` (accept a value per occurrence)
- `multiple_values` (accept multiple values per occurrence)
- `min_values` (minimum number of values across all occurrences)
- `max_values` (maximum number of values across all occurrences)
- `number_of_values` (average number of values per occurrence)

We then have to clarify how these are related to each other, for example:
> NOTE: This does not implicitly set Arg::multiple(true). This is because -o val -o val is multiple occurrences but a single value and -o val1 val2 is a single occurrence with multiple values. For positional arguments this does set Arg::multiple(true) because there is no way to determine the difference between multiple occurrences and multiple values.

(taken from docs.rs, since updated to say `multiple_occurrences`).

I had already been brainstorming on this idea when kbknapp mentioned

> I think the crux of the problem is that we previously didn’t do a good job of articulating the difference between multiple_values, multiple_occurrences, and max_values_per_occurrence.  

I think we can simplify this problem down to
- `takes_values(num: impl clap::IntoRange)` with
  - `impl IntoRange for usize`
  - `impl IntoRange for` ...each range type
  - The range is for values per occurrence
- Remove all other previously mentioned functions as either
  - Redundant
  - Easily confused with `takes_values(range)` but works differently and the value of native support is relatively low

So you can do `take_values(0)`, `take_values(2)`, `take_values(1..)`, `take_values(0..=1)`
- Really, the motivation is to support all the various range types.  Taking in a number is just a convenience while we are at it.  I considered taking in a `bool`, but I think that is making it too magical and isn't as needed.
- This was named based on https://github.com/clap-rs/clap/discussions/2674 going through and `takes_values(1)` being the default.

While I was already talking with kbknapp, I brought this up and is thoughts were:

> That is an interesting idea! It feels like it might incur some cognitive load to understand, although once you see it it makes total sense. I'd love to find a way to play with this idea without throwing it front and center and in the API all of a sudden.

### Notes

For the question from kbknapp 

>  For example, which makes more sense for a positional argument; multiple_values or multiple_occurrences? I think you and I would say multiple_values, but I can totally see how someone may try multiple_occurrences. This was papered over by having just multiple previously and things just working. However, we’ve seen that strategy didn’t work in the long run as it created more ambiguity than it solved.

I would say occurrences, actually.  This would leave room for the two features to compose, like `multiple_occurrences(true).takes_values(2)`.  Not a common pattern but I think it speaks to an API when the features compose together in a natural way which this allows.

RE Delimiters

I would posit that common cases for multiple values are
- `0..=1`
- `2`
- `0..` or `1..` with a delimiter

We should raise awareness of `require_delimiter` in the docs for `take_values`.

### Benefits
- Clarifies our messaging on these settings plus occurrences since the focus is on fixed number of values or delimiters
- Reduces ambiguity around how the flags interact with occurrences and values
- Clear, concise way of specifying all related parameters rather twiddling with each one individual and there being confusion on how they interact
- Uses higher level Rust types rather than a bool and numbers just hanging around (which ties into the Rust API guidelines)
- I suspect this will also help us towards a more maintainable value/occurrence/inferring behavior.

---

_Label `C: args` added by @pksunkara on 2021-08-13 19:30_

---

_Label `T: new feature` added by @pksunkara on 2021-08-13 19:30_

---

_Referenced in [clap-rs/clap#2687](../../clap-rs/clap/issues/2687.md) on 2021-08-13 20:52_

---

_Added to milestone `4.0` by @pksunkara on 2021-08-14 01:04_

---

_Assigned to @epage by @epage on 2021-10-19 15:45_

---

_Label `E: breaking change` added by @epage on 2021-10-19 16:34_

---

_Referenced in [epage/clapng#198](../../epage/clapng/issues/198.md) on 2021-12-06 21:20_

---

_Label `C: args` removed by @epage on 2021-12-08 20:27_

---

_Label `A-builder` added by @epage on 2021-12-08 20:27_

---

_Label `A-derive` added by @epage on 2021-12-08 20:27_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:09_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:09_

---

_Label `A-derive` removed by @epage on 2021-12-13 17:10_

---

_Label `E-hard` added by @epage on 2021-12-13 17:10_

---

_Referenced in [clap-rs/clap#3678](../../clap-rs/clap/pulls/3678.md) on 2022-05-02 19:40_

---

_Assigned to @ldm0 by @ldm0 on 2022-05-02 19:55_

---

_Comment by @ldm0 on 2022-05-04 20:53_

I think this could be a non-breaking change by not changing existing api and behaviour.

Wanna add two methods: `takes_values`, `takes_values_total`

- Without multiple occurrences:

takes_values(X) == takes_values_total(X)
takes_values(1..) == .min_values(1)
takes_values(..4) == .max_values(3)
takes_values(1..4) == .min_values(1).max_values(3)
takes_values(1..=3) == .min_values(1).max_values(3)
takes_values(2) == number of values strictly equals to 2

- with multiple occurrences:

takes_values_total(2..) == total number of values doesn't smaller than 2
takes_values_total(..4) == total number of values doesn't exceed 3
takes_values_total(..=3) == total number of values doesn't exceed 3
takes_values_total(4) == total number of values equals to 4
takes_values(X) == takes_values_total(X) but for each occurrence

`min_values` `max_values`'s internal representation can be easily migrated(to share the same internal representation with `takes_values_total`).

But `number_of_values` cannot be migrated due to it's strange internal representation.

Currently `number_of_values`'s behaviour with multiple occurrences on is pretty strange: when `number_of_values(2)`, clap accepts `--flag 1 --flag 2 3 4`, since `4 % 2 == 0`. 

The reason is that before the refactor for implementing `grouped_values_of`, there is no way to check number of values in each value group. After `takes_values` is introduced, we can use `takes_values(2)` to accept `--flag 1 2 --flag 3 4` and reject `--flag 1 --flag 2 3 4`, which looks much better.

---

_Comment by @ldm0 on 2022-05-04 21:12_

After these apis are introduced, deriving `Vec<[T; N]>` and `[[T; N1]; N2]` could be implemented.

In clap 4, I think `number_of_values` should be removed due to it's strange behaviour.

---

_Comment by @epage on 2022-05-05 02:01_

There are some points that I think need some clarification
-  Your listing says "Without multiple occurrences", do you mean "Independent of multiple occurrences"?
- I appreciate showing what the new API maps to in the old API but I think it'd also help if you were explicit on what the intent is
- Could you explain what is wrong with `number_of_values` that you feel the new API is needed and why `number_of_values` should be deprecated?
- You mention deprecating `number_of_values`, would you also recommend deprecating `min_values` and `max_values`?
- How much of a use case is there for `takes_values_total`?  I'm used to limiting the number of entries for tuple-like arguments where its tied to the occurrence.  Should we instead shift the emphasis to user-side validation or [more generic validation mechanisms](https://github.com/clap-rs/clap/discussions/3476)?
  - As part of our effort to reduce complexity, binary size, and build times, we are wanting to emphasize creating building blocks for users and make things easy in the common cases rather than in every case.

---

_Comment by @ldm0 on 2022-05-05 03:43_

> Your listing says "Without multiple occurrences", do you mean "Independent of multiple occurrences"?

`without multiple occurrences == arg.multiple_occurrence(false)`

> Could you explain what is wrong with number_of_values that you feel the new API is needed and why number_of_values should be deprecated?

check sentences after "But `number_of_values` cannot be migrated due to it's strange internal representation."

> You mention deprecating number_of_values, would you also recommend deprecating min_values and max_values?

Yes, they can be deprecated, but it's not a must. Since they becomes shortcuts of `takes_values_total` after #3678 

> How much of a use case is there for takes_values_total?

Definitly needed. This has little binary size & build times influence and it's makes the logic completes. `min_values` and `max_values` are just shortcuts of them.

@epage 

---

_Comment by @epage on 2022-05-05 17:39_

There is a lot of being elided from the descriptions being given.  Instead of copying your past notes, could you re-write this addressing the following:

- What is the current behavior of `min_values` and `max_values` and what problems does it have?
- What is the current behavior of `number_of_values` and what problems does it have?
- What is the proposed behavior of the new functions? 
- How are the new functions related to the old functions?
- How do the new functions solve problems with existing functions?
- How do the new functions solve any other use cases, like the derive case?
- Why should we not solve the problem with other existing or planned features?
  - We are not just targeting binary size and compile times but providing an API surface where people can find the features they need.   

I just want to re-emphasize that communicating our thoughts and recording them down is important in a collaborative open source project.  We need to get each other on the same page and help people in the future understand the motivation for why things were done.

---

_Comment by @ldm0 on 2022-05-06 03:46_

1. `min_values(N)` == `takes_value_total(N..)` `max_values(N)` == `takes_value_total(1..=N)`, and the only problem is that their functionaliry are duplicated. They can be removed, but I have no bias.
2. Currently number_of_values's behaviour with multiple occurrences on is pretty strange: when number_of_values(2), clap accepts --flag 1 --flag 2 3 4, since 4 % 2 == 0. I dislike it.
3. I think this is clear:
  - without multiple occurrences:
  takes_values(X) == takes_values_total(X)
  takes_values(1..) == .min_values(1)
  takes_values(..4) == .max_values(3)
  takes_values(1..4) == .min_values(1).max_values(3)
  takes_values(1..=3) == .min_values(1).max_values(3)
  takes_values(2) == number of values strictly equals to 2
  - with multiple occurrences:
  takes_values_total(2..) == total number of values doesn't smaller than 2
  takes_values_total(..4) == total number of values doesn't exceed 3
  takes_values_total(..=3) == total number of values doesn't exceed 3
  takes_values_total(4) == total number of values equals to 4
  takes_values(X) == takes_values_total(X) but for each occurrence
4. Stated above。
5. Currently there is no way to accept `--flag 1 2 --flag 3 4` and reject `--flag 1 --flag 2 3 4`, the new api makes it possible. And also, the new api is not for problem solving, it's for simplicity.
6. It makes deriving `Vec<[T; N]>` and `[[T; N1]; N2]` possible.
7. The api is simple enough and expressive enough to be added. And people can easily understand them(compared to `number_of_values`)

---

_Comment by @epage on 2022-05-06 15:10_

Please take the time to write up a clear description rather than repeating what has previously been said.  If it feels like it'll take a long time to do so, consider how long having unclear explanations has already taken.

Think in terms of a random clap user wanting to look at this proposal to understand what is happening.  How well can they understand it?

> Currently number_of_values's behaviour with multiple occurrences on is pretty strange: when number_of_values(2), clap accepts --flag 1 --flag 2 3 4, since 4 % 2 == 0. I dislike it.

This is repeating what was said before. This is an example which help to clarify descriptions but aren't sufficient to describe behavior on their own as it leaves the user to infer what is happening which leaves room for misunderstanding

> I think this is clear:

The fact that this is now the third time I'm asking for a re-explanation I think demonstrates that this is not an effective way of communicating things
- Its describing behavior conditioned on what `multiple_occurrences`  is being set to but doesn't describe how the functions behavior with the opposite setting
- Its describing the API in terms of other parts of the API when I'm looking for a description of intent where it stands alone so there is no ambiguity.
- If `takes_values(1..) == .min_values(1)` and `min_values(N) == takes_value_total(N..)`, then doesn't that mean that `takes_values(1..) == takes_value_total(1..)`?

> It makes deriving Vec<[T; N]> and [[T; N1]; N2] possible.

I can't see how the proposed helps with this because as described, it doesn't seem to handle this.
- See my earlier comment "If `takes_values(1..) == .min_values(1)` and `min_values(N) == takes_value_total(N..)`, then doesn't that mean that `takes_values(1..) == takes_value_total(1..)`?"



---

_Comment by @epage on 2022-05-06 18:21_

How do these new functions interact with `multiple_values` and `takes_value`?

Working on some other Issues reminded me that its not just these flags but how some of them compose together

---

_Referenced in [clap-rs/clap#3702](../../clap-rs/clap/pulls/3702.md) on 2022-05-06 19:41_

---

_Referenced in [clap-rs/clap#3259](../../clap-rs/clap/issues/3259.md) on 2022-05-14 02:39_

---

_Comment by @tmccombs on 2022-06-15 07:32_

Are the issues with the current options resolved by using an action of `ArgAction::Append`? 

For example, if I had:

```
Arg::new("test")
    .short('t')
    .action(ArgAction::Append)
    .min_values(2)
    .max_values(3)
```

would that mean that `--t 1 2 -t 3 4 -t 3 5 6' is valid, and result in a value of `vec![vec!["1", "2"], vec!["3", "4"], vec!["3", "5", "5"]]` and  `-t 1 -t 2 3` is invalid? 

The documentation isn't very clear on this.

---

_Comment by @tmccombs on 2022-06-15 08:21_

Looking through the source code, it looks like it probably doesn't. So I'll try and answer the questions @epage listed in a way that doesn't depend on the new API proposed.

>  * What is the current behavior of `min_values` and `max_values` and what problems does it have?

`min_values` and `max_values` set limits on the total number of values, rather than on the number of values in a single occurrence of the option. So if an option is supplied multiple times, these limits apply to the sum of the number of values in all such occurences. Clap doesn't currently have a way to express limits on the number of values supplied to a single occurrence when `multiple_occurences` is true, or the `Append` action is used.  This is problematic in a number of scenarios such as: 

- If an option can be supplied than once, and each occurence must have at least one (and possibly more) value supplied. Should generally be used with a delimiter, terminator, etc.
- If an option can be optionally supplied with a value for each occurence
- If an option can have at most N values (for some value of n) per occurence

See also https://github.com/clap-rs/clap/issues/3542

>  * What is the current behavior of `number_of_values` and what problems does it have?

Currently, if `number_of_values` is supplied, and `multiple_occurences` is true, or the `Append` action is used, then rather than requiring each occurrence to have that number of values, it requires that the total number of values is a multiple of that number. 

For example, if you supply a `number_of_values` of `2` for an option that can occur multiple times, then `-t 1 -t 3 4`, `-t 1 2 3 4`, `-t -t 1 -t 2 3 4`, and `-t 1 -t 2 -t 3 -t 4` are all considered valid, which is almost certainly not what you want.

I suspect this behavior wasn't intentional, and could probably be considered a bug. However, even if it is fixed, it is somewhat confusing that `number_of_values` is per-occurrence, while `min_values` and `max_values` are total.

> * What is the proposed behavior of the new functions?

From what I understand:

`takes_values_total` takes a possibly open range that specifies the minium and maximum number of values combined across all occurrences. If the number of values in total is outside of the specified range, the arguments are invalid. This is equivalent to a combination of the current`min_values` and `max_values`.

`takes_values` specifies a (possibly open) range of the number of values _per occurrence_. that is each occurrence of the option must have a number of values that lies within the range. If the number of values given to any one occurrence of the option falls outside the given range, it should be considered invalid.


> * How are the new functions related to the old functions?

I'm not sure there is much to add here. The old functions could be deprecated and implemented in terms of the new ones. And removed in a backwards incompatible release. Or possibly they are considered valuable enough to keep. It's also possible the in a backwards incompatible release, the behavior of these functions is changed to be per-occurrence instead of total. Alternatively, new `min_per_occurence`, `max_per_occurence`, and `number_of_values_per_occurence` (bikeshed the names) could be added.

> * How do the new functions solve problems with existing functions?

`takes_values_total` does not solve the problems mentioned above, but might be a slightly nicer API. 

`takes_values` provides an api to constrain the number of arguments on a per-occurrence basis when multiple occurrences of the same option are allowed. 


> * How do the new functions solve any other use cases, like the derive case?

I'm not sure.

> * Why should we not solve the problem with other existing or planned features?

I don't really care how the problem is solved, as long as there is a way to have constraints on the number of values supplied per-occurrence. I mentioned some other possibilities above. Perhaps another way to do it would be to have a different `Action` variant, which would switch into a mode where `min_values`, `max_values`, and `number_of_values` applies to each occurrence rather than the total (or maybe change it so the existing `Append` action behaves that way in v4?)

I would like to mention that the current behavior of `min_values` bit me, and it took me quite a while to figure out what was going on.  At the very least the documentation for `min_values` and `max_values` should be updated to explain that it is the *total* number of values, not the values for a single occurrence.


---

_Comment by @epage on 2022-06-15 17:03_

> I don't really care how the problem is solved, as long as there is a way to have constraints on the number of values supplied per-occurrence

I've updated my original proposal to clarify that `takes_values(range)` is intended to be per occurrence.

And while I appreciate the thorough investigation into ldm0's work, I think the more important aspect is to understand their intent and working through that to come to agreement before they move forward with their implementation.

---

_Comment by @mzagrabe on 2022-07-07 18:32_

@epage What do you think of having a min_occurrences() function in similar fashion to min_values()? This could be used for ArgAction::Append options.

---

_Comment by @epage on 2022-07-11 20:41_

@mzagrabe what is the motivation / use-case for having a baked in `min_occurrences`?  How does that compare to just using either `required` or https://github.com/clap-rs/clap/issues/3008?

---

_Referenced in [clap-rs/clap#3988](../../clap-rs/clap/pulls/3988.md) on 2022-07-25 18:25_

---

_Referenced in [clap-rs/clap#3999](../../clap-rs/clap/pulls/3999.md) on 2022-07-28 20:01_

---

_Referenced in [clap-rs/clap#4001](../../clap-rs/clap/pulls/4001.md) on 2022-07-29 02:48_

---

_Comment by @epage on 2022-08-03 13:33_

In master (v4) I've started work on the original proposal in this issue.

Current status:
- [x] Shift focus from `takes_value` / `multiple_values` to `action` in docs
- [x] Modify `number_of_values` to take a range
- [x] Change `number_of_values` to be per occurrence
- [x] Change `number_of_values` to be `num_args` (ie how many `argv` items are consumed)
- [x] Update `arg!` and derive to use `number_of_values` for `Option<Option<T>>`
- [x] Remove `min_values` and `max_values` from the API
- [x] Remove `takes_value` and `multiple_values` from API
- [x] Resolve how we use `takes_value` and `multiple_values` internally
- [x] Re-evaluate `use_value_delimiter` and `require_value_delimiter` with the new `num_args` behavior
- [x] Re-evaluate where `num_args` shows up in rustdoc output
- [x] Update tutorial to not primarily use `arg!` to better teach actions, drawing attention away from `num_args`

Somewhat related
- [x] Cleaned up rendering of arg values
- [x] Show `...` for `ArgAction::Count` for args in usage
- [x] Correctly show optional values in usage for positionals and args

---

_Referenced in [clap-rs/clap#4020](../../clap-rs/clap/pulls/4020.md) on 2022-08-03 15:36_

---

_Referenced in [clap-rs/clap#4022](../../clap-rs/clap/pulls/4022.md) on 2022-08-03 19:21_

---

_Referenced in [clap-rs/clap#4023](../../clap-rs/clap/pulls/4023.md) on 2022-08-03 19:47_

---

_Referenced in [clap-rs/clap#4024](../../clap-rs/clap/pulls/4024.md) on 2022-08-04 04:50_

---

_Referenced in [clap-rs/clap#4025](../../clap-rs/clap/pulls/4025.md) on 2022-08-04 05:17_

---

_Referenced in [clap-rs/clap#4026](../../clap-rs/clap/pulls/4026.md) on 2022-08-04 05:31_

---

_Referenced in [clap-rs/clap#4028](../../clap-rs/clap/pulls/4028.md) on 2022-08-04 14:40_

---

_Comment by @epage on 2022-08-04 14:46_

While not quite done, we've dropped about 2k from `.text` through this work

This was done by running `cargo bloat --release --example cargo-example --features cargo` against 11883b6 and #4028

---

_Referenced in [clap-rs/clap#4031](../../clap-rs/clap/pulls/4031.md) on 2022-08-04 21:11_

---

_Comment by @epage on 2022-08-09 22:04_

Forgot to call out that #4050 took care of the tutorial

---

_Closed by @epage on 2022-08-09 22:04_

---

_Referenced in [clap-rs/clap#3542](../../clap-rs/clap/issues/3542.md) on 2022-08-09 22:05_

---

_Referenced in [quickwit-oss/quickwit#2236](../../quickwit-oss/quickwit/issues/2236.md) on 2022-11-03 15:02_

---

_Referenced in [FamilySearch/pewpew#110](../../FamilySearch/pewpew/pulls/110.md) on 2023-01-27 21:39_

---

_Referenced in [24seconds/rust-cli-pomodoro#154](../../24seconds/rust-cli-pomodoro/pulls/154.md) on 2023-03-27 02:04_

---

_Referenced in [roc-lang/roc#5405](../../roc-lang/roc/pulls/5405.md) on 2023-05-13 16:46_

---

_Referenced in [kcl-lang/kcl#560](../../kcl-lang/kcl/pulls/560.md) on 2023-05-31 12:21_

---
