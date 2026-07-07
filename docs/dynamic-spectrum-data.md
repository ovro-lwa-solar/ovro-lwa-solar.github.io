# Dynamic Spectrum Data

OVRO-LWA beamforming spectrograms provide a continuous, all-day record of solar radio flux from sunrise to sunset. The beamformer uses 256 antennas in the array core to synthesize a beam of more than 1° that tracks the Sun, yielding full-Stokes dynamic spectra without spatial resolution.

## Raw (Level 0)

Beamforming data are written at full spectral resolution (24 kHz, 3072 channels from 13.4–86.9 MHz) with a regular solar observing cadence of **64 ms**. Raw spectrograms are stored permanently in **HDF** on the OVRO data server.

Level-0 products are partially calibrated: they have a close-to-true flux scale but still contain non-solar background and have not been corrected for the primary-beam response. Quicklook PNG spectrograms are shown on the [LWA Data Portal](https://ovsa.njit.edu/lwa/) observing status page with less than 1 s of latency.

## Level 1.0 — Calibrated spectrograms

Level-1 total-power spectrograms convert raw HDF data into standard **FITS** tables in Stokes I. Each file contains the frequency list, time stamps, and flux density in solar flux units (SFU).

Published products are binned to:

- **256 ms** time resolution (binning factor of 4)
- **96 kHz** frequency resolution (768 channels, 13.4–86.9 MHz)

No additional calibration beyond the Level-0 partial calibration is applied at this stage.

**Naming:** `ovro-lwa.lev1_bmf_256ms_96kHz.YYYY-MM-DD.dspec_I.fits`

## Level 1.5 — Background and beam corrections

Level 1.5 spectrograms add two corrections on top of Level 1.0:

1. **Background subtraction** — each OVRO-LWA cross-dipole responds to the entire sky (horizon to horizon), so all sky sources contribute flux. The non-Solar background is estimated and subtracted.
2. **Primary-beam correction** — dipole gain varies across the sky; the effective beamforming gain to the Sun depends on solar altitude and frequency. Time- and frequency-dependent correction parameters (from the current primary-beam model) are stored in the FITS header.

**Naming:** `ovro-lwa.lev1.5_bmf_256ms_96kHz.YYYY-MM-DD.dspec_I.fits`

## Reading and plotting

### Python

An example notebook for reading and plotting Level 1/1.5 spectrograms is available on Google Colab (linked from the [EOVSA Wiki](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products)). Required packages: **Astropy**, **NumPy**, **Matplotlib**.

### SSW/IDL

A minimal SSW/IDL example is planned.

## Data access

All-day spectrogram FITS files are published under `spec_fits/YYYY/` on the public S3 bucket. See [Data Access](data-access.md) for download instructions.

| Property | Value |
|----------|-------|
| Format | FITS |
| Stokes | I |
| Spectral range | 13.4–86.9 MHz |
| Channels | 768 (96 kHz resolution) |
| Cadence | 256 ms |
| Data rate | ~0.6 GB/day |

## Further reading

- [OVRO-LWA Solar Data Products — spectrograms (EOVSA Wiki)](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products)
- [AWS Registry — OVRO LWA Solar Observation](https://registry.opendata.aws/ovrolwasolar/)
