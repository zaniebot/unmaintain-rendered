```yaml
number: 11015
title: "Add a script to fuzz the parser (courtesy of `pysource-codegen`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: add-fuzzing-script
created_at: 2024-04-18T19:40:47Z
updated_at: 2024-04-19T12:04:24Z
url: https://github.com/astral-sh/ruff/pull/11015
synced_at: 2026-01-12T15:55:34Z
```

# Add a script to fuzz the parser (courtesy of `pysource-codegen`)

---

_@AlexWaygood_

## Summary

This PR adds a script that can be used to fuzz the parser against randomly generated, syntactically correct Python source-code files.

## Test Plan

I reverted the `crates/ruff_python_parser` directory back to how it was as of https://github.com/astral-sh/ruff/commit/13ffb5bc1936493816f01fb5583359e3d8fffca7 (in order to deliberately reintroduce some parser bugs), then ran the script several times with various invocations. Example output with the bugs reintroduced:

<details>

```
(ruff) (add-fuzzing-script)⚡ % python crates/ruff_python_parser/scripts/fuzz.py 0-6 50 100-200                                                                                                         ~/dev/ruff
Concurrently running the fuzzer on 109 randomly generated source-code files...
Ran fuzzer successfully on seed 5
Ran fuzzer successfully on seed 1
Ran fuzzer successfully on seed 104
Ran fuzzer successfully on seed 0
Ran fuzzer successfully on seed 2
Ran fuzzer successfully on seed 3
Ran fuzzer successfully on seed 105
Ran fuzzer successfully on seed 101
Ran fuzzer successfully on seed 4
Ran fuzzer successfully on seed 103
Ran fuzzer successfully on seed 102
Ran fuzzer successfully on seed 100
Ran fuzzer successfully on seed 50
Ran fuzzer successfully on seed 106
Ran fuzzer successfully on seed 107
Ran fuzzer successfully on seed 116
Ran fuzzer successfully on seed 109
Ran fuzzer successfully on seed 108
Ran fuzzer successfully on seed 111
Ran fuzzer successfully on seed 114
Ran fuzzer successfully on seed 110
Ran fuzzer successfully on seed 118
Ran fuzzer successfully on seed 117
Ran fuzzer successfully on seed 113
Ran fuzzer successfully on seed 112
Ran fuzzer successfully on seed 119
Ran fuzzer successfully on seed 115
Ran fuzzer successfully on seed 120
Ran fuzzer successfully on seed 121
Ran fuzzer successfully on seed 124
Ran fuzzer successfully on seed 126
Ran fuzzer successfully on seed 122
Ran fuzzer successfully on seed 125
Ran fuzzer successfully on seed 129
Ran fuzzer successfully on seed 131
Ran fuzzer successfully on seed 127
Ran fuzzer successfully on seed 123
Ran fuzzer successfully on seed 133
Ran fuzzer successfully on seed 130
Ran fuzzer successfully on seed 135
Ran fuzzer successfully on seed 132
Ran fuzzer successfully on seed 128
Ran fuzzer successfully on seed 136
Ran fuzzer successfully on seed 138
Ran fuzzer successfully on seed 137
Ran fuzzer successfully on seed 134
Ran fuzzer successfully on seed 139
Ran fuzzer successfully on seed 140
Ran fuzzer successfully on seed 143
Ran fuzzer successfully on seed 142
Ran fuzzer successfully on seed 141
Ran fuzzer successfully on seed 150
Ran fuzzer successfully on seed 145
Ran fuzzer successfully on seed 144
Ran fuzzer successfully on seed 147
Ran fuzzer successfully on seed 148
Ran fuzzer successfully on seed 153
Ran fuzzer successfully on seed 154
Ran fuzzer successfully on seed 149
Ran fuzzer successfully on seed 151
Ran fuzzer successfully on seed 158
Ran fuzzer successfully on seed 160
Ran fuzzer successfully on seed 156
Ran fuzzer successfully on seed 159
Ran fuzzer successfully on seed 155
Ran fuzzer successfully on seed 157
Ran fuzzer successfully on seed 161
Ran fuzzer successfully on seed 163
Ran fuzzer successfully on seed 162
Ran fuzzer successfully on seed 165
Ran fuzzer successfully on seed 164
Ran fuzzer successfully on seed 166
Ran fuzzer successfully on seed 170
Ran fuzzer successfully on seed 167
Ran fuzzer successfully on seed 168
Ran fuzzer successfully on seed 171
Ran fuzzer successfully on seed 169
Ran fuzzer successfully on seed 175
Ran fuzzer successfully on seed 172
Ran fuzzer successfully on seed 173
Ran fuzzer successfully on seed 180
Ran fuzzer successfully on seed 174
Ran fuzzer successfully on seed 178
Ran fuzzer successfully on seed 181
Ran fuzzer successfully on seed 182
Ran fuzzer successfully on seed 177
Ran fuzzer successfully on seed 183
Ran fuzzer successfully on seed 179
Ran fuzzer successfully on seed 176
Ran fuzzer successfully on seed 186
Ran fuzzer successfully on seed 188
Ran fuzzer successfully on seed 190
Ran fuzzer successfully on seed 187
Ran fuzzer successfully on seed 184
Ran fuzzer successfully on seed 189
Ran fuzzer successfully on seed 194
Ran fuzzer successfully on seed 185
Ran fuzzer successfully on seed 193
Ran fuzzer successfully on seed 195
Ran fuzzer successfully on seed 192
Ran fuzzer successfully on seed 191
Ran fuzzer successfully on seed 197
Ran fuzzer successfully on seed 199
Ran fuzzer successfully on seed 196
Ran fuzzer successfully on seed 198
Ran fuzzer on seed 6
The following code triggers a bug:

for name_3[name_1 > name_2] in name_4:
    pass

Ran fuzzer on seed 146
The following code triggers a bug:

with (name_3 async for name_1 in name_0) if name_5 else name_2:
    pass

Ran fuzzer on seed 152
The following code triggers a bug:

for name_2[name_4 not in name_5] in ():
    pass

Ran fuzzer on seed 200
The following code triggers a bug:

with (name_4 if name_2 else name_5) and name_0:
    pass

Bugs found in the following seeds:
6 146 152 200
```

</details>

Screenshot to show how it looks with colour:

<details>

![image](https://github.com/astral-sh/ruff/assets/66076021/9d637796-a5f5-4c44-99c4-817ab86bee99)

</details>

---

_Review requested from @carljm by @AlexWaygood on 2024-04-18 19:40_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-04-18 19:40_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-04-18 19:40_

---

_@AlexWaygood reviewed on 2024-04-18 19:41_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/scripts/fuzz_parser.py`:1 on 2024-04-18 19:41_

No idea if this is the correct place to put this script! This adds a whole new `scripts/` directory to the crate

---

_Comment by @github-actions[bot] on 2024-04-18 19:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @carljm on `crates/ruff_python_parser/scripts/fuzz_parser.py`:11 on 2024-04-18 20:43_

```suggestion
- Run the fuzzer using seeds 0, 1, 2, 78 and 93 to generate the code:
```

---

_Review comment by @carljm on `crates/ruff_python_parser/scripts/fuzz_parser.py`:165 on 2024-04-18 20:53_

I was thinking we'd have to actually partition the list of seeds, but we don't; we just feed them to the executors one by one, which is a lot simpler (and balances better, too.)

That also means technically we wouldn't have to fully materialize the list of seeds like we do now; instead we _could_ just store the `list[int | range]` here, and have a method that yields seeds one at a time.

But I think this doesn't really matter, and for smaller sizes of list, materializing is probably faster than a generator.

---

_@carljm approved on 2024-04-18 20:53_

Code looks good to me! Comments are just a typo and something that's probably not worth acting on.

The location of the script seems reasonable to me, but you could wait for someone who actually knows something about preferred ruff directory structure to comment on that, if you want to.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/scripts/fuzz_parser.py`:147 on 2024-04-19 05:52_

Did I nerd snip you into making it concurrent :D 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/scripts/fuzz_parser.py`:191 on 2024-04-19 05:53_

This is nice. So we can also just use it to test the parser in general.

---

_@MichaReiser approved on 2024-04-19 05:53_

Thank you

---

_Label `internal` added by @MichaReiser on 2024-04-19 05:53_

---

_@AlexWaygood reviewed on 2024-04-19 09:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/scripts/fuzz_parser.py`:147 on 2024-04-19 09:46_

Maybe 😁 @carljm and I looked at it in our pairing session yesterday

---

_@AlexWaygood reviewed on 2024-04-19 09:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/scripts/fuzz_parser.py`:147 on 2024-04-19 09:46_

Making it concurrent was a very good call, though. It's _much_ faster now :)

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/scripts/fuzz_parser.py`:1 on 2024-04-19 10:13_

We have a `scripts` directory at the project root, I would just put it there in a subdirectory along with a `requirements.in` (and generated `requirements.txt`) file. And, the module docstring could go in the `README.md` of that directory. Refer to `scripts/release` or `scripts/benchmarks`.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/scripts/fuzz_parser.py`:8 on 2024-04-19 10:14_

As mentioned, we can include a `requirements.in` (and `uv` generated `requirements.txt`) file for this)

---

_@dhruvmanila approved on 2024-04-19 10:16_

---

_@dhruvmanila reviewed on 2024-04-19 10:17_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/scripts/fuzz_parser.py`:1 on 2024-04-19 10:17_

Or, if you prefer to keep it in `ruff_python_parser`, we could rename it to `fuzz` instead to make the intent clear

---

_@AlexWaygood reviewed on 2024-04-19 11:26_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/scripts/fuzz_parser.py`:1 on 2024-04-19 11:26_

I moved it to the `scripts/` directory. I'd rather keep the module docstring, though, as it currently doubles as the text that's shown if you pass `-h` or `--help` as a CLI argument to the script:

<details>

```
(ruff) (add-fuzzing-script) % py scripts/fuzz-parser/fuzz.py -h                                                                                                                                        ~/dev/ruff
usage: fuzz.py [-h] [--only-new-bugs] [--quiet] seeds [seeds ...]

Run the parser on randomly generated (but syntactically valid) Python source-code files.

To install all dependencies for this script into an environment using `uv`, run:
    uv pip install -r scripts/fuzz-parser/requirements.txt

Example invocations of the script:
- Run the fuzzer using seeds 0, 1, 2, 78 and 93 to generate the code:
  `python scripts/fuzz-parser/fuzz.py 0-2 78 93`
- Run the fuzzer concurrently using seeds in range 0-10 inclusive,
  but only reporting bugs that are new on your branch:
  `python scripts/fuzz-parser/fuzz.py 0-10 --new-bugs-only`
- Run the fuzzer concurrently on 10,000 different Python source-code files,
  and only print a summary at the end:
  `python scripts/fuzz-parser/fuzz.py 1-10000 --quiet

N.B. The script takes a few seconds to get started, as the script needs to compile
your checked out version of ruff with `--release` as a first step before it
can actually start fuzzing.

positional arguments:
  seeds            Either a single seed, or an inclusive range of seeds in the format `0-5`

options:
  -h, --help       show this help message and exit
  --only-new-bugs  Only report bugs if they exist on the current branch, but *didn't* exist on the released version of Ruff installed into the Python environment we're running in
  --quiet          Print fewer things to the terminal while running the fuzzer
```

</details>

There also doesn't actually seem to be a `README.md` file for the `scripts/` directory right now

---

_@dhruvmanila approved on 2024-04-19 11:33_

Thank you!

---

_Merged by @AlexWaygood on 2024-04-19 11:40_

---

_Closed by @AlexWaygood on 2024-04-19 11:40_

---

_Branch deleted on 2024-04-19 11:40_

---

_Comment by @AlexWaygood on 2024-04-19 11:44_

Cc. @15r10nk -- `pysource-codegen` is awesome :-) an early version of this script helped us find several bugs in our new parser before releasing Ruff 0.4!

---

_Comment by @15r10nk on 2024-04-19 12:04_

That's great :rocket:. It makes me really happy to know that pysource-codegen is useful for you.

---
