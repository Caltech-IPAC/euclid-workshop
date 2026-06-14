# Euclid Data in the Cloud

Hands-on materials for the AAS 248 workshop **"Euclid Data in the Cloud: Access, Analysis, and Science Opportunities"** — Sunday June 14, 2026.

The workshop walks through how to pull Euclid Q1 data products out of the public IRSA cloud (TAP, SIA, direct S3 reads) and put them to work on real science: galaxy cluster identification and AGN selection. Total runtime is 90 minutes.

## Workshop arc

| # | Item | Format | Time |
| --- | --- | --- | --- |
| 1 | **[`1_Intro.pdf`](1_Intro.pdf)** — workshop framing, Euclid mission overview, what Q1 is, data-product acronyms (VIS, NISP, MER, SIR, SPE, PHZ). | slides | 15 min |
| 2 | **[`2_fornaxintro-desai.pdf`](2_fornaxintro-desai.pdf)** — NASA Fornax science platform overview and sign-up (Desai). | slides | 5 min |
| 3 | **[`3_clouddata_raen.pdf`](3_clouddata_raen.pdf)** — cloud data access on IRSA: TAP, SIA, and direct S3 reads (Raen). | slides | 10 min |
| 4 | **[`4_Euclid_data_cloud.ipynb`](4_Euclid_data_cloud.ipynb)** — TAP / SIA / direct-S3 patterns demonstrated on a single EDF-N source: MER catalog, MER mosaic cutout, SIR 1D spectrum, SPE line features. Closes with how to discover tables and columns yourself and a pointer to HATS for population-scale work. | notebook | 10 min |
| 5 | **[`5_Euclid_clusters.ipynb`](5_Euclid_clusters.ipynb)** — applies the same toolkit to a high-richness Q1 galaxy cluster. ADQL `JOIN` across MER and PHZ for candidate members, DBSCAN against a control field 5′ away, over-density overlaid on H-band imaging. | notebook | 20 min |
| 6 | **[`6_Euclid_AGN_assignment.ipynb`](6_Euclid_AGN_assignment.ipynb)** — 30-minute hands-on. Projects ~5k EDF-N AGN candidates onto a pre-trained color-SOM, compares selection methods (WISE, Gaia, DESI, …) on the same manifold, and verifies one candidate with three live cloud reads. [`6_Euclid_AGN_solutions.ipynb`](6_Euclid_AGN_solutions.ipynb) is the same notebook with every TODO filled in. | notebook | 30 min |

## Quick start

```bash
git clone <repo-url>
cd euclid-workshop
pip install -r requirements.txt
jupyter lab          # or: jupyter notebook
```

Open the notebooks in order (4 → 5 → 6). Everything except the AGN notebook's pre-trained SOM is fetched live from IRSA at run time, so you need network access during the workshop.

## Prerequisites

- **Python ≥ 3.10.** Tested against 3.12.
- **Packages** — `astropy`, `astroquery >= 0.4.10`, `minisom`, `numpy < 2`, `scipy < 1.13`, `matplotlib`, `s3fs`, `tqdm`, `ipywidgets`, `requests`, `scikit-learn`. All pinned in [`requirements.txt`](requirements.txt).
- **Network access** to `irsa.ipac.caltech.edu` (TAP, SIA, IBE) and `s3://nasa-irsa-euclid-q1` (anonymous S3). No AWS credentials required.

The same code runs unchanged from a laptop or from a NASA Fornax JupyterLab session; from Fornax (in `us-east-1`, the same AWS region as the bucket) the S3 transfers are substantially faster.

## What ships in the repo

| Path | Size | Purpose |
| --- | --- | --- |
| `data/cache/workshop_som.pkl` | 70 KB | Pre-trained Q1 color SOM (notebook 6). |
| `data/cache/workshop_train_edfn.fits` | 5.9 MB | Training-galaxy BMU positions (notebook 6). |
| `data/cache/workshop_agn_flags_edfn.fits` | 349 KB | EDF-N AGN candidate IDs, positions, redshifts, selection flags (notebook 6). |
| `data/cache/workshop_agn_edfn.fits` | 1.3 MB | Pre-projected fallback if Task 1's live TAP query fails. |
| `data/cache/irsa_spectra_agn/2668535469662563116.npz` | 12 KB | Local cache of the default-seed AGN's SIR spectrum. |
| `AAS247/euclid_q1_clusters.csv` | 4 KB | Q1 cluster catalog (notebook 5). |

Total: ~8 MB. Everything else — MER photometry, mosaic imaging, SIR spectra, SPE line measurements — is pulled live from IRSA.

## References

- Q1 cloud-access tutorial (the original, long-form): https://caltech-ipac.github.io/irsa-tutorials/euclid-cloud-access/
- Q1 cluster tutorial: https://caltech-ipac.github.io/irsa-tutorials/euclid-clusters-tutorial/
- Q1 SPE catalog tutorial: https://caltech-ipac.github.io/irsa-tutorials/euclid-intro-spe-catalog/
- Q1 HATS / Parquet-on-S3 tutorial: https://caltech-ipac.github.io/irsa-tutorials/euclid-q1-hats-intro/
- IRSA holdings for Euclid: https://irsa.ipac.caltech.edu/Missions/euclid.html
- Q1 cluster catalog: Euclid Collaboration: Bhargava et al. 2025, [arXiv:2503.19196](https://arxiv.org/abs/2503.19196)
- Q1 AGN candidate catalog: Euclid Collaboration: Matamoro Zatarain et al. 2025
