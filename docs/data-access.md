# Data Access

OVRO-LWA solar data products are publicly available through the AWS Open Data Registry and the LWA Data Portal.

## AWS Open Data Registry

The [OVRO LWA Solar Observation dataset](https://registry.opendata.aws/ovrolwasolar/) is hosted on Amazon S3 in **us-west-2** under the bucket `ovro-lwa-solar`. No AWS account is required for read access.

### Bucket layout

| Path prefix | Contents |
|-------------|----------|
| `image_hdf/YYYY/MM/DD/` | Spectral image HDF5 products (fine-channel and MFS) |
| `spec_fits/YYYY/` | All-day total-power spectrogram FITS files |

### Quick access

List the bucket contents:

```bash
aws s3 ls --no-sign-request s3://ovro-lwa-solar/
```

Copy a specific file:

```bash
aws s3 cp --no-sign-request \
  s3://ovro-lwa-solar/spec_fits/2024/ovro-lwa.lev1_bmf_256ms_96kHz.2024-04-01.dspec_I.fits \
  .
```

### Dataset properties

| Property | Value |
|----------|-------|
| License | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Current size | ~12 TB |
| Growth rate | ~9 TB/year |
| Update frequency | New observations added regularly |
| Managed by | New Jersey Institute of Technology |
| Contact | Peijin Zhang — peijin.zhang@njit.edu |

### SNS notifications

Subscribe to new-data notifications via the SNS topic:

```
arn:aws:sns:us-west-2:817294255270:ovro-lwa-solar-object_created
```

### Tutorials

- [Get To Know a Dataset: OVRO LWA Solar Observation](https://registry.opendata.aws/ovrolwasolar/) — introductory walkthrough
- [Reading and Plotting OVRO-LWA Spectral Image HDF Data](https://registry.opendata.aws/ovrolwasolar/) — HDF image tutorial

## LWA Data Portal

The **[LWA Data Portal](https://ovsa.njit.edu/lwa/)** provides a web interface for browsing recent spectrograms, spectral images, and quicklook products from the realtime pipeline. Use it to inspect current observing status and download individual files.

## How to cite

When using this dataset, cite the registry entry and associated OVRO-LWA publications:

> OVRO LWA Solar Observation was accessed on `DATE` from https://registry.opendata.aws/ovrolwasolar/.

Product documentation: [OVRO-LWA Solar Data Products (EOVSA Wiki)](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products).

## Data release versions

| Version | Release date | Data period | Documentation |
|---------|--------------|-------------|---------------|
| v1.0 | 2025-09-01 | 2024-04-01 – present | [EOVSA Wiki](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products) |

## Useful links

- [LWA Data Portal](https://ovsa.njit.edu/lwa/)
- [AWS Open Data Registry — OVRO LWA Solar Observation](https://registry.opendata.aws/ovrolwasolar/)
- [OVRO-LWA Solar Data Products (EOVSA Wiki)](https://www.ovsa.njit.edu/wiki/index.php/OVRO-LWA_Solar_Data_Products)
