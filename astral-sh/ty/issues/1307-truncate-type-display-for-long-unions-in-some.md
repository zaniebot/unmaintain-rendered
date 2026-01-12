```yaml
number: 1307
title: Truncate type display for long unions in some situations
type: issue
state: closed
author: AlexWaygood
labels:
  - diagnostics
assignees: []
created_at: 2025-10-04T17:11:52Z
updated_at: 2026-01-12T13:17:31Z
url: https://github.com/astral-sh/ty/issues/1307
synced_at: 2026-01-12T14:02:46Z
```

# Truncate type display for long unions in some situations

---

_Issue opened by @AlexWaygood on 2025-10-04 17:11_

### Summary

One of the diagnostics in the ecosystem report for https://github.com/astral-sh/ruff/pull/20368 is:

```
[error] invalid-argument-type - :1121:30 - Argument is incorrect: Expected `str`, found `Unknown | str | ((sd: @Todo | SpectralDistribution | MultiSpectralDistributions, cmfs: MultiSpectralDistributions | None = None, illuminant: SpectralDistribution | None = None, k: @Todo | None = None, method: str = str, **kwargs: Any) -> @Todo) | ((XYZ: @Todo, method: str = str, **kwargs: Any) -> SpectralDistribution) | ((sd: SpectralDistribution, lef: SpectralDistribution | None = None) -> int | float) | ((XYZ: @Todo) -> @Todo) | ((Y: @Todo) -> @Todo) | ((LV: @Todo, method: str = str, **kwargs: Any) -> @Todo) | partial[Unknown] | ((wavelength: @Todo, cmfs: MultiSpectralDistributions | None = None) -> @Todo) | ((xyY: @Todo) -> @Todo) | ((xy: @Todo) -> @Todo) | ((Lab: @Todo) -> @Todo) | ((Luv: @Todo, illuminant: @Todo = Any) -> @Todo) | ((uv: @Todo) -> @Todo) | ((UVW: @Todo) -> @Todo) | ((Lab_99: @Todo, illuminant: @Todo = Any, k_E: int | float = int, k_CH: int | float = int, method: str = str) -> @Todo) | ((Lab_hdr: @Todo, illuminant: @Todo = Any, Y_s: @Todo = float, Y_abs: @Todo = int, method: str = str) -> @Todo) | ((ICaCb: @Todo) -> @Todo) | ((ICtCp: @Todo, illuminant: @Todo = Any, chromatic_adaptation_transform: @Todo | str | None = str, method: str = str, L_p: int | float = int) -> @Todo) | ((IgPgTg: @Todo) -> @Todo) | ((IPT: @Todo) -> @Todo) | ((XYZ_D65: @Todo, constants: Structure = Structure) -> @Todo) | ((Jzazbz: @Todo, constants: Structure = Structure) -> @Todo) | ((IPT_hdr: @Todo, Y_s: @Todo = float, Y_abs: @Todo = int, method: str = str) -> @Todo) | ((Ljg: @Todo, optimisation_kwargs: dict[Unknown, Unknown] | None = None) -> @Todo) | ((ProLab: @Todo, illuminant: @Todo = Any) -> @Todo) | ((Iab: @Todo) -> @Todo) | ((Yrg: @Todo) -> @Todo) | ((uvV: @Todo) -> @Todo) | ((uvL: @Todo, illuminant: @Todo = Any) -> @Todo) | ((RGB: @Todo) -> @Todo) | ((HSV: @Todo) -> @Todo) | ((HSL: @Todo) -> @Todo) | ((HCL: @Todo, gamma: int | float = int, Y_0: int | float = int) -> @Todo) | ((HYS: @Todo) -> @Todo) | ((CMY: @Todo) -> @Todo) | ((CMYK: @Todo) -> @Todo) | ((Lrgb: @Todo) -> @Todo) | ((YCbCr: @Todo, K: @Todo = Any, in_bits: int = int, in_legal: bool = bool, in_int: bool = bool, out_bits: int = int, out_legal: bool = bool, out_int: bool = bool, clamp_int: bool = bool, **kwargs: Any) -> @Todo) | ((YcCbcCrc: @Todo, in_bits: int = int, in_legal: bool = bool, in_int: bool = bool, is_12_bits_system: bool = bool, **kwargs: Any) -> @Todo) | ((YCoCg: @Todo) -> @Todo) | ((value: @Todo, function: @Todo | str = str, **kwargs: Any) -> @Todo) | ((HEX: @Todo) -> @Todo) | ((keyword: str) -> @Todo) | ((xyY: @Todo, hue_decimals: int = int, value_decimals: int = int, chroma_decimals: int = int) -> str | @Todo) | ((munsell_colour: @Todo) -> @Todo) | (Overload[(sd_test: SpectralDistribution, additional_data: bool = bool, method: str = EllipsisType) -> ColourRendering_Specification_CRI, (sd_test: SpectralDistribution, *, additional_data: bool, method: str = EllipsisType) -> int | float, (sd_test: SpectralDistribution, additional_data: bool, method: str = EllipsisType) -> int | float]) | (Overload[(sd_test: SpectralDistribution, *, additional_data: bool, method: str = EllipsisType) -> int | float, (sd_test: SpectralDistribution, additional_data: bool = bool, method: str = EllipsisType) -> ColourRendering_Specification_CQS, (sd_test: SpectralDistribution, additional_data: bool, method: str = EllipsisType) -> int | float]) | ((CCT_D_uv: @Todo) -> @Todo) | ((mired: @Todo) -> @Todo) | ((specification: CAM_Specification_CIECAM02) -> @Todo) | ((JMh: @Todo) -> CAM_Specification_CIECAM02) | ((specification: CAM_Specification_CAM16) -> @Todo) | ((JMh: @Todo) -> CAM_Specification_CAM16) | ((specification: CAM_Specification_CIECAM16) -> @Todo) | ((JMh: @Todo) -> CAM_Specification_CIECAM16) | ((specification: CAM_Specification_Hellwig2022) -> @Todo) | ((JMh: @Todo) -> CAM_Specification_Hellwig2022) | ((specification: CAM_Specification_sCAM) -> @Todo) | ((JMh: @Todo) -> CAM_Specification_sCAM) | ((JMh: @Todo) -> @Todo) | ((Jpapbp: @Todo) -> @Todo)`
```

That's a ridiculously long display for a type, and it's needlessly long. The user has all the information they need after they're presented with the first union element that's not assignable to `str`. We should instead display something like:

```
[error] invalid-argument-type - :1121:30 - Argument is incorrect: Expected `str`, found `Unknown | str | ((sd: @Todo | SpectralDistribution | MultiSpectralDistributions, cmfs: MultiSpectralDistributions | None = None, illuminant: SpectralDistribution | None = None, k: @Todo | None = None, method: str = str, **kwargs: Any) -> @Todo) | (...30 union elements omitted)`
```

(I didn't actually count how many elements there are in that very long union, but you get the picture.)

### Version

_No response_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-04 17:11_

---

_Closed by @AlexWaygood on 2025-10-08 10:21_

---

_Comment by @adamjstewart on 2026-01-12 12:08_

Would it be possible to add a `ty check --output-format=verbose` option to optionally see the full string? I have a case where the type I'm trying to use is near the end of the union type but I can't see it in the error message.

---

_Comment by @AlexWaygood on 2026-01-12 12:11_

You can use `reveal_type` to see the full type -- does that work for you?

I'm not necessarily opposed to adding `--output-format=verbose`, but it's good to avoid too much complexity in our configuration where possible

---

_Comment by @adamjstewart on 2026-01-12 13:11_

`reveal_type` works for the actual type, not the expected type. Although I guess I could throw the expected type hint class in...

---

_Comment by @MichaReiser on 2026-01-12 13:16_

> I'm not necessarily opposed to adding --output-format=verbose, but it's good to avoid too much complexity in our configuration where possible

I'd prefer to use `-v` instead. We've other diagnostics that output more verbose information when running `ty check -v`. It avoids introducing a new concept.

---

_Comment by @AlexWaygood on 2026-01-12 13:17_

That works for me

---
