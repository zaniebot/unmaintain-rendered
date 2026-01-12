```yaml
number: 2431
title: "[`pyupgrade`]: Removes quotes from annotations"
type: pull_request
state: merged
author: colin99d
labels:
  - rule
assignees: []
merged: true
base: main
head: QuotedAnnotations
created_at: 2023-02-01T03:12:20Z
updated_at: 2023-02-05T14:43:18Z
url: https://github.com/astral-sh/ruff/pull/2431
synced_at: 2026-01-12T15:55:08Z
```

# [`pyupgrade`]: Removes quotes from annotations

---

_@colin99d_

Fixes #827 

---

_Renamed from "[pyupgrade]: Removes quotes from annotations" to "[`pyupgrade`]: Removes quotes from annotations" by @colin99d on 2023-02-01 17:32_

---

_Marked ready for review by @colin99d on 2023-02-04 02:58_

---

_Comment by @colin99d on 2023-02-04 03:21_

@charliermarsh this just needs your input on how to handle rustpython's lack of an Index token (I highlight more in the comments). Feel free to let me know how we should handle it.

---

_Label `rule` added by @charliermarsh on 2023-02-04 21:58_

---

_Comment by @charliermarsh on 2023-02-04 22:04_

Am I correct that the way this works is by passing the rule each function definition, having it identify all the stringified types from the body, and then stripping the quotes?

I'm wondering if we can simplify things here, since we _already_ have to solve this problem in `Checker`. In `Checker`, we track all the deferred string annotations, so that we can validate them at the very end (grep for `in_deferred_string_type_definition`). So we have logic for detecting whether we're in an annotation-compatible place, and we have special-cases for all the `typing` stuff.

Could we instead leverage that? Could we just pass every item in `in_deferred_string_type_definition` to a method that removes the quotes, and generate a fix from that? (i.e., in `check_deferred_string_type_definitions`, for each `expression` string, pass it to the new rule that just de-quotes it?) That way, we wouldn't have to reimplement all of the detection, and we'd have a centralized implementation. But I may be overlooking some of the details here.


---

_Comment by @colin99d on 2023-02-04 22:40_

> check_deferred_string_type_definitions

I'm not 100% sure on the answer to this question, but let me dig in and I will find out.

---

_Comment by @colin99d on 2023-02-04 23:38_

@charliermarsh I implemented a rough POC with your idea. I see two issues:

1. I am not sure how to check for future annotations
2. It seems to be too aggressive, and change a bunch of my "do NOT change" items.

Do you know if there is a simple solution to these things? If so I would love to switch over.

---

_Comment by @charliermarsh on 2023-02-04 23:46_

> I am not sure how to check for future annotations

I think we have a flag for that, `self.annotations_future_enabled`, on `Checker`.

> It seems to be too aggressive, and change a bunch of my "do NOT change" items.

Anything that _shouldn't_ change, and is being changed, might be a bug in the code today. Grep for `Some(Callable::TypedDict) => {` in `ast.rs`. We handle each case and try to either mark the thing as an annotation or not.




---

_Comment by @colin99d on 2023-02-04 23:53_

The `self.annotations_future_enabled` worked perfect when calling rules the normal way, but now that I am in `check_deferred_string_type_definitions`, the method does not work anymore.

Will dig into the other issue further.

---

_Comment by @colin99d on 2023-02-05 00:03_

It looks to me like the existing functionality just checks for a certain Type, while what I built has to decide when a string needs to be converted. Unless there is something I am missing, I don't think I can use what is built to replace what I created. Let me know if you agree, and I can revert my changes.

---

_Comment by @charliermarsh on 2023-02-05 00:44_

It feels like the exact same problem... but I may still be misunderstanding. In the Checker, we have to identify all strings that should be treated as deferred type annotations, so that we can process them later (and find things like referenced to undefined variables). Here, don't we need to find all those deferred type annotations, then remove the quotes (if future annotations is enabled)? 

---

_Comment by @colin99d on 2023-02-05 00:54_

Yeah. The issue is the items inside of the type. For example:
```
class D(typing.TypedDict):
    E: TypedDict(E, {"foo": "int"})
```

Should be converted to:
```
class D(typing.TypedDict):
    E: TypedDict(E, {"foo": int})
```

But instead it it converted into:
```
class D(typing.TypedDict):
    E: TypedDict(E, {foo: int})
```

For almost all of my tests, this approach seems to ignore the rules of when information inside of a type should be uncommented, and just always uncomments it.

---

_Comment by @charliermarsh on 2023-02-05 00:58_

But, I think we _must_ be detecting this correctly, because:

```py
import typing

class D(typing.TypedDict):
    E: typing.TypedDict(E, {"foo": "bar"})
```

If I run `cargo run foo.py --select F -n`, I get:

```
foo.py:4:36: F821 Undefined name `bar`
```

So Ruff knows that "bar" is an annotation, but "foo" is not -- somewhere.

---

_Comment by @charliermarsh on 2023-02-05 00:59_

E.g., this code block:

```rs
// Ex) TypedDict("a", {"a": int})
if args.len() > 1 {
    if let ExprKind::Dict { keys, values } = &args[1].node {
        for key in keys.iter().flatten() {
            self.in_type_definition = false;
            self.visit_expr(key);
            self.in_type_definition = prev_in_type_definition;
        }
        for value in values {
            self.in_type_definition = true;
            self.visit_expr(value);
            self.in_type_definition = prev_in_type_definition;
        }
    }
}
```

We know to treat "foo" as a non-annotation, and "bar" as an annotation -- so can we not use the same logic and code to power this fix? What are you seeing exactly? Are the failing changes on this branch?


---

_Comment by @colin99d on 2023-02-05 01:04_

Look at the differences in `cargo test`. They were all passing before the change, and now most of them are not. There is a chance I just missed something small.

---

_Comment by @charliermarsh on 2023-02-05 01:08_

I'll take a look and see where I'm confused. No need to dig further till I've actually looked at the changes more closely.

---

_Comment by @charliermarsh on 2023-02-05 05:00_

I had to move a few things around, but I think this version mostly works? There are some errors, but they're errors that are also present in `Checker` and so we should fix them there. For example, it does change `"name"` here, incorrectly:

```py
x: DefaultNamedArg(name="name", quox="str")
```

---

_Comment by @charliermarsh on 2023-02-05 05:17_

@colin99d - I think this is good to merge -- do you want to take a look? Or should I close out the issue? 🎉 🎉 🎉 

---

_Comment by @colin99d on 2023-02-05 13:43_

Looks good to me!! Thanks for figuring out how to get the existing functionality working with my PR!

---

_Merged by @charliermarsh on 2023-02-05 14:43_

---

_Closed by @charliermarsh on 2023-02-05 14:43_

---

_Comment by @charliermarsh on 2023-02-05 14:43_

Thanks for doing all the hard work!

---
