```yaml
number: 10141
title: Not sure I agree with PIE807
type: issue
state: open
author: ezyang
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-02-26T20:57:10Z
updated_at: 2024-03-11T17:41:06Z
url: https://github.com/astral-sh/ruff/issues/10141
synced_at: 2026-01-12T15:54:49Z
```

# Not sure I agree with PIE807

---

_@ezyang_

When I say `lambda: {}`, it is clear that this callable takes no arguments, and I will raise an error if you pass me arguments. When you transform this into `dict`, now I could potentially take arguments, and I can only rely on an appropriate type signature to prevent people from inappropriately feeding arguments into this lambda.

cc @Skylion007

---

_Comment by @zanieb on 2024-02-26 23:10_

Could you provide a full example so people know what this looks like in context?

---

_Comment by @ezyang on 2024-02-27 04:33_

Sure thing:

```
def trace_structured(
    name: str,
    # NB: metadata expected to be dict so adding more info is forward compatible
    # Tuple[str, int] is a special case for string interning
    metadata_fn: Callable[[], Union[Dict[str, Any], Tuple[str, int]]] = dict,
    *,
    payload_fn: Callable[[], Optional[Union[str, object]]] = lambda: None,
    suppress_context: bool = False,
):  
    """ 
    metadata is an arbitrary JSON compatible struct, but it's expected to not be
    too long (e.g., less than 1MB)
        
    payload is an arbitrary string, which can be arbitrarily long (but expected to have
    newlines so no lines are too long)
    """ 
    assert "name" not in ["rank", "frame_id", "frame_compile_id", "attempt"]
    assert callable(
        metadata_fn
    ), f"metadata_fn should be callable, but got {type(metadata_fn)}"
    assert callable(
        payload_fn
    ), f"payload_fn should be callable, but got {type(payload_fn)}"
    if trace_log.isEnabledFor(logging.DEBUG):
        record: Dict[str, object] = {}
        record[name] = metadata_fn()
```

`metadata_fn` default is the relevant spot.

---

_Comment by @MichaReiser on 2024-02-27 08:51_

Playground for the provided example: [link](https://play.ruff.rs/1c40b272-2e86-4f11-8dd6-067ffa1969ca)

What I understand is that changing `metadata_fn = lambda: {}`  to `metadata_fn = dict` changes the inferred type of `metadata_fn` from `Callable[[], Dict[str, Any]]` to whatever signature `dict` has. This is problematic because:

* It changes what arguments can be passed when calling the function (only types with the same signature are now valid). Disclaimer: That's what I assume with my TypeScript knowledge, maybe it's different for Python type checkers?
* It changes how `metadata_fn` can be used in the function's body.

Ideally, we would only offer the transform when:
* In a typed code base: If the parameter has a type annotation
* In an untyped code base: Always? Although @ezyang has a good point, the function could now be called with arguments. 

---

_Comment by @AlexWaygood on 2024-02-27 10:53_

> Hmm...I don't think it changes the inferred type. `lambda: {}` vs `dict` is TMU basically the same from the type checker's point of view. You can verify this using `reveal_type` (mypy).

@mikeleppane, they're not identical when you're using them as callback functions. You can see here in this mypy playground gist that mypy (correctly) rejects arbitrary keywords being passed to `lambda x: {}`, whereas it (correctly) accepts arbitrary keywords being passed to `dict`: https://mypy-play.net/?mypy=latest&python=3.12&gist=3876729523c28764df349c9657bff865

---

_Comment by @AlexWaygood on 2024-02-27 11:02_

Interestingly, `lambda: {}` seems, if anything, to be slightly faster, even though it's a pure-Python function whereas the `dict` constructor is written in C? I assume this is because the `dict` constructor has to do some pretty complex argument parsing, whereas `lambda: {}` does not.

```sh
% py -m timeit -n 50000000 -s "x = dict" "x()"                            ~/dev
50000000 loops, best of 5: 18.8 nsec per loop
% py -m timeit -n 50000000 -s "x = lambda: {}" "x()"                      ~/dev
50000000 loops, best of 5: 18.2 nsec per loop
```

---

_Comment by @AlexWaygood on 2024-02-27 11:09_

I still feel like I prefer `dict` and `list` from an aesthetic point of view to `lambda: {}` and `lambda: []`, and these are often options that Python programmers forget. So I do feel like PIE807 provides some value here. The performance difference is absolutely tiny, and I'm guessing the difference in the callback signature is _probably_ only going to matter to you if you're the kind of person who's already using type annotations -- in which case, the transformation is likely safe, since if you want to enforce a narrower signature for the callback function, you can easily use type annotations to do so.

That's my opinion -- but the points raised here were definitely interesting to think about. I can definitely see the argument that `lambda: {}` is more explicit to readers of the code that you should never pass any arguments to the callback function.

---

_Comment by @ezyang on 2024-02-28 05:12_

As a Haskeller, I don't mind doing eta reduction as long as the expressions actually mean the same thing... which they don't in this case. I am sympathetic to the defaultdict(lambda: []) case though, which I do agree is far more idiomatic as defaultdict(list). When the lambda is a default argument for the function, I feel matters are different, since it's in a contravariant position rather than a covariant position.

---

_Comment by @ThiefMaster on 2024-02-28 22:44_

I disabled `PIE807` in my codebase for a similar reason: `lambda: {}` and `lambda: []` are clear ways of indicating that I'm providing a callable that returns an empty (mutable) object.  In places like Marshmallow schemas (`load_default=lambda: []` etc.) this is much clearer than `load_default=dict` which this rule would want me to use.

Is that rule enabled by default though? If not (and I don't think it is!) there's no problem with it IMHO. In my project I explicitly opted into `PIE`, and I then disabled this particular rule because I don't like it. Seems reasonable to me. If someone likes this suggestion, they are welcome to keep that rule enabled / opt into it.

---

_Comment by @henryiii on 2024-02-29 16:34_

It's only faster on Python 3.12; on 3.11:

```console
$ python3.11 -m timeit -n 50000000 -s "x = dict" "x()"
50000000 loops, best of 5: 45.5 nsec per loop
$ python3.11 -m timeit -n 50000000 -s "x = lambda: {}" "x()"
50000000 loops, best of 5: 48.3 nsec per loop
```

On 3.12, it is a bit faster:

```console
$ python3.12 -m timeit -n 50000000 -s "x = dict" "x()"
50000000 loops, best of 5: 49.4 nsec per loop
$ python3.12 -m timeit -n 50000000 -s "x = lambda: {}" "x()"
50000000 loops, best of 5: 44.7 nsec per loop
```

It might be taking advantage of the fact that `{}` is about twice as fast as `dict()` as it avoids the function lookup overhead (like how `f"{x}"` is faster than `str(x)`), it seems 3.12 optimizes on the lambda just a bit better, perhaps. Also benchmarks at this level are pretty tricky.

For fun, I checked 3.13a3 with nogil and JIT enabled, and it seems the dict one has gone up:

```console
$ PYTHON_CONFIGURE_OPTS='--enable-optimizations --with-lto --enable-experimental-jit --disable-gil' PYTHON_CFLAGS='-march=native -mtune=native' pyenv install 3.13.0a3
$ ~/.pyenv/versions/3.13.0a3/bin/python -m timeit -n 50000000 -s "x = dict" "x()"
50000000 loops, best of 5: 62.9 nsec per loop
$ ~/.pyenv/versions/3.13.0a3/bin/python -m timeit -n 50000000 -s "x = lambda: {}" "x()"
50000000 loops, best of 5: 49.1 nsec per loop
```

I like "dict" better (once you get used to functions as objects), just was curious when I saw the perf comparison.

---

_Comment by @AlexWaygood on 2024-02-29 16:37_

> For fun, I checked 3.13a3 with nogil and JIT enabled

heh, I think the JIT might still make things _slower_ on `main` -- be careful there ;) I think Brandt deliberately merged a very basic JIT to start off with so that we could have something in CPython that we could incrementally improve and optimize

---

_Comment by @zanieb on 2024-03-11 17:40_

Is the open question here whether we should reduce the scope of `PIE807`? What specific case would we no longer enforce?

---

_Label `rule` added by @zanieb on 2024-03-11 17:41_

---

_Label `needs-decision` added by @zanieb on 2024-03-11 17:41_

---
