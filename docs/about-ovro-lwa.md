# About OVRO-LWA

OVRO-LWA is an all-sky radio imager by design and can observe the Sun whenever it is above the horizon. The array is located in a valley surrounded by mountains, so the Sun is not visible when its line-of-sight passes through terrain. When the Sun is at very low elevations, array performance degrades and terrestrial radio interference increases. For routine solar observations, data are taken when the solar elevation is greater than approximately **15 degrees**, giving roughly **7–14 hours** of coverage per day depending on season.

The highest time–frequency resolution routinely available is **1 ms** and **24 kHz**, though the actual resolution depends on observing mode and data-rate limits.

Browse live products and status at the **[LWA Data Portal](https://ovsa.njit.edu/lwa/)**. Full product definitions are documented on the [EOVSA Wiki — OVRO-LWA Solar Data Products](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products).

## Observing modes

OVRO-LWA runs multiple modes simultaneously by splitting the raw 352-antenna data stream into parallel processing paths:

| Mode | Description | Storage |
|------|-------------|---------|
| **Beamforming spectroscopy** | 256 core antennas form a ~1° synthesized beam tracking the Sun, producing full-Stokes dynamic spectra at 24 kHz spectral resolution (3072 channels, 13.4–86.9 MHz) and down to 1 ms time resolution. Regular solar ops use 64 ms cadence. | HDF on the data server; quicklook PNGs on the status page |
| **Standard interferometric imaging** ("slow visibilities") | Full 352-element array correlated at all 3072 frequencies, 10 s time resolution. Ideal for active regions, coronal holes, and slowly varying emission. | CASA Measurement Sets in a 7-day rolling buffer (~17 TB/day) |
| **Bursty interferometric imaging** ("fast visibilities") | 48-antenna subset at 768 frequencies (96 kHz resolution), 0.1 s cadence. Targets rapidly varying bursts (type II/III). Pipeline under construction. | CASA Measurement Sets in a 3-day rolling buffer (~9 TB/day) |

Level-0 beamforming spectrograms are generated in near real time (< 1 s latency) and are partially calibrated — close to true flux scale, but without non-solar background subtraction or primary-beam correction.

## Data product levels

| Level | Description |
|-------|-------------|
| **Level 0** | Raw beamforming HDF spectrograms; raw interferometric visibilities (MS format) |
| **Level 1.0** | Calibrated spectrograms (FITS, Stokes I) and spectral images (HDF, complex gain + bandpass + flux calibrated) |
| **Level 1.5** | Additional corrections: background subtraction and primary-beam correction for spectrograms; ionospheric refraction correction for images during quiet-Sun periods |

## Data release v1.0 summary

Current public release (**v1.0**, from 2025-09-01, data from 2024-04-01 onward) includes Stokes I products only. A pipeline paper with full processing details is in preparation.

| Product | Level | Format | Cadence / resolution | Data rate |
|---------|-------|--------|----------------------|-----------|
| All-day total-power spectrograms | 1.0 / 1.5 | FITS | 256 ms, 96 kHz (13.4–86.9 MHz, 768 ch) | ~0.6 GB/day |
| Fine-channel spectral images | 1.0 / 1.5 | HDF | 144 ch, 32–87 MHz, 10 s integration, 20–60 s cadence | ~20–45 GB/day |
| Band-averaged (MFS) spectral images | 1.0 / 1.5 | HDF | 12 ch, 10 s integration, 20–60 s cadence | ~2–4 GB/day |

Naming conventions follow patterns such as:

- `ovro-lwa.lev1_bmf_256ms_96kHz.YYYY-MM-DD.dspec_I.fits`
- `ovro-lwa-352.lev1_fch_10s.YYYY-MM-DDTHHMMSSZ.image_I.hdf`
- `ovro-lwa-352.lev1_mfs_10s.YYYY-MM-DDTHHMMSSZ.image_I.hdf`

Level 1.5 filenames use `lev1.5` in place of `lev1`.

## Further reading

- [OVRO-LWA Solar Data Products (EOVSA Wiki)](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products)
- [LWA Data Portal](https://ovsa.njit.edu/lwa/)
