```yaml
number: 9813
title: "Don't trim last empty line in docstrings"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: docstring-dont-trim-last-line
created_at: 2024-02-04T17:10:31Z
updated_at: 2024-02-05T13:34:27Z
url: https://github.com/astral-sh/ruff/pull/9813
synced_at: 2026-01-10T22:57:09Z
```

# Don't trim last empty line in docstrings

---

_Pull request opened by @MichaReiser on 2024-02-04 17:10_

## Summary

This PR fixes a bug in our formatter where it would remove the last empty line for docstrings when `"""` is not intented:

```python
def test7():
    """
    a, b
    
    
    
    
"""
```

Would get formatted to 

```python
def test7():
    """
    a, b
    
    
    
"""
```
(Removing the last empty line). 

This is inconsistent with Black and how we format docstrings when the closing quotes are indented. The empty line does not get removed for:

```python
def test7():
    """
    a, b
    
    
    
    
    """
```

The root cause of the divergence was that we use `str::lines()` which uses `split_inclusive` underneath that omits the last element if it matches the separator:

> If the last element of the string is matched, that element will be considered the terminator of the preceding substring. That substring will be the last item returned by the iterator.
  ```rust
  let v: Vec<&str> = "Mary had a little lamb\nlittle lamb\nlittle lamb.\n"
      .split_inclusive('\n').collect();
  assert_eq!(v, ["Mary had a little lamb\n", "little lamb\n", "little lamb.\n"]); 
  ```

The PR fixes the divergence by using `split` instead.

Fixes https://github.com/astral-sh/ruff/issues/9804

## Preview

This change is not gated behind preview because it doesn't change the output for already formatted code. 

## Test Plan

Added snapshot test


---

_Label `bug` added by @MichaReiser on 2024-02-04 17:11_

---

_Label `formatter` added by @MichaReiser on 2024-02-04 17:11_

---

_Review requested from @konstin by @MichaReiser on 2024-02-04 17:13_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-02-04 17:13_

---

_@BurntSushi approved on 2024-02-04 17:24_

Nice fix! This is why [`bstr` has a `lines_with_terminator` method](https://docs.rs/bstr/latest/bstr/trait.ByteSlice.html#method.lines_with_terminator). ;-)

---

_Comment by @github-actions[bot] on 2024-02-04 17:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+59 -1 lines in 3 files in 3 projects; 2 project errors; 38 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+56 -0 lines across 1 file)</summary>
<p>

<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L78'>src/bokeh/plotting/glyph_api.py~L78</a>
```diff
                              inner_radius=0.2, outer_radius=0.5)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Arc)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L97'>src/bokeh/plotting/glyph_api.py~L97</a>
```diff
                 plot.asterisk(x=[1,2,3], y=[1,2,3], size=20, color="#F0027F")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Bezier)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L120'>src/bokeh/plotting/glyph_api.py~L120</a>
```diff
                 plot.circle(x=[1, 2, 3], y=[1, 2, 3], size=20)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Block)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L135'>src/bokeh/plotting/glyph_api.py~L135</a>
```diff
                 plot.block(x=[1, 2, 3], y=[1,2,3], width=0.5, height=1, , color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L151'>src/bokeh/plotting/glyph_api.py~L151</a>
```diff
                                   color="#FB8072", fill_alpha=0.2, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L167'>src/bokeh/plotting/glyph_api.py~L167</a>
```diff
                                 color="#FB8072", fill_color=None)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L183'>src/bokeh/plotting/glyph_api.py~L183</a>
```diff
                               color="#DD1C77", fill_alpha=0.2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L199'>src/bokeh/plotting/glyph_api.py~L199</a>
```diff
                               color="#DD1C77", fill_alpha=0.2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L215'>src/bokeh/plotting/glyph_api.py~L215</a>
```diff
                            color="#E6550D", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L231'>src/bokeh/plotting/glyph_api.py~L231</a>
```diff
                           color="#99D594", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L247'>src/bokeh/plotting/glyph_api.py~L247</a>
```diff
                              color="#1C9099", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L263'>src/bokeh/plotting/glyph_api.py~L263</a>
```diff
                                    color="#386CB0", fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L279'>src/bokeh/plotting/glyph_api.py~L279</a>
```diff
                                  color="#386CB0", fill_color=None)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L294'>src/bokeh/plotting/glyph_api.py~L294</a>
```diff
                 plot.dot(x=[1, 2, 3], y=[1, 2, 3], size=20, color="#386CB0")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HArea)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L310'>src/bokeh/plotting/glyph_api.py~L310</a>
```diff
                            fill_color="#99D594")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HAreaStep)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L326'>src/bokeh/plotting/glyph_api.py~L326</a>
```diff
                                 step_mode="after", fill_color="#99D594")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HBar)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L341'>src/bokeh/plotting/glyph_api.py~L341</a>
```diff
                 plot.hbar(y=[1, 2, 3], height=0.5, left=0, right=[1,2,3], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HSpan)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L356'>src/bokeh/plotting/glyph_api.py~L356</a>
```diff
                 plot.hspan(y=[1, 2, 3], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HStrip)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L371'>src/bokeh/plotting/glyph_api.py~L371</a>
```diff
                 plot.hstrip(y0=[1, 2, 5], y1=[3, 4, 8], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Ellipse)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L387'>src/bokeh/plotting/glyph_api.py~L387</a>
```diff
                              color="#386CB0", fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L402'>src/bokeh/plotting/glyph_api.py~L402</a>
```diff
                 plot.hex(x=[1, 2, 3], y=[1, 2, 3], size=[10,20,30], color="#74ADD1")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L418'>src/bokeh/plotting/glyph_api.py~L418</a>
```diff
                              color="#74ADD1", fill_color=None)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HexTile)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L433'>src/bokeh/plotting/glyph_api.py~L433</a>
```diff
                 plot.hex_tile(r=[0, 0, 1], q=[1, 2, 2], fill_color="#74ADD1")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Image)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L442'>src/bokeh/plotting/glyph_api.py~L442</a>
```diff
             If both ``palette`` and ``color_mapper`` are passed, a ``ValueError``
             exception will be raised. If neither is passed, then the ``Greys9``
             palette will be used as a default.
+
         """
 
     @glyph_method(glyphs.ImageRGBA)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L450'>src/bokeh/plotting/glyph_api.py~L450</a>
```diff
         .. note::
             The ``image_rgba`` method accepts images as a two-dimensional array of RGBA
             values (encoded as 32-bit integers).
+
         """
 
     @glyph_method(glyphs.ImageStack)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L473'>src/bokeh/plotting/glyph_api.py~L473</a>
```diff
                 plot.inverted_triangle(x=[1, 2, 3], y=[1, 2, 3], size=20, color="#DE2D26")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Line)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L488'>src/bokeh/plotting/glyph_api.py~L488</a>
```diff
                 p.line(x=[1, 2, 3, 4, 5], y=[6, 7, 2, 4, 5])
 
                 show(p)
+
         """
 
     @glyph_method(glyphs.MultiLine)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L508'>src/bokeh/plotting/glyph_api.py~L508</a>
```diff
                             color=['red','green'])
 
                 show(p)
+
         """
 
     @glyph_method(glyphs.MultiPolygons)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L528'>src/bokeh/plotting/glyph_api.py~L528</a>
```diff
                                 ys=[[[[4, 3, 3, 4]]], [[[1, 3, 1], [1.5, 2, 1.5]]]],
                                 color=['red', 'green'])
                 show(p)
+
         """
 
     @glyph_method(glyphs.Patch)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L543'>src/bokeh/plotting/glyph_api.py~L543</a>
```diff
                 p.patch(x=[1, 2, 3, 2], y=[6, 7, 2, 2], color="#99d8c9")
 
                 show(p)
+
         """
 
     @glyph_method(glyphs.Patches)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L563'>src/bokeh/plotting/glyph_api.py~L563</a>
```diff
                           color=["#43a2ca", "#a8ddb5"])
 
                 show(p)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L578'>src/bokeh/plotting/glyph_api.py~L578</a>
```diff
                 plot.plus(x=[1, 2, 3], y=[1, 2, 3], size=20, color="#DE2D26")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Quad)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L594'>src/bokeh/plotting/glyph_api.py~L594</a>
```diff
                           right=[1.2, 2.5, 3.7], color="#B3DE69")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Quadratic)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L614'>src/bokeh/plotting/glyph_api.py~L614</a>
```diff
                         line_width=2)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Rect)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L635'>src/bokeh/plotting/glyph_api.py~L635</a>
```diff
                 ``Rect`` glyphs are not well defined on logarithmic scales. Use
                 :class:`~bokeh.models.Block` or :class:`~bokeh.models.Quad` glyphs
                 instead.
+
         """
 
     @glyph_method(glyphs.Step)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L650'>src/bokeh/plotting/glyph_api.py~L650</a>
```diff
                 plot.step(x=[1, 2, 3, 4, 5], y=[1, 2, 3, 2, 5], color="#FB8072")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Segment)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L667'>src/bokeh/plotting/glyph_api.py~L667</a>
```diff
                              color="#F4A582", line_width=3)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L682'>src/bokeh/plotting/glyph_api.py~L682</a>
```diff
                 plot.square(x=[1, 2, 3], y=[1, 2, 3], size=[10,20,30], color="#74ADD1")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L698'>src/bokeh/plotting/glyph_api.py~L698</a>
```diff
                                   color="#7FC97F",fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L714'>src/bokeh/plotting/glyph_api.py~L714</a>
```diff
                                 color="#7FC97F", fill_color=None)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L730'>src/bokeh/plotting/glyph_api.py~L730</a>
```diff
                                 color="#7FC97F",fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L746'>src/bokeh/plotting/glyph_api.py~L746</a>
```diff
                               color="#FDAE6B",fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L762'>src/bokeh/plotting/glyph_api.py~L762</a>
```diff
                           color="#1C9099", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L778'>src/bokeh/plotting/glyph_api.py~L778</a>
```diff
                               color="#386CB0", fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Text)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L786'>src/bokeh/plotting/glyph_api.py~L786</a>
```diff
         .. note::
             The location and angle of the text relative to the ``x``, ``y`` coordinates
             is indicated by the alignment and baseline text properties.
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L802'>src/bokeh/plotting/glyph_api.py~L802</a>
```diff
                               color="#99D594", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L818'>src/bokeh/plotting/glyph_api.py~L818</a>
```diff
                                   color="#99D594", fill_color=None)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L834'>src/bokeh/plotting/glyph_api.py~L834</a>
```diff
                               color="#99D594", line_width=2)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VArea)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L850'>src/bokeh/plotting/glyph_api.py~L850</a>
```diff
                            fill_color="#99D594")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VAreaStep)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L866'>src/bokeh/plotting/glyph_api.py~L866</a>
```diff
                                 step_mode="after", fill_color="#99D594")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VBar)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L881'>src/bokeh/plotting/glyph_api.py~L881</a>
```diff
                 plot.vbar(x=[1, 2, 3], width=0.5, bottom=0, top=[1,2,3], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VSpan)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L896'>src/bokeh/plotting/glyph_api.py~L896</a>
```diff
                 plot.vspan(x=[1, 2, 3], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VStrip)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L911'>src/bokeh/plotting/glyph_api.py~L911</a>
```diff
                 plot.vstrip(x0=[1, 2, 5], x1=[3, 4, 8], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Wedge)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L927'>src/bokeh/plotting/glyph_api.py~L927</a>
```diff
                            end_angle=4.1, radius_units="screen", color="#2b8cbe")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L942'>src/bokeh/plotting/glyph_api.py~L942</a>
```diff
                 plot.x(x=[1, 2, 3], y=[1, 2, 3], size=[10, 20, 25], color="#fa9fb5")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L957'>src/bokeh/plotting/glyph_api.py~L957</a>
```diff
                 plot.y(x=[1, 2, 3], y=[1, 2, 3], size=20, color="#DE2D26")
 
                 show(plot)
+
         """
 
     # -------------------------------------------------------------------------
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+1 -0 lines across 1 file)</summary>
<p>

<a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/docker/api/container.py#L647'>docker/api/container.py~L647</a>
```diff
             ... )
             {'CapDrop': ['MKNOD'], 'LxcConf': None, 'Privileged': True,
             'VolumesFrom': ['nostalgic_newton'], 'PublishAllPorts': False}
+
         """
         if not kwargs:
             kwargs = {}
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -1 lines across 1 file)</summary>
<p>

<a href='https://github.com/rotki/rotki/blob/0c90a6080dcc745b0d0973aa1424881e777ad988/rotkehlchen/db/utils.py#L483'>rotkehlchen/db/utils.py~L483</a>
```diff
 def protect_password_sqlcipher(password: str) -> str:
     """A double quote in the password would close the string. To escape it double it
 
-    source: https://stackoverflow.com/a/603579/110395"""
+    source: https://stackoverflow.com/a/603579/110395
+    """
     return password.replace(r'"', r'""')
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff format --no-preview --exclude tests/roots/test-pycode/cp_1251_coded.py</pre>
</p>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+59 -1 lines in 3 files in 3 projects; 1 project error; 39 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+56 -0 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L78'>src/bokeh/plotting/glyph_api.py~L78</a>
```diff
                              inner_radius=0.2, outer_radius=0.5)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Arc)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L97'>src/bokeh/plotting/glyph_api.py~L97</a>
```diff
                 plot.asterisk(x=[1,2,3], y=[1,2,3], size=20, color="#F0027F")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Bezier)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L120'>src/bokeh/plotting/glyph_api.py~L120</a>
```diff
                 plot.circle(x=[1, 2, 3], y=[1, 2, 3], size=20)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Block)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L135'>src/bokeh/plotting/glyph_api.py~L135</a>
```diff
                 plot.block(x=[1, 2, 3], y=[1,2,3], width=0.5, height=1, , color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L151'>src/bokeh/plotting/glyph_api.py~L151</a>
```diff
                                   color="#FB8072", fill_alpha=0.2, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L167'>src/bokeh/plotting/glyph_api.py~L167</a>
```diff
                                 color="#FB8072", fill_color=None)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L183'>src/bokeh/plotting/glyph_api.py~L183</a>
```diff
                               color="#DD1C77", fill_alpha=0.2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L199'>src/bokeh/plotting/glyph_api.py~L199</a>
```diff
                               color="#DD1C77", fill_alpha=0.2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L215'>src/bokeh/plotting/glyph_api.py~L215</a>
```diff
                            color="#E6550D", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L231'>src/bokeh/plotting/glyph_api.py~L231</a>
```diff
                           color="#99D594", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L247'>src/bokeh/plotting/glyph_api.py~L247</a>
```diff
                              color="#1C9099", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L263'>src/bokeh/plotting/glyph_api.py~L263</a>
```diff
                                    color="#386CB0", fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L279'>src/bokeh/plotting/glyph_api.py~L279</a>
```diff
                                  color="#386CB0", fill_color=None)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L294'>src/bokeh/plotting/glyph_api.py~L294</a>
```diff
                 plot.dot(x=[1, 2, 3], y=[1, 2, 3], size=20, color="#386CB0")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HArea)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L310'>src/bokeh/plotting/glyph_api.py~L310</a>
```diff
                            fill_color="#99D594")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HAreaStep)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L326'>src/bokeh/plotting/glyph_api.py~L326</a>
```diff
                                 step_mode="after", fill_color="#99D594")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HBar)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L341'>src/bokeh/plotting/glyph_api.py~L341</a>
```diff
                 plot.hbar(y=[1, 2, 3], height=0.5, left=0, right=[1,2,3], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HSpan)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L356'>src/bokeh/plotting/glyph_api.py~L356</a>
```diff
                 plot.hspan(y=[1, 2, 3], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HStrip)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L371'>src/bokeh/plotting/glyph_api.py~L371</a>
```diff
                 plot.hstrip(y0=[1, 2, 5], y1=[3, 4, 8], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Ellipse)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L387'>src/bokeh/plotting/glyph_api.py~L387</a>
```diff
                              color="#386CB0", fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L402'>src/bokeh/plotting/glyph_api.py~L402</a>
```diff
                 plot.hex(x=[1, 2, 3], y=[1, 2, 3], size=[10,20,30], color="#74ADD1")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L418'>src/bokeh/plotting/glyph_api.py~L418</a>
```diff
                              color="#74ADD1", fill_color=None)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.HexTile)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L433'>src/bokeh/plotting/glyph_api.py~L433</a>
```diff
                 plot.hex_tile(r=[0, 0, 1], q=[1, 2, 2], fill_color="#74ADD1")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Image)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L442'>src/bokeh/plotting/glyph_api.py~L442</a>
```diff
             If both ``palette`` and ``color_mapper`` are passed, a ``ValueError``
             exception will be raised. If neither is passed, then the ``Greys9``
             palette will be used as a default.
+
         """
 
     @glyph_method(glyphs.ImageRGBA)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L450'>src/bokeh/plotting/glyph_api.py~L450</a>
```diff
         .. note::
             The ``image_rgba`` method accepts images as a two-dimensional array of RGBA
             values (encoded as 32-bit integers).
+
         """
 
     @glyph_method(glyphs.ImageStack)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L473'>src/bokeh/plotting/glyph_api.py~L473</a>
```diff
                 plot.inverted_triangle(x=[1, 2, 3], y=[1, 2, 3], size=20, color="#DE2D26")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Line)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L488'>src/bokeh/plotting/glyph_api.py~L488</a>
```diff
                 p.line(x=[1, 2, 3, 4, 5], y=[6, 7, 2, 4, 5])
 
                 show(p)
+
         """
 
     @glyph_method(glyphs.MultiLine)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L508'>src/bokeh/plotting/glyph_api.py~L508</a>
```diff
                             color=['red','green'])
 
                 show(p)
+
         """
 
     @glyph_method(glyphs.MultiPolygons)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L528'>src/bokeh/plotting/glyph_api.py~L528</a>
```diff
                                 ys=[[[[4, 3, 3, 4]]], [[[1, 3, 1], [1.5, 2, 1.5]]]],
                                 color=['red', 'green'])
                 show(p)
+
         """
 
     @glyph_method(glyphs.Patch)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L543'>src/bokeh/plotting/glyph_api.py~L543</a>
```diff
                 p.patch(x=[1, 2, 3, 2], y=[6, 7, 2, 2], color="#99d8c9")
 
                 show(p)
+
         """
 
     @glyph_method(glyphs.Patches)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L563'>src/bokeh/plotting/glyph_api.py~L563</a>
```diff
                           color=["#43a2ca", "#a8ddb5"])
 
                 show(p)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L578'>src/bokeh/plotting/glyph_api.py~L578</a>
```diff
                 plot.plus(x=[1, 2, 3], y=[1, 2, 3], size=20, color="#DE2D26")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Quad)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L594'>src/bokeh/plotting/glyph_api.py~L594</a>
```diff
                           right=[1.2, 2.5, 3.7], color="#B3DE69")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Quadratic)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L614'>src/bokeh/plotting/glyph_api.py~L614</a>
```diff
                         line_width=2)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Rect)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L635'>src/bokeh/plotting/glyph_api.py~L635</a>
```diff
                 ``Rect`` glyphs are not well defined on logarithmic scales. Use
                 :class:`~bokeh.models.Block` or :class:`~bokeh.models.Quad` glyphs
                 instead.
+
         """
 
     @glyph_method(glyphs.Step)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L650'>src/bokeh/plotting/glyph_api.py~L650</a>
```diff
                 plot.step(x=[1, 2, 3, 4, 5], y=[1, 2, 3, 2, 5], color="#FB8072")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Segment)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L667'>src/bokeh/plotting/glyph_api.py~L667</a>
```diff
                              color="#F4A582", line_width=3)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L682'>src/bokeh/plotting/glyph_api.py~L682</a>
```diff
                 plot.square(x=[1, 2, 3], y=[1, 2, 3], size=[10,20,30], color="#74ADD1")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L698'>src/bokeh/plotting/glyph_api.py~L698</a>
```diff
                                   color="#7FC97F",fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L714'>src/bokeh/plotting/glyph_api.py~L714</a>
```diff
                                 color="#7FC97F", fill_color=None)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L730'>src/bokeh/plotting/glyph_api.py~L730</a>
```diff
                                 color="#7FC97F",fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L746'>src/bokeh/plotting/glyph_api.py~L746</a>
```diff
                               color="#FDAE6B",fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L762'>src/bokeh/plotting/glyph_api.py~L762</a>
```diff
                           color="#1C9099", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L778'>src/bokeh/plotting/glyph_api.py~L778</a>
```diff
                               color="#386CB0", fill_color=None, line_width=2)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Text)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L786'>src/bokeh/plotting/glyph_api.py~L786</a>
```diff
         .. note::
             The location and angle of the text relative to the ``x``, ``y`` coordinates
             is indicated by the alignment and baseline text properties.
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L802'>src/bokeh/plotting/glyph_api.py~L802</a>
```diff
                               color="#99D594", line_width=2)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L818'>src/bokeh/plotting/glyph_api.py~L818</a>
```diff
                                   color="#99D594", fill_color=None)
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L834'>src/bokeh/plotting/glyph_api.py~L834</a>
```diff
                               color="#99D594", line_width=2)
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VArea)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L850'>src/bokeh/plotting/glyph_api.py~L850</a>
```diff
                            fill_color="#99D594")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VAreaStep)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L866'>src/bokeh/plotting/glyph_api.py~L866</a>
```diff
                                 step_mode="after", fill_color="#99D594")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VBar)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L881'>src/bokeh/plotting/glyph_api.py~L881</a>
```diff
                 plot.vbar(x=[1, 2, 3], width=0.5, bottom=0, top=[1,2,3], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VSpan)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L896'>src/bokeh/plotting/glyph_api.py~L896</a>
```diff
                 plot.vspan(x=[1, 2, 3], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.VStrip)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L911'>src/bokeh/plotting/glyph_api.py~L911</a>
```diff
                 plot.vstrip(x0=[1, 2, 5], x1=[3, 4, 8], color="#CAB2D6")
 
                 show(plot)
+
         """
 
     @glyph_method(glyphs.Wedge)
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L927'>src/bokeh/plotting/glyph_api.py~L927</a>
```diff
                            end_angle=4.1, radius_units="screen", color="#2b8cbe")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L942'>src/bokeh/plotting/glyph_api.py~L942</a>
```diff
                 plot.x(x=[1, 2, 3], y=[1, 2, 3], size=[10, 20, 25], color="#fa9fb5")
 
                 show(plot)
+
         """
 
     @marker_method()
```
<a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/glyph_api.py#L957'>src/bokeh/plotting/glyph_api.py~L957</a>
```diff
                 plot.y(x=[1, 2, 3], y=[1, 2, 3], size=20, color="#DE2D26")
 
                 show(plot)
+
         """
 
     # -------------------------------------------------------------------------
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+1 -0 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/docker/docker-py/blob/bd164f928ab82e798e30db455903578d06ba2070/docker/api/container.py#L647'>docker/api/container.py~L647</a>
```diff
             ... )
             {'CapDrop': ['MKNOD'], 'LxcConf': None, 'Privileged': True,
             'VolumesFrom': ['nostalgic_newton'], 'PublishAllPorts': False}
+
         """
         if not kwargs:
             kwargs = {}
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -1 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/0c90a6080dcc745b0d0973aa1424881e777ad988/rotkehlchen/db/utils.py#L479'>rotkehlchen/db/utils.py~L479</a>
```diff
 def protect_password_sqlcipher(password: str) -> str:
     """A double quote in the password would close the string. To escape it double it
 
-    source: https://stackoverflow.com/a/603579/110395"""
+    source: https://stackoverflow.com/a/603579/110395
+    """
     return password.replace(r'"', r'""')
 
 
```

</p>
</details>
<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_@konstin approved on 2024-02-04 23:44_

---

_Merged by @MichaReiser on 2024-02-05 13:29_

---

_Closed by @MichaReiser on 2024-02-05 13:29_

---

_Branch deleted on 2024-02-05 13:29_

---
