```yaml
number: 21532
title: Pytest potential bug case - parameter not passed to a function
type: issue
state: closed
author: DeflateAwning
labels:
  - question
assignees: []
created_at: 2025-11-19T21:32:03Z
updated_at: 2025-12-10T17:26:04Z
url: https://github.com/astral-sh/ruff/issues/21532
synced_at: 2026-01-12T15:54:57Z
```

# Pytest potential bug case - parameter not passed to a function

---

_@DeflateAwning_

### Summary

Consider the following test case. I started off without any parameters, then added the `keep_right_geometry` parameter.

When I added it, I added it to the `if keep_right_geometry` part, but forgot to add it to the function called.

Not certain if there's a rule that can be made out of this (and it would almost certainly have to be ignored sometimes, as not ALL parameters are used as direct inputs to a function call).

```python
@pytest.mark.parametrize("keep_right_geometry", [True, False])
def test_spacial_join_WITH_different_col_names(keep_right_geometry: bool) -> None:
    box1 = shapely.geometry.box(1, 1, 10, 10).wkt  # Intersects box2
    box2 = shapely.geometry.box(5, 5, 15, 15).wkt  # Intersects box1. contains box3
    box3 = shapely.geometry.box(-5, -5, 0, 0).wkt  # Does not intersect any other box.
    box4 = shapely.geometry.box(11, 11, 14, 14).wkt  # contained in box2.

    df1 = pl.DataFrame({"name_1": ["box1", "box2", "box4"], "geometry_1": [box1, box2, box4]})
    df2 = pl.DataFrame({"name_2": ["box2", "box3", "box4"], "geometry_2": [box2, box3, box4]})

    result_df = spacial_join(
        df1,
        df2,
        left_on="geometry_1",
        right_on="geometry_2",
        where="intersects",
        how="inner",
        keep_right_geometry=keep_right_geometry, # <- I forgot to add this line for 25 minutes of debugging.
    )
    expected_df = pl.DataFrame(
        {
            "name_1": ["box1", "box2", "box2", "box4", "box4"],
            "geometry_1": [box1, box2, box2, box4, box4],
            "name_2": ["box2", "box2", "box4", "box2", "box4"],
            "geometry_2": [box2, box2, box4, box2, box4],
        }
    )

    if keep_right_geometry is False:
        expected_df = expected_df.drop("geometry_2")

    assert_frame_equal(result_df, expected_df)

```

---

_Comment by @ntBre on 2025-11-19 21:54_

Hmm, yeah I think this would be pretty difficult to implement as a rule. If I understand correctly, it's basically [unused-function-argument (ARG001)](https://docs.astral.sh/ruff/rules/unused-function-argument/#unused-function-argument-arg001) but instead of totally unused it would be something like "not meaningfully used," where the `keep_right_geometry` usage near the end of the function isn't counted, but the argument to `spacial_join` is.

---

_Label `question` added by @ntBre on 2025-11-19 21:54_

---

_Comment by @DeflateAwning on 2025-11-20 03:54_

That's an accurate account, yes! I'm totally aware of ARG001, and still wanted to suggest this as a separate thing, but I'm totally fine if the consensus is that this is a silly/pointless/impossible proposal. 

---

_Comment by @MichaReiser on 2025-12-10 17:26_

Thanks for this rule suggestion. I don't see how Ruff could reliably dedect whether the variable is "meaningfully used", which is a necessity for a new rule. Happy to reconsider this as a new rule with a proposal for a specific heuristic.

---

_Closed by @MichaReiser on 2025-12-10 17:26_

---
