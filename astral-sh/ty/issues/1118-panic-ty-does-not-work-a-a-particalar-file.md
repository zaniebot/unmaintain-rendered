```yaml
number: 1118
title: "[panic] ty does not work a a particalar file"
type: issue
state: closed
author: sehHeiden
labels: []
assignees: []
created_at: 2025-09-03T08:44:50Z
updated_at: 2025-09-03T14:57:59Z
url: https://github.com/astral-sh/ty/issues/1118
synced_at: 2026-01-12T15:54:24Z
```

# [panic] ty does not work a a particalar file

---

_@sehHeiden_

I start ty with:

```
uv run ty check .
```
The platform is amd64 with Win10.
Python 3.13.7

I get this error message:

```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 66/66 files
error[panic]: Panicked at C:\Users\runneradmin\.cargo\git\checkouts\salsa-e6f3bb7c2a062968\918d35d\src\function\execute.rs:215:25 when checking path to project\cli\patch_generator.py`: `infer_definition_types(Id(9da9a)): execute: too many cycle iterations`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: windows x86_64
info: Version: 0.0.1-alpha.19 (e9cb838b3 2025-08-19)
info: Args: ["patch to project\\.venv\\Scripts\\ty.exe", "check", "."]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: infer_definition_types(Id(9dadc))
             at crates\ty_python_semantic\src\types\infer.rs:167
   1: place_by_id(Id(554f6))
             at crates\ty_python_semantic\src\place.rs:688
   2: Type < 'db >::member_lookup_with_policy_(Id(2708b))
             at crates\ty_python_semantic\src\types.rs:635
   3: infer_definition_types(Id(996f0))
             at crates\ty_python_semantic\src\types\infer.rs:167
   4: infer_deferred_types(Id(9d9aa))
             at crates\ty_python_semantic\src\types\infer.rs:207
   5: ClassLiteral < 'db >::explicit_bases_(Id(2a41e))
             at crates\ty_python_semantic\src\types\class.rs:1230
   6: ClassLiteral < 'db >::is_typed_dict_(Id(2a41e))
             at crates\ty_python_semantic\src\types\class.rs:1230
   7: infer_deferred_types(Id(9300a))
             at crates\ty_python_semantic\src\types\infer.rs:207
   8: FunctionType < 'db >::signature_(Id(674ad))
             at crates\ty_python_semantic\src\types\function.rs:642
   9: infer_scope_types(Id(891e))
             at crates\ty_python_semantic\src\types\infer.rs:138
  10: check_file_impl(Id(c0f))
             at crates\ty_project\src\lib.rs:527
```


The repo is private therefore I paste my (own) code here:
```
#!/usr/bin/env python3
"""Simple patch generator for training data."""

from collections import defaultdict
from concurrent.futures import ProcessPoolExecutor, as_completed
from pathlib import Path

import fiona
import geopandas as gpd
import numpy as np
import rioxarray as rxr
import xarray as xr
from pydantic import field_validator
from pydantic_settings import BaseSettings
from rasterio.features import rasterize
from shapely.geometry import box
from tqdm import tqdm


class Config(BaseSettings):
    """Configuration for patch generation."""

    patch_size: int = 512
    raster_dir: Path = Path("data/ground_truth/tile_gt_dop20/training")
    annotation_dir: Path = Path("data/ground_truth/gpkg_gt_dop20")
    output_dir: Path = Path("data/patches")
    filter_annotated: bool = True
    min_area: float = 1.6
    raster_pattern: str = "*.zstd.tif"
    max_workers: int = 4
    model_config = {"env_file": "config/generate_patch.env"}

    @field_validator("filter_annotated", mode="before")
    @classmethod
    def parse_bool(cls, value: str | bool) -> bool:
        """Parse boolean values from environment variables."""
        if isinstance(value, str):
            return value.lower() in ("true", "1", "yes", "on")
        return bool(value)


def _to_dataarray(data: xr.DataArray | xr.Dataset) -> xr.DataArray:
    """Normalize raster input to a squeezed DataArray."""
    if isinstance(data, xr.DataArray):
        return data.squeeze()
    if isinstance(data, xr.Dataset):
        return next(iter(data.data_vars.values())).squeeze()
    error_msg = f"Unexpected type: {type(data)}"
    raise TypeError(error_msg)


def print_stats(annotation_files: list[Path]) -> None:
    """Print annotation statistics."""
    stats = [(f.name, *_get_file_stats(f)) for f in annotation_files if f.exists()]
    for name, objects, area in stats:
        print(f"{name}: {objects} objects, {area:,.0f} m²")
    print(f"Total: {sum(s[1] for s in stats)} objects, {sum(s[2] for s in stats):,.0f} m²")


def _get_file_stats(gpkg_path: Path) -> tuple[int, float]:
    layers = [layer for layer in fiona.listlayers(str(gpkg_path)) if layer != "layer_styles"]
    dfs = [gpd.read_file(gpkg_path, layer=layer) for layer in layers]
    return sum(len(df) for df in dfs), sum(df.geometry.area.sum() for df in dfs)


def collect_mask_files(masks_dir: Path) -> list[Path]:
    """Collect all mask files from training, testing, and validation splits."""
    if not masks_dir.exists():
        return []

    splits = ["training", "testing", "validation"]
    mask_files: list[Path] = []
    for split in splits:
        split_dir = masks_dir / split
        if split_dir.exists():
            mask_files.extend(split_dir.glob("*.tif"))
    return mask_files


def calculate_class_ratio(masks: np.ndarray) -> float | None:
    """Calculate the annotated class ratio from stacked mask arrays."""
    if masks.size == 0:
        return None

    positive = np.sum(masks > 0)
    total = masks.size

    return positive / total


def calculate_prevalence_by_split(masks_dir: Path) -> tuple[dict[str, float | None], float | None]:
    """Calculate annotated class prevalence grouped by split and overall."""
    splits = ["training", "testing", "validation"]
    prevalence_by_split: dict[str, float | None] = {}
    all_positive = all_total = 0

    for split in splits:
        split_dir = masks_dir / split
        mask_files = list(split_dir.glob("*.tif")) if split_dir.exists() else []
        if not mask_files:
            prevalence_by_split[split] = None
            continue

        mask_arrays = [_to_dataarray(rxr.open_rasterio(mf)).values.squeeze() for mf in mask_files]  # type: ignore[arg-type]
        stacked = np.stack(mask_arrays)
        prevalence_by_split[split] = calculate_class_ratio(stacked)

        all_positive += int(np.sum(stacked > 0))
        all_total += stacked.size

    overall = all_positive / all_total if all_total > 0 else None
    return prevalence_by_split, overall


def print_prevalence_stats(prevalence_by_split: dict[str, float | None], overall_prevalence: float | None) -> None:
    """Print prevalence statistics grouped by split and overall."""
    print("\nClass Prevalence by Split:")

    # Print individual split statistics
    for split, ratio in prevalence_by_split.items():
        if ratio is not None:
            print(f"  {split.capitalize()}: {ratio * 100:.2f}% annotated")
        else:
            print(f"  {split.capitalize()}: No masks found")

    # Print overall prevalence (true weighted average)
    if overall_prevalence is not None:
        print(f"\n  Overall (all ground truth): {overall_prevalence * 100:.2f}% annotated")
    else:
        print("\n  Overall: No data available")


def _count_positive_pixels(mask_path: Path) -> int:
    mask = _to_dataarray(rxr.open_rasterio(mask_path))  # type: ignore[arg-type]
    return int((mask > 0).sum().item())


def _count_total_pixels(mask_path: Path) -> int:
    mask = _to_dataarray(rxr.open_rasterio(mask_path))  # type: ignore[arg-type]
    return mask.sizes["y"] * mask.sizes["x"]


def process_raster(raster_path: Path, config: Config) -> tuple[str, int]:
    """
    Process a single raster file.

    Returns:
        Tuple of (split_name, patch_count)

    """
    raster = _to_dataarray(rxr.open_rasterio(raster_path))  # type: ignore[arg-type]
    layer_name = raster_path.stem.replace(".zstd", "")
    split = _get_split(raster_path)
    annotations = _load_annotations(config.annotation_dir / f"{split}.gpkg", layer_name)

    if config.filter_annotated and (annotations is None or annotations.empty):
        return split, 0

    # Setup patch grid and output dirs
    h, w = raster.sizes["y"], raster.sizes["x"]
    ny, nx = h // config.patch_size, w // config.patch_size
    y_off, x_off = (h - ny * config.patch_size) // 2, (w - nx * config.patch_size) // 2

    out_dirs = {k: config.output_dir / k / split for k in ["rasters", "masks", "annotations"]}
    for d in out_dirs.values():
        d.mkdir(parents=True, exist_ok=True)

    patches = 0
    for i in range(ny):
        for j in range(nx):
            y1 = y_off + i * config.patch_size
            x1 = x_off + j * config.patch_size
            patch = raster.isel(y=slice(y1, y1 + config.patch_size), x=slice(x1, x1 + config.patch_size))
            name = f"{layer_name}_{y1:05d}_{x1:05d}"

            # Get annotations for this patch
            clipped = gpd.clip(annotations, box(*patch.rio.bounds())) if annotations is not None and not annotations.empty else None
            if config.filter_annotated and (clipped is None or len(clipped) == 0) and (clipped.geometry.area.sum() < config.min_area):
                continue

            generate_patch(patch, name, out_dirs["rasters"])
            generate_annotations(clipped, name, out_dirs["annotations"])
            generate_mask(clipped, patch, name, out_dirs["masks"], config.patch_size)
            patches += 1

    split = _get_split(raster_path)
    return split, patches


def generate_patch(patch: xr.DataArray, name: str, output_dir: Path) -> None:
    """Generate and save a raster patch."""
    patch.rio.to_raster(output_dir / f"{name}.tif")


def generate_annotations(clipped_annotations: gpd.GeoDataFrame | None, name: str, output_dir: Path) -> None:
    """Generate and save clipped annotations."""
    if clipped_annotations is not None and len(clipped_annotations) > 0:
        clipped_annotations.to_file(output_dir / f"{name}.gpkg", driver="GPKG")


def generate_mask(clipped_annotations: gpd.GeoDataFrame | None, patch: xr.DataArray, name: str, output_dir: Path, patch_size: int) -> None:
    """Generate and save a mask from annotations."""
    if clipped_annotations is not None and len(clipped_annotations) > 0:
        mask = rasterize(
            [(g, 1) for g in clipped_annotations.geometry],
            out_shape=(patch_size, patch_size),
            transform=patch.rio.transform(),
            fill=0,
            dtype="uint8",
        )
    else:
        mask = np.zeros((patch_size, patch_size), dtype="uint8")

    mask_da = xr.DataArray(mask[None], dims=["band", "y", "x"], coords={"band": [1], "y": patch.y, "x": patch.x})
    mask_da.rio.write_crs(patch.rio.crs, inplace=True)
    mask_da.rio.to_raster(output_dir / f"{name}.tif")


def _get_split(raster_path: Path) -> str:
    return next((s for s in ["training", "testing", "validation"] if s in str(raster_path)), "validation")


def _load_annotations(annotation_file: Path, layer_name: str) -> gpd.GeoDataFrame | None:
    if not (annotation_file.exists() and layer_name in fiona.listlayers(str(annotation_file))):
        return None
    annotations = gpd.read_file(annotation_file, layer=layer_name)
    annotations.geometry = annotations.geometry.buffer(0)
    return annotations


def main() -> None:
    """Run patch generation."""
    config = Config()
    print(config.model_dump(), "\n")

    print_stats(list(config.annotation_dir.glob("*.gpkg")))

    rasters = list(config.raster_dir.rglob(config.raster_pattern))
    print(f"\nFound {len(rasters)} raster files")

    # Process rasters in parallel with a progress bar
    patch_counts_by_split: dict[str, int] = defaultdict(int)
    with ProcessPoolExecutor(max_workers=config.max_workers) as executor:
        # Submit all tasks
        future_to_raster = {executor.submit(process_raster, raster, config): raster for raster in rasters}

        # Process completed tasks with a progress bar
        with tqdm(total=len(rasters), desc="Processing rasters", unit="files") as pbar:
            for future in as_completed(future_to_raster):
                raster_path = future_to_raster[future]
                try:
                    split, patch_count = future.result()
                    patch_counts_by_split[split] += patch_count
                except OSError as exc:
                    print(f"\nError processing {raster_path.name}: {exc}")
                finally:
                    pbar.update(1)

    # Print results grouped by split
    print("\nPatches generated by split:")
    total_patches = 0
    for split in ["training", "testing", "validation"]:
        count = patch_counts_by_split.get(split, 0)
        print(f"  {split.capitalize()}: {count:,} patches")
        total_patches += count
    print(f"  Total: {total_patches:,} patches")

    # Calculate and print prevalence grouped by split
    print("\nCalculating prevalence...")
    prevalence_by_split, overall_prevalence = calculate_prevalence_by_split(config.output_dir / "masks")
    print_prevalence_stats(prevalence_by_split, overall_prevalence)


if __name__ == "__main__":
    main()
```
I also use mypy. Got no problem with the code so far. 
If I remember correctly, it started after adding the ProcessPoolExecutor. 

---

_Comment by @sharkdp on 2025-09-03 09:15_

Thank you for reporting this. I narrowed this down to:

```py
import xarray as xr

def _(mask: xr.DataArray):
    mask > 0
```

It is very likely that this is related to #256 

---

_Comment by @sehHeiden on 2025-09-03 11:07_

Wonderful, I did not know how to narrow it down.
Beside, do a full bisect. 

---

_Closed by @sharkdp on 2025-09-03 14:26_

---

_Reopened by @carljm on 2025-09-03 14:57_

---

_Closed by @carljm on 2025-09-03 14:57_

---
