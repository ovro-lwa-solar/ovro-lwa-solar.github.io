# Imaging Data

OVRO-LWA standard interferometric imaging products are produced by the [`lwasolarproc`](https://github.com/peijin94/lwasolarproc) pipeline from slow visibilities recorded by the full 352-element array. Shared FITS wrapping, refraction-correction helpers, and quicklook visualization live in the companion package [`lwa-solar-util`](https://github.com/peijin94/lwa-solar-util) (`lwasolarutl`).

## Pipeline overview

`lwasolarproc` handles preprocessing, WSClean imaging, and realtime orchestration. The full-band workflow (`lwasolarproc-fullband`) performs:

1. Copy or reuse Measurement Sets
2. Apply H5Parm calibration with DP3
3. Run AOFlagger with the bundled `LWA_sun_PZ.lua` strategy
4. Average by 4 channels
5. Phase-only self-calibration (WSClean model visibilities + DP3 `gaincal`)
6. Bright-source subtraction beyond a configurable exclusion radius around the Sun
7. Recenter on the Sun
8. Average by 4 channels again
9. Image MFS Stokes I/V and fine-channel Stokes I with WSClean
10. Convert J2000 FITS products to helioprojective coordinates with beam correction
11. Optionally apply ionospheric refraction correction (`--do-refraction`) to produce Level 1.5 Stokes I products
12. Combine FITS products and write MFS quicklook plots

A realtime task manager (`lwasolarproc-realtime`) watches incoming slow visibilities and publishes FITS, HDF, and PNG products on a rolling basis.

## Product types

### Fine-channel spectral images (FCH)

Independent images at **144 frequency channels** equally spaced between 32 and 87 MHz. Each image integrates 16 channels (384 kHz effective resolution) with a 10 s integration time. Cadence is typically 20–60 s (full 10 s cadence for selected events).

| Level | Naming pattern |
|-------|----------------|
| 1.0 | `ovro-lwa-352.lev1_fch_10s.YYYY-MM-DDTHHMMSSZ.image_I.hdf` |
| 1.5 | `ovro-lwa-352.lev1.5_fch_10s.YYYY-MM-DDTHHMMSSZ.image_I.hdf` |

### Band-averaged MFS spectral images

Multi-frequency synthesis combines visibilities into **12 images** at center frequencies of 34.1, 38.7, 43.2, 47.8, 52.4, 57.0, 61.6, 66.2, 70.8, 75.4, 80.0, and 84.6 MHz (4.6 MHz effective resolution, 10 s integration).

| Level | Naming pattern |
|-------|----------------|
| 1.0 | `ovro-lwa-352.lev1_mfs_10s.YYYY-MM-DDTHHMMSSZ.image_I.hdf` |
| 1.5 | `ovro-lwa-352.lev1.5_mfs_10s.YYYY-MM-DDTHHMMSSZ.image_I.hdf` |

### Level 1.5 refraction correction

When `--do-refraction` is enabled, the pipeline fits frequency-dependent source shifts (expected ∝ 1/f²) using the quiet-Sun disk as a reference and applies the correction to combined Stokes I products. Frames with bright bursts are skipped. Level 1.5 image products are not always available during periods of strong solar activity.

Images are in heliocentric coordinates with a typical dynamic range exceeding 300. FITS conversion utilities are provided for loading into Python or SSW/IDL.

## Realtime operations

The realtime manager (`lwasolarproc-realtime`) supports three modes:

| Mode | Purpose |
|------|---------|
| `realtime` | Follow live slow visibilities; queue frames when solar elevation ≥ 13.5° |
| `backlog` | Replay a fixed UTC timestamp range from the structured slow-data tree |
| `event` | Process a flat event folder of `.ms` or `.ms.tar` archives |

Published products land under dated directory trees:

```
proc_out/{fits,hdf,fig}/slow/lev1/YYYY/MM/DD/
```

Operational Lustre ingest uses `--ingest-lustre` to publish under `/lustre/solarpipe/realtime_pipeline/`.

Default production bands: 13, 18, 23, 27, 32, 36, 41, 46, 50, 55, 59, 64, 69, 73, 78, 82 MHz.

## Reading and plotting

### Python

Example notebooks for reading and plotting Level 1/1.5 spectral images are linked from the [EOVSA Wiki](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products). Required packages: **Astropy**, **SunPy**, **h5py**, **SciPy**, **NumPy**, **Matplotlib**.

Quicklook plotting from FITS:

```python
import matplotlib
matplotlib.use("Agg")
from lwasolarutl.visualization import slow_pipeline_default_plot

fig, axes = slow_pipeline_default_plot("image_I.fits", add_logo=False)
fig.savefig("image_I.quicklook.png", dpi=150, bbox_inches="tight")
```

FITS ↔ HDF5 round trip:

```python
from lwasolarproc import compress_fits_to_h5, recover_fits_from_h5

h5_path = compress_fits_to_h5("image.fits", "image.hdf")
recover_fits_from_h5(h5_path, fits_out="image.recovered.fits")
```

## Data rates

| Product | Level | Data rate |
|---------|-------|-----------|
| Fine-channel images | 1.0 | ~45 GB/day |
| Fine-channel images | 1.5 | ~20–40 GB/day |
| MFS images | 1.0 | ~4 GB/day |
| MFS images | 1.5 | ~2–4 GB/day |

## Further reading

- [`lwasolarproc` on GitHub](https://github.com/peijin94/lwasolarproc)
- [OVRO-LWA Solar Data Products — imaging (EOVSA Wiki)](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products)
- [AWS tutorial — Reading and Plotting OVRO-LWA Spectral Image HDF Data](https://registry.opendata.aws/ovrolwasolar/)
